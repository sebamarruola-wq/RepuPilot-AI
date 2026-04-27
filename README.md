[README.md](https://github.com/user-attachments/files/27141808/README.md)
# RepuPilot AI — Guía de despliegue

## Estructura del proyecto
```
repupilot/
├── server.js          ← Backend (Node.js + Express)
├── package.json       ← Dependencias
├── railway.toml       ← Config para Railway
└── public/
    └── index.html     ← Frontend completo
```

## Despliegue en Railway (recomendado, gratis para empezar)

### Paso 1 — Sube el proyecto a GitHub
1. Crea una cuenta en github.com (si no tienes)
2. Crea un repositorio nuevo llamado `repupilot-ai`
3. Sube todos los archivos de esta carpeta

### Paso 2 — Despliega en Railway
1. Ve a railway.app y crea una cuenta (puedes entrar con GitHub)
2. Haz clic en "New Project" → "Deploy from GitHub repo"
3. Selecciona tu repositorio `repupilot-ai`
4. Railway detecta automáticamente que es un proyecto Node.js

### Paso 3 — Añade tu API key (IMPORTANTE)
En Railway, ve a tu proyecto → Variables → Add Variable:
- **Name:** `ANTHROPIC_API_KEY`
- **Value:** tu clave de Anthropic (obtenla en console.anthropic.com)

### Paso 4 — Obtén tu URL pública
Railway te da una URL tipo: `https://repupilot-ai-production.up.railway.app`
¡Tu web ya está funcionando con IA real!

### Paso 5 — Dominio personalizado (opcional)
En Railway → Settings → Domains → Add Custom Domain
Apunta tu dominio (ej: repupilot.es) a la URL de Railway.

## Obtener tu API key de Anthropic
1. Ve a console.anthropic.com
2. Crea una cuenta o inicia sesión
3. Ve a "API Keys" → "Create Key"
4. Copia la clave y pégala en Railway como variable de entorno

## Costes estimados
- Railway: gratis hasta $5/mes de uso (suficiente para empezar)
- Anthropic API: ~$0.003 por generación (claude-sonnet)
  → 1000 generaciones ≈ 3€
- Dominio: ~10€/año

## Soporte
Para cualquier duda técnica, contacta al desarrollador.
