# 🚀 AWS Serverless ETL Pipeline (IaC con CloudFormation)

Este repositorio contiene una solución 100% Serverless para construir una canalización ETL totalmente automatizada sobre AWS.

Toda la infraestructura es desplegada utilizando **Infraestructura como Código (IaC)** con **AWS CloudFormation**, lo que permite reproducibilidad, escalabilidad y cero configuración manual.

El objetivo es transformar datos crudos de ventas (JSON) en un formato optimizado **Parquet**, almacenarlos en un Data Lake y dejarlos listos para ser consultados con SQL mediante **Amazon Athena**.

## 🏛️ Arquitectura de la Solución

Este pipeline está basado en eventos: todo ocurre automáticamente en cuanto un archivo llega al Data Lake.

![Arquitectura](arquitectura_etl_pipeline.png)

## 🔄 Flujo Completo del ETL

1.  **Ingesta:** Un archivo JSON es cargado en la carpeta de entrada del bucket S3: `orders-json-incoming/`.
2.  **Activación Automática:** S3 detecta el nuevo archivo y ejecuta una función AWS Lambda mediante un trigger nativo.
3.  **Transformación:** La Lambda procesa el archivo utilizando Python + Pandas:
    * Lee el JSON.
    * Aplana la estructura.
    * Convierte los datos al formato Parquet optimizado para análisis.
4.  **Almacenamiento Optimizado:** El archivo procesado se mueve a la carpeta de salida en el Data Lake: `orders_parquet_datalake/`.
5.  **Catalogación:** La Lambda invoca automáticamente un crawler de AWS Glue para actualizar las tablas del Data Catalog.
6.  **Análisis:** En minutos, los datos están listos para ser consultados en Amazon Athena usando SQL estándar.

## 🛠️ Tecnologías Utilizadas

| Servicio | Rol en la solución |
| :--- | :--- |
| **AWS CloudFormation** | IaC para definir y desplegar toda la arquitectura. |
| **Amazon S3** | Data Lake (almacenamiento de crudos + procesados). |
| **AWS Lambda** | Motor de transformación ETL (Python 3.12 + Pandas). |
| **AWS Glue Crawler** | Catalogación automática de metadatos. |
| **Amazon Athena** | Consultas SQL interactivas sobre archivos Parquet. |
| **AWS IAM** | Seguridad y control de permisos (mínimo privilegio). |

## 🚀 Despliegue del Proyecto en AWS

Antes de desplegar asegúrate de tener:
* ✔ AWS CLI instalada.
* ✔ Ejecutado `aws configure`.
* ✔ Permisos para crear recursos (S3, Lambda, Glue, IAM, CloudFormation).

### 1️⃣ Preparar el entorno
Abre tu terminal y navega a la carpeta del proyecto:
```bash
cd "/c/proyectos-aws/project-03-etl-pipeline"
