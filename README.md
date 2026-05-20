# ghactions-lab 🧪🚀

Un repositorio práctico diseñado para experimentar, aprender y probar flujos de trabajo de **GitHub Actions** (CI/CD) aplicados a un proyecto Node.js.

## 📋 Características

* 🔄 **Flujos de Trabajo Reutilizables:** Implementación de pipelines modulares (`workflow_call`) que se pueden reutilizar en múltiples flujos.
* ⚡ **Ejecución en Paralelo (Multi-Versión):** Configuración de flujos que ejecutan pruebas y empaquetado en múltiples versiones de Node.js (18.x y 20.x) simultáneamente.
* 📦 **Gestión de Artefactos:** Carga y almacenamiento de reportes de pruebas y compilados directamente en GitHub.
* 🧪 **Pruebas Unitarias:** Configuración básica con [Jest](https://jestjs.io/) para verificación de código.

---

## 📁 Estructura del Proyecto

```text
ghactions-lab/
├── .github/
│   └── workflows/
│       ├── lab1-basic.yml               # Workflow de prueba inicial (básico)
│       ├── reusable-node-pipeline.yml   # Pipeline modular/reutilizable de Node.js
│       └── consumer-node-pipeline.yml   # Consumidor que ejecuta el pipeline modular
├── src/
│   └── math.js                          # Lógica simple de operaciones (add, subtract)
├── test/
│   └── math.test.js                     # Pruebas unitarias para math.js
├── package.json                         # Configuración y dependencias (Jest, build, etc.)
└── README.md                            # Documentación del proyecto
```

---

## 🛠️ Desarrollo Local

### Requisitos Previos

Asegúrate de tener instalado [Node.js](https://nodejs.org/) (versión 18 o superior recomendada) y `npm`.

### Instalación de Dependencias

```bash
npm install
```

### Ejecutar Pruebas

Para correr las pruebas unitarias locales utilizando Jest:

```bash
npm test
```

### Simular Compilación (Build)

Para simular la compilación del proyecto (crea la carpeta `dist/` y copia los archivos de `src/`):

```bash
npm run build
```

---

## 🚀 Workflows de GitHub Actions

El repositorio incluye los siguientes flujos en la carpeta `.github/workflows/`:

### 1. Lab 1 - Primer workflow básico (`lab1-basic.yml`)
* **Disparador:** Cualquier `push` al repositorio.
* **Acción:** Ejecuta tareas sumamente sencillas en un entorno Ubuntu, imprimiendo un mensaje de bienvenida y listando los archivos con `ls -la`.

### 2. Pipeline Reutilizable (`reusable-node-pipeline.yml`)
Este flujo está parametrizado para poder ser invocado por otros flujos de trabajo.
* **Entradas (Inputs):**
  * `node-version` *(requerido)*: Versión de Node.js a utilizar.
  * `artifact-name` *(opcional)*: Nombre con el que se guardarán los artefactos en GitHub (por defecto `app-artifact`).
* **Salidas (Outputs):**
  * `node-version-used`: Retorna la versión de Node.js que se usó efectivamente en la ejecución.
* **Pasos:** Descarga de código, configuración de Node con caché de dependencias, ejecución de pruebas, build del proyecto y subida de reportes y compilados (`dist/`) como artefactos.

### 3. Consumidor de Pipeline Node.js (`consumer-node-pipeline.yml`)
* **Disparador:** `push` o `pull_request` sobre la rama `main`.
* **Acción:**
  * Invoca al pipeline reusable de manera concurrente para **Node.js 18.x** y **Node.js 20.x**.
  * Al finalizar ambos procesos, un trabajo de post-compilación (`post-build-logic`) imprime los outputs obtenidos indicando que todas las construcciones finalizaron con éxito.
