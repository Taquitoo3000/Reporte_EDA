# Reporte EDA
Este notebook analiza desde una base de datos relacional en Access.
Hay 4 tablas Relacionadas. Hace análisis temporal y de tops autoridades, municipios o conclusiones de quejas en función del tiempo, etc.

## Conección a Access
Para realizar la conección con Access se utiliza la libreria `pyobdc` para utilizar los drivers de Microsoft Access, especificando el path del archivo de la base de datos y haciendo conectando con el comando `pyobdc.conect` como se muestra a continuación:
```python
import pyobdc

conn_str = (
  r'Driver={Microsoft Access Driver (*.mdb, *.accdb)}'
  r'DBQ=' + '/path/file.mdb' + ';'
)

conn = pyobdc.conect(conn_str)
```
