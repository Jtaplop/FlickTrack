# 🎬 FlickTrack

### Your movies. Your taste. Your crew.

---

## 📄 Descripción del proyecto

FlickTrack es una plataforma de seguimiento cinematográfico que permite a los
usuarios llevar un registro personal de las películas y series que consumen,
escribir reviews, crear listas temáticas y formar clubs de cine donde grupos
de personas votan y deciden qué ver juntos.

El sistema consume datos de la API pública de TMDB (The Movie Database) como
fuente de información cinematográfica y construye sobre ella toda la capa
social y de seguimiento.

---

## 🎯 Objetivos del proyecto

- Construir una API REST robusta, documentada y desplegada en producción
- Implementar un sistema de autenticación y autorización completo
- Integrar una API externa con sistema de caché
- Diseñar un modelo de datos relacional complejo con múltiples relaciones
- Aplicar buenas prácticas de desarrollo: testing, documentación, control
  de versiones, contenedores
- Desarrollar un frontend funcional que consuma la API

---

## 🛠️ Stack tecnológico

### Backend (foco principal)

| Tecnología     | Uso                                      |
|----------------|------------------------------------------|
| Python 3.11+   | Lenguaje principal                       |
| FastAPI        | Framework de la API                      |
| PostgreSQL 15+ | Base de datos relacional                 |
| SQLAlchemy 2.0 | ORM                                      |
| Alembic        | Migraciones de base de datos             |
| Redis          | Caché y almacenamiento temporal          |
| Pydantic v2    | Validación de datos y schemas            |
| PyJWT / python-jose | Autenticación JWT                   |
| Bcrypt         | Hashing de contraseñas                   |
| Pytest         | Testing                                  |
| Httpx          | Cliente HTTP para TMDB y testing async   |
| Docker         | Contenedores                             |
| Docker Compose | Orquestación local                       |

### Frontend (secundario)

| Tecnología     | Uso                                      |
|----------------|------------------------------------------|
| React 18+      | Librería UI                              |
| Vite           | Build tool                               |
| Tailwind CSS   | Estilos                                  |
| Axios          | Peticiones HTTP                          |
| React Router   | Navegación                               |
| Recharts       | Gráficos de estadísticas                 |

### Servicios externos

| Servicio       | Uso                                      |
|----------------|------------------------------------------|
| TMDB API v3    | Fuente de datos cinematográficos         |
| Railway/Render | Despliegue en producción                 |
| GitHub Actions | CI/CD                                    |

---
### Estructura de directorios esperada
flicktrack/
├── backend/
│ ├── app/
│ │ ├── init.py
│ │ ├── main.py
│ │ ├── config.py
│ │ ├── database.py
│ │ ├── dependencies.py
│ │ ├── models/
│ │ ├── schemas/
│ │ ├── routers/
│ │ ├── services/
│ │ ├── middleware/
│ │ └── utils/
│ ├── tests/
│ ├── alembic/
│ ├── alembic.ini
│ ├── requirements.txt
│ ├── Dockerfile
│ └── .env.example
├── frontend/
│ ├── src/
│ ├── public/
│ ├── package.json
│ ├── Dockerfile
│ └── ...
├── docker-compose.yml
├── .gitignore
├── .github/
│ └── workflows/
└── README.md

---

## 📋 Requisitos funcionales

### RF-01: Gestión de usuarios

- RF-01.1: Un usuario puede registrarse con email, username y contraseña
- RF-01.2: Un usuario puede iniciar sesión y recibir un token JWT
- RF-01.3: Un usuario puede refrescar su token antes de que expire
- RF-01.4: Un usuario puede ver y editar su perfil (bio, avatar_url)
- RF-01.5: Un usuario puede ver el perfil público de otros usuarios
- RF-01.6: Un usuario puede seguir y dejar de seguir a otros usuarios
- RF-01.7: Un usuario puede ver su lista de seguidores y seguidos
- RF-01.8: Existen dos roles: `user` y `admin`
- RF-01.9: Un admin puede desactivar cuentas de usuario

### RF-02: Catálogo de películas

- RF-02.1: Un usuario puede buscar películas por título
- RF-02.2: Un usuario puede ver el detalle de una película (datos de TMDB)
- RF-02.3: Un usuario puede explorar películas por género
- RF-02.4: El sistema muestra películas trending (actualizadas periódicamente)
- RF-02.5: Los resultados de TMDB se cachean en Redis con TTL configurable
- RF-02.6: Las películas que un usuario marca se persisten en la base de
  datos local con los datos básicos (no depender de TMDB para datos ya
  guardados)

