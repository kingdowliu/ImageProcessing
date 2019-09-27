
> 灰度化：在RGB模型中，如果R=G=B时，则彩色表示一种灰度颜色，其中R=G=B的值叫做灰度值，因此，灰度图像每个像素值只需一个字节存放灰度值（又称强度值、亮度值），灰度范围为0-255。
二值化：二值化可以把灰度图片转换成二值图像，把大于某个临界灰度值的像素灰度设置为灰度极大值，把小于这个值的像素灰度设为灰度极小值，从而实现二值化。
---
<img src="https://img-blog.csdnimg.cn/20190927145950285.gif" width="300" hegiht="313" align=center alt="原始图"/>  **原始图**
---
<img src="https://img-blog.csdnimg.cn/20190927145226527.gif" width="300" hegiht="313" align=center alt="灰度图"/>  **灰度图**
---
<img src="https://img-blog.csdnimg.cn/20190927150136434.gif" width="300" hegiht="313" align=center alt="二值化后的图"/>  **二值化后的图**
我们可以看到对于这张验证码效果还不错，接下来我们用代码实现。
首先我们进行图片转灰度并二值化

接下来我们找到图片切割点并对图片进行切割，对于这种验证码，我们进行简单的垂直切割。首先，我们找到需要进行切割的位置。

**切割后效果图**
1. 第一个字母
<img src="https://img-blog.csdnimg.cn/20190927151119477.gif" width="100" hegiht="113" align=center alt="原始图"/>    
2. 第二个字母
<img src="https://img-blog.csdnimg.cn/20190927151138492.gif" width="100" hegiht="113" align=center alt="原始图"/>    
3. 第三个字母
<img src="https://img-blog.csdnimg.cn/2019092715114624.gif" width="100" hegiht="113" align=center alt="原始图"/>    
4. 第四个字母
<img src="https://img-blog.csdnimg.cn/20190927151152679.gif" width="100" hegiht="113" align=center alt="原始图"/>    


