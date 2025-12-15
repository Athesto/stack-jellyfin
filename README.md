# Jellyfin en Raspberry Pi (Docker)

Este repositorio contiene el stack Docker para ejecutar **Jellyfin** en una
**Raspberry Pi 3**, usando Docker Compose y variables de entorno (`.env`).

El objetivo es mantener:
- el stack ordenado
- los datos separados
- el rendimiento lo más estable posible para hardware limitado

---

## 🧱 Estructura del proyecto

/opt  
└── stacks/  
    └── jellyfin/  
        ├── docker-compose.yml  
        ├── .env  
        └── .env.example  

/srv  
└── stacks/  
    └── jellyfin/  
        ├── config/  
        ├── cache/  
        └── lib/

- `/opt/stacks/jellyfin` → infraestructura (compose, env)
- `/srv/stacks/jellyfin` → datos persistentes
- El contenido multimedia vive en un USB montado

---

## ⚙️ Requisitos

- Raspberry Pi 3
- Raspberry Pi OS
- Docker y Docker Compose
- Almacenamiento USB montado (recomendado)
- Conexión por Ethernet

---

## 📦 Variables de entorno

Las variables se definen en el archivo `.env`.

Ejemplo (`.env.example`):

TZ=Country/City [list](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)

JELLYFIN_CONFIG=/srv/jellyfin/config  
JELLYFIN_DATA=/srv/jellyfin/lib  
JELLYFIN_CACHE=/srv/jellyfin/cache  

MOVIES_DIR=/media/usb/Movies  
SHOWS_DIR=/media/usb/Shows  

JELLYFIN_PUBLISHED_URL=http://localhost:8096  

PUID=1000  
PGID=1000  

⚠️ El archivo `.env` **no debe subirse** al repositorio.

---

## 🚀 Puesta en marcha

Desde la carpeta del stack:

cd /opt/stacks/stack-jellyfin  
docker compose up -d  

Verifica que el `.env` se esté cargando correctamente:

docker compose config  

---

## 🌐 Acceso

- Interfaz web:  
  http://raspberrypi.local:8096  
  http://<IP_DE_LA_PI>:8096  

---

## 📁 Volúmenes importantes

Ruta host | Ruta contenedor | Ruta nativa | Uso  
--------- | ---------------- | --- | ----  
JELLYFIN_CONFIG | /etc/jellyfin | /etc/jellyfin | Configuración  
JELLYFIN_DATA | /var/lib/jellyfin | /var/lib/jellyfin | Base de datos  
JELLYFIN_CACHE | /cache | /var/cache/jellyfin |  Cache / thumbnails  
MOVIES_DIR | /media/Movies | N/A | Películas  
SHOWS_DIR | /media/Shows | N/A | Series  

---

## ⚠️ Notas importantes (Raspberry Pi 3)

Este stack está optimizado para limitaciones reales del hardware:

- No usar transcodificación
- No ejecutar muchos contenedores a la vez
- Usar Direct Play

---

## 🧠 Rendimiento y estabilidad

Recomendaciones adicionales:
- Limitar logs de Docker
- Montar el USB de forma persistente (`/etc/fstab`)
- Usar solo 1–2 usuarios concurrentes

---

## 🔁 Detener / reiniciar

docker compose stop  
docker compose start  
docker compose restart  

---

## 🧹 Logs

docker compose logs -f jellyfin  

---

## 📌 Filosofía del stack

/opt/stacks/jellyfin es infraestructura  
/srv/stacks/jellyfin son datos  

---

## 🚚 Migración

/srv/stack/jellyfin:
config          <- /etc/jellyfin
lib             <- /var/lib/jellyfin
cache           <- /var/cache/jellyfin

```bash
sudo cp -r /etc/jellyfin/ config
sudo cp -r /var/lib/jellyfin lib
sudo cp -r /var/cache/jellyfin cache
sudo chown -R 1000:1000 config lib cache

docker compose up -d && docker compose logs -f
```

---

## 📝 Licencia

Uso personal / educativo.
