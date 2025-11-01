# Evaluación 2: MLOps - Pipelines de Clasificación y Regresión

Este proyecto implementa un pipeline de MLOps completo utilizando Kedro, DVC y Docker para entrenar y evaluar 10 modelos de Machine Learning (5 de clasificación y 5 de regresión) basados en un conjunto de datos de compras de clientes.

**Integrantes:**
Jorge Garrido

---

## 1. Problemas de Negocio

El pipeline resuelve dos problemas de ML:

1.  **Clasificación:** Predecir la **categoría** de un producto (`category`) basado en el perfil del cliente y la tienda.
2.  **Regresión:** Predecir el **monto total** de una transacción (`total_amount`) basado en el mismo perfil.

---

## 2. Arquitectura de la Solución (MLOps)

El proyecto está construido con un stack de MLOps moderno:

* **Kedro:** Para estructurar todo el proyecto en pipelines de datos e ingeniería de ML modulares, robustos y reproducibles.
* **DVC (Data Version Control):** Para versionar nuestros artefactos (datos crudos, modelos entrenados y métricas), manteniendo el repositorio de Git liviano.
* **Docker:** Para empaquetar toda la aplicación (código, librerías y configuración) en una imagen portable que garantiza una ejecución idéntica en cualquier máquina.

---

## 3. Estructura del Proyecto

* `data/01_raw/`: Datos crudos (rastreados por DVC).
* `data/06_models/`: 10 modelos `.pkl` entrenados (rastreados por DVC).
* `data/07_model_output/`: 10 archivos `.json` con las métricas de desempeño (rastreados por DVC).
* `src/evaluacion_2_mlops/nodes/`: Contiene la lógica en Python (nodos):
    * `preprocessing.py`: Funciones para unir CSVs y crear features.
    * `modeling.py`: Funciones para entrenar y evaluar los 10 modelos con `GridSearchCV` (cv=5).
* `src/evaluacion_2_mlops/pipelines/`: Contiene la definición de los pipelines que conectan los nodos.
* `conf/base/catalog.yml`: El "registro" que le dice a Kedro dónde encontrar y guardar todos los datos, modelos y métricas.
* `Dockerfile`: Las instrucciones para construir la imagen de Docker.

---

## 4. Instrucciones de Ejecución

Hay dos maneras de ejecutar este proyecto.

### Opción A: Ejecución con Docker (Recomendada)

Este método es el más simple y garantiza la reproducibilidad.

**Prerrequisitos:**
* Tener **Docker Desktop** instalado y corriendo.

**Pasos:**
1.  Clonar el repositorio.
2.  Construir la imagen de Docker (solo la primera vez):
    ```bash
    docker build -t evaluacion-mlops .
    ```
3.  Ejecutar el pipeline completo:
    ```bash
    docker run --rm evaluacion-mlops kedro run
    ```
    Esto ejecutará el pipeline `__default__`, que procesa los datos, entrena los 10 modelos y guarda todas las métricas.

### Opción B: Ejecución Local (Desarrollo)

**Prerrequisitos:**
* Tener `Python 3.9` instalado.
* Tener `git` y `dvc` instalados.

**Pasos:**
1.  Clonar el repositorio.
2.  Crear y activar un entorno virtual:
    ```bash
    python -m venv venv
    source venv/bin/activate  # o .\venv\Scripts\activate en Windows
    ```
3.  Instalar las dependencias:
    ```bash
    pip install -r src/requirements.txt
    ```
4.  Recuperar los datos (rastreados por DVC):
    ```bash
    # (En un proyecto real, se correría 'dvc pull'. 
    # Para esta entrega, los datos crudos ya están en la carpeta)
    ```
5.  Ejecutar el pipeline de Kedro:
    ```bash
    kedro run
    ```