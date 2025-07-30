# TicketSnap

<div align="center">

[![Python Version](https://img.shields.io/badge/python-3.10%2B-blue?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.104+-009688?style=flat-square&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![WhatsApp](https://img.shields.io/badge/WhatsApp_Business-25D366?style=flat-square&logo=whatsapp&logoColor=white)](https://business.whatsapp.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)](LICENSE)
[![CI/CD](https://img.shields.io/badge/CI%2FCD-GitHub%20Actions-2088FF?style=flat-square&logo=github-actions&logoColor=white)](/.github/workflows/ci.yml)

**Solución empresarial para automatizar el procesamiento de recibos y facturas vía WhatsApp Business API**

*Desarrollado por [Archive México](https://archive.com.mx/)*

</div>

---

## 📌 Índice

- [Descripción](#-descripción)
- [Características principales](#-características-principales)
- [Instalación rápida](#-instalación-rápida)
- [Configuración](#-configuración)
- [Uso](#️-uso)
- [API y ejemplos](#-api-y-ejemplos)
- [Estructura del proyecto](#️-estructura-del-proyecto)
- [Desarrollo](#-desarrollo)
- [Despliegue en producción](#-despliegue-en-producción)
- [Monitoreo](#-monitoreo)
- [Contribuciones](#-contribuciones)
- [Soporte](#-contacto-y-soporte)

---

## 📝 Descripción

**TicketSnap** es una plataforma desarrollada por **[Archive México](https://archive.com.mx/)** que automatiza el flujo completo de captura, reconocimiento y análisis de tickets, boletas y facturas comerciales a través de **WhatsApp Business API**. 

Utiliza tecnología OCR de alta precisión (Taggun) para extraer datos estructurados que se integran directamente con sistemas ERP, contables y de gestión empresarial.

### 🎯 Propuesta de valor

- **🔄 Automatización total**: Desde recepción hasta reporte final sin intervención manual
- **🏢 Integración empresarial**: API REST documentada para sistemas existentes
- **📈 Escalabilidad**: Arquitectura robusta con FastAPI y bases de datos SQL
- **🎯 Precisión OCR**: Extracción confiable de datos comerciales críticos

---

## 🚀 Características principales

<table>
<tr>
<td width="50%">

### 📱 **Funcionalidades Core**
- 📲 **Recepción WhatsApp**: Webhook automatizado
- 🔍 **OCR Empresarial**: Taggun Receipt API
- 💾 **Persistencia SQL**: SQLite/PostgreSQL  
- 📊 **Exportación**: CSV, Excel, JSON
- 🔧 **API REST**: Swagger UI integrado
- 📈 **Monitoreo**: Métricas y logging

</td>
<td width="50%">

### 📋 **Datos Extraídos**
- 🏪 Establecimiento y dirección
- 📅 Fecha y hora de compra
- 🧾 Folio y número de ticket
- 💰 Total, subtotal, impuestos
- 🛒 Lista detallada de productos
- 💳 Método de pago

</td>
</tr>
</table>

---

## ⚡ Instalación rápida

### 📋 Prerrequisitos

- **Python 3.10+**
- **Sistema**: Linux/macOS/Windows (WSL2 recomendado)
- **Memoria**: 2GB mínimo, 4GB recomendado
- **Espacio**: 1GB disponible

### 🚀 Setup en 5 minutos

```bash
# 1. Clonar y acceder al directorio
git clone https://github.com/archive-mx/ticket-snap.git
cd ticket-snap

# 2. Crear entorno virtual
python3 -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# 3. Instalar dependencias
pip install --upgrade pip
pip install -r requirements.txt

# 4. Configurar variables de entorno
cp .env.example .env
# Editar .env con tus credenciales (ver sección siguiente)

# 5. Inicializar base de datos
python -m src.db.init_db

# 6. Levantar servidor
uvicorn src.api.main:app --host 0.0.0.0 --port 8000 --reload
```

### ✅ Verificar instalación

- **API**: http://localhost:8000
- **Documentación**: http://localhost:8000/docs
- **Health check**: http://localhost:8000/health

---

## 🔧 Configuración

### 📄 Archivo .env

```ini
# ===== TAGGUN OCR API =====
TAGGUN_API_KEY=tu_taggun_api_key_aqui

# ===== BASE DE DATOS =====
DATABASE_URL=sqlite:///./tickets.db
# Producción: postgresql://usuario:password@host:puerto/basedatos

# ===== WHATSAPP BUSINESS API =====
WHATSAPP_TOKEN=tu_whatsapp_business_token
WHATSAPP_PHONE_ID=tu_whatsapp_phone_id  
WHATSAPP_VERIFY_TOKEN=tu_webhook_verify_token

# ===== CONFIGURACIÓN APLICACIÓN =====
DEBUG=false
LOG_LEVEL=INFO
MAX_FILE_SIZE=10485760  # 10MB
ALLOWED_FILE_TYPES=jpg,jpeg,png,pdf
```

### 🔑 Obtener credenciales

<details>
<summary><strong>📱 WhatsApp Business API</strong></summary>

1. Accede a [Meta for Developers](https://developers.facebook.com/)
2. Crea una aplicación empresarial
3. Agrega "WhatsApp Business API" 
4. Configura webhook: `https://tu-dominio.com/webhook/whatsapp`
5. Obtén tokens desde el panel de control

</details>

<details>
<summary><strong>🔍 Taggun OCR</strong></summary>

1. Regístrate en [Taggun.io](https://www.taggun.io/)
2. Verifica tu cuenta empresarial
3. Navega a "API Keys" en el dashboard
4. Genera y copia tu API key

</details>

---

## ▶️ Uso

### 🔄 Flujo de trabajo

```mermaid
graph LR
    A[📱 Usuario envía ticket] --> B[📨 Webhook recibe]
    B --> C[🔍 OCR procesa]
    C --> D[📊 Datos extraídos]
    D --> E[💾 Guarda en DB]
    E --> F[📤 Respuesta automática]
```

### 📱 Comandos WhatsApp

| Comando | Función | Ejemplo |
|---------|---------|---------|
| **📸 Imagen** | Procesar ticket | Enviar foto del recibo |
| **`/export`** | Exportar datos | `/export csv 2025-01-01 2025-07-30` |
| **`/stats`** | Estadísticas | `/stats mes` |
| **`/status`** | Estado sistema | `/status` |
| **`/help`** | Ayuda | `/help` |

### 💬 Ejemplo de respuesta

```
✅ Ticket procesado exitosamente

📋 DATOS EXTRAÍDOS:
• Establecimiento: Súper Tacos El Buen Sabor
• Fecha: 15/01/2025 14:30
• Folio: TK-001234
• Total: $285.50 MXN

🛒 PRODUCTOS (3 items):
- Tacos al pastor x3: $180.00
- Refresco 600ml: $45.00  
- Propina: $25.50
- IVA (16%): $35.00

📊 Guardado en sistema • ID: #TK789
📎 Archivo CSV disponible
```

---

## 🔌 API y ejemplos

### 📡 Webhook endpoint

```bash
POST /webhook/whatsapp
Content-Type: application/json

{
  "entry": [{
    "changes": [{
      "value": {
        "messages": [{
          "type": "image",
          "image": {"id": "MEDIA_ID"},
          "from": "521234567890",
          "timestamp": "1640995200"
        }]
      }
    }]
  }]
}
```

### 📊 Endpoints principales

| Método | Endpoint | Descripción |
|--------|----------|-------------|
| `GET` | `/health` | Health check |
| `POST` | `/webhook/whatsapp` | Recibir mensajes |
| `GET` | `/tickets` | Listar tickets |
| `GET` | `/tickets/{id}` | Obtener ticket |
| `POST` | `/export` | Exportar datos |
| `GET` | `/stats` | Estadísticas |

### 🔍 Ejemplo de respuesta API

```json
{
  "id": "tk_789",
  "timestamp": "2025-01-15T14:30:00Z",
  "phone_number": "+521234567890",
  "ocr_data": {
    "establishment": "Súper Tacos El Buen Sabor",
    "total": 285.50,
    "currency": "MXN",
    "date": "2025-01-15",
    "items": [
      {"description": "Tacos al pastor x3", "amount": 180.00},
      {"description": "Refresco 600ml", "amount": 45.00}
    ]
  },
  "status": "processed",
  "export_url": "https://api.archive.com.mx/exports/tk_789.csv"
}
```

---

## 🗂️ Estructura del proyecto

```
ticket-snap/
├── 📄 .env.example              # Template de configuración
├── 📄 README.md                 # Documentación principal  
├── 📄 requirements.txt          # Dependencias producción
├── 📄 requirements-dev.txt      # Dependencias desarrollo
├── 📄 Dockerfile               # Imagen Docker
├── 📄 docker-compose.yml       # Orquestación servicios
├── 📁 .github/                 # CI/CD workflows
│   └── 📁 workflows/
│       └── 📄 ci.yml
├── 📁 src/                     # Código fuente
│   ├── 📁 api/                 # FastAPI endpoints
│   │   ├── 📄 main.py          # App principal
│   │   ├── 📄 routes.py        # Rutas API
│   │   └── 📄 dependencies.py  # Dependencias
│   ├── 📁 bot/                 # WhatsApp integration
│   │   ├── 📄 __init__.py
│   │   ├── 📄 handler.py       # Message handler
│   │   └── 📄 responses.py     # Response templates
│   ├── 📁 core/                # Business logic
│   │   ├── 📄 ocr.py           # Taggun OCR client
│   │   ├── 📄 parser.py        # Data parser
│   │   └── 📄 config.py        # Configuration
│   ├── 📁 db/                  # Database layer
│   │   ├── 📄 models.py        # SQLAlchemy models
│   │   ├── 📄 session.py       # DB session
│   │   └── 📄 init_db.py       # DB initialization
│   └── 📁 utils/               # Utilities
│       ├── 📄 exporter.py      # Data export
│       ├── 📄 logger.py        # Structured logging
│       └── 📄 validators.py    # Data validation
├── 📁 tests/                   # Test suite
│   ├── 📁 unit/               # Unit tests
│   ├── 📁 integration/        # Integration tests
│   └── 📄 conftest.py         # Test configuration
└── 📁 docs/                   # Documentation
    ├── 📄 api.md              # API documentation
    ├── 📄 deployment.md       # Deployment guide
    └── 📄 contributing.md     # Contribution guidelines
```

---

## 🧪 Desarrollo

### 🛠️ Setup de desarrollo

```bash
# Instalar dependencias de desarrollo
pip install -r requirements-dev.txt

# Configurar pre-commit hooks
pre-commit install

# Variables de entorno para testing
cp .env.example .env.test
```

### ✅ Testing

```bash
# Ejecutar todos los tests
pytest

# Con cobertura de código  
pytest --cov=src --cov-report=html --cov-report=term

# Solo tests unitarios
pytest tests/unit/ -v

# Solo tests de integración
pytest tests/integration/ -v

# Test específico
pytest tests/unit/test_ocr.py::test_extract_data -v
```

### 🔧 Calidad de código

```bash
# Formateo automático
black src/ tests/

# Linting
flake8 src/ tests/

# Ordenar imports
isort src/ tests/

# Type checking
mypy src/

# Todo en uno
make lint  # Si tienes Makefile
```

### 📊 Métricas de calidad

- **Cobertura**: >= 85%
- **Complejidad ciclomática**: <= 10
- **Líneas por función**: <= 50
- **Type coverage**: >= 90%

---

## 🚢 Despliegue en producción

### 🐳 Docker (Recomendado)

```bash
# Build
docker build -t archive-mx/ticket-snap:latest .

# Run
docker run -d \
  --name ticket-snap \
  -p 8000:8000 \
  --env-file .env \
  --restart unless-stopped \
  archive-mx/ticket-snap:latest
```

### 🐙 Docker Compose

```yaml
version: '3.8'

services:
  ticket-snap:
    build: .
    ports:
      - "8000:8000"
    env_file: .env
    restart: unless-stopped
    depends_on:
      - postgres
      - redis
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8000/health"]
      interval: 30s
      timeout: 10s
      retries: 3

  postgres:
    image: postgres:15-alpine
    environment:
      POSTGRES_DB: ${POSTGRES_DB:-tickets}
      POSTGRES_USER: ${POSTGRES_USER:-ticketsnap}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
    volumes:
      - postgres_data:/var/lib/postgresql/data
    restart: unless-stopped

  redis:
    image: redis:7-alpine
    restart: unless-stopped
    volumes:
      - redis_data:/data

volumes:
  postgres_data:
  redis_data:
```

### ☁️ Cloud deployment

<details>
<summary><strong>🔵 Azure App Service</strong></summary>

```bash
# Login y deploy
az login
az webapp up --runtime PYTHON:3.10 --name ticket-snap-prod
```

</details>

<details>
<summary><strong>🟢 Google Cloud Run</strong></summary>

```bash  
# Deploy desde código
gcloud run deploy ticket-snap \
  --source . \
  --platform managed \
  --region us-central1 \
  --allow-unauthenticated
```

</details>

### 🔗 Configurar Webhook

Apuntar webhook de WhatsApp a:
```
https://tu-dominio.com/webhook/whatsapp
```

---

## 📊 Monitoreo

### 📈 Métricas clave

- **Throughput**: Tickets procesados por minuto
- **Latencia**: Tiempo promedio de procesamiento  
- **Error rate**: Porcentaje de fallos OCR
- **Uptime**: Disponibilidad del servicio

### 🚨 Alertas

Configure alertas para:
- **CPU > 80%** por más de 5 minutos
- **Memoria > 90%** por más de 2 minutos  
- **Error rate > 5%** en ventana de 10 minutos
- **Response time > 30s** promedio

### 📝 Logs

```bash
# Ver logs en tiempo real
docker logs -f ticket-snap

# Logs con filtro de nivel
docker logs ticket-snap 2>&1 | grep ERROR

# Análisis de performance
grep "Processing time" /var/log/ticket-snap/app.log
```

---

## 🤝 Contribuciones

¡Las contribuciones son bienvenidas! 

### 📝 Proceso

1. **Fork** el repositorio
2. **Crea** una rama: `git checkout -b feature/nueva-funcionalidad`
3. **Desarrolla** y agrega tests
4. **Verifica** calidad: `make lint test`
5. **Commit**: `git commit -m "feat: agregar nueva funcionalidad"`
6. **Push**: `git push origin feature/nueva-funcionalidad`
7. **Pull Request** con descripción detallada

### 📋 Guías

- **Commits**: [Conventional Commits](https://www.conventionalcommits.org/)
- **Código**: Seguir PEP 8 y usar type hints
- **Tests**: Cobertura >= 85% para nuevas funciones
- **Docs**: Actualizar README y docstrings

---

## 📄 Licencia

Este proyecto está licenciado bajo la **Licencia MIT**. Ver [LICENSE](LICENSE) para detalles completos.

```
Copyright (c) 2025 Archive México

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software")...
```

---

## 📬 Contacto y soporte

<div align="center">

### 📞 Canales de soporte

[![Email](https://img.shields.io/badge/Email-info@archive.com.mx-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:info@archive.com.mx)
[![Website](https://img.shields.io/badge/Website-archive.com.mx-4285F4?style=for-the-badge&logo=google-chrome&logoColor=white)](https://archive.com.mx/)
[![Docs](https://img.shields.io/badge/Docs-docs.archive.com.mx-009688?style=for-the-badge&logo=gitbook&logoColor=white)](https://docs.archive.com.mx/ticket-snap)
[![Support](https://img.shields.io/badge/Support-support.archive.com.mx-FF6B6B?style=for-the-badge&logo=lifebuoy&logoColor=white)](https://support.archive.com.mx)

</div>

### 🕐 Horarios de soporte

- **Lunes a Viernes**: 9:00 - 18:00 (GMT-6)
- **Emergencias**: 24/7 vía email
- **Tiempo de respuesta**: < 4 horas hábiles

---

<div align="center">

**TicketSnap** - Automatización empresarial inteligente

*Desarrollado con🧠 por [Archive México](https://archive.com.mx/)*

[![GitHub stars](https://img.shields.io/github/stars/archive-mx/ticket-snap?style=social)](https://github.com/archive-mx/ticket-snap)

</div>