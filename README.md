# CI/CD con GitHub Actions para Despliegue en GitHub Pages

Este repositorio implementa un **pipeline de CI/CD** usando GitHub Actions para:
1. Ejecutar pruebas automáticas con **Node.js**.
2. Desplegar automáticamente el proyecto en **GitHub Pages** cuando se realiza un push a la rama `main`.

---

## 🚀 Documentación del Pipeline

### 1. Desencadenadores (Triggers)
El workflow se ejecuta automáticamente cuando:
- Se realiza un **push** a la rama `main`.
- Se crea o actualiza un **pull request** hacia la rama `main`.

```yaml
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
