---
title: "Feather: data frames para Python y R"
date: 2019-10-11T23:33:07-05:00
tags: ["python", "r"]
draft: false
has_code: true
---

Frecuentemente necesito usar en R un data frame generado en Python por Pandas, o viceversa, tengo datos en un data frame de R que quiero usar en Python.

Una manera común para compartir data frames entre Python y R es exportarlas como archivos CSV. Este método tiene el inconveniente de que perdemos los metadatos de nuestro data frame.

Si tenemos datos de fecha con husos horarios, factores de R, texto con codificación específica o con cualquier otro metadato, este se pierde al exportar un data frame a CSV. 
Es posible que los datos que creímos exportar en Python no sean los mismos que importamos en R.

Para compartir data frames entre Python y R de manera que no perdamos información, podemos usar el formato de archivo Feather. Si guardamos un data frame como un archivo feather, será legible por Python y por R, sin pérdida contenido.

Para usar este formato, tenemos que instalar el paquete Feather en Python y R. 

## Instalación y uso en Python

Podemos instalar feather con `pip`.

```
pip install feather-format
```

Importamos feather y pandas.

``` python
import feather
import pandas as pd
```

Para exportar un data frame de pandas usamos `feather.write_feather()`.

``` python
feather.write_feather(mi_df, "mi_df.feather")
```

Y para importar un data frame usamos ` feather.read_feather()`.

``` python
mi_df = feather.read_feather("mi_df.feather)
```

## Instalación y uso en R

feather se encuentra en CRAN así que lo instalamos con `install.packages()`

``` r
install.packages("feather")
```

Cargamos feather.

``` r
library(feather)
```

Para exportar un data frame usamos `write_feather()`.

```r
write_feather(mi_df)
```

Para importar un data frame usamos `read_feather()`.

```r
mi_df <- read_feather("mi_df.feather")
```
---

Aunque el formato de archivo feather no es tan universal como CSV, la cantidad de tiempo que ahorro al usarlo compensa esta falta de flexibilidad.
