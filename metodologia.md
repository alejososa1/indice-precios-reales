# Metodología — Índice de Precios Reales (Rosario)

## Objetivo

Medir y comparar dos precios para un conjunto de productos de la canasta básica en los principales supermercados de Rosario:

- **Precio de lista:** el precio publicado en góndola o en la plataforma digital del supermercado, sin ningún descuento aplicado.
- **Precio real:** el precio efectivamente pagado por un consumidor que optimiza los beneficios disponibles, considerando descuentos propios del supermercado y descuentos por método de pago de acceso general.

La diferencia entre ambos valores es la **brecha**, expresada en porcentaje sobre el precio de lista.

---

## Qué no entra en el precio real

Los descuentos dirigidos a segmentos específicos de la población (jubilados, beneficiarios de programas sociales, empleados de determinadas empresas, etc.) **no forman parte del índice principal**. Estos descuentos pueden analizarse en capas separadas a futuro, pero no son representativos del precio accesible para el consumidor general.

---

## Fuente de datos

Los precios se relevaron a través de las **aplicaciones móviles y sitios web oficiales** de cada supermercado. Este canal se eligió porque permite:

- Relevamiento sistemático y reproducible.
- Registro de precios con fecha exacta.
- Acceso a promociones vigentes en el mismo momento del relevamiento.

Las visitas al local físico pueden complementar el análisis como **notas de campo**, pero no forman parte de la metodología sistemática dado que no son reproducibles con la misma frecuencia ni condiciones.

---

## Cadenas relevadas

Se seleccionaron las tres cadenas más representativas del mercado de supermercados en Rosario:

- **La Gallega**
- **Coto**
- **Carrefour**

Estas cadenas concentran una parte significativa del consumo masivo en la ciudad y tienen presencia digital activa. Otras cadenas (Disco, Día, Jumbo, entre otras) serán incorporadas en etapas futuras del proyecto.

---

## Modelo de cálculo del precio real

El precio real se calcula aplicando descuentos en capas sucesivas. Cada capa es opcional según la cadena y las promociones vigentes:

```
precio_real = precio_lista
            × (1 - descuento_super)
            × (1 - reintegro_aplicable)
```

### Descuento del supermercado

Promociones propias de la cadena (ofertas de la semana, precios especiales, 2x1, etc.). Se aplica sobre el precio de lista.

### Reintegro por método de pago

Se considera el reintegro disponible para el público general (por ejemplo, Modo, billeteras virtuales, tarjetas de bancos con promociones abiertas). El consumidor de referencia es **racional y optimizador**: se asume que realiza una compra por el monto exacto que maximiza el reintegro sin superar el tope.

```
monto_optimo = tope_reintegro / porcentaje_reintegro

Ejemplo:
tope = $15.000, porcentaje = 30%
monto_optimo = 15.000 / 0.30 = $50.000
```

El beneficio del reintegro se prorratea uniformemente sobre el monto óptimo y se aplica proporcionalmente al precio unitario de cada producto:

```
participacion_producto = precio_producto / monto_optimo
reintegro_del_producto = tope_reintegro × participacion_producto
precio_real_producto   = precio_producto - reintegro_del_producto
```

### Reintegro aplicable por cadena

No todas las cadenas acumulan beneficios de medios de pago externos. El campo `reintegro_aplicable` se define por cadena:

| Cadena | Descuento súper | Reintegro Modo/banco | Observación |
|---|---|---|---|
| Carrefour | Sí | Sí | Acumula beneficios externos |
| La Gallega | Sí | Acumula beneficios externos |
| Coto | Sí | No | Excluye promociones bancarias externas |

> Esta tabla se actualiza con cada relevamiento. Las exclusiones de Coto implican que su precio real es estructuralmente más alto que cadenas equivalentes, independientemente del precio de lista.

---

## Productos y categorías

El relevamiento se organiza por **categorías de la canasta básica**. Dentro de cada categoría se seleccionan los **3 productos más populares entre las marcas líderes**, priorizando aquellos con mayor reconocimiento y consumo masivo.

### Categorías iniciales

| Categoría | Criterio de selección |
|---|---|
| Yerba mate | 3 marcas líderes en volumen de venta |
| Leche | 3 marcas líderes (preferentemente leche entera en sachet o caja 1L) |
| Café | 3 marcas líderes en presentación estándar |

> **Nota:** La categoría galletitas, con la que se inició el proyecto, está en evaluación para determinar si se incorpora a la canasta principal o se mantiene como serie histórica independiente.

Los productos seleccionados son **fijos por temporada**: una vez definidos, se relevan siempre los mismos para garantizar la comparabilidad entre semanas. Cualquier cambio en la selección se documenta con fecha y justificación.

---

## Frecuencia de relevamiento

Semanal. Cada registro incluye:

- Fecha del relevamiento
- Supermercado
- Producto (marca, presentación, gramaje)
- Precio de lista
- Descuento del supermercado (porcentaje y descripción)
- Reintegro aplicable (porcentaje, tope, medio de pago)
- Precio real calculado
- Brecha (%)

---

## Expansiones previstas

- Incorporación de nuevas cadenas: Disco, Día, Jumbo y otros supermercados con presencia en Rosario.
- Estratificación por rango de precio dentro de cada categoría: producto económico, intermedio y premium.
- Análisis de descuentos por segmento poblacional como capa opcional.
- Cruce con índices oficiales de inflación (INDEC) para contextualizar la evolución del índice.