### RF-03: Seguimiento personal

- RF-03.1: Un usuario puede marcar una película como **vista**
  (con fecha opcional)
- RF-03.2: Un usuario puede añadir una película a su **watchlist**
- RF-03.3: Un usuario puede marcar una película como **favorita**
- RF-03.4: Un usuario puede puntuar una película (escala 1-10, permite
  medios: 7.5)
- RF-03.5: Un usuario puede cambiar o eliminar cualquiera de estos estados
- RF-03.6: Un usuario puede ver su historial de películas vistas ordenado
  por fecha
- RF-03.7: Un usuario puede filtrar su historial por estado, género,
  año y rating

### RF-04: Reviews

- RF-04.1: Un usuario puede escribir una review de una película
- RF-04.2: Una review contiene: texto, rating y un flag de spoiler
  (sí/no)
- RF-04.3: Un usuario solo puede escribir una review por película
- RF-04.4: Un usuario puede editar o eliminar su propia review
- RF-04.5: Cualquier usuario puede ver las reviews de una película
- RF-04.6: Las reviews se pueden ordenar por fecha, rating o número
  de likes
- RF-04.7: Un usuario puede dar like a la review de otro usuario
- RF-04.8: Un usuario no puede dar like a su propia review

### RF-05: Listas personalizadas

- RF-05.1: Un usuario puede crear listas con nombre y descripción
- RF-05.2: Una lista puede ser pública o privada
- RF-05.3: Un usuario puede añadir y quitar películas de sus listas
- RF-05.4: Las películas dentro de una lista tienen un orden
  (posición) que el usuario puede modificar
- RF-05.5: Un usuario puede ver las listas públicas de otros usuarios
- RF-05.6: Un usuario puede dar like a una lista pública
- RF-05.7: Un usuario puede duplicar una lista pública a su cuenta

### RF-06: CineClubs

- RF-06.1: Un usuario puede crear un club con nombre, descripción
  e imagen
- RF-06.2: Un club puede ser público (cualquiera se une) o privado
  (solo por invitación)
- RF-06.3: El creador del club es automáticamente admin
- RF-06.4: Un admin del club puede promover miembros a admin
- RF-06.5: Un admin del club puede expulsar miembros
- RF-06.6: Un club tiene un límite máximo de miembros configurable
  por el creador
- RF-06.7: Cualquier miembro puede proponer una película para la
  siguiente sesión
- RF-06.8: Cada miembro puede votar una vez por ronda de votación
- RF-06.9: Un admin puede cerrar la votación y declarar la película
  ganadora
- RF-06.10: Tras ver la película, los miembros pueden comentar en
  un hilo de discusión del club asociado a esa película
- RF-06.11: El club mantiene un historial de todas las películas
  vistas

### RF-07: Feed de actividad

- RF-07.1: El sistema registra eventos de actividad: película vista,
  review publicada, lista creada, unión a club
- RF-07.2: Un usuario puede ver un feed con la actividad de los
  usuarios que sigue
- RF-07.3: El feed está paginado
- RF-07.4: Un usuario puede ver su propio historial de actividad

### RF-08: Estadísticas

- RF-08.1: Un usuario puede ver su dashboard con:
  - Total de películas vistas
  - Total de horas consumidas
  - Distribución por género (porcentajes)
  - Rating promedio que otorga
  - Películas vistas por mes (último año)
  - Top 5 géneros más vistos
  - Racha actual (días consecutivos marcando películas)
- RF-08.2: Estadísticas globales de la plataforma:
  - Películas mejor valoradas por la comunidad
  - Reviews más populares (más likes)
  - Usuarios más activos
  - Listas más seguidas

---

## 📋 Requisitos no funcionales

### RNF-01: Rendimiento
- Las respuestas de la API no deben superar los 500ms en el
  percentil 95
- Las consultas a TMDB deben cachearse en Redis con un TTL mínimo
  de 1 hora para búsquedas y 24 horas para datos de detalle
- Los endpoints de listado deben estar paginados (máximo 20
  elementos por página por defecto)

### RNF-02: Seguridad
- Las contraseñas deben almacenarse hasheadas con bcrypt
- Los tokens JWT deben tener una expiración configurable
  (recomendado: 30 minutos access, 7 días refresh)
- Los endpoints protegidos deben validar el token en cada petición
- Un usuario solo puede modificar o eliminar sus propios recursos
- Los inputs deben validarse con Pydantic antes de llegar a la
  lógica de negocio
- Las variables sensibles (API keys, secrets) deben estar en
  variables de entorno, nunca en el código
