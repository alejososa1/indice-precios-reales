# Índice de Precios Reales — Rosario

Índice que mide la brecha entre el precio de góndola publicado y el precio real que paga un consumidor que optimiza los descuentos disponibles en supermercados de Rosario, Argentina.

---

## El problema

Los supermercados publican precios de lista que no reflejan lo que efectivamente paga la mayoría de los consumidores. Promociones, descuentos de cadena y reintegros por medio de pago son parte del precio habitual — no excepciones. El precio de góndola sin descuento funciona como precio de referencia inflado.

Lo que ningún índice mide: cuánto vale realmente un producto cuando el consumidor usa los beneficios disponibles para cualquier persona.

## La hipótesis

El precio real de un producto no es el precio de lista. Es el precio que surge de aplicar sistemáticamente los descuentos del supermercado y los reintegros por medio de pago de acceso general. Esa brecha es medible, consistente y varía por cadena.

## Metodología

El proyecto tiene una metodología documentada que define con precisión qué se mide y cómo se calcula. Ver [`docs/metodologia.md`](docs/metodologia.md).

En resumen:
- **Precio de lista:** precio publicado en la app o web del supermercado, sin descuentos.
- **Precio real:** precio tras aplicar descuentos del supermercado y reintegros por medio de pago disponibles para el público general.
- **Consumidor de referencia:** optimiza el reintegro disponible — compra por el monto exacto que maximiza el beneficio sin superar el tope.
- **Brecha:** diferencia entre precio de lista y precio real, expresada en porcentaje.

Los descuentos dirigidos a segmentos específicos (jubilados, programas sociales) no forman parte del índice principal.

## Cadenas relevadas

| Cadena | Acumula reintegros externos |
|---|---|
| Carrefour | Sí |
| La Gallega | A verificar |
| Coto | No — excluye promociones bancarias |

## Productos y categorías

Canasta inicial de productos de consumo masivo:

| Categoría | Criterio |
|---|---|
| Yerba mate | 3 marcas líderes |
| Leche | 3 marcas líderes, presentación 1L |
| Café | 3 marcas líderes, presentación estándar |

## Estructura del repositorio

```
indice-precios-reales/
├── data/
│   └── relevamientos.csv      # Datos de precios relevados
├── docs/
│   ├── metodologia.md         # Definición de qué se mide y cómo
│   └── esquema_tablas.md      # Modelo de datos del proyecto
├── notebooks/
│   └── analisis_exploratorio.ipynb   # Próximamente
└── README.md
```

## Estado actual

🟡 **Etapa inicial — construcción de dataset**

- Metodología definida y documentada
- Esquema de tablas diseñado
- Relevamiento de datos en curso (semana 1)

## Próximos pasos

- Completar primer relevamiento con canasta definida
- Análisis exploratorio en Jupyter Notebook
- Construir historial de al menos 4 semanas
- Visualizaciones de brecha por producto y cadena
- Migración a PostgreSQL y pipeline ETL

---

*Proyecto en desarrollo. Los datos son relevados manualmente desde las plataformas digitales oficiales de cada cadena.*
- Construir análisis comparativos
