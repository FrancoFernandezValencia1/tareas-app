# ✅ App de Tareas — React + Node.js (Full-Stack)

Prototipo full-stack: frontend **React (Vite)** + backend **Node.js (Express)** que se comunican por HTTP con JSON.
Cumple con la Guía 8 (plan de acción) y la rúbrica de *Investigación y Prototipo Full-Stack* (datos simulados / mock data en memoria).

| Parte | Tecnología | Dónde corre |
|-------|------------|-------------|
| Frontend | React 18 + Vite | **Vercel** (estático) |
| Backend | Node.js + Express | **Render** (proceso 24/7) |
| Comunicación | HTTP + JSON (API REST) | CORS restringido al dominio del frontend |

## 🌐 URLs en vivo

- **Frontend (Vercel):** https://tareas-app-one.vercel.app
- **API (Render):** https://tareas-api-5mxe.onrender.com/api/tareas

## 📁 Estructura del proyecto

```
tareas-app/
├── server/          ← Backend (Node.js + Express)
│   ├── package.json
│   └── index.js     ← API REST (los 4 endpoints + health check)
└── client/          ← Frontend (React + Vite)
    ├── index.html
    ├── vite.config.js
    ├── .env.example
    └── src/
        ├── main.jsx
        ├── App.jsx          ← Estado global + comunicación con la API
        ├── App.css
        └── components/
            ├── Formulario.jsx
            ├── ListaTareas.jsx
            └── Tarea.jsx
```

## 🚀 Ejecutar en local (dos terminales)

### 1. Backend (http://localhost:3001)
```bash
cd server
npm install
npm run dev
```

### 2. Frontend (http://localhost:5173)
```bash
cd client
npm install
cp .env.example .env   # solo la primera vez
npm run dev
```

### 3. Probar la API sin navegador
```bash
curl http://localhost:3001/api/tareas
curl -X POST http://localhost:3001/api/tareas -H "Content-Type: application/json" -d '{"titulo":"Mi tarea"}'
curl -X PUT  http://localhost:3001/api/tareas/1 -H "Content-Type: application/json" -d '{"completada":true}'
curl -X DELETE http://localhost:3001/api/tareas/1
```

## 📡 Endpoints de la API

| Método | Ruta | Acción | Código |
|--------|------|--------|--------|
| GET | `/api/tareas` | Obtener todas las tareas | 200 |
| POST | `/api/tareas` | Crear una tarea | 201 / 400 |
| PUT | `/api/tareas/:id` | Completar / descompletar | 200 / 404 |
| DELETE | `/api/tareas/:id` | Eliminar una tarea | 204 / 404 |
| GET | `/` | Health check | 200 |

## ☁️ Despliegue

### Backend — Render
1. **New → Web Service** → conectar el repositorio.
2. Configuración:

   | Campo | Valor |
   |-------|-------|
   | Name | `tareas-api` |
   | Root Directory | `server` |
   | Build Command | `npm install` |
   | Start Command | `node index.js` |
   | Instance Type | Free |
3. Environment:
   - `CLIENTE_URL` = `https://tareas-app-one.vercel.app` (restringe CORS al frontend real).
4. URL resultante: `https://tareas-api-5mxe.onrender.com`

### Frontend — Vercel
1. **Add New → Project** → importar el repositorio.
2. **Root Directory** = `client` (Vercel detecta Vite).

   | Campo | Valor |
   |-------|-------|
   | Build Command | `npm run build` |
   | Output Directory | `dist` |
   | Env var `VITE_API_URL` | `https://tareas-api-5mxe.onrender.com/api/tareas` |
3. Deploy → `https://tareas-app-one.vercel.app`

## 🔐 Buenas prácticas aplicadas

- `process.env.PORT` en el servidor (Render asigna el puerto automáticamente).
- CORS restringido con `CLIENTE_URL` (nunca `*` en producción).
- Variables de entorno separadas por entorno; `.env` está en `.gitignore` (nunca se sube a GitHub).
- Los datos viven en memoria (mock data): al reiniciar el servidor se regeneran, como exige la rúbrica de prototipo con datos simulados.
- Deploy automático con cada `git push` a `main` (CI/CD en Vercel y Render).
