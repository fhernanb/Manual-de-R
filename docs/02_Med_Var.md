# Medidas de variabilidad

En este capítulo se mostrará cómo obtener las diferentes medidas de variabilidad con \proglang{R}.

Para ilustrar el uso de las funciones se utilizará la base de datos llamada __aptos2015__, esta base de datos cuenta con 11 variables registradas a apartamentos usados en la ciudad de Medellín. Las variables son: 

1. `precio`: precio de venta del apartamento (millones de pesos),
2. `mt2`: área del apartamento ($m^2$),
3. `ubicación`: lugar de ubicación del aparamentos en la ciudad (cualitativa),
4. `estrato`: nivel socioeconómico donde está el apartamento (2 a 6),
5. `alcobas`: número de alcobas del apartamento,
6. `banos`: número de baños del apartamento,
7. `balcon`: si el apartamento tiene balcón (si o no),
8. `parqueadero`: si el apartamento tiene parqueadero (si o no),
9. `administracion`: valor mensual del servicio de administración (millones de pesos),
10. `avaluo`: valor del apartamento en escrituras (millones de pesos),
11. `terminado`: si el apartamento se encuentra terminado (si o no).

A continuación se presenta el código para definir la url donde están los datos, para cargar la base de datos en R y para mostrar por pantalla un encabezado (usando `head`) de la base de datos.


```r
url <- 'https://raw.githubusercontent.com/fhernanb/datos/master/aptos2015'
datos <- read.table(file=url, header=T)
head(datos)  # Para ver el encabezado de la base de datos
```

```
##   precio   mt2 ubicacion estrato alcobas banos balcon parqueadero
## 1     79 43.16     norte       3       3     1     si          si
## 2     93 56.92     norte       2       2     1     si          si
## 3    100 66.40     norte       3       2     2     no          no
## 4    123 61.85     norte       2       3     2     si          si
## 5    135 89.80     norte       4       3     2     si          no
## 6    140 71.00     norte       3       3     2     no          si
##   administracion   avaluo terminado
## 1          0.050 14.92300        no
## 2          0.069 27.00000        si
## 3          0.000 15.73843        no
## 4          0.130 27.00000        no
## 5          0.000 39.56700        si
## 6          0.120 31.14551        si
```

## Rango \index{rango} \index{range}
Para calcular el rango de una variable cuantitativa se usa la función `range`. Los argumentos básicos de la función `range` son dos y se muestran abajo.


```r
range(x, na.rm)
```

La función `range` entrega el valor mínimo y máximo de la variable ingresada y el valor de rango ($max - min$) se puede obtener restando del máximo el mínimo.


### Ejemplo {-}
Suponga que queremos obtener el rango para la variable precio de los apartamentos.

Para obtener el rango usamos el siguiente código.


```r
range(datos$precio)
```

```
## [1]   25 1700
```

```r
max(datos$precio) - min(datos$precio)
```

```
## [1] 1675
```
Del resultado anterior podemos ver que los precios de todos los apartamentos van desde 25 hasta 1700 millones de pesos, es decir, el rango de la variables precio es 1675 millones de pesos.

### Ejemplo {-}
Suponga que queremos obtener nuevamente el rango para la variable precio de los apartamentos pero diferenciando por el estrato.

Primero vamos a crear una función auxiliar llamada `myrange` que calculará el rango directamente ($max - min$). Luego vamos a partir la información de los precios por cada estrato usando `split`, la partición se almacenará en la lista `precios`. Finalmente se aplicará la función `myrange` a la lista `precios` para obtener los rangos del precio por estrato socioeconómico. El código para realizar esto se muestra a continuación.


```r
myrange <- function(x) max(x) - min(x)
precios <- split(datos$precio, f=datos$estrato)
sapply(precios, myrange)
```

```
##    2    3    4    5    6 
##  103  225  610 1325 1560
```
De los resultados podemos ver claramente que a medida que aumenta de estrato el rango (variabilidad) del precio de los apartamentos aumenta. Apartamentos de estrato bajo tienden a tener precios similares mientras que los precios de venta para apartamentos de estratos altos tienden a ser muy diferentes entre si.













