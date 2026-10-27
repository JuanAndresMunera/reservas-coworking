# Proyecto: App de reservas de salas para coworking (prueba técnica)

## Stack
- Backend: Node + TypeScript (Fastify o Express), PostgreSQL, Zod para validación, Vitest para pruebas.
- Frontend: React + TypeScript (Vite).
- Migraciones versionadas (node-pg-migrate o Drizzle/Prisma migrate). Todo el código en TypeScript.

## Reglas de negocio (fuente de verdad)
- Tarifas/hora COP: pequeña 40.000, mediana 70.000, grande 120.000.
- Duración: 1 a 10 h, en bloques de 30 min.
- Horario: lunes a sábado 07:00–21:00 (America/Bogota). Domingo cerrado. La reserva debe TERMINAR a las 21:00 o antes.
- Descuento: 10% desde 4 h, 20% desde 8 h. Recargo sábado 25% sobre el valor YA descontado.
- Disponibilidad: no puede cruzarse con reservas `confirmed` de la misma sala. Fin == inicio NO es cruce (intervalo semiabierto [inicio, fin)).
- Estados: draft → confirmed → completed | cancelled. Transiciones validadas en backend.
- Cancelar exige motivo.

## Principios no negociables
1. La concurrencia y la idempotencia se garantizan en la BASE DE DATOS (constraints + transacciones), no solo en código.
2. Confirmar dos veces NO duplica ni recalcula el precio pactado: el precio se congela al confirmar.
3. Dinero en enteros (COP) o numeric; nunca floats.
4. Zona horaria: guardar timestamptz; interpretar y resolver fechas en America/Bogota.
5. Validar toda entrada en el servidor con Zod. Nunca confiar en el cliente.
6. Secretos solo en variables de entorno; mantener `.env.example`; `.env` en .gitignore.
7. El asistente de IA nunca calcula precios/disponibilidad: llama al cotizador.

## Forma de trabajar
- Antes de codificar algo no trivial: propón un plan breve y espera mi OK.
- Cambios pequeños y enfocados. Un commit por unidad lógica, con mensaje convencional (feat:, fix:, test:, docs:, chore:).
- Escribe pruebas junto con el código (cotizador y transiciones de estado son obligatorias).
- Si la especificación es ambigua: elige, anótalo en DECISIONES.md y continúa. No me interrumpas por trivialidades.
- Explica en 3-5 líneas el "por qué" de cada decisión no obvia (debo poder defenderla en vivo).
- No añadas dependencias, features ni abstracciones que no pida la especificación.
- Si no estás seguro de algo, dilo; no lo inventes.

## Comandos
(Actualiza esta sección cuando existan: dev, test, migrate, seed, lint.)