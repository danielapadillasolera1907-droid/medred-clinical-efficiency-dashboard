# 🏥 MedRed | Dashboard de Eficiencia Clínica

## 📌 Descripción del proyecto

Este proyecto consiste en el desarrollo de un dashboard interactivo en Power BI para analizar el desempeño operativo de MedRed, una red privada de hospitales. El objetivo es transformar los datos de pacientes, médicos y consultas en información útil para evaluar la productividad médica, los resultados clínicos, los ingresos y la eficiencia de los servicios.

El análisis está orientado a la gerencia de operaciones, que necesita identificar oportunidades de mejora, comprender el comportamiento de los indicadores y tomar decisiones basadas en datos.

El proyecto abarca el proceso de análisis de datos de principio a fin: limpieza y transformación, modelado relacional, creación de medidas DAX, diseño de visualizaciones y comunicación de hallazgos mediante Data Storytelling.


## 🎯 Objetivos del análisis

* Evaluar la productividad de médicos y hospitales.
* Analizar los ingresos generados por las consultas médicas.
* Identificar diferencias en la duración promedio de las consultas según la especialidad.
* Examinar la distribución de los resultados clínicos.
* Comparar el desempeño de los hospitales y las especialidades.
* Identificar médicos con mayores ingresos y volumen de pacientes atendidos.
* Proponer recomendaciones que apoyen la planificación operativa y la asignación de recursos.

## 🛠️ Herramientas y tecnologías

* **Power BI:** desarrollo del dashboard y visualización de indicadores.
* **Power Query:** limpieza, preparación y transformación de datos.
* **DAX:** creación de medidas, indicadores y cálculos analíticos.
* **Modelado de datos:** construcción de relaciones entre las tablas.
* **Data Storytelling:** comunicación de hallazgos y recomendaciones orientadas al negocio.

## 🔎 1. Limpieza y transformación de datos

Se utilizó Power Query para preparar los datos antes de realizar el análisis. Esta etapa fue importante para mejorar su calidad, reducir inconsistencias y facilitar las relaciones entre las tablas.

Entre las tareas realizadas se encuentran:

* **Corrección de formatos de texto:** normalización de nombres de pacientes y médicos que presentaban problemas de capitalización.
* **Tratamiento de valores nulos:** identificación y tratamiento de campos vacíos, como fechas de nacimiento, hospitales, ciudades y diagnósticos.
* **Revisión de valores inválidos:** detección de valores inconsistentes, como categorías de género no contempladas y tarifas de consulta iguales a cero.
* **Eliminación de duplicados:** revisión de registros repetidos en la información de pacientes.
* **Validación de integridad referencial:** identificación de consultas asociadas a pacientes inexistentes y tratamiento de registros huérfanos.
* **Corrección de fechas:** estandarización de fechas que presentaban formatos diferentes.
* **Preparación de los datos para el análisis:** revisión de los tipos de datos y adecuación de los campos necesarios para las medidas y visualizaciones.

Estas transformaciones permiten trabajar con información más consistente y reducen el riesgo de obtener indicadores incorrectos.

## 🗂️ 2. Modelado de datos

Se construyó un modelo relacional para conectar la información de las tres tablas principales:

* **Pacientes:** contiene información demográfica de los pacientes.
* **Médicos:** contiene información de los profesionales, sus especialidades, hospitales y tarifas.
* **Consultas:** registra la actividad médica y funciona como tabla central de los eventos analizados.

Se establecieron las siguientes relaciones:

* Pacientes (1) → (*) Consultas.
* Médicos (1) → (*) Consultas.

Este modelo permite analizar las consultas desde diferentes perspectivas, como el paciente, el médico, la especialidad y el hospital.

También se trabajó con una tabla de fechas para facilitar el análisis temporal y organizar los datos por periodos.

## 🧮 3. Medidas DAX e indicadores

Se crearon medidas DAX para convertir los datos en indicadores que permitan evaluar el desempeño de la red hospitalaria.

### Ingresos Médicos

Calcula los ingresos asociados a las consultas a partir de la tarifa del médico relacionado con cada registro.

```DAX
Ingresos Médicos =
SUMX(
    Consultas,
    RELATED(Medicos[TarifaConsulta])
)
```

**¿Para qué se utiliza?**

Permite analizar los ingresos por médico, especialidad y hospital. Para que el indicador sea correcto, es necesario que las tarifas y las reglas de cálculo representen adecuadamente los ingresos reales.

