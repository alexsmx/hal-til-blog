---
title: "Aplicar una funcion a un data frame, por renglon, usando purrr"
date: 2019-10-19T13:36:32-05:00
tags: ["r"]
draft: false
has_code: true
---

`purrr` es un paquete de `tidyverse` que agrega características de programación funcional a R.

Entre otras cosas, incluye la familia de funciones `map()`, que aplican una función a todos los elementos de una lista, de la misma manera a la familia de funciones `apply()` de `base`, pero con una mejor sintaxis y caraterísticas adicionales.

Al usar `map()` con un data frame, entonces la función es aplicada por columna.

Hay ocasiones en las necesitamos aplicar una función por renglón.

Usando `apply()`, basta con especificar el argumento `MARGIN = 1` para aplicar una función por renglón.

Por ejemplo, para sumar todos los datos de cada renglon de un data frame.

```r
apply(mi_df, sum, MARGIN = 1)
```

`map()` no tiene un argumento `MARGIN`, pero tenemos la función `pmap()` en `purrr` que puede aplicar una función a cada renglón de un data frame.

```r
pmap(mid_df, sum)
```

Notarás que lo anterior solo funciona si todos los elementos de un cada renglón son numéricos, es decir, del tipo de dato que espera la función.

Esto es porque `pmap()`, en realidad, está tomando todos los elementos de una lista como argumentos de la función que estamos aplicando.

Por ejemplo, tomando las dos primeras columnas de **iris**, podemos hacer algo como lo siguiente.

```r
pmap(iris[1:2], function(Sepal.Length, Sepal.Width) {
  Sepal.Length + Sepal.Width
})
```

El resultado será una lista con 150 valores numéricos, que es la cantidad de renglones de iris.

Si deseas usar `pmap()` con data frames, debes hacer referencia a todos los nombres de columnas, aunque no sean usados en la función, de lo contrario, obtendrás un error.

Los siguiente funciona.

```r
pmap(iris, function(Sepal.Length, Sepal.Width, Petal.Length,Petal.Width, Species) {
    Petal.Width ^ 2
})
```

Pero lo siguiente no

```r
pmap(iris, function(Petal.Width) {
    Petal.Width ^ 2
})
```
