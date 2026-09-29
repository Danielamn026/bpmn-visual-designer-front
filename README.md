# ProyectoWebFront

Frontend de un editor visual BPMN (Business Process Model and Notation) desarrollado con Angular para diseñar, visualizar y gestionar flujos de negocio mediante un canvas SVG interactivo tipo drag-and-drop.

## Composición del repositorio

- TypeScript: 77.5%
- HTML: 16%
- CSS: 6.1%
- JavaScript: 0.4%

## Descripción general

Este proyecto permite a una empresa crear procesos de negocio, definir actividades, gateways y conexiones entre nodos, mover elementos en el canvas, ajustar propiedades y preparar la base para persistencia a través de un backend Spring Boot.

La aplicación está orientada a un flujo visual de diseño donde el usuario trabaja directamente sobre un tablero BPMN con:

- actividades tipo rectángulo
- gateways tipo diamante
- conexiones con flechas y etiquetas
- manipulación visual por drag & drop
- redimensionamiento de elementos
- paneles laterales de edición
- integración con servicios HTTP para persistencia

## Stack tecnológico

- Angular 20
- TypeScript
- HTML5
- CSS3
- Tailwind CSS
- RxJS
- Angular SSR
- Proxy de desarrollo para backend
- Spring Boot (backend esperado / integrado en arquitectura)

## Estructura del proyecto

```text
ProyectoWebFront/
├── .gitignore
├── CONTEXTO_PROYECTO.txt
├── package-lock.json
├── front/
│   ├── .editorconfig
│   ├── .gitignore
│   ├── .vscode/
│   ├── README.md
│   ├── angular.json
│   ├── package.json
│   ├── package-lock.json
│   ├── postcss.config.js
│   ├── proxy.conf.json
│   ├── tailwind.config.js
│   ├── tsconfig.json
│   ├── tsconfig.app.json
│   ├── tsconfig.spec.json
│   ├── public/
│   └── src/
│       ├── app/
│       ├── index.html
│       ├── main.server.ts
│       ├── main.ts
│       ├── server.ts
│       └── styles.css
└── README.md
```

## Objetivo del proyecto

El objetivo principal es construir un editor visual de procesos BPMN con una interfaz web moderna que permita:

- definir actividades y decisiones
- conectar nodos de flujo
- organizar procesos por empresa
- mantener consistencia de datos con servicios REST
- preparar la lógica para integración con un backend Java / Spring Boot

## Modelo BPMN implementado

### 1. Activity (Actividad)
Representa una tarea o acción dentro del proceso.

- nombre
- descripción
- posición x, y
- ancho y alto
- rol asociado
- proceso al que pertenece
- estado activo/inactivo

### 2. Gateway (Puerta de enlace)
Representa bifurcaciones o decisiones.

Tipos principales:

- decision-gateway
- parallel-gateway
- exclusive-gateway

### 3. Edge (Conexión / Flecha)
Conecta nodos del flujo.

Soporta:

- Activity → Activity
- Activity → Gateway
- Gateway → Activity
- Gateway → Gateway

Cuenta con campos tipados modernos y compatibilidad con el sistema legacy.

### 4. Process (Proceso)
Agrupa todos los elementos del flujo para un conjunto de trabajo o negocio.

## Arquitectura y organización

Dentro de `front/src/app` se espera una estructura orientada a modelos, servicios, páginas y paneles.

```text
src/app/
├── models/
│   ├── Activity.ts
│   ├── Gateway.ts
│   ├── Edge.ts
│   └── Process.ts
├── services/
│   ├── activity.service.ts
│   ├── gateway.service.ts
│   ├── edge.service.ts
│   └── process.service.ts
└── pages/
    └── dashboard/
        ├── dashboard.ts
        ├── activity-panel.ts
        ├── gateway-panel.ts
        ├── edge-panel.ts
        └── process-form.ts
```

## Estado actual del proyecto

### Completado

- arquitectura Angular con componentes standalone
- canvas SVG interactivo
- drag & drop de actividades
- redimensionamiento de elementos
- conexión entre elementos
- soporte de proxy para backend en desarrollo
- servicio HTTP para `Activity` integrado
- servicio HTTP para `Gateway` integrado
- documentación técnica y comentarios JSDoc amplios

### Pendiente / en progreso

- integración completa de `EdgeService` con backend
- soporte total de conexiones tipadas entre Activity/Gateway
- endpoints backend actualizados para manejar relaciones tipadas
- pruebas E2E del flujo completo
- filtrado por proceso activo en paneles y dashboard

## Servicios y flujo de integración

El patrón de la app usa `BehaviorSubject` + `HttpClient` para mantener un store reactivo local:

- create
- update
- delete
- getById
- loadFromBackend

