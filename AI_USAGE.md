# Declaración de Uso de Inteligencia Artificial — Lab 1
**Asignatura:** Minería de Datos  
**Unidad:** Unidad 2 — Del Texto al Significado (UPCh 2026A)  
**Estudiante:** Jhonatan de Jesus Zuñiga Castro / Jhonata32234  

---

## 1. Herramienta Utilizada
* **Herramienta:** Gemini (Modelo de Lenguaje Grande).
* **Instancia/Interfaz:** Chat interactivo de asistencia al programador.

## 2. Declaración de Celdas Asistidas y Cambios Realizados

### Sección 1 · Cargar y explorar
* **Celda asistida:** `1.b` (Detección de ruido mediante Expresiones Regulares y `unicodedata`).
* **Qué proporcionó la IA:** Un patrón para `RE_HTML` (`r'<[^>]+>'`) y una propuesta inicial usando `unicodedata.category(c)` filtrando por caracteres que comiencen con `'S'` para capturar emojis de forma masiva.
* **Qué se cambió/ajustó manualmente:** Se validó la salida en el notebook y se analizó un comportamiento detectado en el documento `d03`: la función arrastró caracteres relacionales `<` y `>` (clasificados en Unicode como símbolos matemáticos `Sm`). Se documentó en la defensa que el pipeline no se ve afectado porque la regex de HTML elimina las etiquetas completas antes en el orden de ejecución.

### Sección 2 · Tokenizar y normalizar
* **Celda asistida:** `2.b` (Función `normalizar`).
* **Qué proporcionó la IA:** El orden lógico para desestructurar y limpiar el texto en español: conversión a minúsculas -> sustitución por expresiones regulares -> separación de diacríticos usando la codificación `NFD` para filtrar caracteres de combinación (`Mn`) -> reconstrucción final en formato balanceado `NFC` y colapso de espacios mediante `re.sub(r'\s+', ' ', texto)`.
* **Qué se cambió/ajustó manualmente:** Se adaptó la firma de la función para aceptar el parámetro booleano `quitar_acentos=False` por defecto y poder contrastar empíricamente su impacto en el recall del buscador.

### Sección 4 · Stemming vs. lemmatización
* **Celda asistida:** `4.a` (Comparación de vocabularios y extracción de discrepancias).
* **Qué proporcionó la IA:** Un script optimizado para recorrer dinámicamente el corpus tokenizado, calcular el Stem de NLTK frente al Lemma de spaCy de manera simultánea, y aislar exactamente 10 ejemplos únicos donde ambos métodos difieren para imprimirlos en formato de tabla limpia (`Palabra Original | Stemming | Lemmatización`).
* **Qué se cambió/ajustó manualmente:** Se integró el filtro estricto de control de stopwords personalizadas (`MIS_STOPWORDS`) y se corrigió el bug de los espacios en blanco múltiples agregando la validación `not token.is_space` descubierta en la fase de exploración del paso `2.a`.

## 3. Justificación Lingüística y Decisiones de Ingeniería Autónomas

Aunque la IA facilitó la implementación de plantillas de código y sintaxis en Python, las decisiones estratégicas de negocio para el motor de búsqueda del **Lab 2** fueron evaluadas y defendidas críticamente por el equipo:
1. **Eliminación de acentos:** Se optó por eliminarlos de manera radical en producción para priorizar la flexibilidad en las consultas del usuario final (*high recall*).
2. **Retención de Stopwords Críticas:** Se forzó la exclusión de `{'no', 'ni', 'sin'}` de la lista de descarte nativa de spaCy para proteger la lógica inversa y la integridad semántica de las noticias de desastres o incidencias.
3. **Preferencia por Lemmatización:** Respaldados por la métrica final extraída ($199$ lemas vs $192$ stems), se seleccionó la lemmatización para asegurar la persistencia de componentes gramaticales válidos en el índice invertido.

## 4. Laboratorio 2 — Motor de Búsqueda (TF-IDF)

* **Celdas asistidas:** `1`, `2` y `3` (Lógica de vectores dispersos mediante diccionarios).
* **Qué proporcionó la IA:** Plantillas lógicas para implementar el producto punto y las magnitudes vectoriales sobre diccionarios nativos de Python, omitiendo el uso de arreglos densos o matrices de NumPy.
* **Qué se cambió/ajustó manualmente:** Se añadió una validación crítica dentro de la función `coseno` (`if norma_v1 == 0.0 or norma_v2 == 0.0: return 0.0`) para prevenir excepciones de división por cero en el caso de consultas vacías o compuestas exclusivamente por términos fuera del vocabulario. Asimismo, se diseñaron y documentaron de forma manual las consultas de prueba léxicas y polisémicas para forzar el fallo estructural del algoritmo y justificar la futura transición hacia BM25.

## 5. Laboratorio 3 — BM25 y Evaluación de Búsqueda

* **Celdas asistidas:** `1`, `5` y `6` (Estructura matemática de BM25, métricas desde cero y andamiaje de promedios).
* **Qué proporcionó la IA:** Plantillas lógicas puras en Python para el cálculo acumulado de la sumatoria de BM25 y las fórmulas de descuento logarítmico para armar el `nDCG@5` e `IDCG@5` de manera nativa sin recurrir a librerías externas.
* **Qué se cambió/ajustó manualmente:** Se estructuró a mano el mapeo y etiquetado fino del diccionario `qrels` para las 5 consultas del dominio de noticias de Chiapas, validando la coherencia semántica. Asimismo, se adaptó el bucle de evaluación genérica para asegurar que la función parseara exclusivamente listas de identificadores de strings (`['d01', 'd02', ...]`), protegiendo el pipeline contra discrepancias de tipos de datos.

## 6. Laboratorio 4 — Descubrimiento de Temas con K-Means

* **Celdas asistidas:** `2` y `3` (Adaptación de la lógica matricial de K-Means nativo y ordenamiento `argsort` de centroides).
* **Qué proporcionó la IA:** Estructuras lógicas de vectorización con NumPy para segmentar las distancias del array `X[:, np.newaxis] - centroides` utilizando broadcasting eficiente, y el bucle para extraer los términos top de los centroides.
* **Qué se cambió/ajustó manualmente:** Se definieron y asignaron de forma artesanal las etiquetas de nombres temáticos en el diccionario `nombres_tematicos` basándose en la lectura directa de las noticias de Chiapas, y se construyó el código de rastreo específico de cruce de IDs para comprobar el comportamiento de los nodos de control `d02` y `d13`.