
```py
import cv2 # 开源的计算机视觉库

from pathlib import Path # 提供面向对象方式的文件访问
from tqdm import tqdm # 提供进度条显示功能
import os # 提供操作系统相关的访问
import numpy as np # 提供科学计算相关的功能

from lib.cli import DirectoryProcessor, rotate_image # 处理一个目录的文件，然后保存到新的目录中；旋转图片，其实是在 utils 中
from lib.utils import get_folder # 获得一个 folder，不存在则创建
from lib.multithreading import pool_process # 多进程并发计算
from lib.detect_blur import is_blurry # 判断图片是否模糊
from plugins.PluginLoader import PluginLoader # 加载对应的算法

class ExtractTrainingData(DirectoryProcessor): # 从训练集提取头像
	def create_parser(self, subparser, command, description):
		self.optional_arguments = self.get_optional_arguments()
		self.parser = subparser.add_parser(
			command,
			help="Extract the faces from a pictures.",
			description=description,
			epilog="Questions and feedback: \
			https://github.com/deepfakes/faceswap-playground"
		)

 # 参数配置部分省略

	def process(self):
		extractor_name = "Align" # 对应的是 Extract_Align.py
		self.extractor = PluginLoader.get_extractor(extractor_name)()
		processes = self.arguments.processes
		try:
			if processes != 1: # 多进程处理图片
				files = list(self.read_directory())
				for filename, faces in tqdm(pool_process(self.processFiles, files, processes=processes), total = len(files)):
					self.num_faces_detected += 1
					self.faces_detected[os.path.basename(filename)] = faces
			else: # 单进程处理图片
				for filename in tqdm(self.read_directory()):
					try:
						image = cv2.imread(filename)
						self.faces_detected[os.path.basename(filename)] = self.handleImage(image, filename)
					except Exception as e:
						if self.arguments.verbose:
							print('Failed to extract from image: {}. Reason: {}'.format(filename, e))
						pass
		finally:
			self.write_alignments()

	def processFiles(self, filename): # 处理一个单独的图片的函数
		try:
			image = cv2.imread(filename)
			return filename, self.handleImage(image, filename)
		except Exception as e:
			if self.arguments.verbose:
				print('Failed to extract from image: {}. Reason: {}'.format(filename, e))
			pass
		return filename, []

	def getRotatedImageFaces(self, image, angle): # 得到固定角度旋转后的图片的人脸
		rotated_image = rotate_image(image, angle)
		faces = self.get_faces(rotated_image, rotation=angle)
		rotated_faces = [(idx, face) for idx, face in faces]
		return rotated_faces, rotated_image

	def imageRotator(self, image): # 得到一系列旋转后的人脸
		''' rotates the image through rotation_angles to try to find a face '''
		for angle in self.rotation_angles:
			rotated_faces, rotated_image = self.getRotatedImageFaces(image, angle)
			if len(rotated_faces) > 0:
				if self.arguments.verbose:
					print('found face(s) by rotating image {} degrees'.format(angle))
				break
		return rotated_faces, rotated_image

	def handleImage(self, image, filename):
		faces = self.get_faces(image)
		process_faces = [(idx, face) for idx, face in faces]

		# 没有找到人脸，尝试旋转图片
		if self.rotation_angles is not None and len(process_faces) == 0:
			process_faces, image = self.imageRotator(image)

		rvals = []
		for idx, face in process_faces:
			# 画出人脸的标记
			if self.arguments.debug_landmarks:
				for (x, y) in face.landmarksAsXY():
					cv2.circle(image, (x, y), 2, (0, 0, 255), -1)

			resized_image, t_mat = self.extractor.extract(image, face, 256, self.arguments.align_eyes)
			output_file = get_folder(self.output_dir) / Path(filename).stem

			# 检测图片是否模糊
			if self.arguments.blur_thresh is not None:
				aligned_landmarks = self.extractor.transform_points(face.landmarksAsXY(), t_mat, 256, 48)
				feature_mask = self.extractor.get_feature_mask(aligned_landmarks / 256, 256, 48)
				feature_mask = cv2.blur(feature_mask, (10, 10))
				isolated_face = cv2.multiply(feature_mask, resized_image.astype(float)).astype(np.uint8)
				blurry, focus_measure = is_blurry(isolated_face, self.arguments.blur_thresh)
				# print("{} focus measure: {}".format(Path(filename).stem, focus_measure))
				# cv2.imshow("Isolated Face", isolated_face)
				# cv2.waitKey(0)
				# cv2.destroyAllWindows()
				if blurry:
					print("{}'s focus measure of {} was below the blur threshold, moving to \"blurry\"".format(Path(filename).stem, focus_measure))
					output_file = get_folder(Path(self.output_dir) / Path("blurry")) / Path(filename).stem

			cv2.imwrite('{}_{}{}'.format(str(output_file), str(idx), Path(filename).suffix), resized_image) # 生成新图片
			f = {
				"r": face.r,
				"x": face.x,
				"w": face.w,
				"y": face.y,
				"h": face.h,
				"landmarksXY": face.landmarksAsXY()
			}
			rvals.append(f)
		return rvals
```

