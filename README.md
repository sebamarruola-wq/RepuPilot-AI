[package.json](https://github.com/user-attachments/files/27124397/package.json)
[README.md](https://github.com/user-attachments/files/27124401/README.md)[server.js](https://github.com/user-attachments/files/27124406/server.js)const express = require('express');
const cors = require('cors');
const path = require('path');

const app = express();
app.use(cors());
app.use(express.json());
app.use(express.static(path.join(__dirname, 'public')));

const SYSTEM_PROMPT = `Eres el asistente de RepuPilot AI, especializado en gestión de reputación online para negocios locales españoles.

REGLAS ABSOLUTAS:
- Genera respuestas naturales, cortas (máx. 80 palabras) y humanas. Nunca suenes a robot.
- No inventes hechos, datos ni detalles que no estén en la reseña.
- Adapta el tono al tipo de reseña (positiva, neutra, negativa) y al tono elegido.
- Para reseñas negativas: reconoce el problema con empatía, sin sonar defensivo ni atacar al cliente.
- No hagas promesas legalmente arriesgadas (descuentos, compensaciones específicas).
- Evita frases largas y clichés genéricos como "nos complace enormemente".
- Varía las 3 respuestas entre sí en estructura y vocabulario.
- Al final de cada respuesta añade en nueva línea: [⚠ Revisa este texto antes de publicarlo]
- Responde siempre en español.
- Si te dan información del negocio (nombre, sector, ciudad), úsala de forma natural.
- Formato: devuelve EXACTAMENTE 3 respuestas numeradas:
  1. [texto]
  2. [texto]
  3. [texto]`;

app.post('/api/generate', async (req, res) => {
  const apiKey = process.env.ANTHROPIC_API_KEY;
  if (!apiKey) {
    return res.status(500).json({ error: 'API key no configurada en el servidor.' });
  }

  const { prompt } = req.body;
  if (!prompt) {
    return res.status(400).json({ error: 'Falta el campo prompt.' });
  }

  try {
    const response = await fetch('https://api.anthropic.com/v1/messages', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'x-api-key': apiKey,
        'anthropic-version': '2023-06-01'
      },
      body: JSON.stringify({
        model: 'claude-sonnet-4-20250514',
        max_tokens: 1000,
        system: SYSTEM_PROMPT,
        messages: [{ role: 'user', content: prompt }]
      })
    });

    const data = await response.json();

    if (!response.ok) {
      return res.status(response.status).json({ error: data.error?.message || 'Error de API' });
    }

    res.json({ text: data.content?.[0]?.text || '' });
  } catch (err) {
    console.error(err);
    res.status(500).json({ error: 'Error interno del servidor.' });
  }
});

// Todas las rutas devuelven index.html (SPA)
app.get('*', (req, res) => {
  res.sendFile(path.join(__dirname, 'public', 'index.html'));
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => console.log(`RepuPilot AI corriendo en puerto ${PORT}`));
