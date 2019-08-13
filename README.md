# UGATIT-Tutorial
 
# 0x00 前言
UGATIT是一种新的无监督图像到图像转换方法。
UGATIT是Tensorflow实现的，另外也有Pytorch实现的，不过现在很不稳。
UGATIT是Train起来非常吃内存的，我的GTX1060 6G跑太太太慢，所以这里直接搞个练好的预训练模型做实验。
目前官方的[selfie2anime]预训练模型由于涉及商业的原因，并不知道能否放出，就用thewaifuai大佬练好的[cat2dog]预训练模型做一下示范。
Nathan Glover大佬已经放出他个人训练的 [selfie2anime]预训练模型，在下文我们也来测试一下。
[selfie2anime]预训练模型是把[真人脸变动漫脸]以及[动漫脸变真人脸]。
[cat2dog]预训练模型是把[猫脸变狗脸]以及[狗脸变猫脸]。
# 0x01 环境
Python 3.7
CUDA 10.1.168
CUDNN 7.6.2.24
Tensorflow1.13.1 from CUDA 10.1.105 CUDNN 7.5 And sse2
0x02 下载
[Code]Github:
https://codeload.github.com/taki0112/UGATIT/zip/master
[Code]百度云：
https://pan.baidu.com/s/1JUdr5b_vO3MJyyRtWgpWzA
ajsu
[P-Model]cat2dog-百度云：
https://pan.baidu.com/s/1ZvqEsbWVVpID1E_KCje1Aw 
aw35 
[P-Model]selfie2anime-百度云：
https://pan.baidu.com/s/1bQm3nXIOouDCELfPU8NjZA 
50lt 
[Dataset]阿猫阿狗数据集-百度云：
https://pan.baidu.com/s/13gO9n3j7g-ylfVrs2HypYw 
ryvj 
# 0x03 阅读
文件结构：
关键词：
dataset > 数据集
checkpoint > 检测点
results > 结果
pretrained-model > 预训练模型
# 0x04 预训练模型
在你下载的[ugatit-cat2dog-pretrained-model]文件夹的[checkpoint]文件夹中，应得到以下一个名为[UGATIT_light_cat2dog_lsgan_4resblock_6dis_1_1_10_10_1000_sn]的文件夹：

现将位于[ugatit-cat2dog-pretrained-model]的[UGATIT_light.model-24000.meta]复制到[UGATIT_light_cat2dog_lsgan_4resblock_6dis_1_1_10_10_1000_sn]中：

现将此[checkpoint]文件夹移动到[*\UGATIT-master]下：

# 0x05 数据集
在[*\UGATIT-master]下，创建以下红框内的文件结构：

阿猫阿狗数据集[0x02 step 4]自取。
把要转换成狗脸的猫脸放在[testA]。
把要转换成猫脸的狗脸放在[testB]。
图片名字不重要，格式[*.jpg][*.png]，其他没试过。
# 0x06 测试
由于[*\UGATIT-master]下的[UGATIT.py]的[Line 577]的原因：

现将[*\UGATIT-master\checkpoint]下的[UGATIT_light_cat2dog_lsgan_4resblock_6dis_1_1_10_10_1000_sn]重命名为[UGATIT_light_cat2dog_lsgan_4resblock_6dis_1_1_10_10_1000_sn_smoothing]:

在[*\UGATIT-master]下空白处，按Shift不放的同时右键空白处，选择[在此处打开PowerShell窗口]：

在PShell中输入：
python main.py --dataset cat2dog --light True --phase test

红框是一个Tensorflow里的函数的过时提醒，不用管，注意箭头指向的信息：

在[*\UGATIT-master]下，自动新生成了四个文件夹：

打开[results]文件夹，在里面的下级文件里找到[index.html]，打开：

emmm,效果有点差，这只预训练模型还不到火候
接下来测试[selfie2anime]，导入预训练模型跟上面一样的操作，然后在[*\UGATIT-master\dataset]下创建如下文件结构：

然后在[testA]放入几张用于测试的真人图，[testB]就不用放动漫图了，这个模型还没有练好动漫脸转真人脸：

然后跟上面类似在PShell里输入：
python main.py --dataset selfie2anime --light True --phase test
依旧打开[results]文件夹，在里面的下级文件里找到[index.html]，打开：

嗯，，,啊哈哈哈，丑爆了有木有，这个模型还需要多加训练...
# 0x07 附赠数据集
年轻女性-1000张-512px：
https://pan.baidu.com/s/12SFIwbJpC70_ihBqQ-xGOg 
udlm
二次元-1000张-512px：
https://pan.baidu.com/s/1TYgCN_LDKeu1QpDpHOgNOA 
d1yg 
# 0x08 数据集处理
如果要转成256px或其他，则：
import os
from PIL import Image

input_dir=r"填你要转换的图片的存放文件夹的路径"
## 例如 input_dir=r"E:\Github\input\\"
output_dir=r"填你转换完成后的图片的保存文件夹的路径"
## 例如 output_dir=r"E:\Github\output\\"

filename = os.listdir(input_dir)

size_m = 256
size_n = 256
## 这里修改图片尺寸
 
for img in filename:
    image = Image.open(input_dir + img)
    image_size = image.resize((size_m, size_n),Image.ANTIALIAS)
    image_size.save(output_dir+ img)

print("完成！！！")
