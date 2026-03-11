# FastAPI CI/CD Pipeline

Este repositorio contiene una aplicación web sencilla desarrollada con FastAPI y un pipeline de Integración y Despliegue Continuo (CI/CD) configurado usando GitHub Actions.

## Estructura del Proyecto

- `main.py`: Contiene el código principal de la aplicación FastAPI. Define dos endpoints básicos (`/` y `/items/{item_id}`).
- `test_main.py`: Contiene las pruebas unitarias para la aplicación utilizando `pytest` y `TestClient` de FastAPI.
- `requirements.txt`: Lista de dependencias del proyecto (FastAPI, uvicorn, pytest, flake8, etc.).
- `.github/workflows/cicd.yml`: Archivo de configuración de GitHub Actions que define el flujo del CI/CD.

## Flujo de CI/CD (GitHub Actions)

El flujo está diseñado para asegurar la calidad del código mediante pruebas automatizadas y preparar el camino para un despliegue seguro a producción.

### Cuándo se Ejecuta (Triggers)

El pipeline se activa en los siguientes eventos:
1. **Push a la rama `desarrollo`**: Ideal para integración continua mientras se desarrolla en el entorno de pruebas.
2. **Pull Request cerrado en la rama `main`**: El pipeline de despliegue se ejecutará **sólo si** el Pull Request fue aprobado y mergeado a la rama principal.

### Fases del Pipeline (Jobs)

El pipeline consta de dos trabajos principales (Jobs):

#### 1. Continuous Integration (`ci-build-and-test`)

Este trabajo se encarga de validar el código cada vez que se sube un cambio a `desarrollo` o se abre/actualiza un PR.

**Pasos:**
- **Checkout code:** Descarga el código del repositorio.
- **Set up Python:** Prepara el entorno utilizando Python 3.11.
- **Install dependencies:** Actualiza `pip` e instala todas las dependencias listadas en `requirements.txt`.
- **Lint with flake8:** Analiza el código buscando errores de sintaxis, variables no definidas y problemas de formato de acuerdo a la convención de Python.
- **Test with pytest:** Ejecuta todas las pruebas unitarias definidas en `test_main.py` para asegurar que los endpoints funcionan correctamente.

#### 2. Continuous Deployment (`cd-deploy`)

Este trabajo se encarga del despliegue y **solo se activa si el Pull Request hacia `main` fue mergeado exitosamente**.

**Pasos:**
- **Checkout code:** Vuelve a descargar el código validado.
- **Simulate Deployment:** Actualmente, este paso es una simulación que imprime mensajes de éxito. En un escenario real, aquí se configurarían acciones para:
  - Autenticarse en el proveedor de nube (AWS, GCP, Azure, etc.).
  - Construir y subir una imagen Docker.
  - Desplegar la aplicación en servicios como Vercel, Heroku, Render, o un clúster de Kubernetes.

## Cómo probar localmente

Para ejecutar el proyecto y las pruebas de manera local, sigue estos pasos:

1. **Crear y activar un entorno virtual (opcional pero recomendado):**
   ```bash
   python -m venv venv
   source venv/bin/activate  # En Linux/macOS
   # venv\Scripts\activate   # En Windows
   ```

2. **Instalar dependencias:**
   ```bash
   pip install -r requirements.txt
   ```

3. **Ejecutar pruebas:**
   ```bash
   pytest test_main.py
   ```

4. **Ejecutar la aplicación:**
   ```bash
   uvicorn main:app --reload
   ```
   La aplicación estará disponible en `http://127.0.0.1:8000`.
