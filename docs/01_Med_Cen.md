# Medidas de tendencia central

En este capítulo se mostrará cómo obtener las diferentes medidas de tendencia central con \proglang{R}.

Para ilustrar el uso de las funciones se utilizará una base de datos llamada __medidas del cuerpo__, esta base de datos cuenta con 6 variables registradas a un grupo de 36 estudiantes de la universidad. Las variables son: 

1. `edad` del estudiante (años),
2. `peso` del estudiante (kilogramos),
3. `altura` del estudiante (centímetros),
4. `sexo` del estudiante (Hombre, Mujer),
5. `muneca`: perímetro de la muñeca derecha (centímetros),
6. `biceps`: perímetro del biceps derecho (centímetros).

A continuación se presenta el código para definir la url donde están los datos, para cargar la base de datos en R y para mostrar por pantalla un encabezado (usando `head`) de la base de datos.


```r
url <- 'https://raw.githubusercontent.com/fhernanb/datos/master/medidas_cuerpo'
datos <- read.table(file=url, header=T)
head(datos)  # Para ver el encabezado de la base de datos
```

```
##   edad peso altura   sexo muneca biceps
## 1   43 87.3  188.0 Hombre   12.2   35.8
## 2   65 80.0  174.0 Hombre   12.0   35.0
## 3   45 82.3  176.5 Hombre   11.2   38.5
## 4   37 73.6  180.3 Hombre   11.2   32.2
## 5   55 74.1  167.6 Hombre   11.8   32.9
## 6   33 85.9  188.0 Hombre   12.4   38.5
```


## Media \index{media} \index{mean}
Para calcular la media de una variable cuantitativa se usa la función `mean`. Los argumentos básicos de la función `mean` son dos y se muestran a continuación.


```r
mean(x, na.rm)
```


### Ejemplo {-}
Suponga que queremos obtener la altura media del grupo de estudiantes.

Para encontrar la media general se usa la función `mean` sobre el vector númerico `datos$altura`.  


```r
mean(x=datos$altura)
```

```
## [1] 171.5556
```

Del anterior resultado podemos decir que la estatura media o promedio de los estudiantes es 171.5555556 centímetros.

### Ejemplo {-}
Suponga que ahora queremos la altura media pero diferenciando por sexo.  

Para hacer esto se debe primero dividir o partir el vector de altura según los niveles de la variable sexo, esto se consigue por medio de la función `split` y el resultado será una lista con tantos elementos como niveles tenga la variable sexo. Luego a cada uno de los elementos de la lista se le aplica la función `mean` con la ayuda de `sapply` o `tapply`. A continuación el código completo para obtener las alturas medias para hombres y mujeres.


```r
sapply(split(x=datos$altura, f=datos$sexo), mean)
```

```
##   Hombre    Mujer 
## 179.0778 164.0333
```

El resultado es un vector con dos elementos, vemos que la altura media para hombres es 179.0777778 centímetros y que para las mujeres es de  164.0333333 centímetros.

¿Qué sucede si se usa `tapply` en lugar de `sapply`? Substituya en el código anterior la función `sapply` por `tapply` y observe la diferencia entre los resultados.

### Ejemplo {-}
Suponga que se tiene el vector `edad` con las edades de siete personas y supóngase que para el individuo cinco no se tiene información de su edad, eso significa que el vector tendrá un `NA` en la quinta posición. 

¿Cuál será la edad promedio del grupo de personas?


```r
edad <- c(18, 23, 26, 32, NA, 32, 29)
mean(x=edad)
```

```
## [1] NA
```

Al correr el código anterior se obtiene un error y es debido al símbolo `NA` en la quinta posición. Para calcular la media sólo con los datos de los cuales se tiene información, se incluye el argumento `na.rm = TRUE` para que R remueva los `NA`. El código correcto a usar en este caso es:


```r
mean(x=edad, na.rm=TRUE)
```

```
## [1] 26.66667
```

De este último resultado se obtiene que la edad promedio de los individuos es 26.67 años.

## Mediana \index{mediana} \index{median}
Para calcular la mediana de una variable cantitativa se usa la función `median`. Los argumentos básicos de la función `median` son dos y se muestran a continuación.


```r
median(x, na.rm)
```

### Ejemplo {-}
Calcular la edad mediana para los estudiantes de la base de datos.

Para obtener la mediana usamos el siguiente código:

```r
median(x=datos$edad)
```

```
## [1] 28
```
y obtenemos que la mitad de los estudiantes tienen edades mayores o iguales a 28 años.

El resultado anterior se pudo haber obtenido con la función `quantile` e indicando que se desea el cuantil 50 así:

```r
quantile(x=datos$edad, probs=0.5)
```

```
## 50% 
##  28
```

## Moda \index{moda}
La moda de una variable cuantitativa corresponde a valor o valores que más se repiten, una forma sencilla de encontrar la moda es construir una tabla de frecuencias y observar los valores con mayor frecuencia.

### Ejemplo  {-}
Calcular la moda para la variable edad de la base de datos de estudiantes.

Se construye la tabla con la función `table` y se crea el objeto `tabla` para almacenarla.

```r
tabla <- table(datos$edad)
tabla
```

```
## 
## 19 20 21 22 23 24 25 26 28 29 30 32 33 35 37 40 43 45 51 55 65 
##  1  1  1  3  2  1  5  3  2  1  2  1  1  2  3  1  2  1  1  1  1
```
Al mirar con detalle la tabla anterior se observa que el valor que más se repite es la edad de 25 años en 5 ocasiones. Si la tabla hubiese sido mayor, la inspección visual nos podría tomar unos segundos o hasta minutos y podríamos equivocarnos, por esa razón es mejor ordenar los resultados de la tabla.

Para observar los valores con mayor frecuencia de la tabla se puede ordenar la tabla usando la función `sort` de la siguiente manera:

```r
sort(tabla, decreasing=TRUE)
```

```
## 
## 25 22 26 37 23 28 30 35 43 19 20 21 24 29 32 33 40 45 51 55 65 
##  5  3  3  3  2  2  2  2  2  1  1  1  1  1  1  1  1  1  1  1  1
```
De esta manera se ve fácilmente que la variable edad es unimodal con valor de 25 años.

















