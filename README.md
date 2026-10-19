# Práctica: Conectividad y Transformación de Datos en Power BI

## 📌 Descripción del Proyecto
Preparación y limpieza de un set de datos de ventas proveniente de un sistema *legacy* (`Ventas_export_legacy.xlsx`). El objetivo fue realizar las etapas de extracción y transformación de datos (ETL) dentro de Power Query para dejar el modelo listo para análisis.

---

## 🛠️ Pasos de Transformación Aplicados

### 1. Renombrado de Columnas
Se reemplazaron los nombres técnicos del sistema original por nombres descriptivos en formato `snake_case` para facilitar la lectura y el mantenimiento por parte del equipo de análisis:
* `COD_CLI` ➔ `id_cliente`
* `NOM_CLI` ➔ `nombre_cliente`
* `MAIL_CLI` ➔ `email_cliente`
* `TEL_CLI` ➔ `telefono_cliente`
* `CIU_CLI` ➔ `ciudad_cliente`
* `PROV_CLI` ➔ `provincia_cliente`
* `SEG_CLI` ➔ `segmento_cliente`
* `FLG_ACT` ➔ `estado_activo`
* `F_ALTA_CLI` ➔ `fecha_alta_cliente`
* `COD_OP` ➔ `id_operacion`
* `F_VTA` ➔ `fecha_venta`
* `COD_PROD` ➔ `id_producto`
* `DESC_PROD` ➔ `desc_producto`
* `RUBRO_PROD` ➔ `rubro_producto`
* `CANT` ➔ `cantidad`
* `PU_VTA` ➔ `precio_unitario`
* `DTO_PCT` ➔ `descuento_porcentaje`
* `TOT_VTA` ➔ `total_venta`
* `COD_MON` ➔ `codigo_moneda`
* `CANAL_VTA` ➔ `canal_venta`

### 2. Corrección de Tipos de Datos
Se asignó el tipo de dato correspondiente a cada campo para garantizar la integridad de los datos y permitir cálculos/filtros futuros:
* **Fechas (`Date` / `DateTime`):** `fecha_alta_cliente` se configuró como *Fecha* y `fecha_venta` como *Fecha/Hora* para habilitar la inteligencia de tiempo en DAX.
* **Montos y Numéricos Decimales (`Decimal Number`):** `precio_unitario`, `descuento_porcentaje` y `total_venta` se definieron como números decimales para evitar pérdidas de precisión en agregaciones numéricas.
* **Enteros (`Whole Number`):** `cantidad` e `id_producto` se asignaron como números enteros.
* **Identificadores y Texto (`Text`):** `id_cliente` e `id_operacion` se mantuvieron o convirtieron a texto para evitar operaciones matemáticas sobre identificadores.

### 3. Tratamiento de Duplicados y Valores Nulos
* **Duplicados:** Se aplicó la función de eliminación de filas duplicadas en la tabla general para asegurar que no existan registros repetidos en la fuente.
* **Valores Nulos (`null`):**
  * En columnas de contacto o descriptivas (como teléfono/ciudad), los valores `null` se reemplazaron por el texto `"Sin dato"` para conservar los registros de ventas sin perder la transacción.
  * En columnas clave como `id_cliente` o `total_venta`, se eliminaron los nulos mediante filtros para evitar distorsiones en los indicadores principales.

### 4. Normalización de la Estructura (Modelo de Datos)
Para optimizar la estructura del modelo, la tabla original se dividió en dos tablas independientes:
* **`Dim_Clientes` (Tabla de Dimensión):** Contiene exclusivamente la información del cliente (`id_cliente`, `nombre_cliente`, `email_cliente`, etc.). Se aplicó un *Quitar duplicados* sobre `id_cliente` para garantizar la unicidad de la clave primaria.
* **`Fact_Ventas` (Tabla de Hechos):** Contiene la información de las transacciones (`id_operacion`, `fecha_venta`, `total_venta`, etc.) conservando únicamente `id_cliente` como clave foránea para enlazar ambas tablas con una relación de 1 a muchos ($1 : \text{*}$).

---

## 📷 Evidencia de Pasos Aplicados
<img width="1411" height="586" alt="Captura de pantalla Diagrama" src="https://github.com/user-attachments/assets/cd8a70b8-009d-4b28-8a1d-511477f6eebf" />
<img width="407" height="777" alt="Captura de pantalla pasos aplicados" src="https://github.com/user-attachments/assets/e858e80f-dec7-4774-b8fd-7826b6e4c771" />

