# Ansible Daily Ops 🛠️

Este repositorio contiene la infraestructura como código (IaC) y playbooks del día a día para la automatización, mantenimiento y seguridad de servidores.

## 📂 Estructura del Proyecto

- `ansible.cfg`: Configuración base de Ansible (conexiones, usuarios, privilegios).
- `inventory/`: Definición de servidores e IPs separados por entornos o roles.
- `playbooks/`: Scripts de automatización para tareas rutinarias.
- `roles/`: Tareas complejas y modulares para reutilizar en varios playbooks.

## 📜 Catálogo de Playbooks

### Mantenimiento de Sistema y Backups
- `update_system.yml`: Actualiza paquetes del SO (Debian/RedHat) y limpia dependencias huérfanas.
- `cleanup_space.yml`: Libera disco vaciando cachés de paquetes, archivos temporales en `/tmp` y recortando logs antiguos de `journald`.
- `backup_data.yml`: Comprime directorios críticos y los descarga de forma segura a la máquina local.

### Gestión y Seguridad
- `manage_users.yml`: Gestiona altas y bajas de cuentas, y despliega claves públicas SSH.
- `secure_server.yml`: Aplica hardening base configurando el firewall y bloqueando accesos inseguros por SSH (root y contraseñas).
- `health_check.yml`: Audita rápidamente si los servicios esenciales están corriendo en todos los nodos.

### Ecosistema Docker
- `docker_maintenance.yml`: Realiza una limpieza profunda (`prune`) de imágenes, redes y contenedores inactivos por más de 7 días.

### Certificados TLS/SSL
- `deploy_tls.yml`: Distribuye certificados públicos y claves privadas con permisos restrictivos (`0600`), recargando el servicio web si hay cambios.
- `check_tls_expiration.yml`: Lee el certificado del servidor y lanza una alerta si caduca en menos de 15 días.

## 🚀 Uso Básico

**Probar conectividad del inventario:**
```bash
ansible all -i inventory/hosts.ini -m ping
```
**Ejecutar un playbook (ejemplo con mantenimiento de Docker):**
```bash
ansible-playbook -i inventory/hosts.ini playbooks/docker_maintenance.yml
```
**Ejecutar limitando a un grupo específico de servidores:**
```bash
ansible-playbook -i inventory/hosts.ini playbooks/update_system.yml --limit webservers
```