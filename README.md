````markdown
<div align="center">

[![Python Version](https://img.shields.io/badge/python-3.10%2B-blue?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.95+-009688?style=flat-square&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![WhatsApp](https://img.shields.io/badge/WhatsApp_Business-25D366?style=flat-square&logo=whatsapp&logoColor=white)](https://business.whatsapp.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)](LICENSE)

**TicketSnap** • Solución empresarial para automatizar el procesamiento de recibos y facturas vía WhatsApp Business API

</div>

---

## 📌 Índice

- [Descripción](#descripción)
- [Características principales](#características-principales)
- [Instalación](#instalación)
- [Configuración](#configuración)
- [Uso](#uso)
- [Ejemplo de solicitud](#ejemplo-de-solicitud)
- [Estructura del proyecto](#estructura-del-proyecto)
- [Desarrollo y pruebas](#desarrollo-y-pruebas)
- [Despliegue](#despliegue)
- [Monitoreo y mantenimiento](#monitoreo-y-mantenimiento)
- [Contribuciones](#contribuciones)
- [Licencia](#licencia)
- [Contacto y soporte](#contacto-y-soporte)

---

## 📝 Descripción

TicketSnap es una plataforma desarrollada por **Archive México** para automatizar el flujo completo de captura, reconocimiento y análisis de tickets, boletas y facturas comerciales a través de la **WhatsApp Business API**. Utiliza OCR de alta precisión para extraer los datos necesarios y los integra directamente con sistemas ERP o contables.

**Propuesta de valor**:
- **Automatización total**: desde la recepción de la imagen hasta el reporte final.
- **Integración empresarial**: endpoints REST fáciles de consumir.
- **Escalabilidad**: arquitectura basada en FastAPI y bases de datos SQL.
- **Precisión OCR**: extracción confiable con la Taggun Receipt OCR API.

---

## 🚀 Características principales

| Característica             | Descripción                                                                 |
|----------------------------|-----------------------------------------------------------------------------|
| 📲 Recepción vía WhatsApp   | Imágenes recibidas y procesadas automáticamente mediante webhook.           |
| 🔍 OCR empresarial         | Extracción de datos clave (importe, fecha, establecimientos, artículos, etc.).|
| 💾 Almacenamiento SQL      | Persistencia con SQLite o PostgreSQL usando SQLAlchemy ORM.                 |
| 📊 Exportación múltiple    | Reportes en CSV, Excel y formatos personalizados.                            |
| 🔧 API REST                | Documentada con Swagger UI y OpenAPI.                                        |
| 📈 Monitoreo y métricas    | Logging estructurado y métricas de rendimiento integradas.                  |

---

## ⚙️ Instalación

### Requisitos

- Python 3.10+
- Linux/macOS/Windows (se recomienda WSL2 en Windows)
- 2 GB RAM (4 GB recomendado)
- 1 GB de espacio libre

### Pasos

```bash
# 1. Clonar repositorio
git clone https://github.com/archive-mx/ticket-snap.git
cd ticket-snap

# 2. Crear entorno virtual e instalar dependencias
python3 -m venv venv
source venv/bin/activate  # Windows: venv\\Scripts\\activate
pip install --upgrade pip
pip install -r requirements.txt

# 3. Inicializar base de datos
cp .env.example .env
# Edita .env con tus credenciales
python -m src.db.init_db

# 4. Ejecutar servidor local
uvicorn src.api.main:app --host 0.0.0.0 --port 8000 --reload
````

---

## 🔧 Configuración

Edita el archivo `.env` con las siguientes variables:

```ini
# Taggun OCR API
taggun_api_key=TU_TAGGUN_API_KEY

# Base de datos
database_url=sqlite:///./tickets.db  # Para producción usa postgresql://usuario:pass@host:puerto/db

# WhatsApp Business API
token_whatsapp=TU_WHATSAPP_BUSINESS_TOKEN
phone_id=TU_WHATSAPP_PHONE_ID
verify_token=TU_WHATSAPP_VERIFY_TOKEN

# Opciones de la aplicación
debug=false
log_level=INFO
max_file_size=10485760  # 10 MB
types_allowed=jpg,jpeg,png,pdf
```

---

## ▶️ Uso

1. **Enviar ticket**: el usuario envía una imagen al número de WhatsApp.
2. **Procesamiento**: OCR extrae datos y genera un registro.
3. **Almacenamiento**: datos guardados con metadatos y timestamp.
4. **Respuesta**: confirmación con resumen y enlace de descarga.

### 📋 Comandos disponibles

| Comando       | Función            | Ejemplo                                |
| ------------- | ------------------ | -------------------------------------- |
| Imagen/ticket | Procesar ticket    | Enviar imagen con descripción opcional |
| `/export`     | Exportar registros | `/export csv 2025-01-01 2025-07-30`    |
| `/status`     | Estado del sistema | `/status`                              |
| `/help`       | Mostrar ayuda      | `/help`                                |

---

## 💻 Ejemplo de solicitud

```bash
curl -X POST https://tu-dominio.com/webhook/whatsapp \
  -H "Content-Type: application/json" \
  -d '{
    "entry": [{
      "changes": [{
        "value": {
          "messages": [{
            "type": "image",
            "image": {"id": "MEDIA_ID"},
            "from": "521xxxxxxxxxx"
          }]
        }
      }]
    }]
  }'
```

---

## 🗂️ Estructura del proyecto

```
ticket-snap/
├── .env.example        # Plantilla de variables de entorno
├── README.md           # Documentación principal
├── requirements.txt    # Dependencias de Python
├── .github/            # Configuración de CI/CD
│   └── workflows/
│       └── ci.yml
├── src/
│   ├── api/            # FastAPI endpoints
│   ├── bot/            # Lógica de mensajería WhatsApp
│   ├── core/           # Cliente OCR y parser de datos
│   ├── db/             # Modelos y sesión SQLAlchemy
│   └── utils/          # Exportadores y utilidades
├── tests/              # Pruebas unitarias e integración
└── docs/               # Documentación técnica adicional
```

---

## 🧪 Desarrollo y pruebas

```bash
# Instalar deps de desarrollo
pip install -r requirements-dev.txt

# Ejecutar pruebas
pytest --cov=src --cov-report=html

# Formateo y linting
black src/ && flake8 src/ && mypy src/
```

---

## 🚢 Despliegue

### 📦 Docker

```bash
docker build -t archive-mx/ticket-snap:latest .
docker run -d --name ticket-snap -p 8000:8000 --env-file .env archive-mx/ticket-snap:latest
```

### 🐳 Docker Compose

```yaml
version: '3.8'
services:
  ticket-snap:
    build: .
    ports:
      - "8000:8000"
    env_file: .env
    depends_on:
      - postgres

  postgres:
    image: postgres:14
    environment:
      POSTGRES_DB: tickets
      POSTGRES_USER: ticketsnap
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    volumes:
      - data:/var/lib/postgresql/data

volumes:
  data:
```

---

## 📊 Monitoreo y mantenimiento

* **Logging**: niveles INFO, WARNING, ERROR y CRITICAL.
* **Métricas**: tiempo de procesamiento, tasa de éxito OCR, volumen de mensajes.
* **Alertas**: se integran con sistemas externos (opcional).

---

## 🤝 Contribuciones

¡Contribuciones bienvenidas! Sigue estos pasos:

1. Haz fork del repositorio.
2. Crea una rama con tu feature/bugfix: `git checkout -b feature/nueva-funcionalidad`.
3. Realiza tus cambios y pruebas.
4. Envía un pull request describiendo tu aporte.

---

## 📜 Licencia

Licenciado bajo MIT. Ver [LICENSE](LICENSE) para más detalles.

---

## 📬 Contacto y soporte

Para dudas y soporte:

* Email: [info@archive.com.mx](mailto:info@archive.com.mx)
* Documentación: [https://docs.archive.com.mx/ticket-snap](https://docs.archive.com.mx/ticket-snap)
* Soporte: [https://support.archive.com.mx](https://support.archive.com.mx)

<div align="center">
**TicketSnap** - Automatización empresarial inteligente
</div>
```
