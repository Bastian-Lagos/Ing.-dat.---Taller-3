# Predictor del exito de juegos

Este proyecto busca predecir si un videojuego será exitoso, usando datos de la API de RAWG.

## 📊 ¿Qué es el éxito en este proyecto?
Un juego exitoso se define como un juego con puntuaciones buenas (sobre 4 puntos) en RAWG, sobre 75 puntos en metacritic y ademas de poseer mas usuarios interesados que reseñas del videojuego. Esto porque el juego no puede considerarse exitoso si solamente interesa a criticos y no jugadores.

## ¿Que predice este proyecto?
Este proyecto predice si un juego será exitoso dependiendo de la categoría de este, basandose en como se han recepcionado los juegos de esta misma categoría en el pasado cercano, por ejemplo:
mi empresa quiere sacar un juego de tipo shooter, para saber si un shooter tendría buena recepción en el mercado actual, se revisa la base de datos de la API de RAWG y en base a los datos de critica especializada en juegos, rating hecho por jugadores y numero usuarios interesados se define si es que un shooter sería un juego con una buena recepción. Esto sin ni siquiera comenzar el desarrollo de este juego.

##  ¿Por qué la problematica es un problema de clasificación?
El problema de predecir el éxito de un videojuego puede abordarse como uno de clasificación porque el objetivo es asignar a cada juego una etiqueta de "exitoso" o "no exitoso" , según ciertos criterios predefinidos. Esta es la esencia de un problema de clasificación.

## El conjunto de datos seleccionado para saber si un juego fue exitoso son:

1.-nombre : Aunque no se usa directamente como predictor, es importante para identificar los juegos, hacer análisis exploratorio y vincular datos entre fuentes. Se deja como identificador, no se usa para entrenar el modelo.
2.-fecha de publicación: La fecha afecta el contexto del juego: tecnología disponible, tendencias del mercado y competencia. 
3.-rating : Refleja la opinión agregada de los jugadores. Es una de las señales más directas de éxito percibido.
4.-cantidad ratings: Indica cuántas personas calificaron el juego, lo cual es una medida directa de popularidad.
5.-géneros: Algunos géneros son más populares o exitosos comercialmente que otros.
6.-tags: Detallan características únicas del juego (por ejemplo, "roguelike", "co-op", "VR").
7.-metacritic: Opinión crítica profesional. Complementa la percepción de usuarios.
8.-plataformas: Juegos multiplataforma tienen mayor alcance. Algunas plataformas tienen más jugadores o exclusividades exitosas.
9.-usuarios interesados: Muestra intención futura de jugarlo, lo cual es una métrica clave de interés.

Estos datos son importantes porque permiten capturar múltiples dimensiones del éxito de un videojuego: desde la percepción crítica hasta la opinion del público. A través de estas variables se puede construir un perfil más completo del juego, pero centrandose siempre en una recopilación de datos que objetivamente podemos tomar como índice de éxito de un juego, para ser objetivos, la mayoria de los datos deben ser númericos y ser opiniones de gente que haya jugado el juego (eso incluye a los criticos como metacritic).

## Preparación del conjunto de datos y Selección de atributos:

Durante la preparación del conjunto de datos, se realizó una depuración cuidadosa de las variables disponibles. Muchos de los campos presentes en el dataset original fueron excluidos deliberadamente por no aportar valor significativo al objetivo del análisis o por presentar redundancias, ambigüedad o ruido. A continuación listaremos algunos de ellos:

id, slug, name_original, metacritic_url, reddit_url, website: Son útiles como identificadores únicos o enlaces informativos, pero no contienen información predictiva que el modelo pueda usar directamente.

background_image, background_image_additional, reddit_logo: Imágenes y recursos visuales son datos no estructurados que requerirían procesamiento con modelos de visión por computador, lo cual está fuera del alcance de este análisis centrado en datos estructurados.

screenshots_count, movies_count: Aunque cuantifican contenido multimedia, no hay evidencia directa de que influyan de forma significativa en el éxito del juego.

Procesamiento de datos faltantes:

Para atributos como metacritic o rating, donde algunos juegos carecían de información, se insertaron los valores 0.

## Creación de etiquetas:

Para definir si un juego fue exitoso, se utilizó una combinación de métricas:

Se consideró exitoso un juego que superaba los umbrales de 4 puntos en rating, sobre 75 puntos en metacritic y si este posee mas usuarios interesados (usuarios_interesados) que reseñas del videojuego (ratings_count)

la etiqueta generada para saber si un juego es o no exitoso se llama success, y es de tipo booleana, con valor true si es que la categoría de juego ha exitosa y false si es que la categoría de juego no ha sido exitosa 


## 🧪 Cómo ejecutar

1. Crear un archivo `.env` con tu API key de RAWG: 
RAWG_API_KEY=tu_clave
Este paso no es necesario si es que ya existe el archivo csv con los datos de los juegos en el programa.

2. Ejecutar extracción de datos por consola o por la primera casilla de codigo del notebook:
python app.py
Este paso no es necesario si es que ya existe el archivo csv con los datos de los juegos en el programa.

3. Instalar dependencias:
pip install -r requirements.txt


## Reflexión final: Limitaciones y posibles mejoras

### Limitaciones del experimento

Tamaño del dataset: El conjunto de datos utilizado es relativamente pequeño y puede no ser representativo de la diversidad total de videojuegos existentes. Esto limita la capacidad del modelo para generalizar y puede llevar a resultados poco robustos.

Como tiene menos juegos hay menos probabilidad de que se genere ruido, por esto en este caso solo se tomaron los casos de juegos mas reconocidos.

Clases desbalanceadas: Es posible que la cantidad de juegos exitosos y no exitosos esté desbalanceada, lo que puede afectar el rendimiento del modelo y su capacidad para predecir correctamente ambas clases.

Datos faltantes y simplificaciones: Se rellenaron valores faltantes con ceros, lo que puede introducir sesgos. Además, la definición de éxito es rígida y puede no reflejar todos los matices del mercado real.

### Posibles mejoras y estrategias futuras

Aumentar el tamaño y diversidad del dataset: Obtener más datos de diferentes fuentes o ampliar la cantidad de juegos analizados ayudaría a mejorar la capacidad predictiva del modelo.

Ingeniería de atributos: Explorar nuevas variables, como análisis tendencias de ventas, o interacción en redes sociales.

Modelos más avanzados: Probar otros algoritmos de clasificación o redes neuronales, y comparar su desempeño.

### Principales desafíos técnicos y conceptuales

Definición de éxito: Traducir el concepto de "éxito" a una métrica objetiva y medible fue un reto, ya que el éxito puede depender de muchos factores subjetivos y externos al dataset.

Limitaciones de la API y los datos: La información disponible depende de la API de RAWG, que puede tener datos incompletos o inconsistentes para algunos juegos.

Selección de atributos relevantes: Decidir qué variables incluir y cómo procesarlas requirió un análisis cuidadoso para evitar ruido y redundancia.

En resumen, aunque el modelo desarrollado ofrece una aproximación inicial para predecir el éxito de videojuegos, existen múltiples oportunidades de mejora tanto en la calidad de los datos como en los métodos utilizados.