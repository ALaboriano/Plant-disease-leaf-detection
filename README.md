# Detección de enfermedades en las hojas de plantas agrícolas 

## Descripción
Utilizamos YOLOv7, el cual es una técnica de Visión Computacional especializadas en la detección de objetos, para construir un sistema que detecte las principales enfermedades que afectan a las hojas de diferentes plantas agrícolas.

**Resumen de los resultados obtenidos**
[![Leaf disease detection-Cover](https://github.com/ALaboriano/Plant-disease-leaf-detection/assets/52020337/ecb753d9-baa0-43b3-92b9-55b447dec1e5)
](https://youtu.be/Uh2y80x36rI?si=i5McKAcqqSXiqwUz)

## Fuente de datos - Dataset

Todos los experimentos se realizaron utilizando el dataset publico PlantDoc, el cual fue publicado en 2019 por investigadores del Instituto Indio de Tecnología, este conjunto de datos contiene un total de 2,598 imágenes de 13 especies de plantas y 17 clases de enfermedades. Para mayor detalle puede consultar el paper original titulado [PlantDoc: A Dataset for Visual Plant Disease Detection](https://arxiv.org/pdf/1911.10317.pdf), y para descargar el dataset en cualquiera de los formatos de la Fig 1. consultar en [PlantDoc Dataset  resize-416x416](https://public.roboflow.com/object-detection/plantdoc).

![Fig 1. Formatos disponibles - PlantDoc](https://user-images.githubusercontent.com/52020337/208254010-19037e53-838e-455c-a5e9-d7a4815d8d0b.JPG)
 
El formato del dataset utilizado es ``YOLO v7 PyTorch`` y la dimensión escogida fue de 416 x 416.

## Preparación de los datos.   

El dataset descargado solamente contiene subdatasets de ``train`` y ``test``; por lo que a ``train`` se lo dividió en dos subdatasets, uno con data para que el modelo aprenda ``train`` (contiene el 90% de datos) y el otro para validar el aprendizaje del modelo llamado ``valid`` (contiene el 10% de los datos).

Como el objetivo es solamente trabajar con hojas que contengan alguna enfermedad, solamente se conservó aquellas imágenes y etiquetas de plantas que efectivamente presenten alguna enfermedad en sus hojas, es así que del dataset original con 30 clases, se pasó a un dataset de experimentación final con 17 clases.

La distribución de imágenes en los datasets finales quedó de la siguiente manera: 1,409 imágenes en el dataset de entrenamiento, 145 imágenes en el dataset de validación y 148 imágenes en el dataset de test.

![Fig 2. Preparación de la data](https://user-images.githubusercontent.com/52020337/208259463-e307af60-f31b-4993-abb2-ed6de3cb9fd3.JPG)

La distribución relativa de las 17 clases, es similar en los tres datasets construidos. En la figura 3 se observa que para el entrenamiento no se contó con la misma cantidad de imágenes por clase, lo cual se ve reflejado en el entrenamiento del modelo, el cual suele aprender a generalizar mejor mientras más datos para aprender tenga.

![Fig 3. Número de imágenes por clase](https://user-images.githubusercontent.com/52020337/208259540-5a377565-f3c4-4b39-9e9d-3b27bc150a5e.JPG)

## Metodología.
De las principales técnicas que existe para solucionar tareas de detección de objetos, en este caso se ha seleccionado You Only Look Once en su versión más  reciente [YOLOv7](https://arxiv.org/abs/2207.02696).

El código fuente de YOLOv7 utilizado para la experimentación se encuentra disponible en su repositorio oficial de github [Official YOLOv7](https://github.com/WongKinYiu/yolov7).

## Experimentación - Entrenamiento

Los principales parámetros configurados para el entrenamiento del modelo fueron:  
**``!python train.py --img-size 416 --batch 32 --epochs 500 --data './data_custom.yaml' --cfg './models/custom_yolov7.yaml' --weights 'yolov7.pt'``**

- **img**: Parámetro que recibe como argumento el tamaño de las imágenes de entrada.
- **batch**: Parámetro que recibe como argumento el tamaño del lote que utilizará el modelo para aprender.
- **epochs**: Parámetro que recibe como argumento el número de épocas durante las cuales se entrenará el modelo. 
- **data:** Parámetro que recibe como argumento la ruta donde se encuentra el archivo **.yaml** con la configuración de la data customizada (directorio con data de entrenamiento, directorio con data de validación, nc = número de clase y names = nombre de las clases). 
- **cfg**: Parámetro que recibe como argumento la ruta donde se encuentra el archivo **.yaml** con la configuración del modelo customizado (Solamente se modificó el número de clases, se seteó en nc = 17).
- **weights**: Parámetro que recibe como argumento los pesos iniciales con los que se iniciará el entrenamiento .Los pesos iniciales del entrenamiento se configuró utilizando los pesos de la red YOLOv7 entrenado en el dataset de [COCO](https://cocodataset.org/), estos pesos se encuentran disponibles en https://github.com/WongKinYiu/yolov7/releases/download/v0.1/yolov7.pt

Además, para tratar de controlar el Overfiting YOLO tiene implementado el hiperparámetro **early stopping**, el cual controla que si durante 100 épocas el mAP no mejora, se detienen el proceso. También, YOLO tiene implementado **checkpoints** que permiten guardar un modelo por cada época de entrenamiento, además de siempre guardar el mejor modelo en el archivo **“best.pt”** cada vez que en una época se va superando el rendimiento de épocas anteriores, lo cual al final del entrenamiento  hace que se pueda recuperara fácilmente el mejor modelo obtenido durante todas las épocas de entrenamiento.

Finalmente el entrenamiento tomó aproximadamente 3 horas, utilizando **Colab Pro**, el cual generalemente brinda una capacidad de 15GB de GPU.

## Métricas de rendimiento
En la Fig 4. se observa que el modelo aprenda satisfactoriamente durante las 200 primeras épocas, luego como que todas las métricas de rendimiento decaen, tendiendo a presentarse problemas de overfiting, el cual a lo mejor puede controlarse con más imágenes para entrenar o modificando la red original de YOLO para extraer mejor las características para el entrenamiento del modelo.

![Fig 4. Performance datasetvalidación](https://user-images.githubusercontent.com/52020337/208584353-1bd93485-f0ef-41b4-8ecb-c76b3dfebfd7.JPG)

El mAP (Mean Average Presicion) general es de 44%, superior al  38.9% reportado en el paper original [PlantDoc: A Dataset for Visual Plant Disease Detection](https://arxiv.org/pdf/1911.10317.pdf) en el cual utilizaron como técnicas de detección de objetos a Faster-rcnn-inception-resnet. La mayoría de enfermedades son detectadas correctamente por el modelo, y hay enfermedades relacionadas a la hoja del tomate que el modelo no esta logrando detectar satisfactoriamente (Fig. 5.)

![Fig 5. mAP - Test Dataset](https://user-images.githubusercontent.com/52020337/208587710-345aa874-b4b7-4fbd-87a6-d14256c6cf61.JPG)

En la Fig. 6, se puede observar que la deficiente detección de enfermedades en las hojas del tomate es principalmente debido a dos motivos 1) No logra distinguirlas del fondo de la imagen y 2) Lo confunde con otras enfermedades que afectan a la misma hoja.

![Fig 6. Confusion matrix](https://user-images.githubusercontent.com/52020337/208589563-c452eadb-4855-4086-9a20-0ff2a31bf46e.JPG)

## Resultados
En la mayoría de imágenes el modelo logra identificar y clasificar correctamente las zonas dónde se encuentra presente una enfermedad que está afectando la hoja (Fig 7).

![Fig 7. Predicción test-dataset](https://user-images.githubusercontent.com/52020337/208589804-4fa27f13-c0d0-42f9-a37f-2558b9684f53.png)

Finalmente, el modelo entrenado se pasó sobre un video, construído a partir de las siguientes fuentes:   
    - https://www.youtube.com/watch?v=L8sQWnSvZDI   
    - https://www.youtube.com/watch?v=iiOsKkcObSA   
    - https://www.youtube.com/watch?v=vTcPoDX0i5k   
    - https://www.youtube.com/watch?v=7kLHlbFhTDU   
    - https://www.youtube.com/watch?v=5eEjDsLu1DQ&t=47s   
    - https://www.youtube.com/watch?v=38XomXO4Tog   
    - https://www.youtube.com/watch?v=2Y77KEYuw_g   
    - https://www.youtube.com/watch?v=2Y77KEYuw_g   
    - https://www.youtube.com/watch?v=rBbAvUP0dWg  
    - https://www.youtube.com/watch?v=pQJ2Sj1FEcw  
    - https://www.youtube.com/watch?v=puyD8VPrV94  
    - https://www.youtube.com/watch?v=q14i9daHwvQ  
    - https://www.youtube.com/watch?v=QsO2LQTZb0w  

El video lo puede ver en [](), y podrá observar que efectivamente el modelo logra identificar las áreas infectadas de las hojas evaluadas.
