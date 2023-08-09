# EBIA ONBOARDING
***
This is for robust face detection and face recognition. This a Node.js wrapper library for the face detection and face recognition tools implemented from [Ebia Solutions](https://www.ebia.com.ec/).
***
## Table of Contents 📋
* **[Install](#install)**
* **[How to use](#how-to-use)**
* **[Example](#example)**
* **[With TypeScript](#with-typescript)**

<a name="install"></a>
# Install

##  AutoBuild - npm Component
Installing the package will build ebia-onboarding for you and download the models. Note, this might take some time.
``` bash
npm install ebia-onboarding
```

##  Manual build - Library
If you want to use an own build of <a href="https://www.ebia.com.ec/assets/rekognition/react-ebia.js"><b>ebia-onboarding</b></a>:
<script src="https://www.ebia.com.ec/assets/rekognition/react-ebia.js"></script>

If you set these environment variables, the package will use your own build instead of compiling ebia-onboarding:
``` bash
npm install ebia-onboarding
```
<a name="how-to-use"></a>
# How to use

``` javascript
import { EbiaOnboarding } from "ebia-onboarding";
```

## About Country
The functionality depends on the country.
For Ecuador, there is also the option of validation with the consulting Registro Civil Ecuador.
For other countries, it is necessary to send the photo for easy recognition.

### Face Recognition
#### Parameters
*  documento_identidad: It will ask you to upload a photograph of both the front and the back of the identity document.
*  servicio_basico: It will ask you to upload a basic service bill.
*  pais: (EC->Ecuador, BO->Bolivia)
*  prueba_vida: Options: PARPADEO, SONRISA, CARDINAL, BOSTEZO
*  cedula: National identity card number
*  nombres: First Name and Second Name
*  apellidos: First Last Name and Second Last Name
*  direccion: Address
*  foto: URL of the photograph to make the comparison.
*  letra_cedula: Font sizes of the titles for the capture of the photograph of the ID.
*  letra_prueba_vida: Size of the letter of the titles for the capture of the recognition
*  rekognitionSuccess: Successful response method.
*  rekognitionError: Error response method.
*  id_solicitud: Used to identify the process.
*  id_empresa: Company ID registered at ebia.com.ec
*  token: token generated with the input of the user, through api.
Only for Ecuador
*  rc: Use of query to Registro Civil Ecuador
*  huella: The first 6 digits of the fingerprint identifier. 

<a name="example"></a>
# Example

## Example of use without Registro Civil
``` javascript
<EbiaOnboarding
            documento_identidad={false}
            servicio_basico={false}
            pais={"EC"}
            prueba_vida={"PARPADEO"}
            cedula={"1803231727"}
            nombres={"MONICA CECILIA"}
            apellidos={"BENAVIDES PINTO"}
            direccion={"DE LOS HIGOS E1076 Y DE LOS CIRUELOS"}
            foto={"https://elebus-rekognition.s3.amazonaws.com/personas/1710445097.jpg"}
            letra_cedula={70}
            letra_prueba_vida={40}
            rekognitionSuccess={rekognitionSuccess}
            rekognitionError={rekognitionError}
            id_solicitud={23}
            id_empresa={22}
            token={"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJjaGVjayI6dHJ1ZSwiaWF0IjoxNjkxNTkyMjk1LCJleHAiOjE2OTE2Nzg2OTV9.b21NwICHH85Zm_e1lOLimsYsx8uo2BXvfPi1hShp97w"}
/>
```
## Example of use with Registro Civil
``` javascript
<EbiaOnboarding
            cedula={"1803231727"}
            nombres={"MONICA CECILIA"}
	apellidos={"BENAVIDES PINTO"}
	rc={true}
	huella={"V2333E"}
	prueba_vida={"SONRISA"}
	letra_cedula={70}
	letra_prueba_vida={50}
	id_empresa={22}
	id_solicitud={123456}
	rekognitionSuccess={rekognitionSuccess}
            rekognitionError={rekognitionError}
	token={"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJjaGVjayI6dHJ1ZSwiaWF0IjoxNjkxMDk2MjYwLCJleHAiOjE2OTExODI2NjB9.xumxrHfWcBpN89QYIbEgC5it3e_m_j-CbFiYMdZnFYY"}
/>
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

### Simple Example Component Autobuild

This time using the FrontalFaceDetector (you can also use FaceDetector):
``` javascript
const detector = new fr.FrontalFaceDetector()
```

fr.hitEnterToContinue()
```
