# tensorflow-yolo

### yolo和SSD、Faster RCNN等的区别在于没有设置先验框，因此预测出来的坐标并不是误差（SSD、Faster RCNN中），而是真正的位置坐标～，解析请参考https://zhuanlan.zhihu.com/p/24916786 和 https://zhuanlan.zhihu.com/p/32525231 以及 https://zhuanlan.zhihu.com/p/37850811

### Require
	tensorflow-1.0
### download pretrained model

yolo_tiny: <a>https://drive.google.com/file/d/0B-yiAeTLLamRekxqVE01Yi1RRlk/view?usp=sharing</a>

```
	mv yolo_tiny.ckpt models/pretrain/ 
```
### 预测阶段
1、根据置信度确定每个框所属类别（置信度最大的类别）.
2、设置置信度阈值，滤除低于置信度阈值的候选框.
3、对剩余的预测框运行NMS算法，得到最终的预测框。
同样需要注意的是NMS有两种解释，针对所有类别的NMS和针对单一类别的NMS算法，一般实现均为针对所有类别进行NMS操作～

### 有一个疑问，就是在计算mAP的时候，通用的置信度阈值应该为多少？
### Train

#### Train on pascal-voc2007 data 

##### Download pascal-Voc2007 data

1. Download the training, validation and test data

	```
	wget http://host.robots.ox.ac.uk/pascal/VOC/voc2007/VOCtrainval_06-Nov-2007.tar
	wget http://host.robots.ox.ac.uk/pascal/VOC/voc2007/VOCtest_06-Nov-2007.tar
	```

2. Extract all of these tars into one directory named `VOCdevkit`

	```
	tar xvf VOCtrainval_06-Nov-2007.tar
	tar xvf VOCtest_06-Nov-2007.tar
	```

3. It should have this basic structure

	```
  	$VOCdevkit/                           # development kit
  	$VOCdevkit/VOCcode/                   # VOC utility code
  	$VOCdevkit/VOC2007                    # image sets, annotations, etc.
  	# ... and several other directories ...
  	```

4. Create symlinks for the PASCAL VOC dataset

	```
    cd $YOLO_ROOT/data
    ln -s $VOCdevkit VOCdevkit2007
    ```
    Using symlinks is a good idea because you will likely want to share the same PASCAL dataset installation between multiple projects.

#### convert the Pascal-voc data to text_record file

```
python tools/preprocess_pascal_voc.py
```
#### train
```
python tools/train.py -c conf/train.cfg
```
#### Train your customer data

1. transform your training data to text_record file(the format reference to pascal_voc)

2. write your own train-configure file

3. train (python tools/train.py -c $your_configure_file)

### test demo

```
python demo.py
```


