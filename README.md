# U4S8 — Félix Arvizu Ángel Gabriel

**Grupo:** DSM 4-1 · **Matrícula:** 25311014

## Demo en vivo (producción)
https://u4s8-felix-arvizu-angel-gabriel.onrender.com

## Descripción
Sistema web para administrar servicios. Cada usuario crea su cuenta, inicia sesión
y puede crear, editar, eliminar, buscar y paginar **solo sus propios** servicios.

## Stack
- **Django** (Python) — backend, vistas, formularios y autenticación
- **MySQL en Aiven** — base de datos en la nube
- **Tailwind CSS** — estilos de las plantillas
- **Render** — Web Service con gunicorn y whitenoise, HTTPS automático

## Funcionalidades
- Registro, login y logout propios con `django.contrib.auth`
- CRUD de servicios con ModelForms y validación del precio
- Listado con paginación (5 por página) y búsqueda por título
- Mensajes flash y estado vacío
- Seguridad: `DEBUG=False`, `SECRET_KEY` propia, `ALLOWED_HOSTS`, `CSRF_TRUSTED_ORIGINS` y `.env` fuera del repo

## Prueba de aislamiento por usuario (Semana 6)
Registré usuario_a y usuario_b y creé 2 servicios con cada uno.
Con sesión de usuario_b, /servicios/ solo muestra sus 2 servicios, nunca los de usuario_a.
Con usuario_b escribí a mano /servicios/<id>/editar/ con el id de un servicio de usuario_a
y respondió 404 (Not Found), gracias al filtro `propietario=request.user` en `views.py`.

## Historial de unidades
| Práctica | Tema |
|---|---|
| u1s1 | Sitio estático en Render |
| u1s2 | Tailwind CSS |
| u2s3 | Django + MySQL en Aiven |
| u2s4 | Modelos y deploy en Render |
| u3s5 | CRUD con ModelForms |
| u3s6 | Autenticación, sesiones y seguridad |
| u4s7 | Paginación, búsqueda y pulido de UX |
| u4s8 | Deploy final, Release v1.0 y defensa |
