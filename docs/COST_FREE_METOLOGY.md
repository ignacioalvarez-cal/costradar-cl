# Metodología Cost-Free de Seguridad

Este documento justifica las decisiones de seguridad arquitectónica tomadas para el proyecto CostRadar CL. La regla de negocio es innegociable: **$0 costo operativo**.

No se omiten controles por desconocimiento, sino como decisiones conscientes de arquitectura equilibrando riesgo y presupuesto.

## Controles omitidos conscientemente

| Control de Seguridad | Costo Oficial (Google Cloud) | Justificación y Mitigación Gratuita Aplicada |
| :--- | :--- | :--- |
| **Cloud Armor (WAF/DDoS)** | ~$5/mes política + $1/regla | **Omitido.** Se mitiga el riesgo de saturación (Denial of Wallet) limitando `max-instances` en Cloud Run, usando rate limiting a nivel de aplicación (FastAPI) y cuotas estrictas en BigQuery. |
| **Artifact Analysis (Escaneo auto)** | ~$0.26 por imagen | **Omitido.** Usamos la herramienta de código abierto `Trivy` inyectada directamente en el pipeline de GitHub Actions (CI) sin costo extra. |
| **Data Access & VPC Flow Logs** | Cobro por volumen (GiB) | **Omitido.** Los registros granulares están apagados para evitar cargos por volumen. Se usan logs estructurados de la app y los registros gratuitos de Actividad de Administrador de GCP. |
| **Alertas de Cloud Monitoring** | Posible cobro por alerta | **Omitido.** Usaremos un flujo programado de GitHub Actions (workflow) como monitor de salud, enviando alertas gratuitas si la API o el pipeline fallan. |

*Nota Legal:* El proyecto está mapeado a las buenas prácticas de la industria y toma como referencia la Ley 21.719 (Datos Personales, Chile) buscando el principio de minimización (no se recolectan datos personales en la plataforma, salvo los metadatos técnicos inevitables como IPs en logs temporales de los proveedores en EE.UU.). No constituye certificación legal.
