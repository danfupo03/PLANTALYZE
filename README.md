# **PLANTALYZE - Clasificación de especies de plantas**

Este proyecto para la materia de TC3002B busca implementar un modelo de machine learning que pueda clasificar diferentes especies de plantas, verduras y frutas a partir de imágenes.

## Descripción general

El dataset es para clasificación. Cuenta con una distribución de 70/20/10 para entrenamiento, prueba y validación respectivamente. El dataset lo obtuve de Kaggle, especificamente de [Plants Classification](https://www.kaggle.com/datasets/marquis03/plants-classification). Este dataset contiene imágenes de diferentes especies de plantas, etiquetadas con su respectiva clase.

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

El dataset ya estaba separado por lo que no fue necesario realizar un proceso de separación adicional (split), lo que si se realizó fue un proceso de preprocesamiento de las imágenes. Esto para que todas tuvieran un tamaño de 128x128 ya que es crucial para el entrenamiento del modelo. También se dejó listo un proceso de data augmentation para aumentar la cantidad de datos disponibles para el entrenamiento, sólo se creo el proceso y se guardo en un directorio aparte. En caso de querer usarlo, para el entrenamiento habría que unificarlo con el dataset de entrenamiento original.

## Autor

**Daniel Emilio Fuentes Portaluppi**

*Tec de Monterrey*

*[a01708302@tec.mx](mailto:a01708302@tec.mx)*

## Referencias
Marquis03. (2023). Plants Classification. Kaggle. https://www.kaggle.com/datasets/marquis03/plants-classification