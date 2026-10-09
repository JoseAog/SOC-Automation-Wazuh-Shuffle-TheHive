# 🔐 SOC Automation Lab: Wazuh (SIEM) + Shuffle (SOAR) + TheHive 5 (Incident Response)

## 📌 Visión General
Este laboratorio demuestra la construcción e integración de un pipeline completo de seguridad defensiva (Blue Team) automatizado. Ante un ataque de fuerza bruta ejecutado desde **Kali Linux** hacia un endpoint **Windows**, el sistema detecta la amenaza en tiempo real, procesa el evento mediante un flujo de trabajo SOAR y genera una alerta de incidente para la gestión del equipo SOC.

---

## 🏗️ Arquitectura del Laboratorio

- **SIEM (Wazuh v4.7):** Ingesta y correlación de eventos del sistema Windows Agent mediante conexión segura TLS.
- **SOAR (Shuffle):** Recibe las alertas de Wazuh mediante Webhooks, parsea la información en formato JSON y realiza la llamada a la API REST de TheHive.
- **Incident Response (TheHive 5):** Gestión centralizada de casos y alertas dentro de la organización `SOC`.
- **Atacante:** Kali Linux (Hydra).
- **Víctima:** Windows 11 (Agente Wazuh activo).

---

## ⚡ Flujo de Trabajo del Incidente

1. **Ataque:** Simulación de ataque de fuerza bruta contra el servicio RDP usando `Hydra`.
2. **Detección:** Wazuh detecta múltiples fallos de autenticación (Regla `60204 - Multiple Windows logon failures`, Nivel 10 / MITRE ATT&CK T1110 - Brute Force).
3. **Orquestación:** Wazuh dispara un Webhook automático hacia el workflow de Shuffle.
4. **Respuesta:** Shuffle procesa las variables dinámicas del evento y genera una alerta HTTP POST (`status: 201 Created`) en TheHive 5.

---

## 🛠️ Desafíos Técnicos y Troubleshooting

Durante el despliegue e integración se diagnosticaron y resolvieron diversos problemas de arquitectura e infraestructura:

- **Autenticación en TheHive 5 API:** Configuración de cabeceras de autorización (`Bearer Token` / `API Key`) y asignación de permisos/organización adecuados para evitar errores HTTP `401 Unauthorized`.
- **TLS vs Plaintext:** Configuración de certificados propios en Wazuh Manager para asegurar el canal sin desactivar la capa de seguridad.
- **Persistencia en Docker:** Corrección de volúmenes en `docker-compose.yml` para garantizar la persistencia de configuraciones y usuarios tras reiniciar el stack de contenedores.

---

## 📁 Estructura del Repositorio

- `docker-compose.yml`: Archivo de despliegue con los 10 contenedores del entorno.
- `workflow-shuffle.json`: Exportación del flujo de automatización listo para importar en Shuffle.
- `/images`: Capturas de pantalla demostrativas del ciclo de vida del ataque y resolución del caso.

---
*Desarrollado por [Jose Antonio](https://www.linkedin.com/in/jose-antonio-ord%C3%B3%C3%B1ez-godoy-12a536370) — Aspirante a Analista SOC N1.*