注意，基于特征标记的算法对于倾斜的脸效果不好，也可以引入 CNN。

## 人脸转换

人脸转换的基本原理是什么? 假设让你盯着一个人的视频连续看上 100 个小时，接着又给你看一眼另外一个人的照片，接着让你凭着记忆画出来刚才 的照片，你一定画的会很像第一个人的。

我们使用的模型是 `Autoencoder`。有趣的是，这个模型所做的是基于原始的图片再次生成原始的图片。`Autoencoder` 的编码器把图片进行压缩，而解码器把图片进行还原，一个示例如下图:

![](img/387fd552a8d4a1b9b5aabd17cc24eb39-fs8.png)

来源: Building Autoencoders in Keras

在这个基础上，即使我们输入的是另外一个人脸，也会被 `Autoencoder` 编码成为一个类似原来的脸。

为了提升我们最终的效果，我们还需要把人脸共性相关的属性和人脸特性相关的属性进行学习。因此，我们对所有的脸都用一个统一的编码器，这个编码器的目的是学习人脸共性的地方;然后，我们对每个脸有一个单独的解码器，这个解码器是为了学习人脸个性的地方。这样当你用 B 的脸通过 编码器，再使用 A 的解码器的话，你会得到一个与 B 的表情一致，但是 A 的脸。

这个过程用公式表示如下:

```py
X' = Decoder(Encoder(Shuffle(X)))
Loss = L1Loss(X'-X)

A' = Decoder_A(Encoder(Shuffle(A)))
Loss_A = L1Loss(A'-A)

B' = Decoder_B(Encoder(Shuffle(B)))
Loss_B = L1Loss(B'-B)
```

具体来说，在训练过程中，我们输入 A 的图片，通过编码器和解码器还原 A 的脸;然后我们输入 B 的图片，通过相同的编码器但是不同的解码器还原 B 的脸。不断迭代这个过程，直到 loss 降低到一个阈值。在模型训练的时候，我建议把 loss 降低到 0.02，这样的效果会比较好。

这里用的是比较标准的建模方式。值得注意的是，作者通过加入 `PixelShuffler()` 的函数把图像进行了一定的扭曲，而这个扭曲增加了学习的难度，反而让模型能够实现最终的效果。仔细想想这背后的道理，如果你一直在做简单的题目，那么必然不会有什么解决难题的能力。但是，我只要把题目做一些变体，就足以让你成长。

因为在建模中使用的是原图 A 的扭曲来还原 A，应用中是用 B 来还原 A，所以扭曲的方式会极大的影响到最终的结果。因此，如何选择更好的扭曲方式，也是一个重要的问题。

当我们图片融合的时候，会有一个难题，如何又保证效果又防止图片抖动。于是我们还要引入相关的算法处理这些情况。于是我们可以知道，一个看似直接的人脸转换算法在实际操作中需要考虑各种各样的特殊情况，这才是真真的接地气。

### `train.py`

以下是进行训练的算法逻辑：
