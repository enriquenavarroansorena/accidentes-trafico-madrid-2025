# 🚦 Accidentes de tráfico en Madrid 2025: ¿dónde y cuándo ocurren los más graves?

Análisis exploratorio y dashboard en Excel sobre los 21.685 accidentes de tráfico registrados por la
Policía Municipal de Madrid durante 2025.

## 📖 Descripción

Este proyecto analiza los accidentes de tráfico de la ciudad de Madrid en 2025 desde la perspectiva de
la **seguridad vial**: no solo *cuántos* accidentes hay, sino **dónde, cuándo y a quién** le ocurren los
más graves.

La diferencia entre ambas preguntas es el hilo conductor del análisis: el distrito con más accidentes no
tiene por qué ser el más peligroso, y la hora con más siniestros no es necesariamente la que más víctimas
graves deja.

El trabajo cubre el ciclo completo: obtención de los datos originales, transformación y limpieza con
**Power Query**, análisis descriptivo con **tablas dinámicas** y un **dashboard** interactivo, todo dentro
de un único libro de Excel.

## 🗂 Estructura del proyecto

```
├── data/
│   ├── accidentes_trafico_madrid_2025.csv   # Datos originales sin modificar (9,4 MB)
│   └── FUENTE.md                            # Origen, licencia y detalles del dataset
├── accidentes_madrid_2025.xlsx              # Libro con todo el proceso
└── README.md                                # Este archivo
```

Hojas del libro de Excel:

| Hoja | Contenido |
|---|---|
| `Datos_Limpios` | Tabla resultante de Power Query: 51.067 filas × 34 columnas |
| `Analisis` | Tablas dinámicas del análisis descriptivo |
| `Dashboard` | KPIs, gráficos y segmentadores |
| `Informe` | Conclusiones del análisis |

## 📊 Los datos

| | |
|---|---|
| **Fuente** | [Portal de Datos Abiertos del Ayuntamiento de Madrid](https://datos.madrid.es/dataset/300228-0-accidentes-trafico-detalle) |
| **Origen** | Dirección General de la Policía Municipal |
| **Periodo** | Año 2025 completo |
| **Volumen** | 51.067 registros · 19 columnas originales |
| **Licencia** | CC BY 4.0 — © Ayuntamiento de Madrid |

⚠️ **Clave para interpretar los datos:** cada fila **no es un accidente, sino una persona implicada**
en él. Los 51.067 registros corresponden a **21.685 accidentes** (2,4 personas implicadas de media).
La columna `num_expediente` es la que agrupa a los implicados de un mismo siniestro.

## 🛠 Herramientas

- **Microsoft Excel**: Power Query (limpieza y transformación), tablas dinámicas, gráficos dinámicos y segmentadores.
- No se ha usado ningún lenguaje de programación: todo el proceso es reproducible desde el propio libro.

## 🧹 Transformación y limpieza

Todo el proceso está registrado como pasos de Power Query en la consulta `Accidentes`, por lo que es
**reproducible y auditable**: basta con actualizar la consulta para reconstruir la tabla limpia desde el
CSV original.

Resumen de las transformaciones:

1. **Importación** del CSV (UTF-8, delimitador `;`) respetando las comillas de texto.
2. **Tipado manual** de las columnas, en lugar del automático.
3. **Normalización de categorías**: unificación de valores desconocidos bajo la etiqueta `Sin dato`,
   corrección de `LLuvia intensa` y unificación de las dos variantes de `Peatón`.
4. **Tratamiento de huecos**: `positiva_alcohol` y `positiva_droga` solo marcaban los positivos, así que
   los vacíos se convirtieron en `N`.
5. **15 columnas calculadas** para el análisis: `Gravedad`, `Grupo_vehiculo`, `Franja_horaria`,
   `Dia_nombre`, `Es_vulnerable`, `Es_victima`, `Edad_orden`, `Es_interseccion`, entre otras.

### Decisiones de limpieza que merecen explicación

- **No se eliminaron los duplicados.** El dataset contiene 2.065 filas exactamente idénticas, pero **no
  son errores**: son personas distintas con los mismos atributos implicadas en el mismo accidente (por
  ejemplo, dos pasajeros del mismo tramo de edad y sexo en un mismo vehículo). Como no existe un
  identificador de persona, eliminarlas habría supuesto **perder víctimas reales**.
- **La columna `numero` se mantuvo como texto.** El tipado automático de Excel la convertía a número y
  convertía en error las 331 filas con códigos de vía como `+00500E` o `12RE09`.
- **Los vacíos de `lesividad` no se imputaron.** El 44,3% de los registros no tiene dato de lesividad, y
  se ha mantenido como categoría propia `Sin dato` en lugar de asumir que significan "sin lesión".

### Errores detectados durante el proceso

Dos de ellos no generaban ningún mensaje de error y solo se detectaron **cuadrando los totales
esperados con los obtenidos**:

| Problema | Impacto | Solución |
|---|---|---|
| `Text.Contains` distingue mayúsculas: `"Camión"` no capturaba `Tractocamión` | 242 filas mal clasificadas de forma silenciosa | Sustituido por listas de valores exactos con `List.Contains` |
| `QuoteStyle.None` ignoraba las comillas del CSV | 5 filas con `;` dentro del campo `localizacion` se partían en 20 campos y desplazaban todas sus columnas | Cambiado a `QuoteStyle.Csv` |
| Los huecos eran texto vacío `""`, no `null` | Los reemplazos de nulos no surtían efecto | Paso `Table.TransformColumns` que cubre `null`, `""` y espacios |

## 📈 Resultados y conclusiones

> 🚧 En construcción: se completará al finalizar el análisis descriptivo y el dashboard.

## 🔄 Próximos pasos

> 🚧 En construcción.

## 🤝 Contribuciones

Este es un proyecto formativo personal. Cualquier sugerencia de mejora es bienvenida a través de una
issue o un pull request.

## ✒️ Autor

- Enrique Navarro — [@enriquenavarroansorena](https://github.com/enriquenavarroansorena)

Datos: © Ayuntamiento de Madrid, bajo licencia [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).