### Duración Promedio

```DAX
Duración Prom =
AVERAGE(Consultas[Duracion_min])
```

**¿Para qué se utiliza?**

Permite conocer cuánto duran, en promedio, las consultas y comparar la duración entre especialidades y médicos.

### Porcentaje de Altas

```DAX
% Alta =
DIVIDE(
    CALCULATE(
        COUNTROWS(Consultas),
        Consultas[Resultado] = "Alta"
    ),
    COUNTROWS(Consultas),
    0
)
```

**¿Para qué se utiliza?**

Mide la proporción de consultas cuyo resultado fue un alta médica. Facilita la comparación de resultados entre los hospitales y ayuda a identificar diferencias que podrían requerir una revisión adicional.

### Pacientes Únicos

```DAX
Pacientes Únicos =
DISTINCTCOUNT(Consultas[PacienteID])
```

**¿Para qué se utiliza?**

Cuenta los pacientes distintos presentes en el contexto de análisis. Permite comparar el volumen de pacientes atendidos por médico, especialidad u hospital.

### Ingreso por Minuto

```DAX
Ingreso/Minuto =
DIVIDE(
    [Ingresos Médicos],
    SUM(Consultas[Duracion_min]),
    0
)
```

**¿Para qué se utiliza?**

Relaciona los ingresos calculados con el tiempo acumulado de consulta. Puede utilizarse como indicador complementario de productividad económica, siempre considerando las tarifas, los costos y la complejidad de los servicios.

### Otros indicadores del análisis

Además de las medidas anteriores, el dashboard incorpora indicadores para evaluar:

* Total de consultas.
* Porcentaje de altas médicas.
* Especialidad con mayor duración promedio.
* Médicos con mayores ingresos.
* Médicos con mayor cantidad de pacientes únicos.
* Médicos cuya duración promedio de consulta supera el límite superior definido para el análisis.

Estos indicadores ayudan a resumir el desempeño y a dirigir la atención hacia los aspectos más relevantes.

## 📊 4. Diseño del dashboard y justificación de las visualizaciones

Las visualizaciones se seleccionaron según las preguntas de negocio que debía responder el dashboard. El objetivo fue facilitar las comparaciones y permitir que la gerencia identifique rápidamente los principales resultados.

### Tarjetas KPI

**Propósito:** mostrar los principales indicadores de la red hospitalaria en una sola vista.

Se utilizan para presentar métricas como el total de consultas, los ingresos médicos, el porcentaje de altas y la duración promedio.

**Justificación:** las tarjetas permiten consultar rápidamente los resultados generales antes de profundizar en los gráficos.

### Gráfico de barras 100 % apiladas: Resultados por Hospital

**Propósito:** comparar la composición de los resultados de las consultas entre hospitales.

Este gráfico representa la proporción de resultados como Alta, Tratamiento y Cirugía en relación con el total de cada hospital.

**Justificación:** al normalizar cada barra al 100 %, facilita la comparación de proporciones entre hospitales con volúmenes de consultas diferentes. Para conocer las cantidades absolutas, debe complementarse con indicadores de volumen.

### Gráfico de duración promedio por especialidad

**Propósito:** identificar las especialidades que registran consultas de mayor o menor duración promedio.

**Justificación:** las barras permiten comparar las especialidades de forma directa y reconocer diferencias que podrían justificar un análisis más profundo de la complejidad de los casos y de la planificación de agendas.

### Gráfico circular de resultados de las consultas

**Propósito:** mostrar cómo se distribuyen los resultados clínicos dentro del conjunto de datos seleccionado.

**Justificación:** permite visualizar la participación relativa de cada categoría de resultado. Es apropiado para una cantidad limitada de categorías; si se necesitan comparaciones más precisas, un gráfico de barras puede ser complementario.

### Tabla Top 5 de médicos por ingresos y pacientes únicos

**Propósito:** identificar a los médicos con mayores ingresos calculados y observar su volumen de pacientes únicos.

**Justificación:** la tabla permite comparar dos indicadores en un mismo lugar y consultar los valores exactos. También ayuda a detectar diferencias entre volumen de pacientes e ingresos, sin asumir que un ingreso mayor significa automáticamente mayor eficiencia.

### Filtros interactivos

Se incorporaron filtros por hospital, especialidad y nombre del médico.

