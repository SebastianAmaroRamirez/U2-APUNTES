# U2-APUNTES

**Temas de la Unidad 2**

# 🗺️ Unidad II: Graficación 2D

## 📖 Índice de Contenidos

---

### [2.1 Transformación Bidimensional](#21-transformación-bidimensional)
* **2.1.1** [Traslación](#211-traslación)
* **2.1.2** [Escalamiento](#212-escalamiento)
* **2.1.3** [Rotación](#213-rotación)
* **2.1.4** [Sesgado (Shear)](#214-sesgado)

### [2.2 Representación Matricial](#22-representación-matricial-de-las-transformaciones-bidimensionales)

### [2.3 Trazo de Líneas Curvas](#23-trazo-de-líneas-curvas)
* **2.3.1** [Bézier](#231-bézier)
* **2.3.2** [B-spline](#232-b-spline)

### [2.4 Fractales](#24-fractales)

### [2.5 Uso y Creación de Fuentes de Texto](#25-uso-y-creación-de-fuentes-de-texto)

---

## 2.1 Transformación Bidimensional

Las transformaciones bidimensionales son procedimientos para modificar o manipular la visualización de objetos en el plano $X Y$. 
Estas operaciones permiten cambiar la posición, el tamaño o la orientación de una figura geométrica mediante cálculos matemáticos.
Cada punto  $P(x, y)$ del objeto se convierte en un nuevo punto  $P'(x', y')$ tras aplicar una función de transformación.

### 2.1.1 Traslación

Es el movimiento de un objeto de una posición a otra. Se logra sumando distancias de desplazamiento $(tx, ty)$ a las coordenadas originales.

* **Lógica:**

$$x' = x + tx$$$$y' = y + ty$$

<p align="center">
  <img src="https://github.com/user-attachments/assets/e6881b5d-eb47-4861-9d6c-fb33030c6fb4" width="250" title="Evolución de la Graficación">
  <br>
</p>


### 2.1.2 Escalamiento

Altera el tamaño de un objeto. Se multiplican las coordenadas originales por factores de escala $(sx, sy)$.

* **Lógica:**

$$x' = x \cdot sx$$$$y' = y \cdot sy$$

<p align="center">
  <img src="https://github.com/user-attachments/assets/a2cb0b23-1a00-4d85-902a-cbf3234459cf" width="250" title="Evolución de la Graficación">
  <br>
</p>

**Nota:** Si $sx = sy$, el escalamiento es uniforme. Si son diferentes, el objeto se distorsiona.

 **Tipos de escalado:**

* **Escalado uniforme:** El factor de escala es el mismo en las dos coordenadas, es decir Sx=Sy, y por lo tanto varía el tamaño pero no la forma del objeto.

* **Escalado diferencial:** El factor de escala es distinto en cada dirección, es decir Sx es distinto de Sy, y se produce una distorsión en la forma del objeto.

### 2.1.3 Rotación

Gira un objeto alrededor de un punto de pivote (usualmente el origen). Se requiere un ángulo de rotación $\theta$.

* **Lógica (respecto al origen):**

$$x' = x \cos\theta - y \sin\theta$$$$y' = x \sin\theta + y \cos\theta$$

<p align="center">
  <img src="https://github.com/user-attachments/assets/f7c8ff39-70c3-4c2f-8752-6d63349ed7a8" width="300" title="Evolución de la Graficación">
  <br>
  <em>En la figura se muestra la rotación de la casa 45º, con respecto al origen.</em>
</p>

### 2.1.4 Sesgado

Es una transformación que distorsiona la forma de un objeto, como si se deslizara una capa sobre otra. Puede ocurrir en el eje $X$, en el eje $Y$ o en ambos.

* **Sesgado en X:** $x' = x + sh_x \cdot y$

* **Sesgado en Y:** $y' = y + sh_y \cdot x$

El sesgado hace que un cuadrado se convierta en un paralelogramo.

<p align="center">
  <img src="https://github.com/user-attachments/assets/b39378fb-f049-4510-a9a7-11b7a10754c4" width="250" title="Evolución de la Graficación">
  <br>
</p>

---

## 2.2 Representación matricial de las transformaciones bidimensionales

Para realizar transformaciones complejas (como rotar un objeto sobre su propio centro y luego moverlo), es ineficiente realizar cálculos individuales. 
En su lugar, expresamos cada transformación como una matriz.

🌌 **Coordenadas Homogéneas**

En el plano 2D normal $(x, y)$, la traslación es una suma, mientras que la rotación y el escalamiento son multiplicaciones. 
Para unificar todo en solo multiplicaciones, añadimos una tercera coordenada $w$ (usualmente $w=1$).

Así, un punto $P$ se representa como:

$$P = \begin{bmatrix} x \\ y \\ 1 \end{bmatrix}$$


📐 Matrices de Transformación Básicas ($3 \times 3$)

Al usar coordenadas homogéneas, las matrices de transformación se representan de la siguiente manera:

**Traslación**

$$
T(t_x, t_y) = \begin{bmatrix} 
1 & 0 & t_x \\ 
0 & 1 & t_y \\ 
0 & 0 & 1 
\end{bmatrix}
$$

**Escalamiento**

$$
S(s_x, s_y) = \begin{bmatrix} 
s_x & 0 & 0 \\ 
0 & s_y & 0 \\ 
0 & 0 & 1 
\end{bmatrix}
$$

**Rotación**

$$
R(\theta) = \begin{bmatrix} 
\cos\theta & -\sin\theta & 0 \\ 
\sin\theta & \cos\theta & 0 \\ 
0 & 0 & 1 
\end{bmatrix}
$$

🔗 **Composición de Transformaciones (Concatenación)**

La mayor ventaja de la representación matricial es que podemos multiplicar varias matrices para obtener una Matriz de Transformación Combinada ($M_{total}$).

Si queremos aplicar una rotación ($R$), luego un escalamiento ($S$) y finalmente una traslación ($T$), la operación sería:

$$P' = (T \cdot (S \cdot (R \cdot P)))$$

O lo que es lo mismo:

$$P' = M_{total} \cdot P$$

---

#### Ejemplo Animación utilizando las teclas "Flechas"

**Código**

```

import bpy

class ModalMoveOperator(bpy.types.Operator):
    bl_idname = "object.modal_move_operator"
    bl_label = "Modal Move Operator"

    def modal(self, context, event):
        obj = bpy.data.objects.get("MiDibujo")
        
        if not obj:
            return {'CANCELLED'}

        if event.value == 'PRESS':
            # IZQUIERDA / DERECHA (Eje X)
            if event.type == 'LEFT_ARROW':
                obj.location.x -= 0.5
            elif event.type == 'RIGHT_ARROW':
                obj.location.x += 0.5
                
            # ARRIBA / ABAJO (Eje Z - Altura en vista frontal)
            elif event.type == 'UP_ARROW':
                obj.location.z += 0.5
            elif event.type == 'DOWN_ARROW':
                obj.location.z -= 0.5

            # SALIR
            elif event.type == 'ESC':
                return {'FINISHED'}

        return {'RUNNING_MODAL'}

    def invoke(self, context, event):
        context.window_manager.modal_handler_add(self)
        return {'RUNNING_MODAL'}

bpy.utils.register_class(ModalMoveOperator)
bpy.ops.object.modal_move_operator('INVOKE_DEFAULT')

```

#### Visualización del dibujo

<p align="center">
  <img src="https://github.com/user-attachments/assets/bbb8c903-ba42-4a99-ba06-261ad9c756ab" width="400" title="Snorlax">
  <br>
  <em>Realice primero un dibujo para animar</em>
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/45528832-1a85-4ee2-8493-59bb1a6b98e1" width="400" title="Snorlax">
  <br>
  <em>Posteriormente colocar el código y ejecutarlo</em>
</p>

---

## 2.3 Trazo de líneas curvas

En computación gráfica, las curvas no se dibujan a mano alzada, sino que se definen mediante ecuaciones paramétricas. El objetivo es generar una línea suave que pase cerca o a través de un conjunto de puntos de control.

Existen dos enfoques principales:

1. **Interpolación:** La curva debe pasar exactamente por todos los puntos.

2. **Aproximación:** La curva es "atraída" por los puntos de control, pero no necesariamente pasa por ellos (como en Bézier y B-splines).




### 2.3.1 Bézier

Desarrolladas originalmente para el diseño de carrocerías de autos (Renault), estas curvas se definen por puntos de control que actúan como "imanes".

* **Puntos de Anclaje:** El primero y el último punto, donde la curva inicia y termina.

* **Puntos de Control:** Determinan la curvatura y la dirección de la línea.

* **Grados:** Una curva de 3 puntos es cuadrática; una de 4 puntos es cúbica (la más común en diseño gráfico y tipografías).

**Fórmula General (Polinomios de Bernstein):**

$$B(t) = \sum_{i=0}^{n} \binom{n}{i} (1-t)^{n-i} t^i P_i, \quad t \in [0,1]$$



### 2.3.2 B-spline

A diferencia de las curvas de Bézier, donde mover un solo punto de control afecta a toda la curva, las B-splines ofrecen control local.

* **Segmentación:** La curva se construye por tramos. Mover un punto solo afecta a los segmentos cercanos.

* **Continuidad:** Permiten unir múltiples curvas de forma perfecta, sin ángulos marcados en las uniones.

* **Uso:** Modelado 3D profesional (NURBS) y animaciones complejas donde se requiere precisión quirúrgica.

---

#### Ejemplo de una animacion frame x frame

<img width="361" height="297" alt="image" src="https://github.com/user-attachments/assets/d6f71f7d-73a9-4a07-8725-31ab1cb9d8d0" />


---

## 2.4 Fractales

Un fractal es un objeto geométrico cuya estructura básica, fragmentada o aparentemente irregular, se repite a diferentes escalas. El término fue acuñado por Benoît Mandelbrot en 1975.

**Características Principales**

* **Autosimilitud:** Partes del objeto son copias exactas o estadísticas del todo.

* **Dimensión no entera:** A diferencia de las figuras euclidianas (línea = 1D, plano = 2D), los fractales tienen dimensiones fraccionarias.

* **Algoritmos Recursivos:** Se generan mediante la repetición infinita de un proceso geométrico o matemático simple.

### 📐 Tipos de Fractales

1. **Fractales Geométricos (Lineales)**

Se crean reemplazando partes de una figura simple por una forma más compleja de manera recursiva.

* **Copo de Nieve de Koch:** Comienza con un triángulo y añade puntas en cada lado infinitamente.

* **Triángulo de Sierpinski:** Un triángulo que se subdivide en triángulos más pequeños eliminando el central.

2. **Fractales Algorítmicos (No lineales)**

Basados en funciones complejas que se iteran en el plano complejo.

* **Conjunto de Mandelbrot:** Es el fractal más famoso. Se define por la iteración de la función $z_{n+1} = z_n^2 + c$.

* **Conjunto de Julia:** Estrechamente relacionado con el de Mandelbrot, genera formas orgánicas y espirales infinitas.

### 🛠️ Aplicaciones en la Graficación

Los fractales no son solo arte; tienen usos técnicos críticos:

* **Generación de Terrenos (Procedural):** Creación de montañas, nubes y paisajes realistas en videojuegos y cine.

* **Modelado de Plantas:** Uso de Sistemas-L para simular el crecimiento de árboles y helechos.

* **Compresión de Imágenes:** Aprovechamiento de patrones repetitivos para reducir el tamaño de archivos.

---

## 2.5 Uso y creación de fuentes de texto

En la graficación por computadora, las fuentes de texto (tipografías) se tratan de dos formas principales, dependiendo de si se prioriza la velocidad o la calidad de escalado.

1. **Fuentes de Mapas de Bits (Bitmapped Fonts)**

Cada carácter se almacena como una rejilla de píxeles (un patrón de bits).

* **Ventajas:** Son muy rápidas de dibujar en pantalla.

* **Desventajas:** No se pueden escalar sin perder calidad (aparece el efecto de "pixelado"). Si cambias el tamaño, debes tener un mapa de bits diferente para cada tamaño.

* **Uso común:** Terminales de consola, sistemas embebidos y videojuegos de estética retro.

2. Fuentes Vectoriales o de Contorno (Outline Fonts)

Los caracteres se definen mediante ecuaciones matemáticas y Curvas de Bézier (vistas en el tema 2.3).

* **Ventajas:** Son escalables infinitamente sin perder nitidez. Se pueden rotar y transformar mediante matrices sin distorsión.

* **Desventajas:** Requieren más procesamiento, ya que la computadora debe "rellenar" la curva con píxeles (rasterización) cada vez que se dibujan.

* **Estándares:** TrueType (.ttf) y OpenType (.otf).

### 🛠️ El proceso de Renderizado de Texto

Para mostrar una fuente vectorial en pantalla, se siguen estos pasos:

1. **Selección del Glifo:** Se busca el diseño del carácter en el archivo de la fuente.

2. **Transformación:** Se aplican matrices de escalado o rotación si es necesario.

3. **Rasterización:** Se convierten las curvas matemáticas en píxeles.

4. **Anti-aliasing:** Se suavizan los bordes para que la letra no se vea "dentada" en resoluciones bajas.

### 🎨 Creación de Fuentes

La creación de fuentes modernas implica el diseño de cada glifo sobre un plano cartesiano, definiendo puntos de anclaje y manejadores de curvas. Herramientas como FontForge o Glyphs permiten exportar estos diseños a formatos vectoriales que las tarjetas gráficas pueden procesar.

### 📊 Comparativa Rápida de Tipografías

| Tipo de Fuente | Representación | Escalabilidad | Consumo de CPU |
| :--- | :--- | :--- | :--- |
| **Bitmap** | Matriz de píxeles | Pobre (se pixela) | Muy bajo |
| **Vectorial** | Curvas de Bézier | Perfecta (infinita) | Moderado |

---

## Bibliografías APA

- http://fernandez-torres-jose.blogspot.com/2012/09/21-transformaciones-bidimensionales.html

- Valdes, A. S. (2013, 24 septiembre). 2.4 Representación matricial. http://graficacionito.blogspot.com/2013/09/24-representacion-matricial.html

- Martin, J. A. C. (s. f.). Trazo de líneas curvas. http://graficacion-jakiealexander-chanmartin.blogspot.com/2019/10/trazo-de-lineas-curvas.html

- Fractales. (s. f.). http://fernandez-torres-jose.blogspot.com/2012/08/fractales.html

- Guest. (s. f.). Uso y creación de fuentes de texto - PDFCOFFEE.COM. pdfcoffee.com. https://pdfcoffee.com/uso-y-creacion-de-fuentes-de-texto-8-pdf-free.html

- Hearn, D., & Baker, M. P. (2006). Gráficas por computadora con OpenGL. Pearson Educación.

- Rogers, D. F., & Adams, J. A. (1990). Mathematical Elements for Computer Graphics. McGraw-Hill.
