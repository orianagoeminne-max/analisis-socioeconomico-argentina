# Medidas DAX

En el dashboard utilicé distintas medidas DAX para calcular los principales indicadores y hacer que los resultados respondan a los filtros de provincia y año.

## Población

### Población total

Calcula la población total según el contexto seleccionado.

```DAX
Población Total =
SUM(Fact_Poblacion[poblacion_total])
```

### Porcentaje de mujeres

Calcula qué porcentaje de la población corresponde a mujeres.

```DAX
% Mujeres en la Población =
DIVIDE(
    [Población Mujeres],
    [Población Total]
)
```

## Empleo

### Empleo registrado

Calcula el total de empleo registrado para el período y provincia seleccionados.

```DAX
Empleo Registrado (miles) =
SUM(Fact_Empleo[empleados_registrados_miles])
```

### Variación interanual del empleo

Compara el empleo registrado con el mismo período del año anterior.

```DAX
Variación Interanual Empleo % =
VAR EmpleoActual =
    [Empleo Registrado (miles)]
VAR EmpleoAnterior =
    CALCULATE(
        [Empleo Registrado (miles)],
        DATEADD(Dim_Fecha[Date], -1, YEAR)
    )
RETURN
    DIVIDE(
        EmpleoActual - EmpleoAnterior,
        EmpleoAnterior
    )
```

## Exportaciones

### Exportaciones totales

Calcula el valor total de las exportaciones según los filtros aplicados.

```DAX
Exportaciones Totales =
SUM(Fact_Exportaciones[valor_exportaciones])
```

### Variación interanual de las exportaciones

Compara las exportaciones con el año anterior.

```DAX
Variación Interanual Exportaciones % =
VAR ExportacionesActuales =
    [Exportaciones Totales]
VAR ExportacionesAñoAnterior =
    CALCULATE(
        [Exportaciones Totales],
        DATEADD(Dim_Fecha[Date], -1, YEAR)
    )
RETURN
    DIVIDE(
        ExportacionesActuales - ExportacionesAñoAnterior,
        ExportacionesAñoAnterior
    )
```

### Participación en las exportaciones nacionales

Calcula la participación de cada provincia sobre el total de exportaciones.

```DAX
Participación Exportaciones Nacionales % =
DIVIDE(
    [Exportaciones Totales],
    CALCULATE(
        [Exportaciones Totales],
        ALL(Dim_Provincia)
    )
)
```

## Otros indicadores

Además de estas medidas, desarrollé otros cálculos para analizar densidad poblacional, población por sexo, esperanza de vida y exportaciones per cápita.

Estas medidas se utilizan junto con los filtros de provincia y año para analizar los datos desde distintos niveles dentro del dashboard.
