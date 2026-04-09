# [Lec13] Pipelined Processor Design

> 来源：[https://learning.feishu.cn/docx/FH6Gd29T6oUEySx1Z58czLztn2g](https://learning.feishu.cn/docx/FH6Gd29T6oUEySx1Z58czLztn2g)

2023 年，这节课是学生代讲的。所以，整合 2022 talk + 2023 slide。2022 年是 Lec14。

https://www.youtube.com/watch?v=XaW_O9nKPe0

Lecture 14: Pipelined Processor Design

ETH Zürich

Lecturer: Professor Onur Mutlu

Spring 2022

# Summary

重点是 Fine-Gained Multithreading，从 1:25:00 开始，一共 20 分钟。

前面都是 data dependence 和 control dependence 的处理。

lec19 SIMD，lec20 讲 GPU。

understand the principles of execution of a GPU。

也是 fine-gained multithreading （FGMT）

pipeline processor 和 out-of-order processor，

在 ISA 层面，都是冯诺依曼模型。

get faults and want to recover faults

1.  detect faults。

1.  recover from faults

1.  end version execution

# Pipeline

![](img/5a315604b06375c7396eebaa5a3df85d.png)

![](img/2effb222c36f772c757ef69b670bd527.png)

![](img/77d0df13a6bb990b567467119b8c77bc.png)

# Data Dependence

![](img/94fe7485235f8a4ae533f7f07263360f.png)

![](img/50a994bc2cff908e92211e0536cfd12a.png)

![](img/934547985afac7ece0a9d1ff175ddbb1.png)

![](img/e752b6cf147dbf4d24dc284ab76b1c23.png)

read after write dependence

![](img/8249775dcf8298cf5985dfce38f4fe10.png)

![](img/f4e4ad08bd3c8dadf25f81294ab02593.png)

![](img/9e9259f56d9569e5a1403eb773b1e647.png)

![](img/7e247b7cfce556ed2071a92a4975a031.png)

![](img/f122c766c0a146eaf05e597424ed0c3c.png)

![](img/b43cefcdb3ddcb300d5b17670f9d8ea3.png)

![](img/902e862df27dcebb7a45e98febc05c48.png)

![](img/54bf2c5cab07b3cfc80e243d6cbadc64.png)

![](img/c4f9646aa4480be49c6edff34acb8974.png)

![](img/bf19d0a965cbcae48e0af410ff48ca5d.png)

![](img/e36f9f58b1c2926efeea9a3bb7fdbfc1.png)

![](img/debca83769a5fb832a73f6370fc129cf.png)

![](img/66506fa8e3c41518818d3314079dfae1.png)

![](img/1cbd13bc1a05530fa527fffdb8527a2e.png)

# Our Pipeline So Far

![](img/4cc246ba307819366d9155c4b45a074a.png)

# Control Dependence

![](img/bde1b16745f50897d2d606cdfbc3c5b2.png)

![](img/139a03ebfdd4621c4ef7dc6d9784b0a7.png)

![](img/3689af1e8cf0802801036308ae143c97.png)

![](img/fe04773c03fa21a61f72f26e0d520838.png)

![](img/de6c869ef1cd6135bbe795ef320ad4ec.png)

![](img/ee19e5d83b7bca156e42fb45a433b764.png)

# Final Pipelined MIPS Processor

![](img/901e8bb91c789ddfd44ad3313c58dbb3.png)

# Smarter Branch Prediction

![](img/89958113f2cea219dce972017b000f9a.png)

![](img/3d86841e5f945a2426eeeb19ddf78baf.png)

![](img/342d144e0e957e5a152bf708233342de.png)

![](img/7c75c10fc16aad8d75b634d41c1d7460.png)

![](img/a22b020c11bd889bc8b0b7c94841f698.png)

# Pipeline Performance Example

![](img/d1d6ecf7494cc9a6dfc12f716ba219cb.png)

![](img/5bb833b605b78a86b9260ca6329b05a0.png)

![](img/db719d605727171fe00bd1d41d0ffc1f.png)

![](img/26769a0d94fb8017ef50ecfc07e2f40a.png)

![](img/b3c8383c192f47b6de10fa49bda5828e.png)

![](img/743a4dedfdd123656eaef5e6ce50b0b8.png)

![](img/e01e6008ed81e75ef7561d55fc8329cf.png)

![](img/a103c200bc7f0dfd1d5d1ff79527df07.png)

![](img/6a94af834bf459f56a49f4e6b67a4333.png)

![](img/a8435670826e5449c0ebb82d39faabcf.png)

![](img/ce474a0474ae510b36bfa5b9bccdd1b0.png)

![](img/a61954b42932a26e6960fd6e91273a62.png)

![](img/3d87c5ab9310c9aca209aee26dc5d17a.png)

![](img/a66d578471afe7dfa7d46436b8bb030a.png)

![](img/16fe8c8caaa24f959772c56dbf72c284.png)

![](img/a06c8fb371a89fdd0afc69b5986159ab.png)

![](img/a87d6d85afd267a0d9351f5ab08bb729.png)

![](img/e7ce14f555920e6d80e5c540d35a8d6b.png)

![](img/de0973ac9bb8e41f8ff093ce559b2ade.png)

![](img/9a6d8a053ae448cbe455857e178cde29.png)

![](img/cb7bc39ccaaf6b48cc43a2c91c9c0eb5.png)

![](img/c595ae12d654c28f573196f47786c32c.png)

![](img/eb4c2894fb5254c24929300aa01f361b.png)

![](img/14900c8d0980f2fe719fc9c3541d8db7.png)

![](img/f2a7714466b25195c2929293aea1abca.png)

# Further Readings

![](img/2f94a53be2e083e4f5d884b0a91c6e1d.png)

![](img/388a54de2584cf91cc5c6ddce0482be5.png)

![](img/dc90a80e943e1af079d14788ba4b5f06.png)

![](img/5df35abf46e01e7246eea4add06d0780.png)

![](img/3ca7314ad6738d303557b3eee526a312.png)

![](img/8c7836f5235370555779a60e7cc9adf8.png)

![](img/449d12c50656e927553315ef94d95087.png)

![](img/3c27b7e5096689704fbd023fca60f088.png)

![](img/369857b91fa1ccb9823d5a2bbcd466fd.png)

![](img/55831055a01895dc8e57f04d31bd4781.png)

![](img/6f121a9ec48d93c8b674961d5f32a857.png)

![](img/512cb698d6153a834cf517a6d8756367.png)

![](img/2b19f030771373b3b01790004df5f8ba.png)

![](img/2db3648697b57a4c4b219f7e71c040f4.png)

![](img/436a96a38f10efea83188139bba8f7d5.png)

![](img/94ac0e6b2b62c4afb12fb5f710e198ad.png)

![](img/e579a1e980151fff5d077f038f224f75.png)

![](img/893c4160dc41f15816c679e3e9403299.png)

![](img/58bafcb0f6fede8c06e71951cbef3a97.png)

![](img/748a15fddfd5ae033010a7f379b5c739.png)

![](img/67516f73c7f2b1cdadf72e15bf07da98.png)

![](img/f3e420ecb464bb227b190fec45700797.png)

![](img/65a309e1a31c43a5015d3a3694a7b803.png)

![](img/90c9fa646cd2a62abd978ca9dddac164.png)

![](img/77952f50fd15159a495ca833d3df460e.png)

![](img/f5fe0cc80b1bf35af36063b16b2521f5.png)

![](img/5edf50026ce9248ab4d246ff8e4f3030.png)

![](img/4777624e1ce2cf2a6969c3fb084fe348.png)

![](img/167d7c769b6727b73f63740f18c00633.png)

![](img/3d3a382843a08a71e81d788cea8c471f.png)

![](img/b1010fa6532b00acadf16cbb815f8c2b.png)

![](img/cccd6c6213f273eb87081ebad1c0d8e0.png)

![](img/3986e811ce806eaf929d82b98338f741.png)

![](img/77aa6d9fcf1a182110bb1bcb846e60b1.png)

![](img/ec4852df7a3b8a4f411c63baf984e39a.png)

![](img/e794200c051835202bbfe35e808e59b8.png)

![](img/80dbd5c1f35ed8fcaef9f980b6751eaf.png)

![](img/f295b121d16cf6fd5958d0349a391150.png)

![](img/e71978ad15705dc8ce3eddbe3f81b632.png)

![](img/5d10500d4488fe11767466f95049815c.png)

![](img/01cd88c381234d92698d1e3adaca4231.png)

![](img/ba1452b165d7162258f56428ae5041b7.png)

![](img/f152c9d2e0af9526f3764f62bf99a08b.png)

![](img/5eacecebf81f4cb60d1ce421afb20778.png)

![](img/20f5ff73c3c8e2a3141e789b632ac8af.png)

![](img/26954c00206ad1b3884f5165f8040359.png)

![](img/695d9ccc2e6d88ff1a495c64d7000058.png)

![](img/a4f40756a2c707e11152e7b92c4c6245.png)

![](img/a1928fa5e9933c27856ecae515ac78c3.png)

![](img/13512e85011e1d2b570010ad951ff229.png)

![](img/04b732f419f8b38f794dc4a76d4153f8.png)

![](img/6b5da3d3e08d066e3f2cef658b731b69.png)

![](img/2a14d99dffe3ec9b0c5edca1bc038c5a.png)

![](img/da0454716f9148607c1fee56881c0b16.png)

![](img/237c06f4f5ebb9136af891b6df29903f.png)

![](img/01dc1a8083e2401b5a117991105b1b3a.png)

![](img/beefcc74aa44bad8f4b264a404a03a74.png)

![](img/68cabc03b71a0a07027afe64c5aa3290.png)

![](img/ed60181588693010402b91a1c8573d49.png)

![](img/0f83143203a77128516f2010f5bd66f6.png)

![](img/7fcd52aaadbec17f48b49bbd9ff613ab.png)

![](img/9719db40227562f36a403bae842710a7.png)