---
title: "Procesar CSV en Admin de Django"
tags: ["Django", "python",  "csv"]
date: 2019-10-05T09:03:38-05:00
draft: false
---

La semana pasada tuve la tarea  crear una vista del administrador de Django que procesara un CSV, sin ser explícitamente un modelo (para lo cuál ya existe un paquete -> [Django CSV Import](https://pypi.org/project/django-csvimport/)). El caso era que los usuarios finales requieren subir un CSV más amigable que la represenación de los datos que creamos con los modelos.

Investigando un poco encontré varias sugerencias que no se adecuaban a nuestro caso y algunas soluciones bastante limitadas, como procesar el archivo como dividiéndolo con saltos de línea: esta solución sólo funciona si estás seguro que el contenido del archivo no contiene saltos de línea en sí mismo.

Así que si necesitas implementar el procesamiento de un CSV aquí tienes un pequeño script que puedes integrar en tu código:

```
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
        csv_file = self.cleaned_data.get('csv_file', None) # Contiene el archivo si lo subieron

        if csv_file:
            # csv_file es un archivo binario sin ningeun tipo de codificación,
            # hay que codificarlo
            with io.TextIOWrapper(csv_file, encoding="utf-8") as f:
                reader = csv.reader(f)
                # Haz lo que quieras con el archivo


```