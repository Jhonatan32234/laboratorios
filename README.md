# Minería de Datos — UPCh 2026A

Este repositorio contiene los laboratorios de la Unidad 2: "Del Texto al Significado". Se enfoca en el procesamiento de lenguaje natural (NLP) y la implementación de motores de búsqueda básicos (TF-IDF).

## Guía de Configuración del Entorno

Este proyecto utiliza [uv](https://astral.sh/uv) para una gestión de dependencias rápida y eficiente.

### 1. Instalación de `uv`
Si no tienes `uv` instalado, puedes hacerlo con el siguiente comando:
* **Windows:** `powershell -c "irm https://astral.sh/uv/install.ps1 | iex"`
* **macOS/Linux:** `curl -LsSf https://astral.sh/uv/install.sh | sh`

### 2. Preparar el Proyecto
Clona el repositorio y navega hasta la carpeta del proyecto. Luego, sincroniza el entorno:

```bash
uv sync
```
Este comando creará automáticamente un entorno virtual en `.venv` e instalará las dependencias listadas en el archivo `uv.lock` (Pandas, NLTK, spaCy, scikit-learn, etc.).

### 3. Descarga de Recursos (NLTK y spaCy)
Los notebooks están diseñados para ser autogestiónables. Al ejecutarlos, la primera celda se encargará de verificar y descargar los modelos necesarios:

*   **NLTK:** Recursos como `punkt_tab` y `stopwords`.
*   **spaCy:** Modelo de lenguaje español `es_core_news_sm`.

Si deseas hacerlo manualmente desde Python:
```python
import nltk
import spacy
nltk.download(['punkt', 'punkt_tab', 'stopwords'])
spacy.cli.download('es_core_news_sm')
```

### 4. Ejecución de Notebooks
Para levantar el servidor de Jupyter utilizando el entorno gestionado por `uv`:

```bash
uv run jupyter notebook
```

## Pipeline de Preprocesamiento
El motor de búsqueda utiliza un pipeline estandarizado que incluye:
1.  **Normalización:** Conversión a minúsculas y eliminación de acentos/diacríticos.
2.  **Limpieza:** Eliminación de etiquetas HTML, URLs y caracteres no alfabéticos.
3.  **Tokenización y Lemmatización:** Uso de spaCy para extraer lemas y filtrar stopwords críticas (preservando `no`, `ni`, `sin`).