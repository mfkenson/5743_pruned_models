# Prepare pruned models

https://github.com/daquexian/onnx-simplifier/blob/master/onnxsim/onnx_simplifier.py

### going into distiller
```
conda activate distiller
cd distiller/examples/classifier_compression/
```


### pruning

mobilenet
```
ARCHNAME=mobilenet && CONFNAME='mobilenet.schedule_sensitivity.yaml' && \
python compress_classifier.py -a $ARCHNAME --lr 0.0045 -p 50 ~/tiny-imagenet-200 -j 4 -b 128 --epochs 200 --compress=~/pruned_models/$CONFNAME
```

resnet18
```
ARCHNAME=resnet18 && CONFNAME='resnet18.schedule_sensitivity.yaml' && \
python compress_classifier.py -a $ARCHNAME --lr 0.0045 -p 50 ~/tiny-imagenet-200 -j 4 -b 128 --epochs 200 --compress=~/pruned_models/$CONFNAME
```

inceptionv4
```
ARCHNAME=inceptionv4 && CONFNAME='inceptionv4.schedule_sensitivity.yaml' && \
python compress_classifier.py -a $ARCHNAME --lr 0.0045 -p 50 ~/tiny-imagenet-200 -j 4 -b 128 --epochs 200 --compress=~/pruned_models/$CONFNAME

```

### export-onnx
```
ARCHNAME=mobilenet && PTHNAME=mobilenet_34.67_66.84.pth.tar && python compress_classifier.py ~/tiny-imagenet-200 --arch $ARCHNAME --resume-from "~/pruned_models/$PTHNAME" --evaluate --export-onnx
```

### onnx2trt
```
onnx2trt -b1 -w256000000 mobilenet_34.67_66.84.onnx -o mobilenet_34.67_66.84.trt
onnx2trt -b1 -w256000000 mobilenet_36.04_37.97.onnx -o mobilenet_36.04_37.97.trt
onnx2trt -b1 -w256000000 mobilenet_41.75_33.35.onnx -o mobilenet_41.75_33.35.trt
onnx2trt -b1 -w256000000 resnet18_39.52_66.54.onnx -o resnet18_39.52_66.54.trt
onnx2trt -b1 -w256000000 resnet18_40.87_46.49.onnx -o resnet18_40.87_46.49.trt
onnx2trt -b1 -w256000000 resnet18_42.63_15.38.onnx -o resnet18_42.63_15.38.trt
onnx2trt -b1 -w256000000 resnet18_58.17_71.87.onnx -o resnet18_58.17_71.87.trt
onnx2trt -b1 -w256000000 resnet18_58.66_55.21.onnx -o resnet18_58.66_55.21.trt
```
