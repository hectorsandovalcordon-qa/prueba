# Proyecto de Pruebas Automatizadas con ChatGPT y Playwright

Este repositorio está diseñado para automatizar las pruebas de una tienda online utilizando la tecnología Playwright, con la capacidad de integrar **ChatGPT** para generar pruebas dinámicas basadas en el código desplegado, y ejecutarlas en múltiples navegadores.

## Descripción

Este proyecto tiene como objetivo automatizar el proceso de pruebas para una tienda online. Utilizamos **ChatGPT** para generar las pruebas de acuerdo con los cambios del código y **Playwright** para ejecutar las pruebas en múltiples navegadores.

### Flujo de trabajo

1. **Detección de nuevas versiones**: El agente de IA (ChatGPT) detecta que se ha desplegado una nueva versión de la tienda online.
2. **Revisión de changelog**: ChatGPT revisa el changelog para identificar los componentes que han cambiado.
3. **Generación de pruebas dinámicas**: ChatGPT genera automáticamente las pruebas correspondientes para los nuevos componentes.
4. **Ejecución de pruebas**: Las pruebas generadas son ejecutadas en diferentes navegadores utilizando **Playwright**.
5. **Reporte de resultados**: Los resultados de las pruebas se presentan en un dashboard para análisis y monitoreo.

## Requisitos

- **Node.js** (recomendado v14+)
- **Playwright**
- **OpenAI API** para generar pruebas dinámicas (requiere suscripción de pago)
- **GitHub Actions** para integración continua

## Configuración del Proyecto

### 1. Clonar el repositorio

Clona este repositorio en tu máquina local:

```bash
git clone https://github.com/tu-usuario/proyecto-chatgpt-pruebas.git
cd proyecto-chatgpt-pruebas
```
### 2. Instalar dependencias

Primero, asegúrate de tener **Node.js** instalado. Luego, instala las dependencias necesarias ejecutando los siguientes comandos:

```bash
npm install playwright
npm install openai
```
### 3. Configurar la API de OpenAI

- Dirígete a [OpenAI](https://beta.openai.com/signup/) y crea una cuenta para obtener una clave API.
- Guarda tu clave API en una variable de entorno `OPENAI_API_KEY`.

```bash
export OPENAI_API_KEY="tu-clave-api-de-openai"
```
### 3. Crear un script de pruebas

Este script utiliza **Playwright** para realizar pruebas de UI y **ChatGPT** para generar las pruebas dinámicamente basadas en el changelog.

```javascript
const { chromium } = require('playwright');
const { OpenAI } = require('openai');

// Configurar OpenAI API
const openai = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY,
});

async function generarPruebas(changelog) {
  const prompt = `Genera pruebas para una tienda online basadas en los siguientes cambios:\n${changelog}`;

  const respuesta = await openai.chat.completions.create({
    model: "gpt-4",
    messages: [{ role: "system", content: "Eres un generador de pruebas automatizadas para una tienda online." }, { role: "user", content: prompt }],
  });

  return respuesta.choices[0].message.content;
}

async function ejecutarPruebas() {
  const changelog = "Cambios recientes: Se añadió un nuevo botón de 'Comprar' en la página de productos.";
  
  // Generar las pruebas dinámicamente con ChatGPT
  const pruebasGeneradas = await generarPruebas(changelog);
  console.log("Pruebas generadas:", pruebasGeneradas);

  // Ejecutar las pruebas con Playwright
  const browser = await chromium.launch();
  const page = await browser.newPage();

  // Simular las pruebas generadas
  // Aquí iría el código para ejecutar las pruebas en la página de la tienda online

  await browser.close();
}

ejecutarPruebas().catch(console.error);
```
