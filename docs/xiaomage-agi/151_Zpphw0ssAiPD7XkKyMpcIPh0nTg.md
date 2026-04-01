# 本地训练LORA

> 来源：[https://xmgtoagi.feishu.cn/docx/RvLjdS2dyop0fMxK3vFcRPjunLe](https://xmgtoagi.feishu.cn/docx/RvLjdS2dyop0fMxK3vFcRPjunLe)

# 成果展示

# 训练步骤

下载地址：https://github.com/cocktailpeanut/fluxgym

![](img/421ec85d83e8894a0d6bbd5aa2303e37.png)

# Supported Models

1.  Flux1-dev

1.  [Fl](https://medium.com/@zhiwangshi28/why-flux-lora-so-hard-to-train-and-how-to-overcome-it-a0c70bc59eaf)ux1-dev2pro (as explained here: https://medium.com/@zhiwangshi28/why-flux-lora-so-hard-to-train-and-how-to-overcome-it-a0c70bc59eaf)

1.  Flux1-schnell (Couldn't get high quality results, so not really recommended, but feel free to experiment with it)

1.  More?

The models are automatically downloaded when you start training with the model selected.

Y[ou can easi](https://github.com/cocktailpeanut/fluxgym/blob/main/models.yaml)ly add more to the supported models list by editing the models.yaml file. If you want to share some interesting base models, please send a PR.

##  Install Manually

First clone Fluxgym and kohya-ss/sd-scripts:

```
git clone https://github.com/cocktailpeanut/fluxgym
cd fluxgym
git clone -b sd3 https://github.com/kohya-ss/sd-scripts
```

Your folder structure will look like this:

```
/fluxgym
  app.py
  requirements.txt
  /sd-scripts
```

Now activate a venv from the root fluxgym folder:

If you're on Windows:

```
python -m venv env
env\Scripts\activate
```

If your're on Linux:

```
python -m venv env
source env/bin/activate
```

This will create an env folder right below the fluxgym folder:

```
/fluxgym
  app.py
  requirements.txt
  /sd-scripts
  /env
```

Now go to the sd-scripts folder and install dependencies to the activated environment:

```
cd sd-scripts
pip install -r requirements.txt
```

![](img/e0c3715bb0d955b367b0cefd6fd9a0eb.png)

Now come back to the root folder and install the app dependencies:

```
cd ..
pip install -r requirements.txt
```

![](img/34bbf292116db40d652139a9f74b9d35.png)

Finally, install pytorch Nightly:

```
pip install --pre torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121
```

![](img/906f11ae9d574faca6609771d7a87ca7.png)

# **Start**

Go back to the root fluxgym folder, with the venv activated, run:

```
python app.py
```

> Make sure to have the venv activated before running python app.py.

> Windows: env/Scripts/activate Linux: source env/bin/activate

By default fluxgym doesn't generate any sample images during training.

You can however configure Fluxgym to automatically generate sample images for every N steps. Here's what it looks like:

![](img/bcd843ac9a7b6ded7f2ff4909515176f.png)

To turn this on, just set the two fields:

1.  Sample Image Prompts: These prompts will be used to automatically generate images during training. If you want multiple, separate teach prompt with new line.

1.  Sample Image Every N Steps: If your "Expected training steps" is 960 and your "Sample Image Every N Steps" is 100, the images will be generated at step 100, 200, 300, 400, 500, 600, 700, 800, 900, for EACH prompt.

![](img/ba0c073a8330f8f7b42a93ec2e8d89e0.png)