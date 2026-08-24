# Taller Práctico Aprendizaje automático supervisado: Regresión y clasificación parte1

Este taller práctico permite aplicar los fundamentos de aprendizaje automático supervisado estudiados durante la primera mitad del curso, mediante la construcción de un modelo de regresión orientado a un caso empresarial. A partir de un conjunto de datos de operaciones o servicios, los participantes desarrollarán un modelo capaz de estimar tiempos o costos de atención considerando distintas variables relacionadas con la complejidad, el esfuerzo y las características de cada solicitud.

Durante la práctica, los participantes trabajarán con regresión lineal simple y multivariable, función de costo, descenso del gradiente, vectorización, escalamiento de características, selección de la tasa de aprendizaje e ingeniería de características. El taller se desarrollará en un notebook de Jupyter y tendrá como resultado un modelo funcional que permita generar predicciones sobre nuevos casos.

## Estructura

- `CapituloXX/README.md`: guía de laboratorio por capítulo.

## Lista de laboratorios

### Capítulo 1

- [Predicción de tiempos y costos de atención mediante regresión lineal](Capitulo01/README.md#predicción-de-tiempos-y-costos-de-atención-mediante-regresión-lineal)
  - Descripción: Construir un modelo que permita estimar el tiempo o costo de atención de una solicitud empresarial a partir de variables como cantidad de usuarios, número de incidencias, complejidad del servicio y horas requeridas. El participante aplicará regresión lineal con una y múltiples variables, función de costo, descenso del gradiente, vectorización, escalamiento e ingeniería de características, mediante un ejercicio guiado que prioriza la interpretación de resultados sobre la programación desde cero.

Se entrega un conjunto de datos de operaciones o servicios y un notebook base para ejecutar localmente en Visual Studio Code con Python. Los participantes cargan y exploran los datos con Pandas; ejecutan una regresión lineal simple y posteriormente una regresión con múltiples características utilizando código previamente estructurado con Scikit-learn. Mediante celdas guiadas observan el comportamiento de la función de costo y la convergencia del descenso del gradiente, modificando únicamente parámetros previamente definidos. También aplican escalamiento y prueban una característica adicional o polinómica con instrucciones paso a paso. Finalmente, generan estimaciones sobre nuevos casos e interpretan qué variables influyen en la predicción con apoyo de gráficos en Matplotlib o Seaborn. Copilot Chat Standard puede utilizarse como apoyo para comprender fragmentos de código, interpretar resultados o resolver errores de ejecución.
  - Duración estimada: 105 min

## Flujo de colaboración

- Trabajar en `changes_course`.
- Crear Pull Request hacia `main`.
- Merge por `Squash and merge`.
