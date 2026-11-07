---
name: clima
description: Consulta la temperatura y el estado del clima actual de una ciudad (por defecto Ciudad de México) y lo reporta en español. Úsalo cuando el usuario pida el clima, la temperatura o el pronóstico de su ciudad.
argument-hint: "[ciudad]"
---

# Clima

Obtiene el clima actual de una ciudad y lo reporta de forma breve en español.

## Pasos

1. Determina la ciudad: usa el argumento si se dio; si no, usa `Mexico City` (Ciudad de México).
2. Reemplaza los espacios por `+` y consulta wttr.in con Bash:

   ```bash
   curl -s "https://wttr.in/<ciudad>?format=%t+%C&lang=es"
   ```

   Ejemplo de salida: `+17°C Lluvia localizada en las cercanías`

3. Si se pide más detalle (humedad, viento, sensación térmica), usa:

   ```bash
   curl -s "https://wttr.in/<ciudad>?format=%l:+%t+(sensación+%f),+%C,+humedad+%h,+viento+%w&lang=es"
   ```

4. Responde en español, en una sola línea: ciudad, temperatura y estado del clima. Ejemplo:
   `Ciudad de México: **17 °C**, lluvia localizada en las cercanías.`

## Notas

- Si `curl` falla o devuelve vacío/HTML de error, dilo claramente y no inventes datos.
- Para repetirlo periódicamente, combínalo con `/loop` (por ejemplo, `/loop 5m /clima`).
