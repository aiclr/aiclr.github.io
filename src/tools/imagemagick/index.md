ImageMagick is a free and open-source software suite for displaying, converting, and editing raster image and vector image files. It can read and write over 200 image file formats.

- [archlinux wiki](https://wiki.archlinux.org/title/ImageMagick)
- [ImageMagick site](https://imagemagick.org/index.php)
- [github](https://github.com/ImageMagick) 
- [Usage](https://imagemagick.org/Usage/)

## [ImageMagick Commands](https://imagemagick.org/Usage/basics/#im_commands)

- **_identify_** -- Print the details of imagces, that IM sees
- **_Convert_** -- Convert and Modify Images
- **_Mogrify_** -- in-place batch processing
- **_-quality value_** -- JPEG/MIFF/PNG compression level

## 转换格式

将 **_nikon.NEF_** 转换为 **_png_** 格式图片

```sh
convert -verbose nef/nikon.NEF jpg/nikon.png
```

将 **_nef/KUN_34_.NEF\*** 批量转换为 **_jpg_** 格式图片，保存到 **_jpg_** 目录下

```sh
mogrify -verbose -path jpg -format jpg nef/KUN_*.NEF
```

## 转换大小

将 **_lock.PNG_** 压缩为 **_1920x1080_** 大小的 ***lock1080.PNG***图片

```sh
magick -verbose lock.PNG -resize 1920x1080 lock1080.PNG
```

批量压缩

```sh
mogrify -verbose -path jpg1080 -resize 1920x1080 jpg/KUN_35*.jpg
```

ico

```sh
convert -resize 128x -gravity center -crop 128x128+0+0 bougainvilleas.png -background transparent bougainvilleas.ico
```
