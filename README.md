# INFRAESTRUCTURA PLATAFORMAS TECNOLGICAS Y REDES
# Proyecto Final SIS313: Plataforma de Mensajería Instantánea Segura para la Comunidad Universitaria

**Asignatura:** SIS313: Infraestructura, Plataformas Tecnológicas y Redes  
**Semestre:** 2/2025  
**Docente:** Ing. Marcelo Quispe Ortega  

---

## 👥 Miembros del Equipo

| Nombre Completo       | Rol en el Proyecto                  | Contacto (GitHub/Email)      |
|----------------------|------------------------------------|------------------------------|
| Joel Salinas         | Administrador de Sistemas          | github.com/joelsalinas       |


---

## 🎯 I. Objetivo del Proyecto

**Objetivo:**  
Desplegar una solución de mensajería empresarial (Mattermost) que soporte mensajes privados, grupos, envío de archivos, escalable y con acceso autenticado para estudiantes y docentes de la universidad.

---

## 💡 II. Justificación e Importancia

**Justificación:**  
El proyecto proporciona un canal de comunicación en tiempo real, privado y controlado por la universidad, garantizando la seguridad y privacidad de la información frente a plataformas externas como WhatsApp o Telegram. Además, integra servicios web, bases de datos y optimización de networking, asegurando baja latencia y alta disponibilidad para los usuarios.

---

## 🛠️ III. Tecnologías y Conceptos Implementados

### 3.1 Tecnologías Clave

| Tecnología          | Función Específica |
|-------------------|------------------|
| **Mattermost**     | Plataforma de mensajería, control de usuarios, canales privados y grupos, envío de archivos. |
| **NGINX (Proxy Inverso)** | Manejo de WebSockets, HTTPS, balanceo y seguridad de la comunicación entre app y usuarios. |
| **PostgreSQL**     | Base de datos robusta y escalable para almacenamiento de mensajes y datos de usuario. |
| **Redes Host-Only e Internas** | Aislamiento de servidores, comunicación interna segura y acceso desde la máquina anfitriona. |
| **Opcional: Prometheus** | Monitoreo de métricas básicas como CPU, RAM y conexiones concurrentes. |

### 3.2 Conceptos de la Asignatura Puestos en Práctica

| Tema (T) / Laboratorio               | Implementación en el Proyecto |
|-------------------------------------|-------------------------------|
| **T2: Infraestructura de Hardware**<br>Laboratorio 2.1 y 2.2 | Separación de servicios en VMs independientes (app, db, proxy) para escalabilidad; preparación para réplicas de base de datos y failover futuro. |
| **T3: Infraestructura de Networking**<br>Laboratorio 3.1 y 3.2 | Configuración de NGINX como proxy inverso con balanceo de carga y soporte de WebSockets; diseño de redes internas y host-only para comunicación segura y eficiente entre servidores. |
| **T4: Infraestructura de Networking / Servicios Web**<br>Laboratorio 4.1 | Implementación de servicios web con NGINX; soporte de WebSockets para mensajería en tiempo real; (opcional) monitoreo de métricas y tráfico para optimización. |


---

## 🌐 IV. Diseño de la Infraestructura y Topología

### 4.1 Diseño Esquemático

            ┌───────────────────────┐
            │      Estudiantes      │
            │      Docentes         │
            └──────────┬────────────┘
                       │
                (192.168.56.50)
                       │
                ┌────────────┐
                │   PROXY    │ Nginx + WebSockets
                │ mm-proxy   │
                └─────┬──────┘
            ┌─────────┼───────────┐
            │                     │
    (192.168.50.10)        (192.168.50.11) 
    ┌──────────────┐        ┌─────────────┐
    │  mm-app      │        │   mm-db     │
    │  Mattermost  │        │ PostgreSQL  │
    └──────────────┘        └─────────────┘


### 4.2 Estrategia Adoptada

- **Separación de VMs:** Proxy, app y base de datos en máquinas independientes para escalabilidad y seguridad.  
- **Proxy Inverso NGINX:** Maneja tráfico HTTPS y WebSockets, protegiendo el backend.  
- **Redes:** Red interna para comunicación entre servidores, host-only para acceso seguro desde PC, NAT para actualizaciones.  

---

## 📋 V. Guía de Implementación y Puesta en Marcha

### 5.1 Pre-requisitos
- 3 VMs (Ubuntu/Debian) con acceso root/sudo.  
- Repositorio clonado en cada VM.  

### 5.2 Despliegue
1. Configurar IPs fijas para cada VM.  
2. Instalar NGINX en `mm-proxy`.  
3. Configurar Mattermost en `mm-app` y conectarlo a `mm-db`.  
4. Asegurar conexiones con SSL/TLS y WebSockets.  
5. Probar acceso desde la máquina anfitriona (host-only).  

### 5.3 Ficheros de Configuración Clave
- `/etc/nginx/sites-available/mattermost` → Configuración de proxy y WebSockets.  
- `/opt/mattermost/config/config.json` → Configuración de Mattermost.  
- `/var/lib/postgresql/data` → Base de datos PostgreSQL.  

---

## ⚠️ VI. Pruebas y Validación

| Prueba                        | Resultado Esperado                                         | Resultado Obtenido |
|--------------------------------|------------------------------------------------------------|------------------|
| Acceso de usuarios            | Login exitoso de estudiantes y docentes                  | ✅ Correcto       |
| Mensajería privada y grupos    | Envío y recepción de mensajes correctamente              | ✅ Correcto       |
| Envío de archivos              | Archivos enviados y recibidos sin errores                | ✅ Correcto       |
| Escalabilidad básica (T2)      | Proxy y app separados, DB conectada                       | ✅ Correcto       |
| Seguridad (HTTPS, firewall)    | Todo el tráfico HTTP redirigido a HTTPS, puertos bloqueados | ✅ Correcto       |

---

## 📚 VII. Conclusiones y Lecciones Aprendidas

- La plataforma cumple con el objetivo de mensajería segura, escalable y privada.  
- Separación de servicios mejora la escalabilidad y facilita mantenimiento.  
- NGINX como proxy inverso asegura tráfico seguro y manejo de WebSockets.  
- Integración de redes internas y host-only permite aislamiento y control de accesos.  
- Próximos pasos: monitoreo con Prometheus/Grafana y expansión de nodos Mattermost si la comunidad crece.

---

**Autor:** Joel Salinas
