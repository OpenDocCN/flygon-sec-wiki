<!--yml
category: 社会工程
date: 2022-11-10 10:29:55
-->

# 你知道一张图片，背后隐藏了多少信息？-FancyPig's blog

> 来源：[https://www.iculture.cc/sg/pig=1421](https://www.iculture.cc/sg/pig=1421)

## 科普：图片中的EXIF信息

当我们拍摄完一张图片后，其实我们会发现这个图片里是携带着`EXIF`相关信息的

包括但不限于：

*   快速查看相机/手机品牌
*   镜头型号
*   曝光对焦参数
*   快门次数
*   GPS定位等信息

很多对社工感兴趣的小伙伴，这时应该会有一个大胆的想法，那就是图片里如果包含了`GPS定位信息`，那岂不是别人发个朋友圈不用定位我们也知道在哪里了呢？不妨现在就来试一试吧

## 演示：寻找下面图片的EXIF信息

我这里找到了一张之前在天津吃饭的照片，这里我发原图，大家可以用这张图做测试进行验证（3MB多，加载可能有点慢）

<figure class="wp-block-image size-large">![图片[1]-你知道一张图片，背后隐藏了多少信息？-FancyPig's blog](img/d8b75911ea6c3b448b5ffb7297fdb25c.png)</figure>

## 通过电脑自带的工具查看

下载完图片后，右键`属性`，点击`详细信息`

<figure class="wp-block-image size-full">![图片[2]-你知道一张图片，背后隐藏了多少信息？-FancyPig's blog](img/ca50e5a0b3fad9a712bd408c4d8f0d1d.png)</figure>

这里面就可以看到很多参数了，当然包括定位咯

<figure class="wp-block-image size-full">![图片[3]-你知道一张图片，背后隐藏了多少信息？-FancyPig's blog](img/8a87dad0324faee823ff78feaaf47bf0.png)</figure>

如果你不嫌麻烦，可以用在线工具来找到具体定位

我们粗略的填入上方的`39`和`117`

<figure class="wp-block-image size-large">![图片[4]-你知道一张图片，背后隐藏了多少信息？-FancyPig's blog](img/86143696dfe2b0c423ec4951a1f20187.png)</figure>

我把相关链接放到下方，评论自取

## 通过微信小程序进行查看

微信里面可以使用小程序进行查看

<figure class="wp-block-image size-full">![图片[5]-你知道一张图片，背后隐藏了多少信息？-FancyPig's blog](img/e08191d6b69399c79d0e51cdfbf6b498.png)</figure>

## 使用Python实现获取图片EXIF信息

当然，我们这里还给大家准备了适合程序员的处理方式，这里主要就是使用了`exifread`模块读取图片的`exif`信息

### 相关代码

百度地图的key我给大家已经配置好了，只需要修改倒数第二行的图片名称即可`IMG_6623.jpg`

```
import requests
import exifread

class GetPhotoInfo:
    def __init__(self, photo):
        self.photo = photo
        # 百度地图ak  请替换为自己申请的ak
        self.ak = 'w5yCMHrlcHn2Er6WHcjvpHMpNOeYGIX7'
        self.location = self.get_photo_info()

    def get_photo_info(self, ):
        with open(self.photo, 'rb') as f:
            tags = exifread.process_file(f)
        try:
            # 打印照片其中一些信息
            print('拍摄时间：', tags['EXIF DateTimeOriginal'])
            print('照相机制造商：', tags['Image Make'])
            print('照相机型号：', tags['Image Model'])
            print('照片尺寸：', tags['EXIF ExifImageWidth'], tags['EXIF ExifImageLength'])
            # 纬度
            lat_ref = tags["GPS GPSLatitudeRef"].printable
            lat = tags["GPS GPSLatitude"].printable[1:-1].replace(" ", "").replace("/", ",").split(",")
            lat = float(lat[0]) + float(lat[1]) / 60 + float(lat[2]) / float(lat[3]) / 3600
            if lat_ref != "N":
                lat = lat * (-1)
            # 经度
            lon_ref = tags["GPS GPSLongitudeRef"].printable
            lon = tags["GPS GPSLongitude"].printable[1:-1].replace(" ", "").replace("/", ",").split(",")
            lon = float(lon[0]) + float(lon[1]) / 60 + float(lon[2]) / float(lon[3]) / 3600
            if lon_ref != "E":
                lon = lon * (-1)
        except KeyError:
            return "ERROR:请确保照片包含经纬度等EXIF信息。"
        else:
            print("经纬度：", lat, lon)
            return lat, lon

    def get_location(self):
        url = 'http://api.map.baidu.com/reverse_geocoding/v3/?ak={}&output=json' \
              '&coordtype=wgs84ll&location={},{}'.format(self.ak, *self.location)
        response = requests.get(url).json()
        status = response['status']
        if status == 0:
            address = response['result']['formatted_address']
            print('详细地址：', address)
        else:
            print('baidu_map error')

if __name__ == '__main__':
    Main = GetPhotoInfo('IMG_6623.jpg')
    Main.get_location()
```

图片和代码建议放在同一目录下

<figure class="wp-block-image size-full is-resized">![图片[6]-你知道一张图片，背后隐藏了多少信息？-FancyPig's blog](img/6208bcf575aa86c71b3ef5b85f41439e.png)</figure>

然后运行一下程序

<figure class="wp-block-image size-large">![图片[7]-你知道一张图片，背后隐藏了多少信息？-FancyPig's blog](img/1f340cbd251068e3c8e5740e342c7923.png)</figure>

然后就可以看到结果了

<figure class="wp-block-image size-large">![图片[8]-你知道一张图片，背后隐藏了多少信息？-FancyPig's blog](img/db5f6ff03a61bb5353dfdffce72f1854.png)</figure>

## 附录：Pycharm安装

由于本教程需要使用`python`脚本，故这里也简单介绍一下pycharm的安装过程

### 安装简易思路

如果你懒得看下面的过程，就记住：一直点next，如果有没打勾的打勾，简单粗暴。

### 安装详细过程

<figure class="wp-block-image size-full">![图片[9]-你知道一张图片，背后隐藏了多少信息？-FancyPig's blog](img/0350ae8caee647188bc2953bb9470229.png)</figure>

<figure class="wp-block-image size-full">![图片[10]-你知道一张图片，背后隐藏了多少信息？-FancyPig's blog](img/376497274c3fa4292b20117b91d0285c.png)</figure>

这里需要重启，添加环境变量才会生效

<figure class="wp-block-image size-full">![图片[11]-你知道一张图片，背后隐藏了多少信息？-FancyPig's blog](img/bc27b5ea399650458563f650faca31a8.png)</figure>

然后立即重启

<figure class="wp-block-image size-full">![图片[12]-你知道一张图片，背后隐藏了多少信息？-FancyPig's blog](img/3275be711e152f3535c2a6be654f5399.png)</figure>

重启之后，打开并同意用户协议

<figure class="wp-block-image size-full">![图片[13]-你知道一张图片，背后隐藏了多少信息？-FancyPig's blog](img/a4d68f5ec48f33d5e35deff4a043bea7.png)</figure>

然后选择`不发送数据`

<figure class="wp-block-image size-full">![图片[14]-你知道一张图片，背后隐藏了多少信息？-FancyPig's blog](img/27080654a4f5884423718efe14d85006.png)</figure>

### 创建项目

使用`PyCharm`创建一个新的项目

<figure class="wp-block-image size-full">![图片[15]-你知道一张图片，背后隐藏了多少信息？-FancyPig's blog](img/f3c88791e105267afced1339ab7b4e34.png)</figure>

文件位置可以根据你的需要进行修改，我这里选择默认的

<figure class="wp-block-image size-full">![图片[16]-你知道一张图片，背后隐藏了多少信息？-FancyPig's blog](img/574bbdf064adf5e8d1637f496cb8d603.png)</figure>

### 安装exifread模块

在`Terminal`中使用pip安装`exifread`模块

```
pip install exifread
```

<figure class="wp-block-image size-full">![图片[17]-你知道一张图片，背后隐藏了多少信息？-FancyPig's blog](img/1be011dbfa350c7c5972c896ae9a531c.png)</figure>

### 安装Requests模块

在`Terminal`中使用pip安装`Requests`模块

```
pip install Requests
```

### 配置过程中可能会遇到的坑

使用`pip`安装的时候如果你习惯开了`vpn`，很有可能会报错，然后还查不到原因。

NO ZUO NO DIE

<figure class="wp-block-image size-large">![图片[18]-你知道一张图片，背后隐藏了多少信息？-FancyPig's blog](img/3cdc2f7100e628928a487c81f1895511.png)</figure>