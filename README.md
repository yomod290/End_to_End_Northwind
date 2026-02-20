[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?logo=linkedin)](https://www.linkedin.com/in/yonathanmontenegro/)
[![GitHub](https://img.shields.io/badge/GitHub-Repo-181717?logo=github&logoColor=white)](https://github.com/yomod290/End_to_End_Northwind)
[![Notion Project](https://img.shields.io/badge/Notion-Project-black?logo=notion&logoColor=white)](https://yonathan-montenegro.notion.site/AdventureWorks-Azure-Lakehouse-DEV-PROD-ETL-End-to-End-2fcc4265055380a4832cd26f9ea11821)
[![Notion Portafolio](https://img.shields.io/badge/Notion-Portafolio-black?logo=notion&logoColor=white)](https://yonathan-montenegro.notion.site/portafolio-data-engineer)
[![Azure](https://img.shields.io/badge/Azure-Cloud-0078D4?logo=microsoftazure&logoColor=white)](https://azure.microsoft.com/)
[![Databricks](https://img.shields.io/badge/Databricks-Lakehouse-EF3D2C?logo=databricks&logoColor=white)](https://www.databricks.com/)
[![PySpark](https://img.shields.io/badge/PySpark-Data%20Processing-E25A1C?logo=apachespark&logoColor=white)](https://spark.apache.org/docs/latest/api/python/)



# 🚀 Northwind – Azure Lakehouse (DEV/PROD) ETL End-to-End
---
<img width="1589" height="802" alt="image" src="https://github.com/user-attachments/assets/5e32face-4e37-44c2-9f1b-4420f20ec2af" />

## 📌 Descripción del Proyecto

Este proyecto implementa una solución completa de **Data Engineering en Azure**, basada en arquitectura **Lakehouse (Bronze → Silver → Gold)** utilizando:

- Azure Data Factory (ADF)
- Azure Data Lake Storage Gen2
- Azure Synapse Analytics (Serverless SQL + Spark)
- Delta Lake
- GitHub + CI/CD
- Entornos separados DEV y PROD

Se desarrolla un pipeline ETL end-to-end utilizando el dataset clásico **Northwind**, aplicando buenas prácticas empresariales como separación de ambientes, control de versiones, despliegue automatizado y modelado analítico optimizado para BI.

---

## 🎯 Objetivo General

Diseñar e implementar una arquitectura moderna de datos en Azure que permita procesar datos desde su estado crudo hasta un modelo analítico optimizado para reporting, utilizando prácticas reales de Data Engineering en un entorno empresarial con DEV y PROD.

---

## 🎯 Objetivos Específicos

- Implementar arquitectura Medallion (Bronze, Silver, Gold)
- Configurar Azure Data Lake Storage Gen2 como almacenamiento principal
- Desarrollar pipelines de ingesta con Azure Data Factory
- Transformar datos con Spark (Delta Lake)
- Crear vistas analíticas en Synapse Serverless SQL
- Implementar CI/CD con GitHub Actions
- Separar ambientes DEV y PROD
- Consumir la capa Gold desde Power BI

---

# 🏗️ Arquitectura del Proyecto

Fuente de datos
↓
Azure Data Factory
↓
Azure Data Lake Storage Gen2
├── Bronze (Raw)
├── Silver (Curated - Delta)
└── Gold (Vistas analíticas)
↓
Synapse Serverless SQL
↓
Power BI


---

# 🧱 Arquitectura Medallion

## 🥉 Bronze Layer (Raw)

- Datos cargados sin transformación.
- Archivos almacenados en formato parquet/delta.
- Estructura organizada por dominio.

Ejemplo:
/bronze/orders/
/bronze/customers/


---

## 🥈 Silver Layer (Curated)

- Limpieza y transformación de datos.
- Cast de tipos.
- Eliminación de nulos.
- Estandarización.
- Escritura en formato **Delta Lake**.

Ejemplo:
/silver/orders/
/silver/order_details/


---

## 🥇 Gold Layer (Analítica)

- Creación de vistas en Synapse Serverless.
- Modelo optimizado para BI.
- Cálculo de métricas de negocio.

Ejemplo de vista:

```sql
CREATE OR ALTER VIEW gold.gold_order_details AS
SELECT
    OrderID,
    ProductID,
    UnitPrice,
    Quantity,
    Discount,
    (UnitPrice * Quantity) * (1 - Discount) AS TotalAmount
FROM OPENROWSET(
    BULK 'order_details/',
    DATA_SOURCE = 'NorthwindDataLake',
    FORMAT = 'DELTA'
) AS rows;
```

---
# ⚙️ Componentes Técnicos

## 🔹 Azure Data Factory
- Pipeline de ingesta de datos.
- Parametrización por entorno (DEV / PROD).
- Integración con GitHub.
- Publicación de artefactos en la rama `workspace_publish`.
- Orquestación del flujo Bronze → Silver.

## 🔹 Azure Data Lake Storage Gen2
- Contenedores organizados por capa:
  - `/bronze`
  - `/silver`
  - `/gold`
- Estructura organizada por dominio de negocio.
- Separación por entorno.

## 🔹 Azure Synapse Analytics
- Serverless SQL Pool para capa Gold.
- Creación de External Data Source.
- Creación de vistas analíticas.
- Integración directa con Power BI.
- Separación de entornos DEV / PROD.

## 🔹 Spark (Delta Lake)
- Transformaciones Bronze → Silver.
- Escritura en formato Delta.
- Limpieza y estandarización de datos.
- Optimización para consumo analítico.

## 🔹 CI/CD – GitHub Actions
- Despliegue automático desde `workspace_publish`.
- Workflow manual (`workflow_dispatch`).
- Selección dinámica de entorno (DEV / PROD).
- Manejo de secretos mediante GitHub Secrets.
- Uso del action `Azure/Synapse-workspace-deployment`.

---

# 🌍 Gestión de Entornos

| Entorno | Storage Account       | Resource Group        | Workspace              |
|----------|-----------------------|------------------------|------------------------|
| DEV      | adsldevnorthwind      | GR-DEV-NORTHWIND       | devsynapsenorthwind    |
| PROD     | adslprodnorthwind     | GR-PROD-NORTHWIND      | prodsynapsenorthwind   |

El despliegue se realiza mediante GitHub Actions seleccionando:
- Entorno destino
- Rama fuente (`workspace_publish`)

Esto permite replicar una estructura empresarial real con separación controlada de ambientes.

---

# 📊 Consumo en Power BI

- Conexión directa a Synapse Serverless.
- Importación de vistas Gold.
- Modelo relacional optimizado.
- Construcción de métricas y visualizaciones ejecutivas.

### 📈 Métricas implementadas
- Total Sales
- Sales by Customer
- Sales by Product
- Sales by Category
- Revenue por periodo

---

# 🔐 Buenas Prácticas Implementadas

- Separación DEV / PROD.
- Arquitectura Medallion.
- Control de versiones con Git.
- CI/CD automatizado.
- Uso de Delta Lake.
- Modelo analítico desacoplado de la capa transaccional.
- Infraestructura organizada por entorno.

---

# 📈 Resultados

✔ Pipeline ETL completamente funcional  
✔ Arquitectura Lakehouse implementada  
✔ Capa Silver optimizada en Delta  
✔ Vistas Gold para consumo analítico  
✔ Integración con Power BI  
✔ Deploy automatizado con GitHub Actions  
✔ Proyecto estructurado como entorno empresarial real  

---

# 🧠 Aprendizajes Clave

- Diferencia entre `main` y `workspace_publish`.
- Funcionamiento interno del Publish en Synapse.
- Uso de `OPENROWSET` con formato Delta.
- Configuración de External Data Source en Serverless SQL.
- Manejo de errores en CI/CD.
- Diseño de arquitectura empresarial en Azure.
- Modelado analítico para BI.


---

👤 Autor
Yonathan Montenegro Martínez

---

📅 Fecha
Febrero 2026

---
Este proyecto demuestra la implementación completa de una solución moderna de Data Engineering en Azure, aplicando buenas prácticas reales de arquitectura, separación de entornos, despliegue automatizado y modelado analítico.

Representa un escenario empresarial real preparado para escalar en entornos productivos.
