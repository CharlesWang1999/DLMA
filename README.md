# :mortar_board: DLMA: Deep Learning-Enabled Morphmetric Analysis
## DLMA workflow: Aiming fast toxicology screening using zebrafish based on Deep learning (Use Detectron2 framework)
## 👏 Abstract
This AI project is a detection task for identifying `zebrafish organs` and `phenotypes` in micrographs, which is based on the Meta AI project, [Detectron2 version 0.4.1](https://github.com/facebookresearch/detectron2). It mainly used `Mask R-CNN` for training and validating. It has 16 detected objects, including 8 specific organs and 8 specific abnormal phenotypes. 

![infer_](https://user-images.githubusercontent.com/57084033/177120642-c2a074d5-0c78-4a35-99f8-85f1ae02a80c.gif)
<p align="center">Inference results by Mask R-CNN</p>

## :rabbit: How to start
To use our zebrafish AI detection, you have to learn how to install and utilize the detectron2, please refer the [detectron2 instructions](https://detectron2.readthedocs.io/en/latest/tutorials/getting_started.html). 

Firstly, install python3.8 or higher version, detectron2 (0.4.1) and other dependent libraries (see [zebrafish_maskrcnn.py](https://github.com/gonggqing/zebrafish_detection/blob/ddff5e1871fb63bbb34f46db6785534ed34c017a/zebrafish_maskrcnn.py)). Among all the libraries, [`fishutil`](https://github.com/gonggqing/zebrafish_detection/blob/b4dbee1b6be693c0968b62c1cd86daa5472d827d/fishutil.py) and [`fishclass`](https://github.com/gonggqing/zebrafish_detection/blob/b4dbee1b6be693c0968b62c1cd86daa5472d827d/fishclass.py) are two customized python libraries for transforming the model outputs to quantitative parameters of zebrafish.

Then put the [pre-trained model weights](https://drive.google.com/file/d/1yyREJccnKeRDJ4BOnFMt3FNddC_w4fm_/view?usp=sharing) in your specified path.
## :paw_prints: Download the image library and pre-trained model weights

mask r-cnn & resnet_r_101/tensormask & resnet_r_50: download [here](https://drive.google.com/file/d/1md4kusa5aJFiCGDs43W0ECbOcrM8ka7p/view?usp=sharing)

test_image:/train_image: download here [test](https://drive.google.com/file/d/1md4kusa5aJFiCGDs43W0ECbOcrM8ka7p/view?usp=sharing) [train](https://drive.google.com/drive/folders/17j4MX7q8FlSerA01qsoTFl4Hl6ZUmDR8?usp=sharing)

```python
# Modify some necessary paths
# path to model weights
cfg.MODEL.WEIGHTS = os.path.join(cfg.OUTPUT_DIR, "model_final.pth")
...
# path to images that pending to detect
path = 'your/specified/path'
...
# save your results
df.to_csv(os.path.join('.../results/', 'results.csv'))
```
Finally, run the [inference.py](https://github.com/gonggqing/zebrafish_detection/blob/ddff5e1871fb63bbb34f46db6785534ed34c017a/zebrafish_maskrcnn.py) file in `inference mode` locally. 

```python
# get help
# specify the detect images and image type as well as the output folder
python inference.py --help
```

If you want to use TensorMask to identify the objects, please refer to another python file, [zebrafish_tensormask.py](https://github.com/gonggqing/detectron2/blob/main/DLMA-workflow/inference.py). Through this model you can also see the identification results in real-time, which is supported by Open CV.

We have opened our image library and annotations, if you want to re-train this model locally, please download the train.json, test.json, train images and test images(see [images_url.txt](https://github.com/gonggqing/zebrafish_detection/blob/fa6b5911c9373ff5d726fe5b4af44394f8cb81f5/images/images_url.txt)). The json file contains annotations of `COCO style`. After downloading the images and files, register the coco instances in your code, modify the name and path of the register.
```python
# train registry
from detectron2.data.datasets import register_coco_instances
register_coco_instances("zebrafish_train", {}, "/your/path/train.json",
                        "/your/path/train_images/")
register_coco_instances("zebrafish_train", {}, "/your/path/test.json",
                        "/your/path/test_images/")
```
## :hatching_chick: Expected results
After the model inference, we can acquire a `csv file` which cotains the quantitative parameters of specific organs and abnormal phenotypes, these information demonstrate every detail of one specific zebrafish, and we can use them to analyze the developmental status of each zebrafish.

![results](https://user-images.githubusercontent.com/57084033/177250653-fbf07d17-8ba5-4be0-838c-360d66022691.png)

## Developer comments
We will continually update this repository and add more images to our library, if you want to use this work, please cite our [research article](https://pubs.acs.org/doi/10.1021/acs.est.3c00593).
