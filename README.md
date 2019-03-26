# [津南数字制造算法挑战赛](https://tianchi.aliyun.com/competition/entrance/231703/introduction)

基于改进yolov3实现x光违禁品的检测，官方数据集上可以达到0.77map，线上测试0.4244mmap。

## 创新点：

- 数据扩充900到3000。
- 检测尺度从3个拓展到5个，共15个anchors。
- Focal loss

## 实现：

##### 1.下载预训练模型

```
wget https://pjreddie.com/media/files/darknet53.conv.74
```

##### 2.下载代码

```
git clone https://github.com/depth-perception/jinnan_yolov3_baseline.git
```

##### 3.[下载数据集](https://tianchi.aliyun.com/competition/entrance/231703/information)，完成json与xml的转换。

##### 4.参考 [darknet实现](https://blog.csdn.net/lilai619/article/details/79695109)，完成数据的准备和环境的编译。

##### 5.计算anchors，并在yolov3_5l.cfg对应修改(已经修改)

```
./darknet detector calc_anchors cfg/jinnan.data -num_of_clusters 15 -width 416 -height 416
```

##### 6.训练

```
./darknet detector train cfg/yolov3_5l.cfg cfg/jinnan.data darknet53.conv.74 -map
```

注：-map表示训练过程中，每隔一定代数计算map，但这一定程度上会加长训练时间。

##### 断点训练：

```
./darknet detector train cfg/yolov3_5l.cfg cfg/jinnan.data backup/yolov3_last.weights -map
```

##### 7.验证

```
./darknet detector train cfg/yolov3_5l.cfg cfg/jinnan.data backup/yolov3_last.weights -map
```

##### 8.测试图片：

```
./darknet detector test cfg/jinnan.data cfg/yolov3.cfg yolov3.weights -ext_output dog.jpg
```

##### 9.批量测试图片并输出结果到文件

```
./darknet detector test cfg/jinnan.data cfg/yolov3.cfg yolov3.weights -dont_show -ext_output < data/val.txt > result.txt
```



## 参考：

1. https://github.com/pjreddie/darknet
2. https://github.com/AlexeyAB/darknet

