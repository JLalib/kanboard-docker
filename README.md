# 📋 Kanboard Docker - Project Management Kanban Autohospedado

[![GitHub Stars](https://img.shields.io/github/stars/kanboard/kanboard?style=flat-square&logo=github)](https://github.com/kanboard/kanboard)
[![Docker Pulls](https://img.shields.io/docker/pulls/kanboard/kanboard?style=flat-square&logo=docker)](https://hub.docker.com/r/kanboard/kanboard)
[![License](https://img.shields.io/github/license/kanboard/kanboard?style=flat-square)](https://github.com/kanboard/kanboard/blob/master/LICENSE)
[![Version](https://img.shields.io/docker/v/kanboard/kanboard?style=flat-square&logo=docker)](https://hub.docker.com/r/kanboard/kanboard/tags)

## 📋 Descripción general

**Kanboard** es un software de gestión de proyectos completamente autohospedado y open source construido con PHP que proporciona Kanban boards intuitivos con drag-and-drop, swimlanes, múltiples proyectos, gestión de tareas completa con subtareas, comentarios, time tracking, custom fields, archivos adjuntos, plugins extensibles, integración LDAP/Active Directory, OAuth2 (Google, GitHub, GitLab), dashboard de análisis con métricas, activity logs completos, soporte multi-idioma (23 idiomas), lightweight y bajo consumo de recursos, múltiples bases de datos (MySQL, PostgreSQL, SQLite), super simple installation, MIT open source, 9.7k+ GitHub stars, usado por miles de equipos alrededor del mundo.

Este repositorio proporciona una configuración Docker Compose lista para producción para desplegar Kanboard en minutos, ideal para homelabs, equipos pequeños-medianos, y entornos self-hosted que buscan una alternativa ligera a Trello/Jira sin vendor lock-in.

## ✨ Características principales

- 🎯 **Kanban Boards** - Drag-drop tasks, custom columns, swimlanes, task limits, color coding
- 📁 **Múltiples proyectos** - Gestión de proyectos ilimitados, team-based, project templates, roles
- ✅ **Gestión de tareas completa** - Subtasks, comments, attachments, due dates, categories, recurrence
- ⏱️ **Time tracking** - Log hours per task, spent time reporting, estimation tracking
- 🔧 **Custom fields** - Add custom fields (dropdown, text, numeric), per-project config
- 📊 **Analytics** - Project analytics, task distribution, cumulative flow, reports
- 👥 **User management** - Multiple users, roles (admin, manager, member), project-level access
- 🔐 **LDAP/OAuth2** - LDAP integration, OAuth2 (Google, GitHub, GitLab), SSO support
- 🔌 **Plugins extensibles** - Community plugins, custom development, hook system, extensions
- 📝 **Activity logs** - Complete audit trail, change history, who did what when, export
- 🌍 **Multi-language** - 23 languages supported, community translations, easy localization
- 🔌 **REST API** - Full REST API, programmatic access, JSON responses, documentation
- ⚡ **Ultra-ligero** - 256MB-1GB RAM, ideal para Raspberry Pi, NAS, VPS económicos
- 🗄️ **Multi-database** - SQLite (embedded), MySQL 5.7+, PostgreSQL 10+
- 📜 **MIT License** - Open source, sin vendor lock-in, 9.7k+ stars

## 📋 Requisitos del sistema

- **Docker & Docker Compose v2+**
- **RAM**: 256 MB - 1 GB mínimo (ultra-ligero)
- **Disco**: 500 MB - 20GB+ espacio (según proyectos y adjuntos)
- **Puerto TCP**: 8080 (web UI, configurable)
- **Base de datos**: SQLite (embedded), MySQL 5.7+, o PostgreSQL 10+
- **PHP**: 7.4+ (incluido en Docker image)
- **OS**: Linux, Windows, macOS (compatible)
- **Opcional**: Reverse proxy nginx/Caddy para HTTPS

> 💡 **Muy ligero**: Ideal para Raspberry Pi, NAS, VPS económicos. Bajo consumo CPU/RAM. Rápido.

## 🐳 Instalación

### Opción A: SQLite (Simple, recomendado para inicio rápido)

```bash
# 1. Clonar o crear directorio
mkdir kanboard && cd kanboard

# 2. Crear docker-compose.yml
cat > docker-compose.yml << 'EOF'
version: '3.8'

services:
  kanboard:
    image: kanboard/kanboard:latest
    container_name: kanboard
    restart: unless-stopped
    ports:
      - "8080:80"
    volumes:
      - ./data:/var/www/html/data
      - ./plugins:/var/www/html/plugins
    environment:
      - DEBUG=false
      - TIMEZONE=America/Mexico_City
    # SQLite database incrustado en ./data
EOF

# 3. Iniciar Kanboard
docker compose up -d

# 4. Verificar logs
docker compose logs -f
```

### Opción B: PostgreSQL (Producción, multi-usuario)

```bash
# 1. Crear docker-compose.yml con PostgreSQL
cat > docker-compose.yml << 'EOF'
version: '3.8'

services:
  kanboard:
    image: kanboard/kanboard:latest
    container_name: kanboard
    restart: unless-stopped
    ports:
      - "8080:80"
    volumes:
      - ./data:/var/www/html/data
      - ./plugins:/var/www/html/plugins
    environment:
      - DATABASE_URL=postgres://kanboard:password@db:5432/kanboard
      - DEBUG=false
      - TIMEZONE=America/Mexico_City
    depends_on:
      - db

  db:
    image: postgres:15
    container_name: kanboard-db
    restart: unless-stopped
    environment:
      - POSTGRES_DB=kanboard
      - POSTGRES_USER=kanboard
      - POSTGRES_PASSWORD=password
    volumes:
      - db_data:/var/lib/postgresql/data

volumes:
  db_data:
EOF

# 2. Iniciar stack completo
docker compose up -d
```

## ⚙️ Configuración

1. **Variables de entorno principales**:
   - `DATABASE_URL` - Conexión BD (sqlite por defecto, postgres:// para PostgreSQL)
   - `DEBUG` - `true`/`false` (default: false)
   - `TIMEZONE` - Zona horaria (ej: `America/Mexico_City`, `Europe/Madrid`)
   - `PLUGIN_INSTALLER` - `true` para habilitar instalador de plugins vía UI

2. **Volúmenes persistentes**:
   - `./data` → `/var/www/html/data` (BD SQLite, adjuntos, avatares, config)
   - `./plugins` → `/var/www/html/plugins` (plugins comunitarios)

3. **Puertos**: `8080:80` (cambiar `8080` si hay conflicto)

4. **Base de datos**: SQLite por defecto (archivo en `./data/db.sqlite`). Para PostgreSQL/MySQL ver sección avanzada.

5. **Timezone**: Ajustar `TIMEZONE` a tu zona horaria para logs y fechas correctas.

## 🚀 Primeros pasos

1. **Abrir navegador**: `http://localhost:8080` (o `http://IP_DEL_SERVIDOR:8080`)
2. **Login inicial**: Usuario `admin` / Contraseña `admin`
3. **⚠️ CAMBIAR CONTRASEÑA INMEDIATAMENTE**: Profile → Settings → Change password
4. **Crear primer proyecto**: Dashboard → "New project" → Seleccionar template (Basic/Complete)
5. **Configurar columnas y swimlanes**: Project settings → Columns / Swimlanes
6. **Crear tareas**: Click en "+" en cualquier columna → Completar detalles
7. **Drag & drop**: Arrastrar tareas entre columnas para cambiar estado
8. **Explorar plugins**: Admin → Plugins → Install new plugins (desde kanboard.org/plugins)

## 💡 Casos de uso

- 👥 **Team project management** - Equipos pequeños-medianos, Kanban workflow, sprint planning
- 💻 **Agile development** - Software teams, Kanban boards, time tracking, velocity tracking
- ✅ **Task management** - Proyectos personales, colaboración en equipo, múltiples proyectos
- ⚙️ **Workflow automation** - Custom workflows, task routing, automation rules via plugins
- 📊 **Portfolio management** - Múltiples proyectos, resource tracking, analytics cross-project
- 💰 **Replace expensive tools** - Alternativa gratuita a Trello/Jira, self-hosted, no vendor lock-in
- 🔒 **Self-hosted solution** - Privacy-first, complete data control, fully customizable

## 🔒 Acceso remoto seguro

Para exponer Kanboard de forma segura a Internet:

```yaml
# docker-compose.yml con Caddy reverse proxy (HTTPS automático)
services:
  caddy:
    image: caddy:2
    container_name: caddy
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile
      - caddy_data:/data
      - caddy_config:/config
    depends_on:
      - kanboard

  kanboard:
    # ... configuración kanboard (sin ports expuestos)
    expose:
      - "80"

volumes:
  caddy_data:
  caddy_config:
```

```text
# Caddyfile
kanboard.tudominio.com {
    reverse_proxy kanboard:80
}
```

> **Alternativa**: Nginx Proxy Manager, Traefik, o Cloudflare Tunnel para zero-trust access.

## 🛠️ Gestión y mantenimiento

| Acción | Comando |
|--------|---------|
| **Ver estado** | `docker compose ps` |
| **Ver logs** | `docker compose logs -f kanboard` |
| **Detener** | `docker compose down` |
| **Actualizar** | `docker compose pull && docker compose up -d` |
| **Backup datos** | `tar -czf kanboard_backup_$(date +%Y%m%d).tar.gz data/` |
| **Backup BD (PostgreSQL)** | `docker compose exec db pg_dump -U kanboard kanboard > backup.sql` |
| **Restaurar backup** | `tar -xzf kanboard_backup_YYYYMMDD.tar.gz` |
| **Monitorear recursos** | `docker stats kanboard` |

**Consumo típico**:
- CPU: Muy bajo (picos al cargar dashboards)
- RAM: 50-150 MB
- Disco: Creciente según adjuntos y proyectos

## 📝 Licencia

**MIT License** - Kanboard es open source bajo licencia MIT.
- ✅ Uso comercial permitido
- ✅ Modificación permitida
- ✅ Distribución permitida
- ✅ Uso privado permitido
- ❌ Sin garantía
- ❌ Sin liability

Ver [LICENSE](https://github.com/kanboard/kanboard/blob/master/LICENSE) en el repositorio oficial.

---

> 📖 **Artículo original**: [Cómo instalar Kanboard en Docker - Project Management Kanban autohospedado](https://genbyte.blogspot.com/2026/09/como-instalar-kanboard-en-docker.html)
> 🎥 **Video tutorial**: [Canal GENBYTE en YouTube](https://www.youtube.com/@genbyte)
> ⭐ **Repo oficial**: [kanboard/kanboard](https://github.com/kanboard/kanboard)