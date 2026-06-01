# **PLANTALYZE - Clasificación de especies de plantas**

## Descripción general
Plantalyze es un clasificador de especies de plantas que utiliza técnicas de aprendizaje para clasificar diferentes especies de plantas a partir de imágenes.

## Modelos implementados
Se implementaron 5 modelos basados en redes neuronales convolucionales (CNN) para la clasificación de las especies de plantas. Los modelos implementados son:
1. **Modelo 0**: CNN con 7 capas convolucionales en pirámide inversa (128-32). 
2. **Modelo 1**: CNN con 4 capas convolucionales entrenada con Adam. 
3. **Modelo 2**: CNN con 4 capas convolucionales entrenada con Adam, data augmentation y early stopping para evitar overfitting.
4. **Modelo 3**: CNN con 4 capas convolucionales entrenada con SGD y early stopping.
5. **Modelo 4**: CNN con 4 capas convolucionales entrenada con SGD, data augmentation y early stopping para evitar overfitting.

## Dataset

El dataset utilizado para el modelo es el [Plants Classification](https://www.kaggle.com/datasets/marquis03/plants-classification), el cual contiene 30000 imágenes de plantas de 30 especies diferentes. Cada especie de planta contiene 1000 imágenes.

Las clases presentes en el dataset son:
- aloevera
- banana
- bilimbi
- cantaloupe
- cassava
- coconut
- corn
- cucumber
- curcuma
- eggplant
- galangal
- ginger
- guava
- kale
- longbeans
- mango
- melon
- orange
- paddy
- papaya
- peperchili
- pineapple
- pomelo
- shallot
- soybeans
- spinach
- sweetpotatoes
- tobacco
- waterapple
- watermelon

### Preprocesamiento del dataset

### Split del dataset

El dataset obtenido desde Kaggle ya se encontraba dividido en los conjuntos de entrenamiento, validación y prueba, por lo que no fue necesario realizar un proceso de separación adicional (split). La distribución con la que ya se contaba es de 70% para entrenamiento (21000 imágenes), 10% para validación (3000 imágenes) y 20% para prueba (6000 imágenes). Para cada categoría de planta, se asignaron 700 imágenes para entrenamiento, 100 imágenes para validación y 200 imágenes para prueba. 

Los splits más comunes en la literatura son 70:30 o 80:20, por lo que la distribución que ya venía desde Kaggle fue bastante adecuada para el proyecto. En todo caso, se puede considerar un reajuste de los splits en caso de que el modelo no esté obteniendo buenos resultados, pero por el momento se decidió mantener la distribución original.

### Redimensionamiento de las imágenes

Las imágenes se redimensionaron de dos maneras distintas para cada modelo. Para el modelo 0 se redimensionaron a un tamaño de 128x128 píxeles, mientras que para los modelos 1, 2, 3 y 4 se redimensionaron a un tamaño de 224x224 píxeles. Para el primer modelo se decidió reducir el tamaño de las imágenes a 128x128 píxeles debido a que el utilizado en el paper era de 500x500 píxeles, lo que no favorecía a nuestro dataset, mientras que para los modelos 1, 2, 3 y 4 se decidió mantener el tamaño de las imágenes a 224x224 píxeles debido a que el utilizado en el paper era de 224x224 píxeles, esto para mantener la mayor similitud posible con el paper, además de que en otros papers consultados el estándar para el tamaño de las imágenes era el mismo. 

### Data Augmentation
Se utilizó data augmentation para los modelos 2 y 4, con el objetivo de reducir el overfitting de los modelos. Las técnicas de data augmentation utilizadas fueron:

- **Rotation:** Rota aleatoriamente la imagen hasta +/- 10 grados.
- **Width shift:** Desplaza la imagen horizontalmente hasta un 20% de su ancho.
- **Zoom:** Aplica un zoom aleatorio de aproximadamente +/- 30%.
- **Horizontal flip:** Voltea la imagen horizontalmente con cierta probabilidad.

## Modelos

### Hiperparámetros
Para el modelo 0 use:
- Batch size: 128
- Learning rate: 0.0001
- Epochs: 100
- Sin optimizador 

Varios de estos hiperparámetros fueron elegidos de manera arbitraria, ya que el paper no especificaba los hiperparámetros utilizados para entrenar el modelo, sólo el número de épocas. 

Para los modelos 1, 2, 3 y 4 use:
- Batch size: 32
- Learning rate: 0.0002
- Epochs: 50
- Optimizadores: Adam para los modelos 1 y 2, SGD para los modelos 3 y 4

Todos estos hiperparámetros fueron elegidos de acuerdo a lo mencionado en el paper, por lo que se decidió mantener los mismos hiperparámetros para los modelos 1, 2, 3 y 4. Lo que cambié fue el número de épocas de 100 a 50, ya que en las pruebas a partir de 50 épocas el modelo ya no aprendía más. En el paper escogieron 100 épocas para evitar el overfitting, pero cómo se verá más adelante no fue el caso. También cambié para los modelos 3 y 4 el optimizador a SGD, estos se justifica en la sección de cada modelo.

### Modelo 0: CNN con 7 capas convolucionales en pirámide inversa (128-32)

[Modelo 0](./plantalyze_model_arq1_v1.ipynb)

El modelo 0 esta basado en el paper [Classification of color images of leathers tanned with different vegetable tannins by convolution neural network](https://doi.org/10.1016/j.measurement.2025.118600).

La arquitectura original de este paper se componía de 8 capas convolucionales e imagenes de entrada de 500x500 píxeles. Pero al notar que las imágenes del dataset no les favorecía el tamaño tan grande, por lo que decidí reducir el tamaño de las imágenes a 128x128 píxeles, lo que me llevó a reducir también el número de capas convolucionales a 7. 

#### Resultados

| Metric    | Train  | Val    | Test   |
|-----------|--------|--------|--------|
| Loss      | 1.0029 | 1.6846 | 1.9058 |
| Accuracy  | 66.51% | 59.73% | 56.52% |
| Precision | —      | —      | 0.62   |
| Recall    | —      | —      | 0.57   |
| F1        | —      | —      | 0.55   |

![Confusion Matrix](./images/matrix_arq1_v1.png)

#### Conclusiones

El modelo obtuvo una presición muy baja en el conjunto de prueba. Esto podría deberse a que la arquitectura del modelo no es la más adecuada para nuestro dataset, ya que el dataset utilizado en el paper es de solamente 144 imágenes, mientras que el dataset utilizado en este proyecto es de 30000 imágenes. Además, muchas cosas del paper no están justificadas por lo que deja muchas dudas sobre la efectividad de la arquitectura propuesta. Además de que se están clasificando plantas, mientras que en el paper se están clasificando diferentes tipos de cuero, por lo que la tarea es diferente. 

### Modelo 1: CNN con 4 capas convolucionales entrenada con Adam

[Modelo 1](./plantalyze_model_arq2_v1.ipynb)

El modelo 1 esta basado en el paper [CapPlant: a capsule network based
framework for plant disease classification](https://doi.org/10.7717/peerj-cs.752)

La arquitectura original de este paper se componía de 4 capas convolucionales e imagenes de 224x224 píxeles. Para este modelo decidí mantener los hiperparámetros mencionados en el paper. Lo único diferente es que no replica la parte de las 'capsule networks' ya que es una parte del modelo que no comprendí del todo, por lo que decidí omitirla y quedarme sólo con la parte de las capas convolucionales.

#### Resultados

| Metric    | Train  | Val    | Test   |
|-----------|--------|--------|--------|
| Loss      | 0.0788 | 1.8981 | 2.3673 |
| Accuracy  | 95.64% | 67.03% | 62.08% |
| Precision | —      | —      | 0.62   |
| Recall    | —      | —      | 0.62   |
| F1        | —      | —      | 0.62   |

![Confusion Matrix](./images/matrix_arq2_v1.png)

#### Conclusiones
Este modelo fue bastante mejor comparado con el otro ya que la arquitectura se adaptaba mejor a nuestro dataset, ya que no sólo el dataset era parecido en tamaño al del paper, sino también el tema era similar, ya que ambos modelos estaban enfocados en la clasificación de plantas. Sin embargo se pudo notar un claro caso de overfitting, ya que la precisión en el conjunto de entrenamiento es muy alta, mientras que la precisión en el conjunto de validación y prueba es mucho más baja. Por lo que sería necesario implementar técnicas para evitar el overfitting.

### Modelo 2: CNN con 4 capas convolucionales entrenada con Adam, data augmentation y early stopping para evitar overfitting

El modelo 2 sigue basado en el paper [CapPlant: a capsule network based
framework for plant disease classification](https://doi.org/10.7717/peerj-cs.752) estás técnicas fueron obtenidas de los siguientes artículos:
- [¿Cómo combatir el overfitting en el Machine Learning?](https://codificandobits.com/tutorial/como-combatir-el-overfitting/)
- [8 técnicas sencillas para prevenir el sobreajuste](https://medium.com/data-science/8-simple-techniques-to-prevent-overfitting-4d443da2ef7d)

Se sigue manteniendo la misma arquitectura del modelo 1, pero se implementan técnicas para evitar el overfitting, como lo son el data augmentation y el early stopping. El data augmentation se implementa con las técnicas mencionadas en la sección de data augmentation, mientras que el early stopping se implementa deteniendo el entrenamiento si la precisión en el conjunto de validación no mejora después de 5 épocas. En el modelo se puede notar como se detuvo en la época 23.

[Modelo 2](./plantalyze_model_arq2_v2.ipynb)

| Metric    | Train  | Val    | Test   |
|-----------|--------|--------|--------|
| Loss      | 1.0385 | 1.3012 | 1.3390 |
| Accuracy  | 67.94% | 66.53% | 63.33% |
| Precision | —      | —      | 0.64   |
| Recall    | —      | —      | 0.63   |
| F1        | —      | —      | 0.63   |

![Confusion Matrix](./images/matrix_arq2_v2.png)

#### Conclusiones
Este modelo obtuvo una mejora significativa en comparación con el modelo 1, ya que la precisión en el conjunto de prueba aumentó y la diferencia de "accuracy" entre el conjunto de entrenamiento y el conjunto de prueba es menor, lo que indica que las técnicas implementadas para evitar el overfitting fueron efectivas. Sin embargo, existe aún una brecha entre la precisión del conjunto de entrenamiento y el conjunto de validación y prueba, por lo que es el área de oportunidad para seguir mejorando el modelo.

### Modelo 3: CNN con 4 capas convolucionales entrenada con SGD y early stopping

El modelo 3 sigue basado en el paper [CapPlant: a capsule network based
framework for plant disease classification](https://doi.org/10.7717/peerj-cs.752)
Pero la parte del optmizer SGD se basa en los papers [DEEP INTERPRETABLE ARCHITECTURE FOR PLANT DISEASES CLASSIFICATION](https://arxiv.org/abs/1905.13523) y [Dual-Stream Architecture Enhanced by Soft-Attention Mechanism for Plant Species Classification](https://doi.org/10.3390/plants13182655)

Se sigue manteniendo la misma arquitectura del modelo 2, pero se cambia el optimizador a SGD. Esto debido a que otros papers que revisé para la clasificación de plantas utilizaban el optimizador SGD, por lo que, cómo hipótesis, decidí probarlo para ver si obtenía mejores resultados.

[Modelo 3](./plantalyze_model_arq2_v3.ipynb)

| Metric    | Train  | Val    | Test   |
|-----------|--------|--------|--------|
| Loss      | 0.1299 | 1.4506 | 1.7037 |
| Accuracy  | 95.80% | 68.93% | 64.23% |
| Precision | —      | —      | 0.64   |
| Recall    | —      | —      | 0.64   |
| F1        | —      | —      | 0.64   |

![Confusion Matrix](./images/matrix_arq2_v3.png)

#### Conclusiones
Este modelo comparado con el modelo 1 obtuvo una mejora significativa, ya que la precisión en el conjunto de validación y prueba aumentó considerablemente, lo que indica que el cambio del optimizador a SGD fue efectivo. Sin embargo, al igual que dicho modelo se puede notar un claro caso de overfitting, a pesar de que se implementó el early stopping, por lo que sería necesario implementar data augmentation nuevamente para evitar el overfitting.

### Modelo 4: CNN con 4 capas convolucionales entrenada con SGD, data augmentation y early stopping para evitar overfitting

El modelo 4 sigue basado en el paper [CapPlant: a capsule network based
framework for plant disease classification](https://doi.org/10.7717/peerj-cs.752) y los papers de optimizadores mencionados en el modelo 3 más las técnicas para evitar el overfitting mencionadas en el modelo 2. 

Para este último modelo se decidió mantener la misma arquitectura que el modelo 3, pero se implementó data augmentation para evitar el overfitting, ya que cómo se vió en el modelo 2 el data augmentation fue bastante efectivo para reducir el overfitting, por lo que se decidió implementarlo nuevamente para este modelo.

[Modelo 4](./plantalyze_model_arq2_v4.ipynb)

| Metric    | Train  | Val    | Test   |
|-----------|--------|--------|--------|
| Loss      | 1.0096 | 1.2268 | 1.2380 |
| Accuracy  | 68.64% | 66.40% | 65.85% |
| Precision | —      | —      | 0.67   |
| Recall    | —      | —      | 0.66   |
| F1        | —      | —      | 0.65   |

![Confusion Matrix](./images/matrix_arq2_v4.png)

#### Conclusiones

Sin duda este modelo fue el mejor de todos, ya que la precisión en el conjunto de validación y prueba aumentó considerablemente, lo que indica que las técnicas implementadas para evitar el overfitting fueron efectivas. Además, la brecha entre la precisión del conjunto de entrenamiento y el conjunto de validación y prueba es mucho menor en comparación con los otros modelos. Sin embargo, aún existe una brecha entre la precisión del conjunto de entrenamiento y el conjunto de validación y prueba, por lo que es el área de oportunidad para seguir mejorando el modelo.

## Pasos a seguir

Tomando en cuenta los resultados obtenidos, lo siguiente sería mejorar el modelo 4, ya que es el mejor modelo de todos, por lo que se podría intentar mejorarlo implementando técnicas como el transfer learning, o implementando la "capsule network" que se menciona en el paper.

## Disclaimer
Para el formato de este README y la presentación de los datos en los distintos notebooks se tomó como referencia el formato utilizado por el ingeniero Daniel Cajas en su proyecto [Animal Classifier](https://github.com/DanielSebasCM/ml_benji/) y mi compañera Mónica Martínez en su proyecto [Modelos Lineales y No Lineales para la Predicción de la Esperanza de Vida: Implementación Manual y Comparativa con Random Forest](https://github.com/MonicaMMartinezV/Mod2.ImplementacionTecnicaDeAprendizajeMaquina).

## Autor

**Daniel Emilio Fuentes Portaluppi**

*Tec de Monterrey*

*[a01708302@tec.mx](mailto:a01708302@tec.mx)*

## Referencias
Marquis03. (2023). Plants Classification. Kaggle. https://www.kaggle.com/datasets/marquis03/plants-classification

Omur, S., Efendioglu, N. O., & Sinecen, M. (2025). Classification of color images of leathers tanned with different vegetable tannins by convolution neural network. Measurement, 257, 118600. https://doi.org/10.1016/j.measurement.2025.118600

Samin, O. B., Omar, M., & Mansoor, M. (2021). CapPlant: a capsule network based framework for plant disease classification. PeerJ Computer Science, 7, e752. https://doi.org/10.7717/peerj-cs.752

M. Brahimi, S. Mahmoudi, K. Boukhalfa, and A. Moussaoui, "Deep interpretable architecture for plant diseases classification," arXiv preprint arXiv:1905.13523, 2019. [Online]. Available: https://arxiv.org/abs/1905.13523

Khan, I. U., Khan, H. A., & Lee, J. W. (2024). Dual-Stream architecture enhanced by Soft-Attention mechanism for plant species classification. Plants, 13(18), 2655. https://doi.org/10.3390/plants13182655

Chuan-En Lin, D. (2020, June). 8 técnicas sencillas para prevenir el sobreajuste. Medium. Retrieved May 31, 2026, from https://medium.com/data-science/8-simple-techniques-to-prevent-overfitting-4d443da2ef7d

¿Cómo combatir el overfitting en el Machine Learning? | Codificando Bits. (2025, September 11). Codificando Bits. https://codificandobits.com/tutorial/como-combatir-el-overfitting/

Lee, A. (2024, May 30). Adam vs SGD : What are the optimizers in neural network and when do we use? Medium. Retrieved May 31, 2026, from https://medium.com/@pumadd1227/adam-vs-sgd-what-are-the-optimizers-in-neural-network-and-when-do-we-use-238478a0eaea

DanielSebasCM. (n.d.). GitHub - DanielSebasCM/ml_benji. GitHub. https://github.com/DanielSebasCM/ml_benji/

MonicaMMartinezV - Overview. (n.d.). GitHub. https://github.com/MonicaMMartinezV