Índice de brecha entre precios de góndola y precios reales frecuentes en supermercados argentinos.

## El problema

Los supermercados publican precios de góndola inflados que no reflejan 
el precio real al que se puede conseguir un producto de forma habitual. 
Las promociones, descuentos y precios con tarjeta aparecen como excepciones,
cuando en realidad son el precio verdadero. En ese caso, el precio de góndola 
sin descuento deja de ser el precio normal y funciona como un precio inflado de referencia.

## La hipótesis

Un producto que aparece a un precio bajo 2 o 3 veces en el historial 
no está "de oferta" — ese es su precio real. 
El precio de góndola sin descuento es el precio anzuelo.

## Objetivo

Construir un índice que mida la brecha entre:

- el precio oficial publicado
- y el precio real frecuente observado

El proyecto comienza con la categoría de galletitas y busca escalar 
progresivamente hacia otros productos de consumo masivo.

## Metodología

Los datos se relevan manualmente utilizando información pública de Precios Claros.

Cada producto registra:

- precio oficial
- precio real observado
- porcentaje de brecha
- fecha de relevamiento

Se considera precio real aquel que aparece con frecuencia en el historial 
(2 o 3 registros), no el precio excepcional de una única promoción.

La brecha porcentual se calcula como:

(precio oficial - precio real) / precio real × 100

## Estado actual

🟡 Etapa inicial — relevamiento manual

- Categoría actual: galletitas
- Dataset inicial en CSV
- Construcción de historial semanal
- Primeras pruebas de análisis exploratorio

## Próximos pasos

- Ampliar historial temporal
- Incorporar nuevas categorías
- Automatizar recolección de datos
- Desarrollar visualizaciones
- Construir análisis comparativos
