## Info
This repo contains converter and quantizer for ultralytics yolov5 model.
Based of fork https://github.com/zldrobit/yolov5 of original yolov5 repo with changed converting process. Converted works 
with old yolov5 release 3 and with release 5 as well. 

`models/tf.py` uses TF2 API to construct a tf.Keras model according to `*.yaml` config files and reads weights from `*.pt`, without using ONNX. 
Repo also contain android app for test on photos and videos.

**Because this branch persistently rebases to master branch of ultralytics/yolov5, use `git pull --rebase` or `git pull -f` instead of `git pull`.**

## Usage
### 1. Git clone `yolov5`

```
git clone https://github.com/HFVladimir/yolov5.git
cd yolov5
```

### 2. Depends on task checkout target branch
* For converting yolov5 release3 model 
```
git checkout tf-export
```
* For converting yolov5 release4 and release5 model 
```
git checkout tf-android
```
* For testing with mobile app
```
git checkout tf-android
```

### 3. Install requirements
```
pip install -r requirements.txt
pip install tensorflow==2.4.0
```

### 3. Convert and verify
- Convert weights to TensorFlow SavedModel, GraphDef and fp16 TFLite model
- Depends on speed/accuracy tradeoff pick image size `320` or `640`. Tested both.
- As the results you'll get tflite weights in `weights/<weights_filename>.tflite`
```
PYTHONPATH=. python models/tf.py --weights weights/<weights_filename>.pt --cfg models/<yaml_config>.yaml --img 320
```

### 4. Run android demo app
Open `android/app` in Android studio. Do not forget to checkout `tf-android`
1. Set mode (image or video) by changing the [manifest file](https://github.com/HFVladimir/yolov5/blob/041177a9ac51dddd43aee0cff5c16f25fde19353/android/app/src/main/AndroidManifest.xml#L28)
    *  .MainActivity for testing on photo
    *  .DetectorActivity for testing on video
2. Put tflite model in [assets folder](https://github.com/HFVladimir/yolov5/tree/tf-android/android/app/src/main/assets)
3. Change model file name and input image size
    * Image size should be the same as used for converting model
    * For video change filename [here](https://github.com/HFVladimir/yolov5/blob/041177a9ac51dddd43aee0cff5c16f25fde19353/android/app/src/main/java/org/tensorflow/lite/examples/detection/tflite/DetectorFactory.java#L39)
     and size [here](https://github.com/HFVladimir/yolov5/blob/041177a9ac51dddd43aee0cff5c16f25fde19353/android/app/src/main/java/org/tensorflow/lite/examples/detection/tflite/DetectorFactory.java#L42)
    * For image change filename [here](https://github.com/HFVladimir/yolov5/blob/041177a9ac51dddd43aee0cff5c16f25fde19353/android/app/src/main/java/org/tensorflow/lite/examples/detection/MainActivity.java#L83)
     and size [here](https://github.com/HFVladimir/yolov5/blob/041177a9ac51dddd43aee0cff5c16f25fde19353/android/app/src/main/java/org/tensorflow/lite/examples/detection/MainActivity.java#L79)
4. Additionally:
    * For yolov5 release3 following [loop](https://github.com/HFVladimir/yolov5/blob/041177a9ac51dddd43aee0cff5c16f25fde19353/android/app/src/main/java/org/tensorflow/lite/examples/detection/tflite/YoloV5Classifier.java#L409)
     should be commented. For release4 and 5 uncomment it.
    * You can change photo in .MainActivity [here](https://github.com/HFVladimir/yolov5/blob/041177a9ac51dddd43aee0cff5c16f25fde19353/android/app/src/main/java/org/tensorflow/lite/examples/detection/MainActivity.java#L62). 
    Image should be in assets folder.
    * You can enable/disable GPU [here](https://github.com/HFVladimir/yolov5/blob/041177a9ac51dddd43aee0cff5c16f25fde19353/android/app/src/main/java/org/tensorflow/lite/examples/detection/tflite/YoloV5Classifier.java#L239)
 
### 4. For additional documentation 
Read [Original readme](https://github.com/HFVladimir/yolov5/blob/tf-android/README_ORIGIN.md)
 and ask questions at [issue](https://github.com/ultralytics/yolov5/pull/1127) at ultralytics/yolov5 repo.
 
 


