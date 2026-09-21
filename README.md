# 📋 Kanboard Docker - Project Management Kanban Autohospedado

[![GitHub Stars](https://img.shields.io/github/stars/kanboard/kanboard?style=flat-square&logo=github)](https://github.com/kanboard/kanboard)
[![Docker Pulls](https://img.shields.io/docker/pulls/kanboard/kanboard?style=flat-square&logo=docker)](https://hub.docker.com/r/kanboard/kanboard)
[![License](https://img.shields.io/github/license/kanboard/kanboard?style=flat-square)](https://github.com/kanboard/kanboard/blob/master/LICENSE)
[![Version](https://img.shields.io/docker/v/kanboard/kanboard?style=flat-square&logo=docker)](https://hub.docker.com/r/kanboard/kanboard/tags)

## 📋 Descripción general

**Kanboard** es un software de gestión de proyectos completamente autohospedado y open source construido con PHP que proporciona Kanban boards intuitivos con drag-and-drop, swimlanes, múltiples proyectos, gestión de tareas completa con subtareas, comentarios, time tracking, custom fields, archivos adjuntos, plugins extensibles, integración LDAP/Active Directory, OAuth2 (Google, GitHub, GitLab), dashboard de análisis con métricas, activity logs completos, soporte multi-idioma (23 idiomas), lightweight y bajo consumo de recursos, múltiples bases de datos (MySQL, PostgreSQL, SQLite), super simple installation, MIT open source, 9.7k+ GitHub stars, usado por miles de equipos alrededor del mundo.

Esta implementación Docker permite desplegar Kanboard en minutos con persistencia de datos, configuración flexible de base de datos (SQLite por defecto, PostgreSQL/MySQL opcional) y listo para producción detrás de un reverse proxy.

## ✨ Características principales

- 🎯 **Kanban Boards** - Drag-drop tasks, columnas personalizadas, swimlanes, límites de tareas, color coding
- 📁 **Múltiples proyectos** - Gestión de múltiples proyectos, team-based, plantillas de proyecto, roles
- ✅ **Gestión de tareas completa** - Subtareas, comentarios, adjuntos, fechas límite, categorías, recurrencia
- ⏱️ **Time tracking** - Registro de horas por tarea, reporting de tiempo invertido, tracking de estimaciones
- 🔧 **Custom fields** - Campos personalizados (dropdown, texto, numérico), configuración por proyecto
- 📊 **Analytics** - Dashboard de análisis del proyecto, distribución de tareas, cumulative flow, reportes
- 👥 **Gestión de usuarios** - Múltiples usuarios, roles (admin, manager, member), acceso a nivel de proyecto
- 🔐 **LDAP/OAuth2** - Integración LDAP/Active Directory, OAuth2 (Google, GitHub, GitLab), soporte SSO
- 🔌 **Plugins extensibles** - Plugins comunitarios, desarrollo personalizado, sistema de hooks, extensiones
- 📝 **Activity logs** - Auditoría completa, historial de cambios, quién hizo qué y cuándo, exportación
- 🌍 **Multi-idioma** - 23 idiomas soportados, traducciones comunitarias, fácil localización
- 🔌 **REST API** - API REST completa, acceso programático, respuestas JSON, documentación
- ⚡ **Ultra-ligero** - Ideal para Raspberry Pi, NAS, VPS económicos. Bajo consumo CPU/RAM (50-150MB típicos)

## 📋 Requisitos del sistema

- **Docker & Docker Compose v2+**
- **RAM**: 256 MB - 1 GB mínimo (ultra-ligero)
- **Espacio en disco**: 500 MB - 20GB+ (según proyectos y adjuntos)
- **Puerto TCP**: 8080 (web UI, configurable)
- **Base de datos**: SQLite (embedded), MySQL 5.7+, o PostgreSQL 10+
- **PHP**: 7.4+ (incluido en la imagen Docker)
- **OS**: Linux, Windows, macOS (compatible)
- **Opcional**: Reverse proxy nginx/Caddy para HTTPS

> 💡 **Muy ligero**: Ideal para Raspberry Pi, NAS, VPS económicos. Bajo consumo CPU/RAM. Rápido.

## 🐳 Instalación

### Paso 1: Crear `docker-compose.yml` (simple con SQLite)

```yaml
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
```

### Paso 2: Iniciar Kanboard

```bash
# Levantar contenedores en background
docker compose up -d

# Espera ~5 segundos para que inicie
# Ver logs en tiempo real
docker compose logs -f
```

### Paso 3: Acceder a Kanboard

```bash
# Abre en navegador:
http://localhost:8080

# Desde otro dispositivo en la red:
http://192.168.1.100:8080

# Credenciales por defecto: admin/admin (¡cambiar inmediatamente!)
```

**Endpoints disponibles:**
- 📊 **Kanboard Web UI**: `http://localhost:8080`
- 🔗 **REST API**: `http://localhost:8080/api/v1`

## ⚙️ Configuración

1. **Zona horaria** - Modifica `TIMEZONE` en docker-compose.yml (ej: `Europe/Madrid`, `America/Mexico_City`)
2. **Puerto externo** - Cambia `"8080:80"` por `"PUERTO_DESEADO:80"` si el 8080 está ocupado
3. **Persistencia de datos** - Los volúmenes `./data` y `./plugins` mantienen datos y plugins entre reinicios
4. **Base de datos** - Por defecto usa SQLite en `./data`. Ver sección avanzada para PostgreSQL/MySQL
5. **Debug** - Cambia `DEBUG=true` solo para troubleshooting
6. **Plugins** - Descarga plugins de kanboard.org/plugins, extrae en `./plugins/nombre-plugin`, habilita en Admin → Plugins

## 🚀 Primeros pasos

1. Abre `http://localhost:8080` en tu navegador
2. Login con **admin/admin** (credenciales por defecto)
3. El dashboard principal aparece vacío
4. Crea un **nuevo proyecto** → Selecciona template (básico o completo)
5. Agrega **swimlanes y columnas personalizadas** según tu workflow
6. Empieza a **crear tareas (cards)** en las columnas
7. **Drag-drop** tasks para mover entre columnas
8. Click en task para **agregar detalles, comentarios, tiempo**

⚠️ **Cambiar contraseña por defecto**: Primera cosa → Profile → Settings → cambiar admin password.

💡 **Plugins**: Descarga plugins comunitarios desde kanboard.org/plugins. Agrega Gantt, custom fields, integraciones Slack, etc.

## 💡 Casos de uso

- **Team project management**: Equipos pequeños-medianos, Kanban workflow, sprint planning
- **Agile development**: Software teams, Kanban boards, time tracking, velocity tracking
- **Task management**: Proyectos personales, colaboración en equipo, múltiples proyectos
- **Workflow automation**: Workflows personalizados, routing de tareas, reglas de automatización
- **Portfolio management**: Múltiples proyectos, resource tracking, analytics consolidados
- **Replace expensive tools**: Alternativa gratuita a Trello/Jira, self-hosted, sin vendor lock-in
- **Self-hosted solution**: Privacy-first, control total de datos, personalizable

## 🔒 Acceso remoto seguro

Para exponer Kanboard de forma segura a internet:

1. **Reverse Proxy** (nginx/Caddy/Traefik) con terminación SSL
2. **Autenticación adicional**: Authelia, Authentik, o Cloudflare Access
3. **VPN**: WireGuard, Tailscale, o OpenVPN para acceso solo a red privada
4. **Firewall**: Restringe puerto 8080 solo a IPs de confianza si no usas proxy

Ejemplo básico con Caddy (HTTPS automático):
```caddy
kanboard.tudominio.com {
    reverse_proxy kanboard:80
}
```

## 🛠️ Gestión y mantenimiento

| Acción | Comando |
|--------|---------|
| **Ver estado** | `docker compose ps` |
| **Ver logs** | `docker compose logs -f kanboard` |
| **Detener** | `docker compose down` |
| **Actualizar versión** | `docker compose pull && docker compose up -d` |
| **Backup datos** | `tar -czf kanboard_backup_$(date +%Y%m%d).tar.gz data/` |
| **Backup completo (con BD)** | `docker compose exec kanboard tar -czf /var/www/html/data/backup.tar.gz /var/www/html/data` |
| **Monitorear consumo** | `docker stats kanboard` |

**Consumo típico:**
- CPU: Muy bajo (picos al cargar dashboards)
- RAM: 50-150MB
- Disco: Según proyectos y adjuntos

## 📝 Licencia

**MIT License** - Software libre y open source. Ver [LICENSE](https://github.com/kanboard/kanboard/blob/master/LICENSE) en el repositorio oficial.

---

> 📖 **Guía completa y vídeo tutorial**: [Cómo instalar Kanboard en Docker - Project Management Kanban autohospedado](https://genbyte.blogspot.com/2026/09/como-instalar-kanboard-en-docker.html)