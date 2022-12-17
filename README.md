# Detección de enfermedades en las hojas de plantas agrícolas 

## Descripción
Se va a utilizar técnicas de Visión Computacional especializadas en la detección de objetos, de manera que se pueda construir un sistema de detección de enfermedades que afectan a las hojas de diferentes plantas agrícolas.
## Fuente de datos - Dataset

Todos los experimentos se realizaron utilizando el dataset publico PlantDoc, el cual fue publicado en 2019 por investigadores del Instituto Indio de Tecnología, este conjunto de datos contiene un total de 2,598 imágenes de 13 especies de plantas y 17 clases de enfermedades. Para mayor detalle puede consultar el paper original titulado [PlantDoc: A Dataset for Visual Plant Disease Detection](https://arxiv.org/pdf/1911.10317.pdf), y para descargar el dataset en cualquiera de los formatos de la Fig 1. consultar en [PlantDoc Dataset  resize-416x416](https://public.roboflow.com/object-detection/plantdoc).

![Fig 1. Formatos disponibles - PlantDoc](https://user-images.githubusercontent.com/52020337/208254010-19037e53-838e-455c-a5e9-d7a4815d8d0b.JPG)
 
El formato del dataset utilizado es ``YOLO v7 PyTorch`` y la dimensión escogida fue de 416 x 416.

## Preparación de los datos.   

El dataset descargado solamente contiene subdatasets de ``train`` y ``test``; por lo que a ``train`` se lo dividió en dos subdatasets, uno con data para que el modelo aprenda ``train`` (contiene el 90% de datos) y el otro llamado ``valid`` (contiene el 10% de los datos).

Como el objetivo es solamente trabajar con hojas que contengan alguna enfermedad, solamente se conservó aquellas imágenes y etiquetas de plantas que efectivamente presenten alguna enfermedad en sus hojas, es así que del dataset original con 30 clases, se pasó a un dataset de experimentación final con 17 clases.

## Metodología.
De las principales técnicas que existe para solucionar tareas de detección de objetos, en este caso se ha seleccionado You Only Look Once en su versión más  reciente [YOLOv7](https://arxiv.org/abs/2207.02696).



La distribución de imágenes en el dataset final empleado, quedó de la siguiente manera: 1,409 imágenes en el dataset de entrenamiento, 145 imágenes en el dataset de validación y 148 imágenes en el dataset de test.

La distribución relativa de las 17 clases, es similar en los tres datasets construidos. En la figura 2 se observa que para el entrenamiento no se contó con la misma cantidad de imágenes por clase, lo cual se ve reflejado en el entrenamiento del modelo, el cual suele aprender a generalizar mejor mientras más dataos para aprender tenga.

![Número de imágenes por clase](https://user-images.githubusercontent.com/52020337/208254400-18495cfb-7a4b-4fbe-b8ab-4e49f2cec595.JPG)