- CORS configurado correctamente

### RNF-03: Calidad de código
- Cobertura de tests mínima: 60%
- Tests unitarios para la lógica de negocio (services)
- Tests de integración para los endpoints principales
- Type hints en todas las funciones y métodos
- Código formateado con black o ruff
- Linting con ruff

### RNF-04: Documentación
- Todos los endpoints documentados en Swagger/OpenAPI
  (FastAPI lo genera automáticamente si se hace bien)
- README con instrucciones de instalación y ejecución
- Archivo .env.example con todas las variables necesarias
- Docstrings en services y funciones complejas

### RNF-05: Infraestructura
- El proyecto debe poder levantarse con un solo comando:
  `docker-compose up`
- La base de datos debe tener migraciones gestionadas con Alembic
- El proyecto debe estar desplegado y accesible públicamente
- Pipeline CI/CD que ejecute tests en cada push a main

---

## 🗄️ Modelo de datos

A continuación se describen las entidades principales y sus relaciones.
El diseño exacto de las tablas, tipos de datos, constraints e índices
es responsabilidad del desarrollador.

### Entidades

| Entidad          | Descripción                                          |
|------------------|------------------------------------------------------|
| User             | Usuario registrado en la plataforma                  |
| Movie            | Película almacenada localmente (origen TMDB)         |
| Genre            | Género cinematográfico                               |
| UserMovie        | Relación usuario-película (estado, rating, fecha)    |
| Review           | Reseña escrita por un usuario sobre una película     |
| ReviewLike       | Like de un usuario a una review                      |
| List             | Lista temática creada por un usuario                 |
| ListMovie        | Película dentro de una lista (con posición)          |
| ListLike         | Like de un usuario a una lista                       |
| Follow           | Relación de seguimiento entre usuarios               |
| Club             | Club de cine                                         |
| ClubMember       | Membresía de un usuario en un club (con rol)         |
| ClubProposal     | Película propuesta en un club para votación          |
| ClubVote         | Voto de un miembro a una propuesta                   |
| ClubWatched      | Película marcada como vista por el club              |
| ClubComment      | Comentario en la discusión de una película del club  |
| ActivityEvent    | Evento de actividad de un usuario                    |

### Relaciones clave
User ──1:N──► UserMovie ◄──N:1── Movie
User ──1:N──► Review ◄──N:1──── Movie
User ──1:N──► List ──1:N──► ListMovie ◄──N:1── Movie
User ──N:M──► User (Follow)
User ──N:M──► Club (ClubMember con rol)
Club ──1:N──► ClubProposal ──1:N──► ClubVote
Club ──1:N──► ClubWatched ──1:N──► ClubComment
Movie ──N:M──► Genre

### Consideraciones de diseño

- Decidir qué datos de TMDB se persisten localmente y cuáles se
  consultan bajo demanda
- Las tablas intermedias de relaciones N:M pueden necesitar campos
  adicionales (fecha, orden, rol, etc.)
- Los índices deben colocarse en campos que se usen frecuentemente
  en filtros y búsquedas
- Considerar soft delete (campo `is_active` o `deleted_at`) vs
  hard delete según la entidad
- Los timestamps (`created_at`, `updated_at`) son obligatorios en
  todas las entidades

---

## 🔌 Endpoints de la API

Se listan los recursos y las operaciones esperadas. El diseño exacto
de URLs, query params, request/response bodies y códigos de estado
es responsabilidad del desarrollador. Deben seguir convenciones REST.

### Auth
POST /auth/register
POST /auth/login
POST /auth/refresh

### Users
GET /users/me
PUT /users/me
GET /users/{username}
GET /users/{username}/stats
POST /users/{username}/follow
DELETE /users/{username}/follow
GET /users/{username}/followers
GET /users/{username}/following
GET /users/{username}/activity

### Movies
GET /movies/search?query=...
GET /movies/trending
GET /movies/genres
GET /movies/{movie_id}
GET /movies/{movie_id}/reviews

### User Movies (tracking)
POST /me/movies/{movie_id}/track
PUT /me/movies/{movie_id}/track
DELETE /me/movies/{movie_id}/track
GET /me/movies?status=...&genre=...&sort=...
GET /me/movies/watchlist
GET /me/movies/favorites

### Reviews
POST /movies/{movie_id}/reviews
PUT /reviews/{review_id}
DELETE /reviews/{review_id}
POST /reviews/{review_id}/like
DELETE /reviews/{review_id}/like

