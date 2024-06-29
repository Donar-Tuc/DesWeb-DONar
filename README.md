6. Creación de un Pipeline de Integración Continua (CI)
   
Vamos a utilizar GitHub Actions para crear un pipeline de integración continua paso a paso. Este pipeline se encargará de compilar, probar y desplegar la aplicación automáticamente cada vez que se hagan cambios en las ramas develop o main.

Paso 1: Crear el Archivo de Configuración

Crear un archivo de flujo de trabajo:
Ubica tu repositorio en GitHub.
Navega a la pestaña Actions.
Haz clic en New workflow o Set up a workflow yourself.
Guarda el archivo como .github/workflows/ci.yml.

Paso 2: Configurar el Archivo ci.yml

Aquí tienes una configuración básica para un pipeline de CI:
yaml

Copiar código
name: CI Pipeline

on:
  push:
    branches:
      - develop
      - main
  pull_request:
    branches:
      - develop

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Check out repository
      uses: actions/checkout@v2

    - name: Set up Node.js
      uses: actions/setup-node@v2
      with:
        node-version: '14'

    - name: Install dependencies
      run: npm install

    - name: Run tests
      run: npm test

    - name: Build project
      run: npm run build

    - name: Deploy to staging environment
      if: github.ref == 'refs/heads/develop'
      run: echo "Deploy to staging server"

    - name: Deploy to production environment
      if: github.ref == 'refs/heads/main'
      run: echo "Deploy to production server"

Explicación de Configuraciones

Eventos que disparan el pipeline:
El pipeline se ejecuta cada vez que se hace push a las ramas develop y main.
Se ejecuta también en cada pull request hacia develop.

Jobs y Steps:
runs-on: ubuntu-latest: Define el sistema operativo en el que se ejecutará el pipeline.
steps: Define los pasos que se ejecutarán dentro del job build.

Pasos del Pipeline:
Check out repository: Utiliza la acción actions/checkout@v2 para clonar el repositorio.
yaml

Copiar código
- name: Check out repository
  uses: actions/checkout@v2


Set up Node.js: Utiliza la acción actions/setup-node@v2 para configurar Node.js.
yaml
Copiar código
- name: Set up Node.js
  uses: actions/setup-node@v2
  with:
    node-version: '14'


Install dependencies: Instala las dependencias del proyecto utilizando npm install.
yaml
Copiar código
- name: Install dependencies
  run: npm install


Run tests: Ejecuta las pruebas del proyecto con npm test.
yaml
Copiar código
- name: Run tests
  run: npm test


Build project: Construye el proyecto utilizando npm run build.
yaml
Copiar código
- name: Build project
  run: npm run build


Deploy to staging environment: Despliega en el entorno de staging si la rama es develop.
yaml
Copiar código
- name: Deploy to staging environment
  if: github.ref == 'refs/heads/develop'
  run: echo "Deploy to staging server"


Deploy to production environment: Despliega en el entorno de producción si la rama es main.
yaml
Copiar código
- name: Deploy to production environment
  if: github.ref == 'refs/heads/main'
  run: echo "Deploy to production server"


Paso 3: Comprobar el Pipeline

Realizar un Push o Crear un Pull Request:
Haz un push a la rama develop o main para activar el pipeline.
O bien, crea un pull request hacia la rama develop.

Verificar la Ejecución del Pipeline:

Navega a la pestaña Actions en tu repositorio de GitHub.
Selecciona el flujo de trabajo que has creado para ver el estado y los resultados de cada paso.

Paso 4: Mejorar y Personalizar el Pipeline

Agregar más pasos o condiciones:
Puedes agregar más pasos, como la implementación en servidores específicos o la ejecución de scripts adicionales.

Integrar con otras herramientas:
Puedes integrar GitHub Actions con otras herramientas como Slack para recibir notificaciones, o con servicios de despliegue como AWS, Heroku, etc.

Configurar variables de entorno y secretos:
Utiliza los secretos de GitHub (Settings > Secrets) para almacenar de manera segura credenciales y otros datos sensibles.

