# Reporte EDA
Este notebook analiza desde una base de datos relacional en Access.
Hay 4 tablas Relacionadas. Hace análisis temporal y de tops autoridades, municipios o conclusiones de quejas en función del tiempo, etc.

## Conección a Access
Para realizar la conección con Access se utiliza la libreria `pyobdc` para utilizar los drivers de Microsoft Access, especificando el path del archivo de la base de datos y haciendo conectando con el comando `pyobdc.conect()` como se muestra a continuación:
```python
import pyobdc

conn_str = (
  r'Driver={Microsoft Access Driver (*.mdb, *.accdb)}'
  r'DBQ=' + '/path/file.mdb' + ';'
)

conn = pyobdc.conect(conn_str)
```
Se realizan las SQL Querys utilizando la librería de `pandas` por medio del comando `pd.read_sql()`. Finalmente se cierra la conección de pyobdc.

## Preprocesamiento de Datos
Una vez se cargaron las tablas `Quejas`, `Expediente`, `Recomendaciones` y `NoRecomendaciones`. Se realiza un INNER MERGE entre `Quejas` y `Expediente` utilizando como columna `Quejas.Expediente` y `Expediente.Expediente`.
```python
df = pd.merge(
    quejas,
    expediente,
    on='Expediente',
    how='inner',
    suffixes=('', '_expediente')
)
df = df[df['Expediente'].str.contains('/2025', na=False)]
df = df.sort_values(['FechaInicio', 'Expediente'])
df = df.reset_index(drop=True)
```
Se filtran los expedientes del 2025, se ordenan por la fecha de inicio y su número de expediente y finalmente se hace un reset al indice del dataframe. Siendo este nuestro dataframe principal que contiene relacion uno a varios entre las columnas

# E-commerce Database Schema

## Diagrama
```mermaid
erDiagram
    CUSTOMERS ||--o{ ORDERS : places
    ORDERS ||--o{ ORDER_ITEMS : contains
    PRODUCTS ||--o{ ORDER_ITEMS : included_in
    CATEGORIES ||--o{ PRODUCTS : categorizes

    CUSTOMERS {
        int customer_id PK
        string name
        string email
    }
    
    ORDERS {
        int order_id PK
        int customer_id FK
        datetime order_date
    }
