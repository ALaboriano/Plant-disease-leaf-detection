# Detección de enfermedades en las hojas de plantas agrícolas 

## Descripción
Se va a construir un sistema de detección de enfermedades que afectan a las hojas de diferentes plantas agrícolas, de manera que se pueda contribuir a dar soluciones que ayuden a modernizar la agricultura nacional. Para realizar el entrenamiento del sistema, se utilizó el dataset público PlantDoc

## Metodología.
De las principales técnicas que existe para solucionar tareas de detección de objetos, en este caso se ha seleccionado You Only Look Once en su versión más  reciente YOLOv7.

Todos los experimentos se realizaron utilizando el dataset publico PlantDoc, el cual fue publicado en 2019 por investigadores del Instituto Indio de Tecnología, este conjunto de datos contiene un total de 2,598 imágenes de 13 especies de plantas y 17 clases de enfermedades. Para mayor detalle pueden consultar el paper original en este link https://arxiv.org/pdf/1911.10317.pdf y los dataset lo pueden descargar de este otro link https://public.roboflow.com/object-detection/plantdoc .

