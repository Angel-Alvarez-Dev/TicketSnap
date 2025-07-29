# TicketSnap

<div align="center">

[![Python Version](https://img.shields.io/badge/python-3.10%2B-blue?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.95+-009688?style=flat-square&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![WhatsApp](https://img.shields.io/badge/WhatsApp_Business-25D366?style=flat-square&logo=whatsapp&logoColor=white)](https://business.whatsapp.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)](LICENSE)

**Solución empresarial para la automatización de procesamiento de tickets y recibos mediante WhatsApp Business API**

*Desarrollado por [Archive](https://archive.com.mx/)*

[Características](#características) • 
[Instalación](#instalación) • 
[Configuración](#configuración) • 
[Documentación](#documentación)

</div>

---

## Descripción

TicketSnap es una solución empresarial desarrollada por **[Archive](https://archive.com.mx/)** que automatiza el procesamiento y análisis de tickets comerciales a través de WhatsApp Business API. La plataforma utiliza tecnología OCR avanzada para extraer datos estructurados de recibos, boletas y facturas, facilitando su integración con sistemas contables y de gestión empresarial.

### Propuesta de Valor

- **Automatización Completa**: Procesamiento sin intervención manual desde la captura hasta la exportación
- **Integración Empresarial**: Compatible con sistemas ERP y plataformas contables existentes
- **Escalabilidad**: Arquitectura diseñada para manejar alto volumen de transacciones
- **Precisión**: OCR especializado en documentos comerciales con alta exactitud

---

## Características

### Funcionalidades Principales

| Característica | Descripción |
|---------------|-------------|
| **Recepción Automatizada** | Procesamiento de imágenes vía WhatsApp Business API |
| **OCR Empresarial** | Extracción de datos con Taggun Receipt OCR API |
| **Base de Datos SQL** | Almacenamiento persistente con SQLAlchemy |
| **Exportación Masiva** | Generación de reportes en CSV, Excel y formatos personalizados |
| **API REST** | Endpoints para integración con sistemas terceros |
| **Monitoreo** | Logging y métricas de performance integradas |

### Tecnologías

- **Backend**: FastAPI + Python 3.10+
- **Base de Datos**: SQLite/PostgreSQL con SQLAlchemy ORM
- **OCR**: Taggun Receipt API
- **Mensajería**: PyWa (WhatsApp Cloud API Client)
- **CI/CD**: GitHub Actions
- **Monitoreo**: Logging estructurado y métricas

---

## Instalación

### Requisitos del Sistema

- Python 3.10 o superior
- Sistema operativo: Linux/macOS/Windows con WSL2
- Memoria RAM: 2GB mínimo, 4GB recomendado
- Espacio en disco: 1GB para instalación base

### Configuración del Entorno

#### Linux/macOS

```bash
# Clonar repositorio
git clone https://github.com/archive-mx/ticket-snap.git
cd ticket-snap

# Crear entorno virtual
python3 -m venv venv
source venv/bin/activate

# Instalar dependencias
pip install -r requirements.txt
```

#### Windows (WSL2)

```bash
# Instalar WSL2 y Ubuntu
wsl --install -d Ubuntu

# Actualizar sistema
sudo apt update && sudo apt upgrade -y
sudo apt install -y python3 python3-venv python3-pip

# Seguir pasos de Linux/macOS
```

### Inicialización

```bash
# Configurar variables de entorno
cp .env.example .env
# Editar .env con credenciales correspondientes

# Inicializar base de datos
python -m src.db.init_db

# Iniciar servidor
uvicorn src.api.main:app --host 0.0.0.0 --port 8000
```

---

## Configuración

### Variables de Entorno

Configure las siguientes variables en el archivo `.env`:

```ini
# Configuración de OCR
TAGGUN_API_KEY=your_taggun_api_key

# Base de Datos
DATABASE_URL=sqlite:///./tickets.db
# Para producción: postgresql://user:password@host:port/database

# WhatsApp Business API
WHATSAPP_TOKEN=your_whatsapp_business_token
WHATSAPP_PHONE_ID=your_phone_number_id
WHATSAPP_VERIFY_TOKEN=your_webhook_verify_token

# Configuración de Aplicación
DEBUG=false
LOG_LEVEL=INFO
MAX_FILE_SIZE=10485760
ALLOWED_FILE_TYPES=jpg,jpeg,png,pdf
```

### Obtención de Credenciales

#### WhatsApp Business API

1. Acceder a [Meta for Developers](https://developers.facebook.com/)
2. Crear aplicación empresarial
3. Configurar WhatsApp Business API
4. Obtener tokens de acceso y configurar webhooks

#### Taggun OCR API

1. Registrarse en [Taggun](https://www.taggun.io/)
2. Verificar cuenta empresarial
3. Obtener API Key desde el panel de administración
4. Configurar límites y facturación según necesidades

---

## Uso

### Operación Básica

1. **Envío de Ticket**: Usuario envía imagen de ticket al número de WhatsApp configurado
2. **Procesamiento**: Sistema procesa imagen con OCR y extrae datos estructurados
3. **Almacenamiento**: Datos se guardan en base de datos con timestamp y metadatos
4. **Respuesta**: Usuario recibe confirmación con datos extraídos y enlace de descarga

### Comandos Disponibles

| Comando | Función | Sintaxis |
|---------|---------|----------|
| Imagen + Texto | Procesar ticket | Enviar imagen con descripción opcional |
| `/export` | Exportar registros | `/export [csv\|excel] [fecha_inicio] [fecha_fin]` |
| `/status` | Estado del sistema | `/status` |
| `/help` | Ayuda | `/help` |

### Formato de Respuesta

```
✓ Ticket procesado exitosamente

INFORMACIÓN EXTRAÍDA:
• Establecimiento: Restaurante El Buen Sabor
• Fecha: 2024-01-15 14:30:00
• Folio: ABC123456
• Total: $1,250.00 MXN
• Artículos: 3 items procesados

DESGLOSE:
- Subtotal: $1,086.21
- IVA (16%): $173.79
- Total: $1,250.00

Los datos han sido guardados en el sistema.
Archivo de exportación disponible en: [enlace]
```

---

## Arquitectura del Sistema

### Estructura del Proyecto

```
ticket-snap/
├── .env.example              # Plantilla de configuración
├── README.md                 # Documentación principal
├── requirements.txt          # Dependencias Python
├── .gitignore               # Exclusiones de Git
├── .github/
│   └── workflows/
│       └── ci.yml           # Pipeline de integración continua
├── src/                     # Código fuente
│   ├── api/
│   │   └── main.py          # Aplicación FastAPI principal
│   ├── bot/
│   │   ├── __init__.py
│   │   └── handler.py       # Manejo de mensajes WhatsApp
│   ├── core/
│   │   ├── ocr.py           # Cliente OCR Taggun
│   │   └── parser.py        # Procesamiento de datos
│   ├── db/
│   │   ├── models.py        # Modelos de base de datos
│   │   └── session.py       # Gestión de conexiones
│   └── utils/
│       └── exporter.py      # Funciones de exportación
├── tests/
│   ├── unit/               # Pruebas unitarias
│   └── integration/        # Pruebas de integración
└── docs/                   # Documentación técnica
    ├── api.md              # Especificación API
    └── deployment.md       # Guía de despliegue
```

### Flujo de Procesamiento

1. **Recepción**: Webhook recibe mensaje de WhatsApp
2. **Validación**: Verificación de formato y tamaño de archivo
3. **OCR**: Procesamiento con Taggun API
4. **Parseo**: Estructuración de datos extraídos
5. **Persistencia**: Almacenamiento en base de datos
6. **Exportación**: Generación de archivos de salida
7. **Respuesta**: Envío de confirmación al usuario

---

## Desarrollo y Pruebas

### Entorno de Desarrollo

```bash
# Instalar dependencias de desarrollo
pip install -r requirements-dev.txt

# Configurar pre-commit hooks
pre-commit install

# Ejecutar tests
pytest --cov=src --cov-report=html

# Linting y formateo
black src/
flake8 src/
mypy src/
```

### Estándares de Calidad

- **Cobertura de Código**: Mínimo 85%
- **Linting**: Cumplimiento con PEP 8 via flake8
- **Type Hints**: Tipado estático con mypy
- **Seguridad**: Análisis con bandit
- **Documentación**: Docstrings en formato Google

---

## Despliegue en Producción

### Docker

```bash
# Construir imagen
docker build -t archive-mx/ticket-snap:latest .

# Ejecutar contenedor
docker run -d \
  --name ticket-snap \
  -p 8000:8000 \
  --env-file .env \
  archive-mx/ticket-snap:latest
```

### Docker Compose

```yaml
version: '3.8'
services:
  ticket-snap:
    build: .
    ports:
      - "8000:8000"
    env_file:
      - .env
    depends_on:
      - postgres
  
  postgres:
    image: postgres:14
    environment:
      POSTGRES_DB: tickets
      POSTGRES_USER: ticketsnap
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

### Configuración de Webhook

Configurar el webhook de WhatsApp para apuntar a:
```
https://your-domain.com/webhook/whatsapp
```

Con método POST para recepción de mensajes y GET para verificación.

---

## Monitoreo y Mantenimiento

### Logging

El sistema implementa logging estructurado con los siguientes niveles:
- `INFO`: Operaciones normales
- `WARNING`: Situaciones que requieren atención
- `ERROR`: Errores que no impiden la operación
- `CRITICAL`: Errores que requieren intervención inmediata

### Métricas

- Tiempo de procesamiento por ticket
- Tasa de éxito de OCR
- Volumen de mensajes procesados
- Errores por tipo y frecuencia

---

## Soporte y Mantenimiento

### Soporte Técnico

Para soporte técnico, contactar a:
- **Email**: info@archive.com.mx
- **Sitio Web**: [archive.com.mx](https://archive.com.mx/)
- **Documentación**: [docs.archive.com.mx/ticket-snap](https://docs.archive.com.mx/ticket-snap)
- **Portal de Soporte**: [support.archive.com.mx](https://support.archive.com.mx)

### Actualizaciones

Las actualizaciones se distribuyen siguiendo versionado semántico:
- **Major**: Cambios incompatibles hacia atrás
- **Minor**: Nueva funcionalidad compatible
- **Patch**: Correcciones de bugs

---

## Licencia

Este proyecto está licenciado bajo los términos de la Licencia MIT. Ver el archivo [LICENSE](LICENSE) para más detalles.

```
Copyright (c) 2024 Archive México

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software...
```

---

<div align="center">

**TicketSnap** - Desarrollado por **[Archive México](https://archive.com.mx/)**

*Automatización empresarial inteligente*

</div>