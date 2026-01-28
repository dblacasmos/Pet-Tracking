# 🐾 PET TRACKING – Sistema de Análisis Emocional y Conductual

### Big Data · Streaming · Machine Learning · AWS · Power BI  
[🇪🇸 Español](./README.es.md) | [🇬🇧 English](./README.md)

---

## 🚀 Visión general

**Pet Tracking** es un sistema de análisis de datos diseñado para **monitorizar, interpretar y anticipar el bienestar animal** a partir de señales fisiológicas, comportamiento y sonido.  
El proyecto se plantea como un **sistema Big Data completo**, combinando **ingesta batch y streaming**, procesamiento distribuido, **Machine Learning integrado directamente en el warehouse** y visualización orientada a la toma de decisiones.

No es un modelo aislado.  
Es una **arquitectura de datos de extremo a extremo**, pensada para escalar y operar en entornos reales.

---

## 🎯 Qué hace el sistema

- Procesa datos heterogéneos (sensores, audio, actividad, eventos).
- Detecta patrones de comportamiento y estados emocionales.
- Identifica situaciones de riesgo mediante reglas y Machine Learning.
- Expone resultados mediante consultas analíticas y dashboards.
- Reduce la latencia entre evento y alerta en escenarios críticos.

---

## 🧠 Arquitectura del sistema (alto nivel)

```plaintext
[Dispositivos IoT / Sensores / Audio / Reportes]
↓
Capa de Ingesta
├── AWS Glue + S3 (Batch)
├── Kinesis + Lambda + Firehose (Streaming)
↓
Data Lake y Almacenamiento Analítico
Amazon S3 / Athena / Redshift
↓
Procesamiento y Machine Learning
EMR (Spark) / Redshift ML
↓
Capa de Decisión
Dashboards Power BI
```

---

## 📥 Fuentes de datos

| Fuente | Datos |
|------|------|
| Wearables y sensores | Ritmo cardíaco, temperatura, actividad, GPS |
| Micrófonos | Vocalizaciones y patrones sonoros |
| Cámaras | Movimiento y postura (extensión conceptual) |
| Reportes humanos | Etiquetas emocionales (alegría, ansiedad, dolor, hambre, etc.) |

---

## 🔄 Ingesta de datos
Ingesta Batch
Origen: Reportes periódicos y datos históricos.

Servicios AWS:

Amazon S3

AWS Glue Studio

Glue Data Catalog

Glue Data Quality

Operaciones clave:

Normalización de esquemas

Forzado de tipos

Validaciones de calidad

Preparación para pipelines reproducibles

Ingesta Streaming (Tiempo real)
Origen: Dispositivos IoT emitiendo eventos continuamente.

Servicios AWS:

Amazon Kinesis Data Streams

AWS Lambda (transformación y enriquecimiento JSON)

Amazon Firehose (entrega a S3)

Formato:

Parquet + Snappy

Particionado por year / month / day

---

## 🧱 Diseño del Data Lake y almacenamiento
Bucket principal:
s3://pet-tracking-data-bucket/

```plaintext
├── raw/
│   ├── batch/
│   └── stream/
├── processed/
├── firehose-output/
├── warehouse/
│   ├── athena/
│   └── redshift/
├── dashboards/
├── logs/
├── archive/
└── s3-management/
```

---

### Políticas de ciclo de vida

| Ruta | Acción | Retención |
|------|--------|-----------|
| raw/batch/ | Glacier | 30 días |
| raw/stream/ | Eliminar | 7 días |
| processed/ | Eliminar | 90 días |
| firehose-output/ | Eliminar | 60 días |

---

## ⚙️ Procesamiento analítico
AWS EMR (Apache Spark)
Procesamiento distribuido para:

Limpieza de datos

Agregaciones por mascota

Clasificación de actividad

Detección de anomalías y alertas

Jobs PySpark ejecutados sobre clusters EMR.

Resultados persistidos en S3 para consumo posterior.

Athena
Consultas SQL ad-hoc sobre datos Parquet particionados.

Uso para exploración, validación y análisis ligero.

Ejemplo:

```sql
SELECT emotion, COUNT(*) AS freq
FROM pet_behavior_parquet
GROUP BY emotion
ORDER BY freq DESC;
```

---

## 🤖 Machine Learning dentro del warehouse
Redshift + Redshift ML
Modelo no supervisado K-Means entrenado directamente en Redshift.

Features utilizadas:

age

heart_rate_bpm

activity_steps

gps_lat, gps_lon

El modelo se utiliza directamente en consultas SQL para asignar clústeres de comportamiento.

No se requieren pipelines ML externos.

Esto permite analítica y Machine Learning en una única capa.

---

## 📊 Visualización y toma de decisiones
Dashboards en Power BI

Estado emocional

Nivel de actividad

Distribución de comportamientos

Indicadores de riesgo y alertas

Diseñados para:

Veterinarios

Cuidadores

Usuarios no técnicos

El foco está en la decisión, no en el detalle técnico.

---

🧩 Por qué este no es un proyecto de juguete
Arquitectura híbrida batch + streaming.

Validaciones explícitas de calidad del dato.

Separación clara entre raw, processed y capa analítica.

Procesamiento distribuido con Spark.

Machine Learning integrado en el warehouse, no en notebooks.

Diseño orientado a escalabilidad, gobernanza y reproducibilidad.

---

## 🛠️ Servicios AWS utilizados
| Capa | Servicios |
|------|-----------|
| Ingesta Batch | S3, Glue Studio, Glue Catalog, Glue Data Quality |
| Ingesta Streaming | Kinesis, Lambda, Firehose |
| Almacenamiento | S3, Athena, Redshift |
| Procesamiento | EMR (Spark), Redshift ML |
| Visualización | Power BI |

---

🧪 Próximos pasos
Deep Learning multimodal (CNN/LSTM) para emociones complejas.

Integración con Amazon SageMaker para orquestación ML.

Aplicación móvil con notificaciones en tiempo real.

Uso de AWS IoT Core para gestión directa de dispositivos.

Detección de concept drift y reentrenamiento de modelos.

---

## 📁 Estructura del proyecto
```plaintext
Copiar código
pet-tracking-project/
├── glue/
├── kinesis/
├── emr/
├── warehouse/
├── dashboards/
├── logs/
└── docs/
```

---

## 📚 Licencia
Licencia MIT.
Uso, modificación y distribución permitidos con atribución.

---

© 2025 – Pet Tracking | Sistema de Analítica Big Data e IA