### Clubs
POST /clubs
GET /clubs
GET /clubs/{club_id}
PUT /clubs/{club_id}
DELETE /clubs/{club_id}
POST /clubs/{club_id}/join
DELETE /clubs/{club_id}/leave
POST /clubs/{club_id}/members/{user_id}/promote
DELETE /clubs/{club_id}/members/{user_id}
GET /clubs/{club_id}/members
POST /clubs/{club_id}/proposals
GET /clubs/{club_id}/proposals
POST /clubs/{club_id}/proposals/{proposal_id}/vote
POST /clubs/{club_id}/proposals/close
GET /clubs/{club_id}/history
GET /clubs/{club_id}/history/{watched_id}/comments
POST /clubs/{club_id}/history/{watched_id}/comments

### Feed
GET /feed?page=...&limit=...

### Stats
GET /stats/me
GET /stats/global

---

## 🗓️ Fases de desarrollo

El proyecto se divide en 4 fases. Cada fase debe completarse antes
de pasar a la siguiente. Al final de cada fase el código debe estar
en un estado funcional y sin errores.

### Fase A — Cimientos (Semana 1)
□ Setup del proyecto (estructura, entorno virtual, dependencias)
□ Docker Compose con PostgreSQL y Redis funcionando
□ Configuración de la aplicación (variables de entorno)
□ Conexión a base de datos con SQLAlchemy
□ Configuración de Alembic
□ Modelos: User, Movie, Genre
□ Auth completo: registro, login, JWT, refresh
□ Integración TMDB: búsqueda y detalle de películas
□ Caché de TMDB en Redis
□ Tests de auth y TMDB service

### Fase B — Core features (Semana 2)
□ Tracking de películas (vista, watchlist, favorita, rating)
□ Historial con filtros y paginación
□ Reviews: CRUD + likes + spoiler flag
□ Listas: CRUD + añadir/quitar/reordenar películas
□ Likes en listas + duplicar lista
□ Follow/Unfollow entre usuarios
□ Perfil público con actividad
□ Tests de todos los endpoints implementados


### Fase C — Social y analytics (Semana 3)
□ CineClubs: crear, unirse, gestionar miembros
□ Sistema de propuestas y votaciones
□ Historial del club + hilos de comentarios
□ Feed de actividad (registro de eventos + feed paginado)
□ Dashboard de estadísticas personales
□ Estadísticas globales
□ Trending movies (cacheado en Redis)
□ Tests de clubs, feed y stats


### Fase D — Frontend, deploy y cierre (Semana 4)
□ Frontend funcional conectado a la API
□ Páginas mínimas: login, búsqueda, detalle, perfil,
listas, club, dashboard
□ Dockerizar frontend
□ Despliegue completo (backend + frontend + BD)
□ Pipeline CI/CD con GitHub Actions (tests automáticos)
□ README final con instrucciones de instalación
□ Revisión de código, limpieza, formateo
□ Cobertura de tests ≥ 60%


---

## ✅ Criterios de aceptación

El proyecto se considerará completo cuando:

- [ ] La API responde correctamente a todos los endpoints documentados
- [ ] Un usuario puede registrarse, loguearse y realizar todas las
      operaciones descritas
- [ ] Las búsquedas de películas devuelven datos reales de TMDB
- [ ] Redis cachea las respuestas de TMDB correctamente
- [ ] La base de datos tiene migraciones y puede recrearse desde cero
- [ ] Los tests pasan y cubren al menos el 60% del código
- [ ] El proyecto se levanta con `docker-compose up`
- [ ] El proyecto está desplegado y accesible públicamente
- [ ] El frontend permite interactuar con las funcionalidades
      principales
- [ ] La documentación Swagger está completa y es navegable
- [ ] El código tiene type hints, está formateado y sin errores
      de linting
- [ ] El repositorio tiene commits descriptivos y organizados

---

## 📎 Notas adicionales

- La API de TMDB requiere registro gratuito para obtener una API key:
  https://www.themoviedb.org/documentation/api
- El rate limit de TMDB es de ~40 requests/segundo. El sistema de
  caché debe diseñarse teniendo esto en cuenta
- Los datos de TMDB están en inglés por defecto. Se puede pasar
  el parámetro `language=es-ES` para obtenerlos en español
- El proyecto debe tener un archivo `.env.example` documentando
  todas las variables necesarias pero sin valores reales
- Se recomienda usar conventional commits para los mensajes de git

---

## 🚀 Cómo empezar

1. Lee este documento completo
2. Crea el repositorio en GitHub
3. Regístrate en TMDB y obtén tu API key
4. Configura tu entorno de desarrollo local
5. Empieza por la Fase A

---

*Última actualización: Marzo 2026*

