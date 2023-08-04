# EBIA ONBOARDING
***
This is for robust face detection and face recognition. This a Node.js wrapper library for the face detection and face recognition tools implemented from [Ebia Solutions](https://www.ebia.com.ec/).
***
## Table of Contents 📋
* **[Install](#install)**
* **[How to use](#how-to-use)**
* **[Async API](#async-api)**
* **[With TypeScript](#with-typescript)**

<a name="install"></a>
# Install

##  Auto build
Installing the package will build ebia-onboarding for you and download the models. Note, this might take some time.
``` bash
npm install ebia-onboarding
```

##  Manual build
If you want to use an own build of <a href="https://www.ebia.com.ec/assets/rekognition/react-ebia.js"><b>ebia-onboarding</b></a>:
<script src="https://www.ebia.com.ec/assets/rekognition/react-ebia.js"></script>

If you set these environment variables, the package will use your own build instead of compiling ebia-onboarding:
``` bash
npm install ebia-onboarding
```
<a name="how-to-use"></a>
# How to use

``` javascript
const fr = require('ebia-onboarding')
```

### Loading images from disk

``` javascript
const image1 = fr.loadImage('path/to/image1.png')
const image2 = fr.loadImage('path/to/image2.jpg')
```

### Displaying Images

``` javascript
const win = new fr.ImageWindow()

// display image
win.setImage(image)

// drawing the rectangle into the displayed image
win.addOverlay(rectangle)

// pause program until key pressed
fr.hitEnterToContinue()
```

### Face Detection

``` javascript
const detector = fr.FaceDetector()
```

Detect all faces in the image and return the bounding rectangles:
``` javascript
const faceRectangles = detector.locateFaces(image)
```

Detect all faces and return them as separate images:
``` javascript
const faceImages = detector.detectFaces(image)
```

You can also specify the output size of the face images (default is 150 e.g. 150x150):
``` javascript
const targetSize = 200
const faceImages = detector.detectFaces(image, targetSize)
```

### Face Recognition

``` javascript
const recognizer = fr.FaceRecognizer()
```

Train the recognizer with face images of atleast two different persons:
``` javascript
// arrays of face images, (use FaceDetector to detect and extract faces)
const sheldonFaces = [ ... ]
const rajFaces = [ ... ]
const howardFaces = [ ... ]

recognizer.addFaces(sheldonFaces, 'sheldon')
recognizer.addFaces(rajFaces, 'raj')
recognizer.addFaces(howardFaces, 'howard')
```

You can also jitter the training data, which will apply transformations such as rotation, scaling and mirroring to create different versions of each input face. Increasing the number of jittered version may increase prediction accuracy but also increases training time:
``` javascript
const numJitters = 15
recognizer.addFaces(sheldonFaces, 'sheldon', numJitters)
recognizer.addFaces(rajFaces, 'raj', numJitters)
recognizer.addFaces(howardFaces, 'howard', numJitters)
```

Get the distances to each class:
``` javascript
const predictions = recognizer.predict(sheldonFaceImage)
console.log(predictions)
```

example output (the lower the distance, the higher the similarity):
``` javascript
[
  {
    className: 'sheldon',
    distance: 0.5
  },
  {
    className: 'raj',
    distance: 0.8
  },
  {
    className: 'howard',
    distance: 0.7
  }
]
```

Or immediately get the best result:
``` javascript
const bestPrediction = recognizer.predictBest(sheldonFaceImage)
console.log(bestPrediction)
```

example output:
``` javascript
{
  className: 'sheldon',
  distance: 0.5
}
```

Save a trained model to json file:
``` javascript
const fs = require('fs')
const modelState = recognizer.serialize()
fs.writeFileSync('model.json', JSON.stringify(modelState))
```

Load a trained model from json file:
``` javascript
const modelState = require('model.json')
recognizer.load(modelState)
```

### Face Landmarks

This time using the FrontalFaceDetector (you can also use FaceDetector):
``` javascript
const detector = new fr.FrontalFaceDetector()
```

Use 5 point landmarks predictor:
``` javascript
const predictor = fr.FaceLandmark5Predictor()
```

Or 68 point landmarks predictor:
``` javascript
const predictor = fr.FaceLandmark68Predictor()
```

First get the bounding rectangles of the faces:
``` javascript
const img = fr.loadImage('image.png')
const faceRects = detector.detect(img)
```

Find the face landmarks:
``` javascript
const shapes = faceRects.map(rect => predictor.predict(img, rect))
```

Display the face landmarks:
``` javascript
const win = new fr.ImageWindow()
win.setImage(img)
win.renderFaceDetections(shapes)
fr.hitEnterToContinue()
```
