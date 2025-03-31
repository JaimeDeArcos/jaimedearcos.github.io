---
date: 2025-01-15T11:00:00.000Z
layout: post
title: Implementa un analizador de consultas en lenguaje natural para bases de datos SQL con GPT
subtitle: Transforma preguntas naturales en consultas SQL automatizadas
description: Aprende a implementar un sistema que convierte consultas en lenguaje natural en sentencias SQL usando OpenAI GPT, con un tutorial paso a paso y ejemplos prácticos.
image: /assets/img/uploads/gpt-sql-integration.png
optimized_image: /assets/img/uploads/gpt-sql-integration.png
category: code
tags:
  - code
  - gpt
  - openai
  - sql
  - ai
  - nlp
author: jaimedearcos
paginate: false
---

> Convertir preguntas naturales en consultas SQL de forma automática simplifica la interacción con bases de datos, permitiendo que usuarios sin conocimientos técnicos extraigan datos relevantes. En este tutorial, aprenderás a implementar esta solución utilizando la API de OpenAI GPT.

## Requisitos Previos

- Cuenta y clave API de OpenAI
- Python instalado en tu entorno
- Base de datos SQL (ej.: PostgreSQL, MySQL)

## Paso 1: Configuración del entorno

Primero, instala las dependencias necesarias:

```bash
pip install openai python-dotenv sqlalchemy psycopg2-binary
```

Guarda tu clave API en un archivo `.env`:

```env
OPENAI_API_KEY='tu-api-key'
```

## Paso 2: Conexión a la base de datos

Utiliza SQLAlchemy para conectarte a tu base de datos:

```python
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker
import os
from dotenv import load_dotenv

load_dotenv()

DATABASE_URL = os.getenv("DATABASE_URL")
engine = create_engine(DATABASE_URL)
Session = sessionmaker(bind=engine)
session = Session()
```

## Paso 3: Obtener esquema de la base de datos

Genera una descripción del esquema que será utilizada por GPT:

```python
from sqlalchemy import inspect

def get_database_schema(engine):
    inspector = inspect(engine)
    schema_description = ""
    for table_name in inspector.get_table_names():
        schema_description += f"Table: {table_name}\n"
        for column in inspector.get_columns(table_name):
            schema_description += f"- {column['name']} ({column['type']})\n"
        schema_description += "\n"
    return schema_description

schema_description = get_database_schema(engine)
print(schema_description)
```

## Paso 4: Preparar la consulta con GPT

Utiliza la API de OpenAI para transformar preguntas en consultas SQL, incluyendo el esquema de la base de datos:

```python
import openai
import os

openai.api_key = os.getenv("OPENAI_API_KEY")

def generate_sql(question, schema):
    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[
            {"role": "system", "content": f"Convierte preguntas en lenguaje natural a consultas SQL. Esquema de la base de datos:\n{schema}"},
            {"role": "user", "content": question}
        ]
    )
    sql_query = response.choices[0].message.content.strip()
    return sql_query

query = generate_sql("¿Cuántos usuarios se registraron el último mes?", schema_description)
print(query)
```

## Paso 5: Ejecutar consultas generadas

Ejecuta la consulta generada y obtiene resultados:

```python
result = session.execute(query)
for row in result:
    print(row)
```

## Paso 6: Manejo de errores y seguridad

Para evitar problemas con consultas inseguras o errores, siempre valida las consultas generadas por GPT antes de ejecutarlas directamente:

```python
def execute_safe_query(sql_query):
    try:
        result = session.execute(sql_query)
        return result.fetchall()
    except Exception as e:
        print(f"Error al ejecutar consulta: {e}")
        return None

results = execute_safe_query(query)
print(results)
```

## Buenas prácticas

- Siempre revisa o limita el acceso de los usuarios a consultas críticas o sensibles.
- Implementa validaciones adicionales para asegurar la integridad y seguridad de tu base de datos.
- Utiliza tokens y modelos específicos de OpenAI optimizados para tareas SQL.

## Conclusión

Utilizar GPT para transformar consultas en lenguaje natural a SQL facilita enormemente la accesibilidad a los datos. Implementando este sistema, podrás ofrecer una experiencia de usuario más amigable y eficiente.

# Recursos adicionales

- [Documentación API OpenAI](https://platform.openai.com/docs/api-reference)
- [SQLAlchemy ORM Documentation](https://docs.sqlalchemy.org/en/20/)
- [Python Dotenv](https://pypi.org/project/python-dotenv/)

