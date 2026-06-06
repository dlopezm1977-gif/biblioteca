# Mi Biblioteca

Aplicación web personal para gestionar libros leídos y pendientes de lectura. Funciona como PWA (Progressive Web App), por lo que puede instalarse en móvil o escritorio y usarse sin conexión.

## Tecnologías

- **Frontend**: HTML + CSS + JavaScript vanilla (sin frameworks)
- **Base de datos**: Firebase Realtime Database (REST API)
- **Offline**: Service Worker + localStorage como caché local
- **Gráficos**: Chart.js 4.4
- **Tipografías**: Bebas Neue, DM Sans, DM Mono (Google Fonts)

## Estructura de ficheros

```
biblioteca/
├── index.html      # Toda la aplicación (HTML, CSS y JS en un único fichero)
├── logo.png        # Icono de la app (header, favicon y PWA)
├── manifest.json   # Configuración PWA
└── sw.js           # Service Worker para caché offline
```

## Funcionalidades

### Pestañas
- **Leídos**: biblioteca principal con los libros terminados
- **Pendientes**: lista de libros por leer
- **Estadísticas**: resumen visual de hábitos de lectura

### Vista de leídos
- Libros agrupados por autor en orden alfabético
- Cada grupo es colapsable/desplegable
- Dentro de cada autor los libros se ordenan por fecha de lectura (más reciente primero)
- Filtro por valoración (emojis de color)
- Búsqueda por título o autor

### Ficha de libro
Cada libro almacena los siguientes campos:

| Campo | Descripción |
|---|---|
| `titulo` | Título del libro (obligatorio) |
| `autor` | Nombre del autor |
| `estado` | `leido` o `pendiente` |
| `fechaLectura` | Fecha en formato `YYYY-MM-DD` |
| `valoracion` | `morado`, `rojo`, `naranja`, `amarillo` o `verde` |
| `protagonistas` | Personajes principales |
| `trama` | Reseña personal |
| `palabrasClave` | Array de etiquetas |
| `fechaCreacion` | Timestamp de creación (Unix ms) |

### Sistema de valoración

El color de la barra lateral izquierda de cada tarjeta indica la valoración:

| Color | Significado |
|---|---|
| 🟣 Morado | Me encanta |
| 🔴 Rojo | Me gusta mucho |
| 🟠 Naranja | Me gusta algo |
| 🟡 Amarillo | Me gusta poco |
| 🟢 Verde | No me gusta nada |

### Estadísticas
- Tarjetas de resumen (total leídos, leídos en el período, pendientes)
- Gráfico de dona por valoración
- Gráfico de barras por año
- Top autores y top palabras clave
- Agrupación de libros por valoración

## Base de datos (Firebase)

La URL de la base de datos está definida en `index.html`:

```javascript
const DB_URL = 'https://biblioteca-654fc-default-rtdb.europe-west1.firebasedatabase.app';
```

Los libros se almacenan bajo el nodo `/libros` con la siguiente estructura:

```json
{
  "-NxABC123": {
    "titulo": "El nombre del viento",
    "autor": "Patrick Rothfuss",
    "estado": "leido",
    "fechaLectura": "2024-03-15",
    "valoracion": "morado",
    "protagonistas": "Kvothe",
    "trama": "Historia de un mago legendario...",
    "palabrasClave": ["fantasía", "magia"],
    "fechaCreacion": 1710500000000
  }
}
```

## Modo offline

El Service Worker cachea todos los assets estáticos. Si no hay conexión:
- Se muestra el aviso `⚠️ Sin conexión` en el header
- Los datos se sirven desde `localStorage`
- Los cambios realizados sin conexión **no se sincronizan** al volver a tener red

## Ejecutar en local

Cualquier servidor HTTP estático sirve. Con Python:

```bash
python -m http.server 8080
```

Abrir en el navegador: `http://localhost:8080`

## Actualizar la caché del Service Worker

Cada vez que se modifica `index.html` hay que incrementar la versión en `sw.js`:

```javascript
const CACHE = 'bib-v8'; // incrementar este número
```

Y recargar con **Ctrl+Shift+R** en el navegador.
