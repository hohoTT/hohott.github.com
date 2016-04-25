---
layout:     post
title:      "Python 代码完成图片转字符画的小工具"
subtitle:   " \"Python 代码完成图片转字符画\""
date:       2016-04-21 20:00:00
author:     "hohoTT"
header-img: "img/post-bg-alitrip.jpg"
catalog: true
tags:
    - Python
---

## 功能介绍

该项目实现图片转化为字符图画，具体图片具体分析，一张图片从图像到字符不是一蹴而就的，需要经过很多步骤，成品是 **一系列字符** 的组合，我们可以把字符看作是比较大块的像素，`一个字符能表现一种颜色`（暂且这么理解吧），**字符的种类越多，可以表现的颜色也越多，图片也会更有层次感** 。

## 概念介绍

我们是要转换一张彩色的图片，这么这么多的颜色，要怎么对应到字符上去？这里就要介绍 **灰度值** 的概念了。

    灰度值：指黑白图像中点的颜色深度，范围一般从0到255，白色为255，黑色为0，故黑白图片也称灰度图像

**灰度值** 大的用列表开头的符号，**灰度值** 小的用列表末尾的符号。

## 代码实现

我们要做的是将下方的图片内容转换为字符输出

<div align="center"><img src="http://www.hohott.wang/img/writing_img/2016-4-21/doraemon.png"/></div>


`以下为实现图片转字符的具体代码部分`

```python
# coding=utf-8

__author__ = 'hohoTT'

from PIL import Image

IMG = "img/doraemon.png"
#IMG = "img/testFive.jpg"
WIDTH = 60
HEIGHT = 40
#OUTPUT = "img/ImgTransformCharImg.txt"
#OUTPUT = "outputFile/testFive.txt"

ascii_char = list("$@B%8&WM#*oahkbdpqwmZO0QLCJUYXzcvunxrjft/\|()1{}[]?-_+~<>i!lI;:,\"^`'. ")


# 将256灰度映射到70个字符上
def get_char(r, b, g, alpha=256):
    if alpha == 0:
        return ' '
    length = len(ascii_char)
    gray = int(0.2126 * r + 0.7152 * g + 0.0722 * b)

    unit = (256.0 + 1)/length
    return ascii_char[int(gray/unit)]

if __name__ == '__main__':

    im = Image.open(IMG)
    im = im.resize((WIDTH, HEIGHT), Image.NEAREST)

    txt = ""

    for i in range(HEIGHT):
        for j in range(WIDTH):
            txt += get_char(*im.getpixel((j, i)))
        txt += '\n'

    print txt

    # 字符画输出到文件
    if OUTPUT:
        with open(OUTPUT, 'w') as f:
            f.write(txt)
    else:
        with open("output.txt", 'w') as f:
            f.write(txt)
```

## 项目运行结果

为了测试时效果明显，我将结果进行了控制台的输出打印，不过代码实现的部分同时还会将字符画输出到 **ImgTransformCharImg.txt** 命名的文件中，可以自行到保存的路径下进行文件效果的查看。

以下为控制台输出打印的 **字符画**


<div align="center"><img src="http://www.hohott.wang/img/writing_img/2016-4-21/result.png"/></div>

还不错吧，嘻嘻 ^_^
小伙伴们可以选择其他的图片进行测试

* 最好是轮廓清晰的图片，背景明亮的额图片

* 同时可能需要根据图片更改代码中 **ascii_char="..."** 以及 **灰色度** 部分的修改


## 著作权声明
本文首次发布于 [hohoTT's Blog](http://www.hohott.wang/)，转载请保留 [http://www.hohott.wang/2016/04/22/Python-ImgTransformCharImg/](http://www.hohott.wang/2016/04/22/Python-ImgTransformCharImg/) 以上链接并附上作者 **hohoTT**。如果您喜欢，还可以关注我的微信公众号 **hohoTT** , 或者扫一扫下方的微信公众号二维码即可 ^_^。
<div align="center"><img src="http://www.hohott.wang/img/WeiXinImg.jpg"/></div>