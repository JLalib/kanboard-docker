# 📋 Kanboard Docker - Project Management Kanban Autohospedado

[![GitHub](https://img.shields.io/badge/GitHub-kanboard%2Fkanboard-blue?logo=github)](https://github.com/kanboard/kanboard)
[![Docker](https://img.shields.io/badge/Docker-kanboard%2Fkanboard-blue?logo=docker)](https://hub.docker.com/r/kanboard/kanboard)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)

## 📋 Descripción general

**Kanboard** es un software de gestión de proyectos completamente autohospedado y open source construido con PHP que proporciona Kanban boards intuitivos con drag-and-drop, swimlanes, múltiples proyectos, gestión de tareas completa con subtareas, comentarios, time tracking, custom fields, archivos adjuntos, plugins extensibles, integración LDAP/Active Directory, OAuth2 (Google, GitHub, GitLab), dashboard de análisis con métricas, activity logs completos, soporte multi-idioma (23 idiomas), lightweight y bajo consumo de recursos, múltiples bases de datos (MySQL, PostgreSQL, SQLite), super simple installation, MIT open source, 9.7k+ GitHub stars, usado por miles de equipos alrededor del mundo.

Este repositorio proporciona una configuración Docker Compose lista para producción para desplegar Kanboard en minutos.

## ✨ Características principales

- **Kanban boards** con drag-and-drop, columnas personalizadas, swimlanes, límites de tareas, codificación por colores
- **Múltiples proyectos** con gestión por equipos, plantillas de proyecto, roles
- **Gestión de tareas completa**: subtareas, comentarios, adjuntos, fechas límite, categorías, recurrencia
- **Time tracking**: registro de horas por tarea, reportes de tiempo invertido, seguimiento de estimaciones
- **Custom fields**: campos personalizados (dropdown, texto, numérico) configurables por proyecto
- **Analytics**: analíticas de proyecto, distribución de tareas, diagrama de flujo acumulativo, reportes
- **Gestión de usuarios**: múltiples usuarios, roles (admin, manager, member), acceso a nivel de proyecto
- **LDAP/OAuth2**: integración LDAP/Active Directory, OAuth2 (Google, GitHub, GitLab), soporte SSO
- **Plugins extensibles**: plugins comunitarios, desarrollo personalizado, sistema de hooks, extensiones
- **Activity logs**: auditoría completa, historial de cambios, quién hizo qué y cuándo, exportación
- **Multi-idioma**: 23 idiomas soportados, traducciones comunitarias, fácil localización
- **API REST completa**: acceso programático, respuestas JSON, documentación
- **Ultra-ligero**: ideal para Raspberry Pi, NAS, VPS económicos (50-150MB RAM típicos)

## 📋 Requisitos del sistema

- Docker & Docker Compose v2+
- 256 MB - 1 GB RAM mínimo (ultra-ligero)
- 500 MB - 20GB+ espacio disco (según proyectos)
- Puerto TCP: 8080 (web UI, configurable)
- Base de datos: SQLite (embedded), MySQL 5.7+, o PostgreSQL 10+
- PHP: 7.4+ (incluido en Docker image)
- OS: Linux, Windows, macOS (compatible)
- Opcional: Reverse proxy nginx/Caddy para HTTPS

## 🐳 Instalación

### Paso 1: docker-compose.yml (simple con SQLite)

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
docker compose up -d
# Espera ~5 segundos para que inicie
docker compose logs -f
```

### Paso 3: Acceder a Kanboard

```bash
# Abre en navegador:
http://localhost:8080

# Desde otro dispositivo:
http://192.168.1.100:8080

# Default credentials: admin/admin (cambiar!)
```

**📊 Kanboard Web UI:** `http://localhost:8080`  
**🔗 API REST:** `http://localhost:8080/api/v1`

## ⚙️ Configuración

1. **Zona horaria**: Modifica `TIMEZONE` en docker-compose.yml (ej: `Europe/Madrid`, `America/Mexico_City`)
2. **Puerto**: Cambia `"8080:80"` por el puerto deseado (ej: `"80:80"` para puerto estándar)
3. **Base de datos**: Por defecto usa SQLite en `./data`. Para PostgreSQL/MySQL ver sección avanzada
4. **Persistencia**: Los datos se guardan en `./data` y plugins en `./plugins` (carpetas locales)
5. **Debug**: Cambia `DEBUG=true` para desarrollo/troubleshooting

## 🚀 Primeros pasos

1. Abre `http://localhost:8080` en tu navegador
2. Login con `admin/admin` (credenciales por defecto)
3. El dashboard principal aparece vacío
4. Crea un nuevo proyecto → Selecciona template (básico o completo)
5. Agrega swimlanes y columnas personalizadas
6. Empieza creando tareas (cards) en las columnas
7. Drag-and-drop para mover tareas entre columnas
8. Click en tarea para agregar detalles, comentarios, tiempo

⚠️ **Cambiar contraseña por defecto**: Primera cosa → Profile → Settings → cambiar admin password

💡 **Plugins**: Descarga plugins comunitarios desde [kanboard.org/plugins](https://kanboard.org/plugins). Agrega Gantt, custom fields, integraciones.

## 💡 Casos de uso

- **Team project management**: Equipos pequeños-medianos, Kanban workflow, sprint planning
- **Agile development**: Software teams, Kanban boards, time tracking, velocity
- **Task management**: Proyectos personales, colaboración en equipo, múltiples proyectos
- **Workflow automation**: Workflows personalizados, enrutamiento de tareas, reglas de automatización
- **Portfolio management**: Múltiples proyectos, seguimiento de recursos, analíticas
- **Replace expensive tools**: Alternativa gratuita a Trello/Jira, self-hosted, sin vendor lock-in
- **Self-hosted solution**: Privacy-first, control total de datos, personalizable

## 🔒 Acceso remoto seguro

Para exponer Kanboard de forma segura a internet:

1. **Reverse Proxy** (nginx/Caddy/Traefik) con terminación SSL
2. **Authelia/Keycloak** para autenticación adicional (2FA, SSO)
3. **Tailscale/ZeroTier** para acceso VPN sin exponer puertos
4. **Cloudflare Tunnel** para acceso sin abrir puertos en router

Ejemplo con Caddy (HTTPS automático):
```caddy
kanboard.tudominio.com {
    reverse_proxy kanboard:80
}
```

## 🛠️ Gestión y mantenimiento

```bash
# Ver estado
docker compose ps

# Ver logs
docker compose logs -f kanboard

# Detener Kanboard
docker compose down

# Actualizar versión
docker compose pull
docker compose up -d
# Datos persistentes se mantienen en ./data

# Backup de datos
docker compose exec kanboard tar -czf /var/www/html/data/backup.tar.gz /var/www/html/data
# O simplemente backup la carpeta:
tar -czf kanboard_backup_$(date +%Y%m%d).tar.gz data/

# Monitorear consumo
docker stats kanboard
# Típicamente:
# CPU: bajo
# RAM: 50-150MB
# Pico: bajo carga
```

## 📝 Licencia

MIT License - Ver [LICENSE](https://github.com/kanboard/kanboard/blob/master/LICENSE) en el repositorio oficial de Kanboard.

---

> 📖 **Guía completa y vídeo tutorial**: [Cómo instalar Kanboard en Docker - Project Management Kanban autohospedado](https://genbyte.blogspot.com/2026/09/como-instalar-kanboard-en-docker.html)