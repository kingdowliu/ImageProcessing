
> 灰度化：在RGB模型中，如果R=G=B时，则彩色表示一种灰度颜色，其中R=G=B的值叫做灰度值，因此，灰度图像每个像素值只需一个字节存放灰度值（又称强度值、亮度值），灰度范围为0-255。
>  二值化：二值化可以把灰度图片转换成二值图像，把大于某个临界灰度值的像素灰度设置为灰度极大值，把小于这个值的像素灰度设为灰度极小值，从而实现二值化。

**原始图**
<img src="https://img-blog.csdnimg.cn/20190927145950285.gif" width="300" hegiht="313" align=center alt="原始图"/>
**灰度图**
<img src="https://img-blog.csdnimg.cn/20190927145226527.gif" width="300" hegiht="313" align=center alt="灰度图"/>
**二值化后的图**
<img src="https://img-blog.csdnimg.cn/20190927150136434.gif" width="300" hegiht="313" align=center alt="二值化后的图"/>
我们可以看到对于这张验证码效果还不错，接下来我们用代码实现。
首先我们进行图片转灰度并二值化
```python
def GrayscaleAndBinarization(image):
    '''
    灰度并二值化图片，初步二值化后，会存在一些噪声点，我们使用这样一个方法进行去除（不能实现完全去除），
    即当图片中存在一个点灰度值为0，但其周围九宫格中像素点的灰度值均为255时，我们认为这个点为孤立点，将其
    灰度改为255
    :param image: 处理前的图片
    :return:处理后的图片
    '''
    threshold = 17  # 需要自己调节阈值

    tmp_image = image.convert('L')  # 灰度化
    new_image = Image.new('L', tmp_image.size, 0)

    # 初步二值化
    for i in range(tmp_image.size[1]):
        for j in range(tmp_image.size[0]):
            if tmp_image.getpixel((j, i)) > threshold:
                new_image.putpixel((j, i), 255)
            else:
                new_image.putpixel((j, i), 0)

    # new_image.show()    # 去噪前

    # 去噪，去除独立点，将前后左右等九宫格中灰度通道值均为255的像素点的通道值设为255
    for i in range(1, new_image.size[1] - 1):
        for j in range(1, new_image.size[0] - 1):
            if new_image.getpixel((j, i)) == 0 and new_image.getpixel((j - 1, i)) == 255 and new_image.getpixel((j + 1, i)) == 255 and \
                    new_image.getpixel((j, i - 1)) == 255 and new_image.getpixel((j - 1, i - 1)) == 255 and new_image.getpixel((j + 1, i - 1)) == 255 and\
                new_image.getpixel((j, i + 1)) == 255 and new_image.getpixel((j - 1, i + 1)) == 255 and new_image.getpixel((j + 1, i + 1)) == 255:
                new_image.putpixel((j, i), 255)
    # new_image.show()    # 去噪后

    return new_image
```
接下来我们找到图片切割点并对图片进行切割，对于这种验证码，我们进行简单的垂直切割。首先，我们找到需要进行切割的位置。
```python
def SplitImage(image):
    '''
    切割图像并保存，关键在于寻找切割位置
    :param image: 待切割的图片
    :return: 无
    '''
    splitSite = []	# 记录切割的位置
    splitSite.append(0)
    new_image = image

    tmp_list = [0 for i in range(image.size[0])]
    for i in range(image.size[0]):
        for j in range(image.size[1]):
            if image.getpixel((i, j)) == 0:
                tmp_list[i] = 1
                break

    # print(tmp_list)
    for index in range(0, len(tmp_list) - 1):
        if tmp_list[index] == 1 and tmp_list[index + 1] == 0:
            splitSite.append(index + 1)

    plt.imshow(image)
    for item in splitSite[1:]:
        plt.plot([item for i in range(image.size[1])], [i for i in range(image.size[1])])
    plt.show()	# 使用 matplotlib 模块进行预览切割效果
    splitSite.append(image.size[0])

    # 对图片进行切割
    for index in range(1, len(splitSite) - 1):
        box = (splitSite[index - 1], 0, splitSite[index], image.size[1])	# box中四个参数，分别为距离图片左侧、上侧、右侧、下侧的像素距离
        new_image = image.crop(box)
        new_image.save('./{}.gif'.format(str(index)))	# 保存切割后的图片，位置在当前目录
```
**切割后效果图**
1. 第一个字母
<img src="https://img-blog.csdnimg.cn/20190927151119477.gif" width="100" hegiht="113" align=center alt="原始图"/>
2. 第二个字母
<img src="https://img-blog.csdnimg.cn/20190927151138492.gif" width="100" hegiht="113" align=center alt="原始图"/>
3. 第三个字母
<img src="https://img-blog.csdnimg.cn/2019092715114624.gif" width="100" hegiht="113" align=center alt="原始图"/>
4. 第四个字母
<img src="https://img-blog.csdnimg.cn/20190927151152679.gif" width="100" hegiht="113" align=center alt="原始图"/>


