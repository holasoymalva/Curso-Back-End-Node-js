# Módulo 1: Fundamentos de Node.js 🚀

## 📋 Descripción
Este módulo proporciona una introducción completa a Node.js y sus conceptos fundamentales, incluyendo el event loop, módulos, paquetes npm y JavaScript moderno para backend.

## 🎯 Objetivos de Aprendizaje
- Comprender la arquitectura y funcionamiento de Node.js
- Dominar el sistema de módulos y gestión de paquetes
- Implementar programación asíncrona efectiva
- Utilizar características modernas de JavaScript en el backend

## 📚 Contenido

### 1. Introducción a Node.js y su Ecosistema

#### 1.1 ¿Qué es Node.js?
Node.js es un entorno de ejecución de JavaScript construido sobre el motor V8 de Chrome. Permite ejecutar JavaScript en el servidor de manera asíncrona y orientada a eventos.

##### Ejemplo: Primer servidor HTTP
```javascript
const http = require('http');

const server = http.createServer((req, res) => {
    res.writeHead(200, { 'Content-Type': 'text/plain' });
    res.end('¡Hola Mundo desde Node.js!');
});

const PORT = 3000;
server.listen(PORT, () => {
    console.log(`Servidor corriendo en http://localhost:${PORT}`);
});
```

#### 1.2 Event Loop
El Event Loop es el mecanismo que permite a Node.js realizar operaciones no bloqueantes a pesar de ser single-threaded.

##### Ejemplo: Demostración del Event Loop
```javascript
console.log('1. Inicio del programa');

setTimeout(() => {
    console.log('2. Temporizador de 0 segundos');
}, 0);

Promise.resolve().then(() => {
    console.log('3. Promesa resuelta');
});

process.nextTick(() => {
    console.log('4. Next tick');
});

console.log('5. Fin del programa');

// Salida:
// 1. Inicio del programa
// 5. Fin del programa
// 4. Next tick
// 3. Promesa resuelta
// 2. Temporizador de 0 segundos
```

#### 1.3 Sistema de Módulos

##### CommonJS (Tradicional)
```javascript
// math.js
module.exports = {
    suma: (a, b) => a + b,
    resta: (a, b) => a - b,
    multiplica: (a, b) => a * b,
    divide: (a, b) => a / b
};

// app.js
const matematicas = require('./math');
console.log(matematicas.suma(5, 3)); // 8
```

##### ES Modules (Moderno)
```javascript
// math.mjs
export const suma = (a, b) => a + b;
export const resta = (a, b) => a - b;
export const multiplica = (a, b) => a * b;
export const divide = (a, b) => a / b;

// app.mjs
import { suma, resta } from './math.mjs';
console.log(suma(5, 3)); // 8
```

### 2. JavaScript Moderno para Backend

#### 2.1 Async/Await
```javascript
// Ejemplo de función asíncrona para leer archivos
const fs = require('fs').promises;

async function leerArchivos() {
    try {
        const archivo1 = await fs.readFile('archivo1.txt', 'utf8');
        const archivo2 = await fs.readFile('archivo2.txt', 'utf8');
        
        console.log('Contenido archivo 1:', archivo1);
        console.log('Contenido archivo 2:', archivo2);
    } catch (error) {
        console.error('Error al leer archivos:', error);
    }
}

leerArchivos();
```

#### 2.2 Promises
```javascript
// Ejemplo de una promesa personalizada
function dividir(a, b) {
    return new Promise((resolve, reject) => {
        if (b === 0) {
            reject(new Error('No se puede dividir por cero'));
        } else {
            resolve(a / b);
        }
    });
}

// Uso de la promesa
dividir(10, 2)
    .then(resultado => console.log('Resultado:', resultado))
    .catch(error => console.error('Error:', error.message));

// Ejemplo con Promise.all
const promesas = [
    dividir(10, 2),
    dividir(20, 4),
    dividir(30, 6)
];

Promise.all(promesas)
    .then(resultados => console.log('Todos los resultados:', resultados))
    .catch(error => console.error('Error en alguna división:', error.message));
```

#### 2.3 Destructuring y Spread Operator
```javascript
// Destructuring de objetos
const config = {
    host: 'localhost',
    puerto: 3000,
    database: 'miapp'
};

const { host, puerto, database } = config;
console.log(`Conectando a ${host}:${puerto}`);

// Spread operator con arrays
const numeros = [1, 2, 3];
const masNumeros = [...numeros, 4, 5, 6];
console.log(masNumeros); // [1, 2, 3, 4, 5, 6]

// Spread con objetos
const usuarioBase = {
    nombre: 'Juan',
    edad: 25
};

const usuarioCompleto = {
    ...usuarioBase,
    rol: 'admin',
    activo: true
};
```

## 🛠️ Proyecto Práctico: Sistema de Logs

```javascript
// logger.js
import fs from 'fs/promises';
import path from 'path';

class Logger {
    constructor(filename) {
        this.filename = filename;
        this.path = path.join(process.cwd(), 'logs', filename);
    }

    async log(message) {
        const timestamp = new Date().toISOString();
        const logEntry = `[${timestamp}] ${message}\n`;

        try {
            await fs.mkdir(path.dirname(this.path), { recursive: true });
            await fs.appendFile(this.path, logEntry);
            console.log(`Log guardado: ${message}`);
        } catch (error) {
            console.error('Error al escribir log:', error);
        }
    }

    async getLogs() {
        try {
            const content = await fs.readFile(this.path, 'utf8');
            return content.split('\n').filter(line => line.length > 0);
        } catch (error) {
            if (error.code === 'ENOENT') {
                return [];
            }
            throw error;
        }
    }
}

// Uso del logger
const logger = new Logger('app.log');

async function ejemploUso() {
    await logger.log('Aplicación iniciada');
    await logger.log('Usuario autenticado');
    await logger.log('Error en la base de datos');

    const logs = await logger.getLogs();
    console.log('Todos los logs:', logs);
}

ejemploUso();
```

## 📝 Ejercicios Prácticos

1. **Event Loop y Asincronía**
   - Crear un programa que lea múltiples archivos de forma asíncrona
   - Implementar un sistema de cola de tareas

2. **Módulos y npm**
   - Crear un paquete npm propio
   - Implementar un sistema de plugins

3. **JavaScript Moderno**
   - Convertir código callbacks a Promises y luego a async/await
   - Implementar un API utilizando las características modernas de JS

## 🔍 Recursos Adicionales

- [Documentación oficial de Node.js](https://nodejs.org/docs)
- [Event Loop Visualización](https://nodejs.dev/learn/the-nodejs-event-loop)
- [npm Docs](https://docs.npmjs.com/)

## ⚡ Tips y Mejores Prácticas

1. Siempre maneja errores en operaciones asíncronas
2. Utiliza `const` y `let` en lugar de `var`
3. Aprovecha el destructuring para código más limpio
4. Mantén el código modular y reutilizable
5. Usa ESLint para mantener consistencia en el código

## 📚 Tareas

1. Crear un servidor HTTP básico que:
   - Sirva archivos estáticos
   - Maneje diferentes rutas
   - Implemente logging
   - Maneje errores apropiadamente

2. Implementar un sistema de módulos que:
   - Use tanto CommonJS como ES Modules
   - Implemente lazy loading
   - Maneje configuraciones

## ✅ Criterios de Evaluación

- Comprensión del Event Loop (20%)
- Implementación correcta de async/await (20%)
- Uso apropiado de módulos (20%)
- Manejo de errores (20%)
- Código limpio y mejores prácticas (20%)

---

¿Preguntas? No dudes en abrir un issue en este repositorio.
