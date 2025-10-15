# 🚀 Apache Security Hardening

[![Apache](https://img.shields.io/badge/Server-Apache-orange)]()
[![Security](https://img.shields.io/badge/Security-Hardening-red)]()
[![OWASP CRS](https://img.shields.io/badge/OWASP-CRS-blue)]()
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

> Repositorio con prácticas de endurecimiento y protección de servidores web Apache. Incluye configuraciones, comprobaciones, capturas y recomendaciones para aplicar en entornos de laboratorio y producción controlada.

---

## 🗂️ Estructura del repositorio

```
Apache_Hardening/
├── 1 - HSTS, CSP/          # HSTS, CSP, certificados y buenas prácticas TLS
├── 2 - WAF/                # ModSecurity y reglas
├── 3 - OWASP/              # OWASP CRS y reglas personalizadas
├── 4 - DDOS/               # mod_evasive y mitigación de tráfico excesivo
└── README.md               # Este documento
```

---

## 📌 Resumen de las prácticas

### Práctica 1 — HSTS, CSP y TLS
Implementación y verificación de:
- Redirección HTTP → HTTPS y configuración de VirtualHost para TLS.
- HSTS (`Strict-Transport-Security`) con `includeSubDomains` y periodo recomendado.
- Content Security Policy (CSP) para mitigar XSS y control de recursos.
- Desactivación de `autoindex` y buenas prácticas en DocumentRoot.
- Evidencias y comandos para comprobar encabezados con `curl -I`.

**Objetivos:** Asegurar transporte confidencial, reducir exposición a XSS y proteger información en tránsito.

---

### Práctica 2 — WAF (ModSecurity)
- Instalación y activación de **ModSecurity** en Apache.
- Configuración básica (`SecRuleEngine On`) y pruebas de bloqueo de ataques XSS/SQLi.
- Ejemplos de reglas personalizadas y ajuste de logs para auditoría.
- Validación mediante peticiones maliciosas controladas y captura de respuestas (403).

**Objetivos:** Inspección y filtrado de tráfico HTTP para bloquear ataques comunes antes de alcanzar la aplicación.

---

### Práctica 3 — OWASP CRS (Core Rule Set)
- Integración de **OWASP CRS** con ModSecurity.
- Organización de `crs-setup.conf` y despliegue de reglas en `/etc/modsecurity/`.
- Pruebas de detección ante inyecciones de comandos, SQLi y RCE simuladas.
- Estrategia para tuning: false positives, whitelist, y reglas personalizadas específicas de aplicación.

**Objetivo:** Proveer una capa de defensa basada en reglas estándar de la industria (OWASP).

---

### Práctica 4 — Mitigación DDoS (mod_evasive)
- Instalación y configuración de **mod_evasive** para limitar la tasa de peticiones por IP.
- Parámetros recomendados: `DOSPageCount`, `DOSSiteCount`, `DOSBlockingPeriod`, `DOSWhitelist`.
- Pruebas de carga simulada con `ab` (ApacheBench) y análisis de logs.
- Recomendaciones para despliegue en entornos frontales y uso combinado con CDNs y rate-limiting en capas superiores.

**Objetivo:** Detectar y mitigar ráfagas de tráfico que busquen saturar recursos del servidor.

---

## 🔧 Comprobaciones y comandos útiles

- Verificar encabezados y HSTS/CSP:
```bash
curl -I -k https://tu-dominio.local
```

- Reiniciar Apache:
```bash
sudo systemctl restart apache2
```

- Probar ModSecurity bloqueos:
```bash
curl -k "https://tu-dominio.local/?q=<script>alert(1)</script>"
```

- Simular carga (ab):
```bash
ab -n 1000 -c 50 https://tu-dominio.local/
```

---

## 🛡️ Recomendaciones de despliegue

- Habilitar TLS 1.2+ y deshabilitar cifrados débiles (RC4, 3DES).
- HSTS con `max-age` amplio y `includeSubDomains` tras periodo de pruebas.
- CSP restrictiva en modo report-only antes de forzar.
- Monitorización de logs de ModSecurity y tuning periódico.
- Usar WAF + CDN (Cloudflare, Fastly) para mitigación DDoS a gran escala.
- Mantener reglas CRS actualizadas y versionadas.

---

## 📷 Evidencias y recursos
Cada práctica incluye capturas en la carpeta `assets/` y el detalle de configuración dentro de su subcarpeta. Además, hay referencias a imágenes Docker construidas para cada versión del proyecto.

---

## 📄 Licencia

Este repositorio se publica bajo **MIT License**. Consulta el archivo [`LICENSE`](LICENSE).

---

## ✍️ Autor
**Alex Rosell** — Técnico en Ciberseguridad y Administración de Sistemas.  
Repositorio: `Apache_Hardening`
