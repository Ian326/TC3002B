# Predicción de Ranking en TETR.IO
## Descripción del Proyecto 📝
En este documento se proporciona una visión general del proyecto "Predicción del ranking de los jugadores de tetr.io" en el cuál se utilizan redes neuronales y un conjunto de datos con información compilada de la API de [tetr.io](https://tetr-io.translate.goog/about/api/?_x_tr_sl=en&_x_tr_tl=es&_x_tr_hl=es&_x_tr_pto=tc).

## Contenido 📚
*Archivos*
* *content*
  - **tl-data-09-2023.csv** - El dataset a utilizar, que se puede encontrar en [TETR.IO Player data](https://www.kaggle.com/datasets/inferno2332/tl-data)
* Code
  - **TetrisRankPrediction.ipynb** - Script donde se obtiene, configura, entrena y utiliza el modelo

## Dataset 🔍
* Nombre del dataset: TETR.IO Player data
* Cantidad de registros: ~45,200
* Cantidad de variables a utilizar: 9
* Cantidad de labels a predecir: 1
* Cantidad de valores posibles a predecir: 17

  
### Variables
  * **ID** - Asignado al jugador
  * **Username** - Nombre de usuario del jugador
  * **Games Played** - Cantidad de juegos en la modalidad multijugador
  * **Games Won** - Cantidad de juegos ganados en la modalidad de multijugador
  * **TR** - Valor numerico asignado por tetr.io basado en su cantidad de partidas, puntos totales, tiempo de juego, entre otras variables internas. (No es directamente proporcional al utilizado en un sistema de ELO)
  * **Glicko (User's Glicko-2 rating)** - Puntuacion obtenida por el jugador en partidas clasificatorias (acumulable)
  * **RD (User's rating deviation)** - Margen de error sobre la puntacion del jugador obtenida en partidas clasificatorias
  * **TETRA LEAGUE rank** - LABEL (Variable a predecir)
  * **APM** Cantidad de ataques promedio del jugador en partidas multijugador en los ultimos 10 juegos
  * **PPS** Cantidad de piezas colocadas por el jugador en partidas multijugador en los ultimos 10 juegos
  * **VS** Cantidad de puntos promedios alcanzados por el jugador en partidas multijugador en los ultimos 10 juegos (SCORE)
  * **Verified** Validacion si la cuenta de usuario está verificada o no
  * **Country** Pais de origen de la cuenta de usuario
  * **40 LINES personal best** Puntuación máxima alcanzada por el jugador en el modo de juego "40LINES" (menor es mejor)
  * **BLITZ personal best** Puntuación máxima alcanzada por el jugador en el modo de juego "BLITZ" (mayor es mejor)

## Objetivo 🎯
El objetivo principal de este proyecto es la predicción del nivel de un jugador de tetris basado en sus estadísticas de juego, sin tomar en cuenta el ranking otorgado por el mismo juego; pudiendo asi tener un mejor entendimiento de su nivel "real" dentro del juego, sin estar limitado por factores como una mala racha, o el nivel promedio de sus oponentes.

Si bien, el modelo desarrollado mediante el uso de redes neuronales, no presenta una contribución significativa en materia de investigación, puede verse como una alternativa a los métodos tradicionales del calculo del poder de un jugador dentro de juegos competitivos como lo es el sistema de ELO que se usa mayormente.

## Literatura consultada 📖
Como base de la metodología utilizada en este proyecto, se adoptaron algunas ideas propuestas en papers como son
*Novice skill estimation in online multiplayer games* (Chaoyun Zhang et al. 2022) & *Neural networks for standardizing ratings in League of Legends* (Erik Karlsson & Andréas Jansson 2022)

## Arquitectura ⚙️
El modelo neuronal utilizado en este proyecto es un **Perceptrón Multicapa (MLP)**, implementado con la API Sequential de Keras. Este tipo de red neuronal es una arquitectura de aprendizaje profundo que consiste en capas densamente conectadas (fully connected layers), donde cada neurona de una capa está conectada a todas las neuronas de la capa siguiente. 

En este caso, el modelo tiene tres capas densas: dos capas ocultas con `64 neuronas` cada una y una capa de salida con `17 neuronas`, que corresponde al número de clases en el problema de clasificación.

La primera capa oculta utiliza la función de activación **ReLU (Rectified Linear Unit)**, que es ampliamente utilizada en redes neuronales debido a su capacidad para introducir no linealidad y evitar problemas como el desvanecimiento del gradiente. La segunda capa oculta también utiliza ReLU, lo que permite que el modelo aprenda representaciones más complejas de los datos. Ambas capas están diseñadas para extraer características relevantes de las entradas y procesarlas de manera jerárquica.

La capa de salida utiliza la función de activación **Softmax**, que convierte las salidas de la red en probabilidades, asegurando que la suma de las probabilidades sea igual a 1. Esto es ideal para problemas de clasificación multiclase, como el presente, donde el objetivo es predecir la categoría (o rango) de un jugador en el juego Tetr.io. Cada neurona en la capa de salida representa una clase, y la neurona con la mayor probabilidad indica la predicción del modelo.

El modelo se compila utilizando el optimizador Adam, que es una variante del descenso de gradiente estocástico que ajusta dinámicamente las tasas de aprendizaje para cada parámetro. La función de pérdida utilizada es categorical_crossentropy, que es adecuada para problemas de clasificación multiclase. Además, la métrica de evaluación seleccionada es la exactitud (accuracy), que mide qué tan bien el modelo clasifica correctamente las instancias en el conjunto de datos. Esta arquitectura es simple pero efectiva para problemas de clasificación supervisada con datos tabulares.

## Resultados y evaluacion inicial 💹

El modelo entrenado alcanzó una exactitud (accuracy) en el conjunto de prueba de aproximadamente `84.49%`, mientras que en el conjunto de entrenamiento logró una exactitud de `87.93%`, lo que indica que el modelo generaliza bien y no presenta un sobreajuste significativo. El valor de pérdida (loss) en el conjunto de prueba fue de `0.4839`, mientras que en el conjunto de entrenamiento fue de `0.3776`, lo que sugiere que el modelo logró minimizar la función de pérdida de manera efectiva durante el entrenamiento. Además, el gráfico de precisión y pérdida por épocas muestra una convergencia estable, lo que confirma que el modelo fue entrenado correctamente.
Acc vs Epoch            | Loss vs Epoch
:-------------------------:|:-------------------------:
![image](https://github.com/user-attachments/assets/2ff74f07-2f67-47ab-93cb-261fa34df099) | ![image](https://github.com/user-attachments/assets/7dbcf704-97a2-4b68-95c0-9783c71d6a9f)

En cuanto a las métricas de clasificación, el precision, recall y F1-score para cada clase se calcularon utilizando el informe de clasificación. Estas métricas muestran un desempeño variable entre las clases, con algunas categorías alcanzando valores altos y otras más bajas, lo que podría deberse a un desbalance en los datos. La matriz de confusión revela que el modelo predice correctamente la mayoría de las clases, aunque existen confusiones entre algunas categorías específicas. Esto podría indicar que ciertas clases tienen características similares (especialmente en los rangos mas altos del competitivo multijugador). En general, los resultados son prometedores, pero podrían mejorarse con técnicas como el ajuste de hiperparámetros o el aumento de datos.
<p align="center">
  <img width="460" height="300" src="https://github.com/user-attachments/assets/4a8f8544-111a-4702-8884-54c8423c7470">
</p>

![image](https://github.com/user-attachments/assets/522126a4-8553-469b-bbdd-de33fc4b32b8)


## Resultados mejorados 🚀
Se realizaron varias mejoras al modelo para optimizar su desempeño. En primer lugar, se implementó un escalamiento de los datos utilizando la técnica de `MinMaxScaler`, que normaliza los valores de las características entre 0 y 1. Esto permitió que el modelo procesara los datos de manera más eficiente, reduciendo la influencia de valores extremos y mejorando la convergencia durante el entrenamiento. Este paso es crucial en redes neuronales, ya que evita que las características con valores más grandes dominen el proceso de aprendizaje.

Además, se incrementó el número de neuronas en las capas ocultas del modelo. Originalmente, cada capa oculta tenía `64 neuronas` , pero en la versión mejorada se aumentó a `96 neuronas por capa`. Este cambio permitió al modelo capturar patrones más complejos en los datos, lo que resultó en un mejor ajuste a las características del conjunto de entrenamiento y una mayor capacidad de generalización en el conjunto de prueba. Este aumento en la capacidad del modelo fue clave para mejorar su desempeño.

Los resultados muestran una mejora significativa en las métricas de desempeño. La exactitud (accuracy) en el conjunto de entrenamiento aumentó de `84.49%` a `94.88%`, mientras que la pérdida (loss) disminuyó de `0.4839` a `0.1207`, lo que indica que el modelo aprendió de manera más efectiva. En el conjunto de prueba, la exactitud pasó de `87.93%` a `94.65%`, y la pérdida se redujo de `0.3776` a `0.1312`, lo que demuestra que el modelo generaliza mejor y es menos propenso a errores en datos no vistos.
Acc vs Epoch            | Loss vs Epoch
:-------------------------:|:-------------------------:
![image](https://github.com/user-attachments/assets/1e50fd27-c4b0-4d77-91b9-04bcb902cee0) | ![image](https://github.com/user-attachments/assets/c2627b40-4a2b-4b8b-9d5b-d7a439f0ae13)


Finalmente, la matriz de confusión y las métricas como precision, recall y F1-score también mostraron mejoras notables. Las confusiones entre clases disminuyeron, y las métricas de clasificación reflejan un desempeño más consistente en todas las categorías. Esto sugiere que el modelo ahora es más robusto y capaz de distinguir entre las diferentes clases con mayor precisión, lo que lo hace más confiable para predecir el rango de los jugadores en Tetr.io.
<p align="center">
  <img width="460" height="300" src="https://github.com/user-attachments/assets/2df0ac88-f513-49bc-b50e-510b3c2b9f6d">
</p>

![image](https://github.com/user-attachments/assets/82c10bcf-83b8-4fd7-a294-c31a7fd09a02)



## Referencias
[1] [Karlsson, E., & Jansson, A. (2022). Neural networks for standardizing ratings in League of Legends (Bachelor's thesis, Örebro University). Örebro University.](https://www.diva-portal.org/smash/get/diva2:1718213/FULLTEXT01.pdf)

[2] [Sen, D., Roy, R. K., Majumdar, R., Chatterjee, K., & Ganguly, D. (n.d.). Prediction of the final rank of players in PUBG with the optimal number of features [Conference paper or manuscript]. Departments of Computer Science and Engineering, University of Calcutta & Government College of Engineering and Leather Technology.](https://arxiv.org/pdf/2107.09016)

[3] [Zhang, C., Wang, K., Chen, H., Fan, G., Jie, Y., Wen, L., & Zheng, B. (2022, October). QuickSkill: Novice skill estimation in online multiplayer games. In Proceedings of the 31st ACM International Conference on Information and Knowledge Management (CIKM '22) (pp. 4149–4158). ACM. https://doi.org/10.1145/3511808.3550700](https://arxiv.org/pdf/2208.07704)

## Uso
Execute the `TetrisRankPrediction.ipynb` notebook under the `Code` folder. 

If you want to test it on the cloud, download the dataset required (`tl-data-09-2023.csv`) under the `content` folder and upload it directly into the Google Colab 'Runtime File Folder'

-------------------
This repo is under construction. Expect weekly changes according to the requirements of my course. ;)

![](https://media.tenor.com/80HFRoLbNWcAAAAM/shrugging-shoulders-shrug-shoulders.gif)
