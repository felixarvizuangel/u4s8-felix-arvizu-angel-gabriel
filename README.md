# U4S8 — Félix Arvizu Ángel Gabriel

**Grupo:** DSM 4-1 · **Matrícula:** 25311014 · **Materia:** Aplicaciones Web · **Universidad Tecnológica de Hermosillo**

![Release](https://img.shields.io/badge/release-v1.0-green) ![Django](https://img.shields.io/badge/Django-6.1-0C4B33) ![MySQL](https://img.shields.io/badge/MySQL-Aiven-4479A1) ![Render](https://img.shields.io/badge/deploy-Render-46E3B7)

## Demo en vivo (producción)
- **Sitio:** https://u4s8-felix-arvizu-angel-gabriel.onrender.com
- **Login:** https://u4s8-felix-arvizu-angel-gabriel.onrender.com/login/
- **Servicios:** https://u4s8-felix-arvizu-angel-gabriel.onrender.com/servicios/
- **Release v1.0:** https://github.com/felixarvizuangel/u4s8-felix-arvizu-angel-gabriel/releases/tag/v1.0

> El plan Free de Render se duerme tras 15 minutos sin visitas; la primera carga puede tardar unos segundos.

---

## Descripción
Aplicación web hecha con Django para administrar servicios. Cada persona crea su cuenta,
inicia sesión y puede crear, editar, eliminar, buscar y paginar **solo sus propios** servicios.
Los datos se guardan en MySQL en la nube (Aiven) y el sitio está publicado en Render con HTTPS.

## Stack
| Tecnología | Uso |
|---|---|
| **Python + Django** | Backend, modelos, vistas, formularios y autenticación |
| **MySQL en Aiven** | Base de datos en la nube (conexión con PyMySQL) |
| **Tailwind CSS (Play CDN)** | Estilos de todas las plantillas |
| **Render** | Web Service con gunicorn, whitenoise y HTTPS automático |
| **python-decouple** | Lectura de variables de entorno desde `.env` |
| **Git + GitHub** | Control de versiones, un repositorio por práctica, Release v1.0 |

## Funcionalidades
- Registro, inicio y cierre de sesión propios con `django.contrib.auth`
- Contraseñas guardadas con hash (PBKDF2), nunca en texto plano
- CRUD de servicios con `ModelForm` y validación personalizada del precio (`clean_precio`)
- Cada usuario ve y edita solo sus servicios (`filter(propietario=request.user)`)
- Listado con paginación (5 por página) y búsqueda por título (`?q=...`)
- Mensajes flash y estado vacío
- Diseño responsivo y accesible (`aria-label`, foco visible, tabla con desplazamiento horizontal)

## Estructura del proyecto
```
miproyecto-django_FAAG/
├── core/
│   ├── models.py          # modelo Servicio (con propietario → User)
│   ├── forms.py           # ServicioForm con clean_precio
│   ├── views.py           # inicio, CRUD, registro, login, logout
│   ├── urls.py            # rutas de la app
│   └── templates/core/
│       ├── base.html      # molde con barra de sesión y mensajes
│       ├── inicio.html
│       ├── servicios.html # listado con búsqueda y paginación
│       ├── servicio_form.html
│       ├── servicio_confirm_delete.html
│       ├── registro.html
│       └── login.html
├── miproyecto_FAAG/
│   ├── settings.py        # lee .env; LOGIN_URL y LOGIN_REDIRECT_URL
│   ├── urls.py
│   └── wsgi.py
├── manage.py
├── requirements.txt
└── .gitignore             # excluye .env, venv/, staticfiles/, __pycache__/
```

## Rutas principales
| Ruta | Qué hace | ¿Pide sesión? |
|---|---|---|
| `/` | Página de inicio con datos de la conexión a MySQL | No |
| `/registro/` | Crear cuenta | No |
| `/login/` | Iniciar sesión | No |
| `/logout/` | Cerrar sesión | — |
| `/servicios/` | Listado propio con búsqueda y paginación | Sí |
| `/servicios/nuevo/` | Crear servicio | Sí |
| `/servicios/<id>/editar/` | Editar servicio propio (ajeno → 404) | Sí |
| `/servicios/<id>/eliminar/` | Eliminar servicio propio con confirmación | Sí |
| `/admin/` | Panel de administración de Django | Superusuario |

## Seguridad en producción
| Variable | Valor en Render | Para qué sirve |
|---|---|---|
| `DEBUG` | `False` | Oculta código y variables internas si ocurre un error |
| `SECRET_KEY` | Clave nueva, distinta de la local | Firma cookies de sesión y tokens CSRF |
| `ALLOWED_HOSTS` | Dominio sin `https://` | Evita ataques de "Host header" |
| `CSRF_TRUSTED_ORIGINS` | Dominio con `https://` | Permite enviar los formularios en producción |
| `DB_NAME`, `DB_USER`, `DB_PASSWORD`, `DB_HOST`, `DB_PORT` | Datos de Aiven | Conexión a MySQL |

- El archivo `.env` nunca se sube a GitHub (está en `.gitignore`).
- Render sirve el sitio con HTTPS (candado en el navegador).
- El respaldo de la base (`dumpdata`) se guarda fuera del repositorio.

## Prueba de aislamiento por usuario (Semana 6)
Registré usuario_a y usuario_b y creé 2 servicios con cada uno.
Con sesión de usuario_b, `/servicios/` solo muestra sus 2 servicios, nunca los de usuario_a.
Con usuario_b escribí a mano `/servicios/<id>/editar/` con el id de un servicio de usuario_a
y respondió **404 (Not Found)**, gracias a `get_object_or_404(Servicio, pk=pk, propietario=request.user)` en `views.py`.

## Cómo correrlo en local
```powershell
git clone https://github.com/felixarvizuangel/u4s8-felix-arvizu-angel-gabriel.git
cd u4s8-felix-arvizu-angel-gabriel
python -m venv venv
.\venv\Scripts\Activate.ps1
pip install -r requirements.txt
# crear .env con DB_NAME, DB_USER, DB_PASSWORD, DB_HOST, DB_PORT, SECRET_KEY, DEBUG=True, ALLOWED_HOSTS=127.0.0.1,localhost
python manage.py migrate
python manage.py runserver
```

## Deploy en Render
- **Build Command:** `pip install -r requirements.txt && python manage.py collectstatic --noinput && python manage.py migrate`
- **Start Command:** `gunicorn miproyecto_FAAG.wsgi`
- **Variables de entorno:** las 9 de la tabla de seguridad.

---

## Historial del curso — todas las unidades y prácticas

### Unidad 1 · Portafolio estático (HTML + Tailwind)
| Práctica | Tema | Render | GitHub |
|---|---|---|---|
| u1s1 | Sitio estático en Render | https://portafolio-faag-6kte.onrender.com | https://github.com/felixarvizuangel/portafolio_FAAG |
| u1s2 | Tailwind CSS | https://ine-personal-faag6.onrender.com | https://github.com/felixarvizuangel/ine_personal_FAAG |

### Unidad 2 · App Django + MySQL en Render
| Práctica | Tema | Render | GitHub |
|---|---|---|---|
| u2s3 | Django + MySQL en Aiven | https://u2s3-felix-arvizu-angel-gabriel.onrender.com/servicios/ | https://github.com/felixarvizuangel/u2s3-felix-arvizu-angel-gabriel |
| u2s4 | Modelos y deploy en Render | https://u2s4-felix-arvizu-angel-gabriel.onrender.com/admin/core/servicio/ | https://github.com/felixarvizuangel/u2s4-felix-arvizu-angel-gabriel |

### Unidad 3 · CRUD dinámico y sesiones
| Práctica | Tema | Render | GitHub |
|---|---|---|---|
| u3s5 | CRUD completo con ModelForms | https://u3s5-felix-arvizu-angel-gabriel.onrender.com | https://github.com/felixarvizuangel/u3s5-felix-arvizu-angel-gabriel |
| u3s6 | Autenticación, sesiones y seguridad | https://u3s6-felix-arvizu-angel-gabriel.onrender.com/login/ | https://github.com/felixarvizuangel/u3s6-felix-arvizu-angel-gabriel |

### Unidad 4 · Proyecto integrador final
| Práctica | Tema | Render | GitHub |
|---|---|---|---|
| u4s7 | Paginación, búsqueda y pulido de UX | https://u4s7-felix-arvizu-angel-gabriel.onrender.com/login/ | https://github.com/felixarvizuangel/u4s7-felix-arvizu-angel-gabriel |
| u4s8 | Deploy final, Release v1.0 y defensa | https://u4s8-felix-arvizu-angel-gabriel.onrender.com/login/ | https://github.com/felixarvizuangel/u4s8-felix-arvizu-angel-gabriel |

### Qué se agregó en cada semana
- **Semana 1:** primer sitio HTML publicado en Render.
- **Semana 2:** diseño con Tailwind CSS (Play CDN).
- **Semana 3:** proyecto Django conectado a MySQL en Aiven; vista de inicio con datos de la conexión.
- **Semana 4:** modelo `Servicio` con `propietario`, migraciones, panel `/admin` y deploy con gunicorn + whitenoise.
- **Semana 5:** CRUD propio con `ModelForm`, validación `clean_precio` y plantilla `base.html`.
- **Semana 6:** registro, login y logout propios; `@login_required`; mensajes flash; aislamiento por usuario (404 en datos ajenos).
- **Semana 7:** paginación con `Paginator`, búsqueda `titulo__icontains`, estado vacío, mejoras de accesibilidad y diseño responsivo.
- **Semana 8:** auditoría de seguridad, respaldo de la base, README definitivo, repositorio `u4s8`, deploy final y Release `v1.0`.

---

## Autor
**Félix Arvizu Ángel Gabriel** — DSM 4-1, UTH
- GitHub: https://github.com/felixarvizuangel
- Correo: a25311014@uthermosillo.edu.mx