**Justificación:** permiten que la gerencia explore los resultados según sus necesidades, compare grupos específicos y encuentre información relevante sin tener que revisar tablas extensas.

## 📈 5. Principales hallazgos

Los siguientes hallazgos se identificaron durante la interpretación del dashboard:

### 1. Neurología registra una duración promedio de consulta elevada

El dashboard muestra que Neurología presenta una de las mayores duraciones promedio entre las especialidades analizadas.

Una posible explicación es que algunos casos neurológicos requieran evaluaciones detalladas. Sin embargo, esta hipótesis debe contrastarse con información sobre los diagnósticos y la complejidad de las consultas.

**Recomendación:** analizar la duración según el tipo de caso y la carga de trabajo para determinar si es necesario ajustar la programación de citas o la distribución de recursos.

### 2. Clínica del Norte presenta un volumen destacado de cirugías

En el análisis se observó que Clínica del Norte registra 413 cirugías, equivalentes al 4,47 % según el indicador utilizado en el dashboard.

Este resultado debe interpretarse considerando el total que sirve como denominador y el volumen de actividad de cada hospital.

**Recomendación:** comparar las cantidades y proporciones de cirugías entre hospitales, junto con la especialidad y la complejidad de los casos, para evaluar las necesidades de recursos y la distribución de la actividad.

### 3. Una médica de Neurología encabeza los ingresos registrados

La Dra. Liliana María Cadenas aparece entre los resultados analizados como la profesional con mayores ingresos, con un monto registrado de $125.550.000 y 373 pacientes atendidos.

Este resultado permite identificar una diferencia relevante en los ingresos y el volumen de pacientes. No demuestra por sí solo que una especialidad sea más rentable o eficiente que otra.

**Recomendación:** comparar tarifas, cantidad de consultas, tiempo de atención y complejidad de los casos para comprender qué factores explican las diferencias observadas.

## 💡 6. Valor para la gerencia

El dashboard fue diseñado para una audiencia ejecutiva que dispone de poco tiempo para revisar información operativa.

En lugar de exigir la revisión individual de miles de registros, reúne los principales indicadores, comparaciones y filtros en una herramienta interactiva.

Su objetivo es que una persona de gerencia pueda comprender los principales resultados en menos de 10 minutos y profundizar únicamente en los indicadores que requieren atención.

Entre los beneficios esperados se encuentran:

* **Acceso rápido a la información:** centralización de indicadores relevantes en un solo lugar.
* **Menor esfuerzo de exploración:** presentación visual de los resultados en lugar de revisar registros individualmente.
* **Comparaciones más sencillas:** análisis de hospitales, especialidades y médicos mediante filtros interactivos.
* **Apoyo a la toma de decisiones:** identificación de diferencias que pueden orientar investigaciones y acciones posteriores.
* **Comunicación ejecutiva:** presentación de hallazgos y recomendaciones de forma clara y estructurada.

El objetivo de comprender los resultados en menos de 10 minutos corresponde al diseño del dashboard. 
## 📝 7. Conclusión

El proyecto MedRed demuestra cómo Power BI, Power Query y DAX pueden utilizarse para transformar datos operativos de salud en indicadores y visualizaciones orientados al negocio.

A través de la limpieza de datos, el modelado relacional, la creación de medidas y el diseño de un dashboard interactivo, fue posible explorar diferencias en la duración de las consultas, los resultados clínicos, la actividad hospitalaria y los ingresos registrados.

Los hallazgos permiten plantear preguntas y recomendaciones sobre la programación de consultas, la distribución de recursos y el análisis de ingresos. Estas propuestas deben validarse con información adicional antes de implementar cambios operativos.

El proyecto integra habilidades técnicas de análisis de datos con la capacidad de interpretar resultados y comunicarlos a una audiencia ejecutiva.
## 📊 Visualización del Dashboard

### Dashboard principal

![Dashboard principal de MedRed](medred-dashboard-principal.png)

### Análisis de médicos

![Análisis de médicos de MedRed](medred-analisis-medicos.png)
![Análisis de médicos de MedRed](medred-analisis-medicos2.png)

## 👩‍💻 Autora
Daniela Padilla Solera 

Proyecto desarrollado como parte de mi portafolio de análisis de datos, con el objetivo de aplicar conocimientos de Power BI, Power Query, DAX, modelado de datos y Data Storytelling a un caso de negocio.


