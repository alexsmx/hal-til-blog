---
title: "Procesar archivo CSV en Admin de Django"
tags: ["Django", "Django Admin", "python", "csv"]
date: 2019-10-05T09:03:38-05:00
draft: false
has_code: true
comments: true
---

La semana pasada tuve la tarea crear una vista del administrador de Django que procesara un CSV, sin ser explícitamente un modelo (para lo cuál ya existe un paquete -> [Django CSV Import](https://pypi.org/project/django-csvimport/)). El caso era que los usuarios finales requieren subir un CSV más amigable que la representación de los datos que creamos con los modelos.

Investigando un poco encontré varias sugerencias que no se adecuaban a nuestro caso y algunas soluciones bastante limitadas, como procesar el archivo dividiéndolo con saltos de línea ('cadena'.split('\n')): esta solución sólo funciona si estás seguro que el contenido del archivo no contiene saltos de línea en sí mismo.


Aquí esta un ejemplo de parte del código que me permitió resolver el problema:

```python
from django import forms

class CSVContainingForm(forms.ModelForm):
    """
    Formulario creado a medida que agrega el campo CSV a un formulario de modelo.
    """
    
    class Meta:
        model = MyModel

    csv_file = forms.FileField(
        label="Archivo de datos", 
        required=False,
    )

     def save(self, commit=True):
        """
        Esta función se llamará cuando el modelo sea salvado.
        """
        # 1) Obtener el archivo del formulario
        csv_file = self.cleaned_data.get('csv_file', None)

        if csv_file:
            # 2) Codificamos
            with io.TextIOWrapper(csv_file, encoding="utf-8") as f:
                reader = csv.reader(f)
                # Procesa cada línea del CSV como necesites

        return super(PTForm, self).save(commit=commit)
```

Los puntos importantes del código anterior son los siguientes:

1. `csv_file` contiene una referencia al archivo que puede ser leída con el método `read()`, pero no nos sirve porque devuelve un archivo leído como binario sin codificación, el módulo CSV no puede procesar.

2. `io.TextIOWrapper` nos permite leer el archivo y codificarlo como UTF-8, listo para ser procesado por el paquete `csv` de python.

Estos son los dos puntos importantes.
Este formulario puede ser usado para reemplazar el formulario default de un `ModelAdmin`.
