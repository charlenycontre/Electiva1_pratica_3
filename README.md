 Pipeline CI/CD con GitHub Actions y Despliegue Automático en GitHub Pages

Este proyecto incluye la configuración completa de un **pipeline de Integración Continua y Despliegue Continuo (CI/CD)** utilizando **GitHub Actions**.  
El objetivo principal es que cada vez que hagamos cambios en el código y los subamos a la rama `main`, se realicen automáticamente las pruebas necesarias y, si todo está correcto, el proyecto se publique sin que tengamos que hacerlo manualmente.

Esto nos ahorra bastante tiempo, reduce errores y nos permite tener siempre la última versión del proyecto en línea, lista para ser vista desde cualquier navegador.



 -¿Cómo funciona este pipeline?-

Este pipeline está dividido en **dos fases principales**: **Build** y **Deploy**.

. Build (Construcción y pruebas)
   - Se ejecuta cada vez que hacemos un `push` o un `pull request` hacia la rama `main`.
   - El workflow descarga el código del repositorio.
   - Configura el entorno de Node.js (versión 18 en este caso).
   - Instala todas las dependencias necesarias con `npm install`.
   - Ejecuta las pruebas con `npm test` para asegurarse de que el código no tenga errores.

. Deploy (Despliegue automático)
   * Esta fase solo se ejecuta si la fase de construcción ha terminado correctamente.
   * Configura GitHub Pages para que el contenido del proyecto se pueda mostrar como un sitio web.
   * Usa la acción `peaceiris/actions-gh-pages` para subir los archivos a la rama `gh-pages`.
   * Publica automáticamente el sitio, forzando una rama huérfana para no acumular historial innecesario.
   * Desactiva Jekyll para permitir que se muestren archivos con nombres que contengan guiones bajos.

 
-Detalles técnicos del archivo de configuración

```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
      - run: npm install
      - run: npm test

  deploy:
    needs: build
    runs-on: ubuntu-latest
    permissions:
      contents: write
      pages: write
      id-token: write
    steps:
      - uses: actions/checkout@v3
      - name: Setup Pages
        uses: actions/configure-pages@v3
      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: .
          publish_branch: gh-pages
          force_orphan: true
          enable_jekyll: false
