<!--yml
category: 社会工程
date: 2022-11-10 10:29:10
-->

# 图片定位远程查岗？别轻易用这些软件了！附一个自制工具-FancyPig's blog

> 来源：[https://www.iculture.cc/sg/pig=5677](https://www.iculture.cc/sg/pig=5677)

## 法律声明

本教程仅作为JAVA学习提供爱好培养，请勿用于非法用途。

根据**《中国人民共和国刑法总成》（第15版）**第二百五十三条之一规定：

第一条：出售或者**提供行踪轨迹信息**，被他人用于犯罪的；

第三条：非法获取、出售或者提供**行踪轨迹信息**、通信内容、征信信息、财产信息五十条以上的；

属于`情节严重`，将处**三年以下有期徒刑**或者**拘役**，并处或者单处罚金

## 引入

早在19年的时候就流行着通过照片社工的

这张图片好像是一个日本的女星，具体叫什么咱也不知道，咱也不敢问。

<figure class="wp-block-image size-large">![图片[1]-图片定位远程查岗？别轻易用这些软件了！附一个自制工具-FancyPig's blog](img/023c4e542d5f99c0736bc5d32e8c7308.png)</figure>

然后就有细心网友，发现了背景里有**天汽车城**

<figure class="wp-block-image size-full">![图片[2]-图片定位远程查岗？别轻易用这些软件了！附一个自制工具-FancyPig's blog](img/d88d2e18e8f8716a6ff4676c6305d9d0.png)</figure>

然后到搜索引擎里查询一下，关联出来一些常用的搜索

<figure class="wp-block-image size-full">![图片[3]-图片定位远程查岗？别轻易用这些软件了！附一个自制工具-FancyPig's blog](img/40cd1dc13c41de4850fd46b7d1f49dee.png)</figure>

然后，再将图片翻转过来，观察到中间的区域

<figure class="wp-block-image size-full">![图片[4]-图片定位远程查岗？别轻易用这些软件了！附一个自制工具-FancyPig's blog](img/5de87fe6f6f7d21af21891d6afc94488.png)</figure>

然后再通过强大的Google地图，找到与之匹配的图片，是不是绝了！

<figure class="wp-block-image size-full">![图片[5]-图片定位远程查岗？别轻易用这些软件了！附一个自制工具-FancyPig's blog](img/3909ac437e0c208c6f1c451fec06c68b.png)</figure>

这里只是拍照相关的，由此引出的图片类的社工，除了本身肉眼可见的关键建筑标志，我们还可以从技术手段上发现一些新的特点。

## 相关阅读

之前我们介绍过图片的EXIF属性，可以找到照片拍摄的地理位置

这里值得补充的是，照片在QQ、微信、微博等社交平台大部分都会进行压缩处理，因此地理位置信息会被抹掉。

在19年的时候，微信官方也对图片原图的相关问题做出过声明

<figure class="wp-block-image size-full">![图片[6]-图片定位远程查岗？别轻易用这些软件了！附一个自制工具-FancyPig's blog](img/9cff77953a2e5dc27d2c270311462a94.png)</figure>

看起来社交是有些困难了，但是如果有其他方式能拿到原图，会不会有一些操作空间？

*   USB拷贝？估计不行
*   照片发送原图到电脑？有些难度？
*   远程同步工具？BINGO!相册同步软件试一试？

因此如果将图片通过某种方式自动同步到云端，便可以查看到**图片原图**。

*   例如，**百度网盘**中相册图片自动保存的功能，可以存储原图。

与此类似的软件还有很多，从应用商店随便一搜都是

<figure class="wp-container-2 wp-block-gallery-1 wp-block-gallery columns-2 is-cropped">

即便你不开启定位，相册传到云端，地理位置都可以显示出来

因此，使用了这类软件的童鞋们，建议没有特殊需求还是尽量弃了坑吧，同步相册从某种意义上讲已经把个人的一些资料泄露给了软件的运营商。

不过，暗箭难防啊！其实，手机系统自带的软件就有根据定位筛选照片的功能，甚至还可以通过地图来展示

<figure class="wp-block-image size-full">![图片[9]-图片定位远程查岗？别轻易用这些软件了！附一个自制工具-FancyPig's blog](img/66f72a0d0ea698c73cea4068ce1a6b17.png)</figure>

很多人害怕使用在线工具上传检验图片位置会暴露自己的信息。

因此，之前的文章介绍过通过**Python**来读取图片的地理位置，代码在下面会分享出来

*   倒数第二行要修改你的图片名称，运行时和python脚本在同一路径

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

这次我们分享JAVA代码，通过JAVA的代码来读取图片地理位置方法

```
package com.easylinkin.bm.extractor;

import com.alibaba.fastjson.JSONObject;
import com.drew.imaging.ImageMetadataReader;
import com.drew.imaging.ImageProcessingException;
import com.drew.metadata.Directory;
import com.drew.metadata.Metadata;
import com.drew.metadata.Tag;
import com.easylinkin.bm.util.HttpUtils;
import lombok.extern.slf4j.Slf4j;

import java.io.File;
import java.io.IOException;

/**
 * @author zhengwen
 **/
@Slf4j
public class ImgTestCode {
    public static void main(String[] args) throws Exception {

        File file = new File("C:\\Users\\fancypig\\Desktop\\pic_location\\IMG_6623.jpg");
        readImageInfo(file);
    }

    /**
     * 提取照片里面的信息
     *
     * @param file 照片文件
     * @throws ImageProcessingException
     * @throws Exception
     */
    private static void readImageInfo(File file) throws ImageProcessingException, Exception {
        Metadata metadata = ImageMetadataReader.readMetadata(file);

        System.out.println("---打印全部详情---");
        for (Directory directory : metadata.getDirectories()) {
            for (Tag tag : directory.getTags()) {
                System.out.format("[%s] - %s = %s\n",
                        directory.getName(), tag.getTagName(), tag.getDescription());
            }
            if (directory.hasErrors()) {
                for (String error : directory.getErrors()) {
                    System.err.format("ERROR: %s", error);
                }
            }
        }

        System.out.println("--打印常用信息---");

        Double lat = null;
        Double lng = null;
        for (Directory directory : metadata.getDirectories()) {
            for (Tag tag : directory.getTags()) {
                String tagName = tag.getTagName();  //标签名
                String desc = tag.getDescription(); //标签信息
                if (tagName.equals("Image Height")) {
                    System.err.println("图片高度: " + desc);
                } else if (tagName.equals("Image Width")) {
                    System.err.println("图片宽度: " + desc);
                } else if (tagName.equals("Date/Time Original")) {
                    System.err.println("拍摄时间: " + desc);
                } else if (tagName.equals("GPS Latitude")) {
                    System.err.println("纬度 : " + desc);
                    System.err.println("纬度(度分秒格式) : " + pointToLatlong(desc));
                    lat = latLng2Decimal(desc);
                } else if (tagName.equals("GPS Longitude")) {
                    System.err.println("经度: " + desc);
                    System.err.println("经度(度分秒格式): " + pointToLatlong(desc));
                    lng = latLng2Decimal(desc);
                }
            }
        }
        System.err.println("--经纬度转地址--");
        //经纬度转地主使用百度api
        convertGpsToLoaction(lat, lng);

    }

    /**
     * 经纬度格式  转换为  度分秒格式 ,如果需要的话可以调用该方法进行转换
     *
     * @param point 坐标点
     * @return
     */
    public static String pointToLatlong(String point) {
        Double du = Double.parseDouble(point.substring(0, point.indexOf("°")).trim());
        Double fen = Double.parseDouble(point.substring(point.indexOf("°") + 1, point.indexOf("'")).trim());
        Double miao = Double.parseDouble(point.substring(point.indexOf("'") + 1, point.indexOf("\"")).trim());
        Double duStr = du + fen / 60 + miao / 60 / 60;
        return duStr.toString();
    }

    /***
     * 经纬度坐标格式转换（* °转十进制格式）
     * @param gps
     */
    public static double latLng2Decimal(String gps) {
        String a = gps.split("°")[0].replace(" ", "");
        String b = gps.split("°")[1].split("'")[0].replace(" ", "");
        String c = gps.split("°")[1].split("'")[1].replace(" ", "").replace("\"", "");
        double gps_dou = Double.parseDouble(a) + Double.parseDouble(b) / 60 + Double.parseDouble(c) / 60 / 60;
        return gps_dou;
    }

    /**
     * api_key：注册的百度api的key
     * coords：经纬度坐标
     * http://api.map.baidu.com/reverse_geocoding/v3/?ak="+api_key+"&output=json&coordtype=wgs84ll&location="+coords
     * <p>
     * 经纬度转地址信息
     *
     * @param gps_latitude  维度
     * @param gps_longitude 精度
     */
    private static void convertGpsToLoaction(double gps_latitude, double gps_longitude) throws IOException {
        String apiKey = "YNxcSCAphFvuPD4LwcgWXwC3SEZZc7Ra";

        String res = "";
        String url = "http://api.map.baidu.com/reverse_geocoding/v3/?ak=" + apiKey + "&output=json&coordtype=wgs84ll&location=" + (gps_latitude + "," + gps_longitude);
        System.err.println("【url】" + url);

        res = HttpUtils.httpGet(url);
        JSONObject object = JSONObject.parseObject(res);
        if (object.containsKey("result")) {
            JSONObject result = object.getJSONObject("result");
            if (result.containsKey("addressComponent")) {
                JSONObject address = object.getJSONObject("result").getJSONObject("addressComponent");
                System.err.println("拍摄地点：" + address.get("country") + " " + address.get("province") + " " + address.get("city") + " " + address.get("district") + " "
                        + address.get("street") + " " + result.get("formatted_address") + " " + result.get("business"));
            }
        }
    }

}
```

<figure class="wp-block-image size-full">![图片[10]-图片定位远程查岗？别轻易用这些软件了！附一个自制工具-FancyPig's blog](img/913a15d9f7f16f24fc4588feda205e15.png)</figure>

## 自制一个检测工具

当然，很多人可能会觉得java代码用起来很不方便，甚至表示自己没有学过，我们这一期专门给大家准备了一个**本地检测的工具**

### 在线检测入口

### 使用方法

还是上一期的图，我们可以在浏览器里拖入检测

<figure class="wp-block-image size-large">![图片[11]-图片定位远程查岗？别轻易用这些软件了！附一个自制工具-FancyPig's blog](img/028fe76dbbbff764e6464105e742a02e.png)</figure>

实现方法就是通过将图片中的数据通过JSON进行解析，**图片不会上传到我们的服务器，相关代码证明**

这里只是通过json解析方式将本地图片中的参数解析出来

```
 class="uploader"
          drag
          action="https://jsonplaceholder.typicode.com/posts/"
          :http-request="processImage"
          :before-upload="readExif"
          accept=".jpg, .jpeg, .heic"
          :show-file-list="false" 
```

通过上面图片的经纬度，我们可以复制到Google地图中，便可以得到精确的地理位置了！

<figure class="wp-block-image size-large">![图片[12]-图片定位远程查岗？别轻易用这些软件了！附一个自制工具-FancyPig's blog](img/9c7cce3e9dde235c6fd851d2d6e8a43c.png)</figure>

### 小工具制作方法

本项目通过vue编写，这里仅教学一次，后续全部vue项目启动和打包的过程，将不再讲解。

首先需要下载vscode和Nodejs，我们给大家准备了下载链接

打开VSCODE导入文件夹

<figure class="wp-block-image size-full">![图片[13]-图片定位远程查岗？别轻易用这些软件了！附一个自制工具-FancyPig's blog](img/accf8730a02bea1f70f6a25927659452.png)</figure>

选择fancypig-exif-viewer-1.0.0

<figure class="wp-block-image size-full">![图片[14]-图片定位远程查岗？别轻易用这些软件了！附一个自制工具-FancyPig's blog](img/22466ae32ed84a7f82850ba8792299da.png)</figure>

然后进入项目，启动终端

<figure class="wp-block-image size-full">![图片[15]-图片定位远程查岗？别轻易用这些软件了！附一个自制工具-FancyPig's blog](img/9a9e36c57ac4b901acc1618dbdb0f02f.png)</figure>

加载环境依赖

```
npm install
```

方便测试效果，可以本地运行

```
npm run serve
```

可以通过下面的路径查看效果

<figure class="wp-block-image size-full">![图片[16]-图片定位远程查岗？别轻易用这些软件了！附一个自制工具-FancyPig's blog](img/cf363d60d74f55e3812ed8850aab725f.png)</figure>

访问查看效果

<figure class="wp-block-image size-large">![图片[17]-图片定位远程查岗？别轻易用这些软件了！附一个自制工具-FancyPig's blog](img/c0b7d39f600a9c354d65abd749eec093.png)</figure>

当然，如果你想部署到公网，做一个公益的小工具给热心网友们，可以输入

```
npm run build
```

<figure class="wp-block-image size-full">![图片[18]-图片定位远程查岗？别轻易用这些软件了！附一个自制工具-FancyPig's blog](img/ff3c775011df09182ad9cbc8a83d18be.png)

<figcaption>等待打包完成</figcaption>

</figure>

<figure class="wp-block-image size-full">![图片[19]-图片定位远程查岗？别轻易用这些软件了！附一个自制工具-FancyPig's blog](img/6c98dc7f83ba73223db2ff74536ffa92.png)</figure>

会将打包好的文件放到dist目录中

然后我们可以将文件上传到自己的服务器网站中便可以访问了

<figure class="wp-block-image size-full">![图片[20]-图片定位远程查岗？别轻易用这些软件了！附一个自制工具-FancyPig's blog](img/ac4e5f2cdb66348e0b315cf41fdaf40f.png)</figure>

公网上就可以访问咯！

<figure class="wp-block-image size-large">![图片[21]-图片定位远程查岗？别轻易用这些软件了！附一个自制工具-FancyPig's blog](img/d65b5a98a4aab4544cce114e6d81cbe2.png)</figure>

</figure>