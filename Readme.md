# Expresiones en el rostro | Reconocimiento facial 

> De manera básica, el cual se utilizará la cámara de tu PC para probar la app, y además deberás poner tu mejor cara para ello (tarea difícil no?).

* El código queda a disposición

Documentación de:

https://github.com/justadudewhohacks/face-api.js

* Utilizará tu cámara para las pruebas, sino cómo?

En el archivo inde.html lo siguiente:

```html
  <video id="video" width="720" height="560" autoplay muted></video>
```

Creamos la variable de video, utilizando el id de `video`:

```js
const video = document.getElementById('video');
```

```js
function startCamara(){
  navigator.getUserMedia = (
    navigator.getUserMedia || 
    navigator.webkitGetUserMedio ||
    navigator.mozGetUser ||
    navigator.msGetUser);

  navigator.getUserMedia({ video: {} },
    stream => video.srcObject = stream,
    err => console.log(err)
  )
}
```

```js
    .withFaceLandmarks() //dibuja el rostro
    .withFaceExpressions(); //detecta expresiones | emociones
```

```js
Promise.all([
  faceapi.nets.tinyFaceDetector.loadFromUri('/models'),
  faceapi.nets.faceLandmark68Net.loadFromUri('/models'),
  faceapi.nets.faceRecognitionNet.loadFromUri('/models'),
  faceapi.nets.faceExpressionNet.loadFromUri('/models')
]).then(startCamara);
```


```js
    faceapi.draw.drawDetections(canvas, resizesDetections); //detecta el rostro y lo enmarca
    faceapi.draw.drawFaceLandmarks(canvas, resizesDetections); //detecta el rostro y lo dibuja
    faceapi.draw.drawFaceExpressions(canvas, resizesDetections);//detecta el estado de ánimo
```


```js
video.addEventListener('play', ()=>{
  const canvas = faceapi.createCanvasFromMedia(video);
  document.body.append(canvas);

  const displaySize = {width: video.width, height: video.height};
  faceapi.matchDimensions(canvas, displaySize);

  setInterval(async() =>{
    const detections = await faceapi.detectAllFaces(video, new faceapi.TinyFaceDetectorOptions())
    .withFaceLandmarks() //dibuja el rostro
    .withFaceExpressions(); //detecta expresiones | emociones

    const resizesDetections = faceapi.resizeResults(detections, displaySize);
    canvas.getContext('2d').clearRect(0,0, canvas.width, canvas.height);
    faceapi.draw.drawDetections(canvas, resizesDetections); //detecta el rostro y lo enmarca
    faceapi.draw.drawFaceLandmarks(canvas, resizesDetections); //detecta el rostro y lo dibuja
    faceapi.draw.drawFaceExpressions(canvas, resizesDetections);//detecta el estado de ánimo

  }, 500); // medio segundo
})
```



