# 📡 Despliegue de LibreNMS con Docker Compose

Este repositorio contiene una configuración completa de `docker-compose` para desplegar [LibreNMS](https://www.librenms.org/), un sistema de monitoreo de red gratuito, de código abierto y altamente extensible.

## 📦 ¿Qué es LibreNMS?

**LibreNMS** es una plataforma de monitoreo de red que ofrece:

- Descubrimiento automático de dispositivos vía SNMP
- Alertas personalizables
- Visualización de rendimiento y tráfico en tiempo real
- Integraciones con herramientas de terceros como Slack, Telegram, Prometheus, Grafana, entre otros
- API RESTful y soporte para múltiples usuarios

---

## 🗂️ Estructura del Proyecto

```

librenms/
├── compose.yml         # Archivo principal de docker-compose
├── librenms.env        # Variables de entorno para LibreNMS
├── msmtpd.env          # Configuración para el servidor de correo
└── librenms/
├── .env            # Variables opcionales adicionales
└── logs/
└── librenms.log

````

---

## 🚀 Instrucciones de Despliegue

### 1. Requisitos previos

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/)
- Archivo `.env` con las siguientes variables:

```env
TZ=America/Bogota
PUID=1000
PGID=1000

MYSQL_DATABASE=librenms
MYSQL_USER=librenms
MYSQL_PASSWORD=librenms
MARIADB_ROOT_PASSWORD=librenms
````

Guárdalo en el mismo directorio del `compose.yml` o personalízalo según tu entorno.

### 2. Levantar los contenedores

```bash
docker compose up -d
```

Accede a la interfaz web en: [http://localhost:8000](http://localhost:8000)

---

## ⚙️ Servicios incluidos

| Servicio     | Descripción                                                  |
| ------------ | ------------------------------------------------------------ |
| `db`         | Base de datos MariaDB (almacena la configuración y métricas) |
| `redis`      | Cache para mejorar el rendimiento del sistema                |
| `msmtpd`     | Envío de correos salientes (alertas, notificaciones)         |
| `librenms`   | Aplicación principal con interfaz web                        |
| `dispatcher` | Proceso en segundo plano para manejar polling/alertas        |
| `syslogng`   | Receptor de logs remotos vía Syslog                          |
| `snmptrapd`  | Receptor de traps SNMP (alertas SNMP desde dispositivos)     |

---

## 🛠️ Comandos útiles

| Comando                             | Acción                                 |
| ----------------------------------- | -------------------------------------- |
| `docker compose ps`                 | Verifica el estado de los contenedores |
| `docker compose logs -f`            | Ver logs en tiempo real                |
| `docker compose down`               | Detiene y elimina los contenedores     |
| `docker compose exec librenms bash` | Accede al contenedor LibreNMS          |

---

## 📚 Monitoreo de equipos de red, servidores y maquinas virtuales

Para adjuntar un equipo al monitoreo hecho por LibreNMS, es neccesario que el dispositivo tenga activado el protocolo SNMP 
y configurada una comunidad de lectura. Esto con el fin de poder configurar desde la interfaz de LibreNMS el seguimiento a dicho
dispositivo especificando la version del SNMP, la IP del dispositivo, puerto activo para SNMP y la comunidad de lectura (O también de lectoescritura).

La configuracion de activacion de SNMP varia del SO del equipo, especificaciones y fabricante. Consultar la documentacion oficial del fabricante o
del SO para la activacion de SNMP en cada dispositivo.

---

## 📚 Documentación oficial

* [Documentación de instalación con Docker](https://docs.librenms.org/Installation/Install-LibreNMS-Using-Docker/)
* [Guía de uso general](https://docs.librenms.org/)

---

## 📌 Notas adicionales

* Asegúrate de abrir los puertos necesarios (`8000`, `514`, `162`) en tu firewall si accedes desde otra máquina.
* Los datos de LibreNMS y la base de datos se almacenan en volúmenes persistentes locales.
* Recuerda completar la configuración inicial desde la interfaz web al primer ingreso.

---

## 🧾 Licencia

LibreNMS es software libre bajo licencia GPLv3. Este repositorio de despliegue es libre bajo licencia MIT.
