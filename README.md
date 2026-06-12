# Sistema de Monitorización e Infraestructura como Código (IaC)

Este repositorio contiene la configuración completa para el despliegue automatizado de una infraestructura de monitorización basada en **Zabbix 7.0 LTS**, **MariaDB** y **Grafana**, totalmente contenedorizada mediante **Docker Compose**. 

El diseño está orientado a entornos de producción, implementando estrategias de persistencia de datos, aislamiento de red y automatización de respuesta ante incidentes, además de mecanismos de recuperación ante desastres mediante la inyección automática de respaldos.

## Arquitectura de Servicios

El archivo `docker-compose.yml` orquesta cinco servicios interconectados en una red aislada denominada `zabbix-net`:

1. **mariadb-server (`zabbix-db`):** Motor de base de datos relacional (versión 10.11). Automatiza la restauración del esquema relacional y datos históricos al arrancar mediante la lectura del script alojado en `/docker-entrypoint-initdb.d/`.
2. **zabbix-server (`zabbix-server`):** Núcleo central del sistema de monitorización, encargado del procesamiento de datos y evaluación de *triggers*.
3. **zabbix-web (`zabbix-frontend`):** Interfaz gráfica de usuario basada en Nginx y PHP, publicada en el puerto `8080`.
4. **grafana (`grafana`):** Plataforma avanzada de analítica y visualización, con preinstalación automatizada del plugin de Zabbix y mapeo de volumen local para mantener la persistencia de los *dashboards*.
5. **zabbix-agent (`zabbix-agent-central`):** Agente de monitorización local configurado en modo `host` y modo `root` para la supervisión directa del nodo anfitrión.
