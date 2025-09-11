## yuv是什么
`YUV （Y'CbCr）`是一种像素格式，见于视频编码与静态图像。与 RGB 格式不同，YUV 分别由一个称为 Y（相当于灰度）的`“亮度”分量`（Luminance or Luma）和两个称为 U（蓝色投影 Cb Color_blue?）和 V（红色投影 Cr Color_red?）的`“色度”分量`（Chrominance or Chroma）表示，由此得名。

## 查看yuv格式图像
安装好ffmpeg后，使用ffplay查看：（终端中乘号为xyz的x）
```bash
ffplay -f rawvideo -video_size -pixel_format nv12 640x480 raw_640x480.yuv
```

## FFMPEG JPG转YUV命令  
-s 1024x576 可以直接resize
```bash
// jpg转nv21  
ffmpeg -i image_1920x1080.jpg -pix_fmt nv21 image_1920x1080.nv21.yuv
// jpg转bgr24  
ffmpeg -i image_1920x1080.jpg -pix_fmt bgr24 image_1920x1080.bgr24.rgb
// jpg转bgr32  
ffmpeg -i image_1920x1080.jpg -pix_fmt bgr32 image_1920x1080.bgr32.rgb
// nv21转jpg  
ffmpeg -y -s 1920x1080 -pix_fmt nv21 -i image.nv21.yuv image.jpg
```
## FFMPEG修改YUV尺寸
ffmpeg -s 原始尺寸 -i 路径及名称 -vf scale=更改的尺寸 新的路径和名称

目标1: 参与合众项目，完成感知SDK全功能运行以及上车实测运行
完成情况：到桐乡现场进行支持，上车跑通semantics与facula功能
