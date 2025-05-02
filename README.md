# Aprendizaje Federado con MNIST

Este repositorio contiene el desarrollo de un flujo de trabajo de aprendizaje federado utilizando la base de datos MNIST y un modelo de TensorFlow personalizado. Esta actividad fue realizada como parte del curso de Computación en la Nube del Tecnológico de Monterrey.

## Estructura del Repositorio

```
.
├── TheModel.py              # Definición del modelo global de TensorFlow
├── local_train.ipynb        # Entrenamiento local para cada cliente (por integrante del equipo)
├── global_model.py          # Agregación global usando FedAvg y otros dos métodos
├── Partes_Modelo/           # Modelos entrenados por separado
└── README.md                # Este archivo
```

## Descripción de la Actividad

1. **Repositorio Base:** Se consultó el repositorio de clase en [Cloud_Computing_ITESM](https://github.com/JesusASmx/Cloud_Computing_ITESM/tree/main/Federado) como punto de partida.

2. **Modelo Personalizado:** Se implementó un modelo de TensorFlow distinto al visto en clase. Este se encuentra en el archivo `TheModel.py`.

3. **División de Datos:** Fuera del repositorio (por motivos de confidencialidad), se dividió el conjunto MNIST en `n` partes estadísticamente equivalentes, siendo `n` el número de integrantes del equipo (5).

4. **Entrenamiento Local:** Cada integrante entrenó su subconjunto de datos localmente. El código correspondiente se encuentra en `local_train.ipynb`, e incluye:
   - Ciclo de entrenamiento local.
   - Curvas de aprendizaje (accuracy y loss).
   - Reporte de clasificación (`classification_report` de `sklearn`).

5. **Agregación Global:**
   - El archivo `global_model.py` contiene la lógica de agregación de los modelos locales.
   - Se implementaron tres métodos de agregación:
     - **FedAvg (Federated Averaging):** Promedio ponderado de los pesos.
     - **FedMedian:** Promedio basado en la mediana de los parámetros.
     - **FedProx:** Agregación con penalización por desviación del modelo global.

6. **Análisis de Resultados:**
   El aprendizaje federado es una técnica de machine learning descentralizada donde diferentes dispositivos colaboran en entrenar un modelo en común con datos privados. De manera que, para poder comprender el rendimiento de cada modelo separado, se han utilizado tres técnicas para evaluar un modelo global: FedAvg, FedMedian y FedWeightedAvg. A grandes rasgos, FedAvg es un algoritmo que toma todos los modelos y básicamente hace un promedio, como su nombre lo dice, así como FedMedian utiliza la media en vez del promedio, y finalmente FedWeightedAvg sí hace un promedio pero dando prioridad al tamaño de los datos y darles así un peso.

   Ahora, para el análisis de los resultados, al parecer, el algoritmo que mejor resultado tuvo fue FedMedian. Es posible que FedMedian haya tenido un mayor puntaje en las tres métricas porque al tomar en cuenta la media, reduce la información de pesos extremos, es decir, outliers, y a su vez, tiene mayor robustez cuando se trata del desbalanceo de datos. Por otro lado, las técnicas de promedio tuvieron un puntaje menor en las tres métricas. Por lo que respecta a FedWeightedAvg, este toma en cuenta el que tenga el mayor cantidad de datos, sin embargo, todos los modelos trabajaron con una cantidad similar de datos. Es así como FedAvg también fallo, pero a diferencia del anterior, este sí toma a todos los modelos por igual, el problema es que si los datos están muy desbalanceados o sesgados, esos pesos dominarán en el modelo global.
