# Esquema de tablas — Índice de Precios Reales (Rosario)

## Visión general

El modelo se organiza en cuatro tablas. La separación entre datos fijos (productos, cadenas) y datos que cambian en el tiempo (relevamientos, promociones) es el principio central del diseño.

```
productos ──┐
            ├──► relevamientos ◄── promociones
cadenas  ───┘         │
                      └──► (precio_real calculado)
```

---

## Tabla: productos

Contiene los datos descriptivos de cada producto. **No cambia con el tiempo.** Si un producto deja de relevarse, se marca como inactivo pero no se elimina.

| Campo | Tipo | Descripción |
|---|---|---|
| id_producto | INTEGER (PK) | Identificador único |
| nombre | TEXT | Nombre del producto |
| marca | TEXT | Marca comercial |
| categoria | TEXT | Categoría (yerba, leche, café, etc.) |
| gramaje | TEXT | Presentación (ej: 500g, 1L) |
| codigo_barras | TEXT | Código de barras (opcional) |
| activo | BOOLEAN | TRUE si se sigue relevando |

**Ejemplo de filas:**

| id | nombre | marca | categoria | gramaje |
|---|---|---|---|---|
| 1 | Yerba Amanda | Amanda | yerba | 500g |
| 2 | Leche La Serenísima | La Serenísima | leche | 1L |
| 3 | Café La Virginia | La Virginia | café | 100g |

---

## Tabla: cadenas

Contiene los datos de cada supermercado. Incluye un campo clave que indica si la cadena acumula reintegros externos o los excluye.

| Campo | Tipo | Descripción |
|---|---|---|
| id_cadena | INTEGER (PK) | Identificador único |
| nombre | TEXT | Nombre de la cadena |
| acumula_reintegro | BOOLEAN | TRUE si acepta reintegros de medios de pago externos |
| observaciones | TEXT | Notas sobre exclusiones o condiciones especiales |

**Ejemplo de filas:**

| id | nombre | acumula_reintegro | observaciones |
|---|---|---|---|
| 1 | Carrefour | TRUE | Acumula Modo y promociones bancarias |
| 2 | La Gallega | NULL | A verificar |
| 3 | Coto | FALSE | Excluye promociones bancarias externas |

---

## Tabla: promociones

Contiene cada promoción vigente por cadena y período. Una misma promoción (ej: Modo 30% durante mayo) se registra **una sola vez** con su fecha de inicio y fin. Cuando cambian las condiciones, se crea un nuevo registro.

Descuentos de góndola y reintegros por medio de pago se tratan igual: ambos son un porcentaje que reduce el precio. El campo `tipo` los diferencia para análisis futuros.

| Campo | Tipo | Descripción |
|---|---|---|
| id_promocion | INTEGER (PK) | Identificador único |
| id_cadena | INTEGER (FK) | Cadena donde aplica |
| fecha_desde | DATE | Inicio de vigencia |
| fecha_hasta | DATE | Fin de vigencia |
| tipo | TEXT | 'descuento_super' o 'reintegro_medio_pago' |
| porcentaje | DECIMAL | Porcentaje de descuento (ej: 0.30 para 30%) |
| tope_ars | DECIMAL | Tope máximo del reintegro en pesos (NULL si no aplica) |
| medio_de_pago | TEXT | Nombre del medio (ej: Modo, Cuenta DNI) — NULL si es descuento de góndola |

**Ejemplo de filas:**

| id | id_cadena | fecha_desde | fecha_hasta | tipo | porcentaje | tope_ars | medio_de_pago |
|---|---|---|---|---|---|---|---|
| 1 | 1 | 2025-05-01 | 2025-05-31 | reintegro_medio_pago | 0.30 | 15000 | Modo |
| 2 | 1 | 2025-05-05 | 2025-05-11 | descuento_super | 0.20 | NULL | NULL |

---

## Tabla: relevamientos

Es la tabla principal. Registra el precio de cada producto en cada cadena en cada fecha. Se vincula a `productos`, `cadenas` y `promociones`.

El campo `id_promocion` es **nullable**: si no hay promoción vigente esa semana, el campo queda vacío y el precio real es igual al precio de lista.

| Campo | Tipo | Descripción |
|---|---|---|
| id_relevamiento | INTEGER (PK) | Identificador único |
| id_producto | INTEGER (FK) | Producto relevado |
| id_cadena | INTEGER (FK) | Cadena donde se relevó |
| id_promocion | INTEGER (FK, nullable) | Promoción aplicada — NULL si no hay |
| fecha | DATE | Fecha del relevamiento |
| precio_lista | DECIMAL | Precio sin descuentos |
| descuento_super_pct | DECIMAL | Descuento directo del súper (0 si no aplica) |
| precio_real | DECIMAL | Precio final calculado |
| brecha_pct | DECIMAL | (precio_lista - precio_real) / precio_lista |

**Ejemplo de filas:**

| id | id_producto | id_cadena | id_promocion | fecha | precio_lista | precio_real | brecha_pct |
|---|---|---|---|---|---|---|---|
| 1 | 1 | 1 | 1 | 2025-05-07 | 1200 | 840 | 0.30 |
| 2 | 1 | 3 | NULL | 2025-05-07 | 1150 | 1150 | 0.00 |

> Fila 1: Yerba Amanda en Carrefour con reintegro Modo 30% → precio real $840.
> Fila 2: Yerba Amanda en Coto sin promoción → precio real = precio lista.

---

## Fórmula de precio real

```
monto_optimo        = tope_ars / porcentaje
participacion       = precio_lista / monto_optimo
reintegro_producto  = tope_ars × participacion
precio_real         = precio_lista - reintegro_producto

brecha_pct          = (precio_lista - precio_real) / precio_lista
```

Si `id_promocion` es NULL:
```
precio_real = precio_lista
brecha_pct  = 0
```

---

## Expansiones previstas

- Agregar tabla `segmentos` para modelar descuentos especiales (jubilados, programas sociales) como capa opcional.
- Estratificación de productos por rango: económico, intermedio, premium.
- Incorporación de nuevas cadenas: Disco, Día, Jumbo.
