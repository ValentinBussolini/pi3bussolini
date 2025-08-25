# Documento Técnico – Proyecto Integrador AWS: Pipeline ELT con Airbyte y Carga de JSON a PostgreSQL (Docker)

## 1. Objetivo
El objetivo del proyecto fue implementar un **pipeline ELT** que permita extraer datos de una API externa, cargarlos en AWS S3 y transformarlos para análisis posterior, utilizando Airbyte como herramienta de integración.  

Adicionalmente, se incorporó un flujo independiente para la **carga de datos locales en formato JSON hacia una base de datos PostgreSQL en Docker**, garantizando integridad, trazabilidad y disponibilidad para análisis.

---

## 2. Alcance
- Integración de datos de la **API Dólar API**.  
- Configuración de **AWS S3** como data lake para almacenar los datos provenientes de la API.  
- Creación de un **usuario IAM** para que Airbyte acceda a AWS con los permisos adecuados.  
- Configuración de Airbyte para extracción, carga y validación de los datos en **formato Parquet** en la capa raw de S3.  
- Diseño de modelo relacional en **PostgreSQL (Docker)** para ingestión de datos JSON locales.  
- Uso de **Python y Jupyter Notebook** para lectura, transformación y carga de JSON a PostgreSQL.  
- Validaciones de integridad y calidad de datos en ambos flujos.  
- Gestión y versionado del proyecto con **Git y GitHub**.  

---

## 3. Actividades Realizadas

### 3.1. Selección de la fuente de datos (API)
- Se identificó **Dólar API** como fuente de información financiera para el pipeline.  
- Se revisaron endpoints y formatos de respuesta, confirmando disponibilidad en JSON.  

### 3.2. Configuración de AWS
- Creación de un **bucket S3** para almacenar la capa raw.  
- Definición de rutas, estructura de carpetas y región de despliegue.  
- Creación de usuario **IAM** específico (`airbyte`) con:
  - Permisos de lectura/escritura en el bucket S3.  
  - Acceso seguro mediante **Access Key ID** y **Secret Access Key**.  

### 3.3. Configuración de Airbyte
- Creación de **source** apuntando a la API Dólar API.  
- Configuración de **destination** hacia el bucket S3.  
- Parámetros de conexión:
  - Formato de salida: **Parquet**  
  - Frecuencia: **cada 24 h** o ejecución manual  
  - Sin compresión adicional ni flattening de datos  

### 3.4. Validación del flujo API–S3
- Ejecución de extracción y verificación de:
  - Datos obtenidos correctamente en formato JSON.  
  - Archivos almacenados en S3 como Parquet.  
  - Pipeline operativo tanto en ejecución programada como manual.  

### 3.5. Carga de datos JSON locales a PostgreSQL en Docker
- Flujo independiente del pipeline API–S3, destinado a almacenar datos locales en **PostgreSQL (Docker)**.  
- Pasos realizados:
  1. Preparación del entorno Docker:
     - Contenedor PostgreSQL con volúmenes persistentes.  
     - Configuración de credenciales, puertos y acceso local.  
  2. Creación de esquema y tablas (DBeaver):
     - Conexión al contenedor PostgreSQL.  
     - Diseño del modelo relacional con claves primarias/foráneas.  
     - Tipos de datos definidos: `INTEGER`, `NUMERIC`, `VARCHAR`, `DATE`, `JSONB`.  
  3. Carga de datos desde JSON local (Python Notebook):
     - Lectura y parseo de JSON con `pandas`.  
     - Normalización de datos para compatibilidad con el esquema.  
     - Inserción en tablas mediante `sqlalchemy` y control de errores.  
     - Registro de logs de carga.  
  4. Validación de integridad:
     - Queries de control: conteo de registros, formatos de fecha, cumplimiento de claves.  
     - Documentación de resultados y ajustes.  

### 3.6. Uso de Git y GitHub
- Control de versiones implementado para todo el ciclo de vida del proyecto:
  - Repositorio GitHub privado para código, notebooks, documentación y archivos de configuración (`.env`, `docker-compose.yml`, `.gitignore`).  
  - Ramas de desarrollo para aislar cambios.  
  - Commits descriptivos indicando cambios funcionales o correcciones (`fix:`, `feat:`, `docs:`).  
  - Uso de `.gitignore` para excluir:
    - Archivos de entorno y credenciales.  
    - Carpetas `data/` y `notebooks/` con datos sensibles o temporales.  
    - Archivos binarios y pesados.  
  - Pull requests y revisiones internas antes de integrar cambios.  
  - Historial completo para auditoría y trazabilidad.  

---

## 4. Resultados
- **Flujo API–S3** operativo, programado y validado.  
- **Flujo JSON local–PostgreSQL** implementado y probado.  
- Base de datos **PostgreSQL en Docker** lista para análisis y explotación de datos.  
- Scripts y notebooks documentados y versionados en GitHub.  
- Validaciones completadas garantizando integridad, consistencia y trazabilidad.  
- **Separación clara de responsabilidades**:
  - Airbyte gestiona extracción de la API y carga en S3.  
  - Python gestiona carga de JSON locales a PostgreSQL.  
