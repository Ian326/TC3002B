# Prediccion de Ranking en TETR.IO
## Descripcion del Proyecto 📝
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

## Resultados y evaluacion inicial

## Resultados mejorados

## Referencias
[Karlsson, E., & Jansson, A. (2022). Neural networks for standardizing ratings in League of Legends (Bachelor's thesis, Örebro University). Örebro University.](https://www.diva-portal.org/smash/get/diva2:1718213/FULLTEXT01.pdf)

[Sen, D., Roy, R. K., Majumdar, R., Chatterjee, K., & Ganguly, D. (n.d.). Prediction of the final rank of players in PUBG with the optimal number of features [Conference paper or manuscript]. Departments of Computer Science and Engineering, University of Calcutta & Government College of Engineering and Leather Technology.](https://arxiv.org/pdf/2107.09016)

[Zhang, C., Wang, K., Chen, H., Fan, G., Jie, Y., Wen, L., & Zheng, B. (2022, October). QuickSkill: Novice skill estimation in online multiplayer games. In Proceedings of the 31st ACM International Conference on Information and Knowledge Management (CIKM '22) (pp. 4149–4158). ACM. https://doi.org/10.1145/3511808.3550700](https://arxiv.org/pdf/2208.07704)

## Uso
Execute the `TetrisRankPrediction.ipynb` notebook under the `Code` folder. 

If you want to test it on the cloud, download the dataset required (`tl-data-09-2023.csv`) under the `content` folder and upload it directly into the Google Colab 'Runtime File Folder'

-------------------
This repo is under construction. Expect weekly changes according to the requirements of my course. ;)

![](https://media.tenor.com/80HFRoLbNWcAAAAM/shrugging-shoulders-shrug-shoulders.gif)