Este enfoque permite que la UI reaccione automáticamente ante cambios en los datos del backend sin duplicar lógica en cada componente.

## Proxy y configuración de desarrollo

El proyecto usa un proxy para evitar problemas de CORS durante el desarrollo.

Archivo relevante:

- `front/proxy.conf.json`

Configuración simplificada:

```json
{
  "/api": {
    "target": "http://localhost:8080",
    "secure": false,
    "changeOrigin": true,
    "logLevel": "debug"
  }
}
```

Esto hace que desde Angular se pueda hacer llamadas como:

```ts
this.http.get('/api/activity')
```

y la petición sea reenviada al backend en `localhost:8080`.

## Backend esperado

La arquitectura indica que el frontend se comunica con un backend Spring Boot y expone endpoints de estilo REST.

### ActivityController

- `POST /api/activity`
- `PUT /api/activity`
- `GET /api/activity`
- `GET /api/activity/{id}`
- `DELETE /api/activity/{id}`

### GatewayController

- `POST /api/gateway`
- `PUT /api/gateway`
- `GET /api/gateway`
- `GET /api/gateway/{id}`
- `DELETE /api/gateway/{id}`

### EdgeController

- `POST /api/edge`
- `PUT /api/edge`
- `GET /api/edge`
- `GET /api/edge/{id}`
- `DELETE /api/edge/{id}`

## Requisitos previos

Antes de ejecutar el proyecto necesitas:

- Node.js 18 o superior
- npm o pnpm
- Angular CLI (si quieres trabajar desde terminal)
- un backend Spring Boot ejecutándose en `localhost:8080` si quieres persistencia completa

## Instalación

Desde la raíz del repositorio:

```bash
cd front
npm install
```

## Ejecución en modo desarrollo

```bash
cd front
npm start
```

Esto inicia Angular con la configuración del proxy y normalmente abre la app en:

```text
http://localhost:4200
```

## Scripts disponibles

En `front/package.json` se incluyen los siguientes scripts:

```json
"scripts": {
  "ng": "ng",
  "start": "ng serve --proxy-config proxy.conf.json",
  "build": "ng build",
  "watch": "ng build --watch --configuration development",
  "test": "ng test",
  "serve:ssr:front": "node dist/front/server/server.mjs"
}
```

## Comandos útiles

### Instalar dependencias

```bash
cd front
npm install
```

### Iniciar la aplicación

```bash
cd front
npm start
```

### Compilar para producción

```bash
cd front
npm run build
```

### Ejecutar tests

```bash
cd front
npm test
```

## Flujo de trabajo típico

1. El usuario abre el dashboard en `localhost:4200`
2. Crea una actividad o un gateway
3. Conecta nodos con flechas
4. Mueve o redimensiona elementos sobre el canvas
5. El frontend guarda los cambios mediante servicios HTTP
6. El backend procesa la operación y devuelve respuesta JSON
7. El frontend actualiza el estado local y el render del canvas

## Documentación del proyecto

El repositorio incluye un archivo de contexto técnico muy detallado:

- `CONTEXTO_PROYECTO.txt`

Ese archivo contiene información sobre:

- objetivos del proyecto
- estructura del frontend
- servicios HTTP
- endpoints del backend
- detalles del modelo BPMN
- roadmap de integración
- checklist de desarrollo

## Roadmap sugerido

- completar integración de `GatewayService`
- terminar `EdgeService` con backend tipado
- verificar persistencia end-to-end
- agregar filtrado por proceso activo
- mejorar validaciones de UI y errores
- añadir pruebas unitarias y E2E
- documentar el manejo de roles y procesos en la app

## Notas importantes

- El repositorio contiene la app frontend dentro del directorio `front/`.
- El proyecto se encuentra en una etapa de integración continua con backend.
- La lógica de negocio está orientada a BPMN y procesos empresariales visuales.
- Hay un documento de contexto muy útil llamado `CONTEXTO_PROYECTO.txt` que describe mejor la intención y avances actuales.

## Resumen breve

`ProyectoWebFront` es un editor visual BPMN desarrollado en Angular 20 que permite crear procesos, actividades, gateways y conexiones en un canvas interactivo. Actualmente el frontend cuenta con una base sólida de renderizado visual y servicios HTTP, con integración avanzada para actividades y gateways, y con una etapa pendiente para completar la gestión de edges y persistencia total con el backend.

## Licencia

No se especifica una licencia en el repositorio en este momento. Si se desea usar el proyecto en un entorno profesional o institucional, conviene revisar la política del dueño del repositorio antes de distribuirlo.

## Autor / propietario

Proyecto asociado a la cuenta GitHub:

- `Danielamn026`
- repositorio: `ProyectoWebFront`

---

Este README fue generado con información del repositorio y del contexto técnico del proyecto para resumir la intención, stack, estructura y estado actual del mismo.
