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
* *Uso de coordenadas homogéneas y concatenación de matrices.*

### [2.3 Trazo de Líneas Curvas](#23-trazo-de-líneas-curvas)
* **2.3.1** [Bézier](#231-bézier)
* **2.3.2** [B-spline](#232-b-spline)

### [2.4 Fractales](#24-fractales)
* *Geometría autosimilar y algoritmos recursivos.*

### [2.5 Uso y Creación de Fuentes de Texto](#25-uso-y-creación-de-fuentes-de-texto)
* *Tipografía vectorial vs. mapas de bits.*

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



---

## 2.3 Trazo de líneas curvas





### 2.3.1 Bézier




### 2.3.2 B-spline


---

## 2.4 Fractales
*(Ejemplos: Conjunto de Mandelbrot, Copo de Koch)*



---

## 2.5 Uso y creación de fuentes de texto
*(Sistemas de fuentes TrueType, OpenType y Bitmap)*
