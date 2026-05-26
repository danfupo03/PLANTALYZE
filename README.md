# **PLANTALYZE - Clasificación de especies de plantas**

## Descripción general
Plantalyze es un clasificador de especies de plantas que utiliza técnicas de aprendizaje para clasificar diferentes especies de plantas a partir de imágenes.

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

El dataset obtenido desde Kaggle ya se encontraba dividido en los conjuntos de entrenamiento, validación y prueba, por lo que no fue necesario realizar un proceso de separación adicional (split). La distribución con la que ya se contaba es de 70% para entrenamiento (21000 imágenes), 10% para validación (3000 imágenes) y 20% para prueba (6000 imágenes). Para cada categoría de planta, se asignaron 700 imágenes para entrenamiento, 100 imágenes para validación y 200 imágenes para prueba. 

Los splits más comunes en la literatura son 70:30 o 80:20, por lo que la distribución que ya venía desde Kaggle fue bastante adecuada para el proyecto. En todo caso, se puede considerar un reajuste de los splits en caso de que el modelo no esté obteniendo buenos resultados, pero por el momento se decidió mantener la distribución original.

Lo que si se realizó fue un proceso de preprocesamiento de las imágenes, el cual consistió en redimensionar cada imagen a un tamaño de 128x128 píxeles. Esto debido a que la uniformidad en el tamaño de las imágenes es importante para el entrenamiento del modelo, ya que permite que la red neuronal pueda procesar las imágenes de manera mas eficiente y obtener mejores resultados.

**Data Augmentation**
En este primer avance se dejó listo un proceso de data augmentation para aumentar la cantidad de imágenes disponibles para el entrenamiento, sólo se creo el proceso y se guardo en un directorio aparte. En caso de querer utilizarlo, para el entrenamiento, habría que unificarlo con el dataset de entrenamiento original.

## Autor

**Daniel Emilio Fuentes Portaluppi**

*Tec de Monterrey*

*[a01708302@tec.mx](mailto:a01708302@tec.mx)*

## Referencias
Marquis03. (2023). Plants Classification. Kaggle. https://www.kaggle.com/datasets/marquis03/plants-classification