+++
title = "Afinación pitagórica"
author = ["Abraham Aguilar"]
publishDate = 2022-05-14T00:00:00-05:00
tags = ["música"]
draft = false
series = "Escalas bien formadas"
+++

Un instrumento de cuerdas, por ejemplo, un piano, produce sonido vibrando cuerdas a diferentes frecuencias. Para escoger a qué frecuencia vibra cada cuerda utilizamos algún proceso que llamamos _sistema de afinación_. Existen muchos sistemas de afinación pero en esta pequeña notita nos enfocaremos en el sistema de afinación pitagórico. Este consiste en fijar una frecuencia base y multiplicarla por \\(3/2\\) para obtener una segunda frecuencia. A esta segunda frecuencia la volvemos a multiplicar por \\(3/2\\) para obtener una tercera y así sucesivamente hasta conseguir \\(12\\) frecuencias que corresponden a las notas que conocemos en la escala cromática. Si empezamos el proceso en fa, obtenemos el conjunto fa, do, sol, re, la, mi, si, fa#, do#, sol#, re#, y la#, en ese orden. A esta sucesión la conocemos como el _círculo de quintas_.

El proceso no termina del todo ahí puesto que por convención las notas que están a una razón de \\(2/1\\) de distancia entre ellas se consideran la mismas nota. Es decir, \\(fa \times 2\\) es de nuevo \\(fa\\). Decimos que \\(fa\\) y \\(fa \times 2\\) están en la misma _clase tonal_ (_pitch class_) y decimos que \\(fa \times 2\\) está a una octava de distancia de \\(fa\\). Así surge la convención de asignar números a cada octava, por ejemplo, en la afinación convencional la frecuencia de \\(440\\) se llama \\(la\_4\\), que implica que \\(220\\) es \\(la\_3\\), \\(880\\) es \\(la\_5\\) y así sucesivamente multiplicandoo por potencias de \\(2\\).

Entonces, en corto, el proceso que seguimos para afinar todas las notas en un piano consiste en conseguir un representante de cada clase tonal apilando quintas y de rellenar cada octava con representantes de las clases tonales dentro de cada octava.

Podemos generalizar el proceso abriendonos a la posibilidad de que las quintas sean no sólo de razón \\(3/2\\). Si \\(\mu\\) es un número racional entre \\(2^{1/2}\\) (el tritono) y \\(2\\) (la octava), el _sistema pitagórico generalizado asociado a_ \\(\mu\\) es \\(P\_{\mu} = \\{\mu^{b} \mid b \in \mathbb{N}\\}\\). Es decir, \\(P\_{\mu}\\) es el conjunto de todas las razones necesarias por las cuales hay que multiplicar alguna frecuencia base para computar todas las clases tonales posibles. En otras palabras, un elemento \\(\mu^{b} \in P\_{\mu}\\) está asociado a la clase tonal que se encuentra a \\(b\\) quintas de distancia de la frecuencia inicial.

Puesto que nuestra convención es llamar de la misma manera a las notas que están a una potencia de \\(2\\) de distancia, hay que poner particular atención a los sistemas que contiene quintas que son potencias racionales de \\(2\\). Si \\(\mu = 2^{M/N}\\) entonces podemos identificar a \\(P\_{\mu}\\) con \\(Z\_N\\) pues \\(2^a\mu^b\\) está en la misma clase tonal de \\(2^c\mu^d\\) sí y sólo si \\(b \equiv\_{mod\ N}d\\) dado que \\(2^a\mu^{tN+r}=2^{a+Mt}\mu^r\\). O sea, uno puede identificar naturalmente a \\(P\_{\mu}\\) con \\(\mathbb{Z}\\) o con \\(\mathbb{Z}\_N\\) con la función \\(\mu^b \mapsto b\\).


### Haciendo un mini programita {#haciendo-un-mini-programita}

Con el interés de calcular algunos sistemas pitagóricos generalizados podemos hacer un programita que nos calcule las frecuencias automáticamente. Lo hacemos en Lisp porque Lisp es mi lenguaje de programación preferido pero también lo voy a traducir a python porque python es muy popular y si te interesa al menos un poco la programación es probable que puedas leer python aunque no tentas tanta fuidez al leer Lisp.

Para representar un sistema pitagórico en Lisp es muy sencillo. Lo podemos hacer con una clausura léxica. Definimos una variable con alcance léxico en la que guardamos el valor de nuestra quinta formal. Luego regresamos una función que eleva a la potencia \\(n\\) nuestro valor \\(\mu\\):

```common-lisp

(defun pysys (mu)
  (let ((m mu))
    (lambda (b) (expt m b))))

CL-USER> (pysys 1.5)

#<FUNCTION (LAMBDA (B) :IN PYSYS) {70084A8DEB}>

CL-USER> (funcall * 1)

1.5

CL-USER>
```

Así que la función `pysys` (por pythagorean system), dada una \\(\mu\\), regresa el sistema pitagórico asociado a \\(\mu\\). En python se puede escribir de la siguiente manera:

```python
def pySys(mu):
    return (lambda b: (mu ** b))
```

ahora bien, para tener una implementación completa de nuestro proceso de afinación necesitamos una función que ponga las clases tonales dentro de una octava. Esta también es muy sencilla. Va a recibir, primero una frecuencia arbitraria \\(f\\) y luego una frequencia base \\(r\\), si la frecuencia \\(f\\) se encuentra más cerca que una octava de \\(r\\) regresamos la misma frecuencia \\(f\\), pero si no, dividimos entre o multiplicamos por \\(2\\) la frecuencia \\(f\\) para acercarla a la octava. La función en Lisp queda así:

```common-lisp

(defun octavize (freq root)
  (cond
    ((> freq (* root 2)) (octavize (/ freq 2) root))
    ((< freq root) (octavize (* freq 2) root))
    (t freq)))
```

en python podemos hacer:

```python

def octavize(freq, root):
    if freq > (root * 2):
        return octavize(freq / 2, root)
    elif freq < root:
        return octavize(freq * 2, root)
    else:
        return freq
```

Para calcular algunas frecuencias símplemente fijamos algúna frecuencia base, digamos... \\(la = 440 Hz\\) y multiplicamos por nuestro sistema pitagórigo. En este ejemplo conseguimos el sistema pitagórico asociado a \\(1.5= \frac{3}{2}\\) con nuestra función `pysys` y luego multiplicamos la frecuencia \\(440\\) por el sistema en \\(1\\).

```common-lisp

CL-USER> (pysys 1.5)
#<FUNCTION (LAMBDA (B) :IN PYSYS) {70089EF29B}>
CL-USER> (* 440 (funcall * 1))
660.0
```

Para hacer las cosas más rápido podemos hacer una función como la siguiente:

```common-lisp

(defun collect-n-ratios (n mu)
  (let ((sys (py-sys mu)))
    (loop for i from 0 to (- n 1) collecting
          (funcall sys 0 i))))
```

Esta función calcula \\(n\\) elementos del sistema pitagórico asociado a \\(\mu\\):

```common-lisp

CL-USER> (collect-n-ratios 7 1.5)

(1.0 1.5 2.25 3.375 5.0625 7.59375 11.390625)
```

y para calcular algunas frecuencias basta utilizar `mapcar` en la lista que obtenemos con `collect-n-ratios`:

```common-lisp

(defun generate-scale (r n mu)
  (mapcar (lambda (x) (* r x))
          (collect-n-ratios n mu)))

(defun generate-scale-octavized (r n mu)
  (mapcar (lambda (x) (octavize x r))
          (mapcar (lambda (x) (* r x))
                  (collect-n-ratios n mu))))

CL-USER> (generate-scale-octavized 440 7 1.5)
(440.0 660.0 495.0 742.5 556.875 835.3125 626.4844)
```

En python podemos hacerlo de la siguiente manera:

```python

def stackFifths(root, ratio, times):
    lst = []
    for i in range(times):
        lst.insert(0, ratio ** i)
    lst.sort()
    lst = list(map(lambda x : x * root, lst))
    return lst

def generateScale(root, ratio, times):
    return list(map(lambda x : octavize (x, root), stackFifths(root,ratio,times)))
```


## Escalas bien formadas {#escalas-bien-formadas}

Pero ¿Cuántas notas generar? ¿Hay algúna razón por la cual no generar 8 notas?. Por ejemplo, tenemos en la música occidental la escala pentatónica, la escala diatónica y la cromática, pero nuestros sistemas pitagóricos nos ofrecen infinitas notas, bueno, al menos los que no son potencias racionales de \\(2\\). ¿Existe alguna propiedad notable en los números \\(5\\), \\(7\\), y \\(12\\)?, nos preguntamos ¿Existen números buenos \\(n\\) para generar escalas de \\(n\\) clases tonales?. Bueno, pues las escalas pentatónica, diatónica y cromática cumplen una propiedad bastante simpática. Si primero las generamos apilando quintas y las ordenamos en un círculo, obtenemos un polígono de \\(5\\), \\(7\\) o \\(12\\) lados. Pero si después las conectamos de tal manera que se sigue un orden de escala se preserva la simetría del polígono.

{{< figure src="/images/Image002.png" >}}

Si a estas notas las re-etiquetamos de tal manera que sus nombres sean los elementos de \\(\mathbb{Z}\_N\\) entonces veremos que el hecho de que preserven las simetrías tiene que ver con que exista un automorfismo \\(\mathbb{Z}\_N \to \mathbb{Z}\_N\\) que las ordene en orden de escala. No está dentro del alcance de esta notita demostrar el siguiente teorema pero lo citamos de todas maneras porque es, además de muy lindo, lo que responde las preguntas del párrafo anterior. El teorema lo saqué del [artículo del que saqué todo esto](https://www.researchgate.net/publication/215646485_Aspects_of_Well-Formed_Scales).

**Teorema:** Sea \\(\mu\\) una quinta formal y \\(Z\_B = 0, \ldots, B-1\\) las classes tonales dentro del sistema asociado a \\(\mu\\). El conjunto \\(Z\_B\\) es una **escala bien formada** si y sólo si \\(\frac{A}{B}\\) es un (semi)convergente en la expansión en fracción continua de \\(log\_2(\mu)\\) y el automorfismo que pone en orden de escala a \\(Z\_B\\) es:

\\[\omega\_B: Z\_B \to Z\_B: z \mapsto zb\_k (-1)^k\\]

en donde \\(\frac{a\_k}{b\_k}\\) es el convergente completo anterior a \\(\frac{A}{B}\\).


### Ejemplo 1 {#ejemplo-1}

Como ejemplo calculamos las escalas bien formadas de \\(\mu = 3/2\\). Tenemos que la expansión en fracción continua de \\(log\_2(3/2)\\) es \\(log\_(3/2)=[0,1, 1, 2, 2, 3, 1, 5, 2, 23, 2, 2, 1, 1, 55, 1, 4]\\). Entonces la suceción de (semi)convergentes empezando en \\(k = 1\\) es: \\([0,1]=1,[0,1,1]=\frac{1}{2}\\),
\\([0,1,1,1]=\frac{2}{3}\\), \\([0,1,1,2]=\frac{3}{5}\\), \\([0,1,1,2,1]=\frac{4}{7}\\), \\([0,1,1,2,2]=\frac{7}{12}\\), \\([0,1,1,2,2,1]=\frac{10}{17}\\), y así sucesivamente. De tal manera que las escalas bien formadas asociadas a \\(\mu = 1.5\\) son las que tienen $1,2,3,5,7,12,17,...$ notas.

Consideremos ahora la escala pentatónica representada por \\(\mathbb{Z}\_5\\). Si comenzamos en fa a apilar quintas obtenemos fa do sol re la. En la escala pentatónica el convergente que nos da el 5 es \\([0,1,1,2]=\frac{3}{5}\\) y el convergente anterior completo es \\([0,1,1]= \frac{1}{2}\\) por lo tanto el automorfismo que pone las notas fa do sol re la en orden de escala debe ser \\(z \mapsto z2(-1)^2\_{mod\ 5}= z2\_{mod\ 5}\\)

| solfeo | \\(z\\) | \\(2z\\) | \\(2z \_{mod\ 5}\\) | solfeo |
|--------|---------|----------|---------------------|--------|
| fa     | 0       | 0        | 0                   | fa     |
| do     | 1       | 2        | 2                   | sol    |
| sol    | 2       | 4        | 4                   | la     |
| re     | 3       | 6        | 1                   | do     |
| la     | 4       | 8        | 3                   | re     |

{{< figure src="/images/Image003.png" >}}

para \\(B = 7\\) el convergente es de hecho el semiconvergente \\([0,1,1,2,1]= \frac{4}{7}\\) y el convergente anterior es \\([0,1,1,2]= \frac{3}{5}\\). Por lo tanto el automorfismo que acomoda las notas en orden de escala es \\(z \mapsto z5(-1)^3\_{mod\ 7}= -5z \_{mod\ 7} = 2 z\_{mod\ 7}\\). Ahora, si apilamos quintas en fa obtenemos fa do sol re la mi si y la tabla queda:

| solfeo | \\(z\\) | \\(2z\\) | \\(2z \_{mod\ 7}\\) | solfeo |
|--------|---------|----------|---------------------|--------|
| fa     | 0       | 0        | 0                   | fa     |
| do     | 1       | 2        | 2                   | sol    |
| sol    | 2       | 4        | 4                   | la     |
| re     | 3       | 6        | 6                   | si     |
| la     | 4       | 8        | 1                   | do     |
| mi     | 5       | 10       | 3                   | re     |
| si     | 6       | 12       | 5                   | mi     |

{{< figure src="/images/Image004.png" >}}


### Ejemplo 2 {#ejemplo-2}

Ahora calculamos la tabla para \\(9\\) EDO (equal divisions of octave, es decir, divisiones iguales de la octava) \\(\mu = 2^{5/9}\\). La expansión de \\(log\_2(2^{5/9})\\) es \\([1,1,4]\\) y la secuencia de (semi-)convergentes es: \\([0,1]=1\\), \\([0,1,1]=\frac{1}{2}\\), \\([0,1,1,1]=\frac{2}{3}\\), \\([0,1,1,2]=\frac{3}{5}\\), \\([0,1,1,3]=\frac{4}{7}\\), \\([0,1,1,4]=\frac{5}{9}\\). Para la escala de \\(5\\) representada por \\(\mathbb{Z}\_B\\) el automorfismo que buscamos es \\(z\mapsto z2\_{mod 5}\\)

| solfeo | \\(z\\) | \\(2z\\) | \\(2z mod 5\\) | solfeo |
|--------|---------|----------|----------------|--------|
| fa     | 0       | 0        | 0              | fa     |
| do     | 1       | 2        | 2              | sol    |
| sol    | 2       | 4        | 4              | la     |
| re     | 3       | 6        | 1              | do     |
| la     | 4       | 8        | 3              | re     |

y para la escala de 9 tonos tenemos la siguiente tabla:

| solfeo | \\(z\\) | \\(2z\\) | \\(2z mod 9\\) | solfeo |
|--------|---------|----------|----------------|--------|
| fa     | 0       | 0        | 0              | fa     |
| do     | 1       | 2        | 2              | sol    |
| sol    | 2       | 4        | 4              | la     |
| re     | 3       | 6        | 6              | si     |
| la     | 4       | 8        | 8              | bet    |
| mi     | 5       | 10       | 1              | do     |
| si     | 6       | 12       | 3              | re     |
| al     | 7       | 14       | 5              | mi     |
| bet    | 8       | 16       | 7              | al     |

en donde _al_ y _bet_ son las primeras sílabas de _alfa_ y _beta_ que usamos aquí para extender el sistema de solfeo normal.

{{< figure src="/images/Image005.png" >}}


## Afinación en Kontakt {#afinación-en-kontakt}

Ahora explicamos un poquito cómo escuchar estas escalas. Una manera sencilla y, por supuesto muy útil, que hay para escuchar estas escalas es haciendo un script para el sampler [kontakt](https://www.native-instruments.com/en/products/komplete/samplers/kontakt-6/). Para esto, utilizaremos la herramienta [scale workshop](https://sevish.com/scaleworkshop/). Esa app nos permite, entre otras cosas, especificar en el espacio todas las razones entre las notas de una escala y exportarla en varios formatos, entre ellos, el de afinación en kontakt.

Para entrar manualmente las razones de una escala, scale workshop acepta diferentes notaciones. Los números que contienen un punto `.` son cents, los números que contienen una diagonal `/` son razones, los que están escritos en el formato `n\m` son \\(n\\) grados de \\(m\\) divisiones iguales de octava. El último número se refiere a la _octava formal_ sea octava `2/1` o lo que llaman una _pseudo octava_.

Aprovechando que hicimos un programita para calcular las frecuencias me parece más cómodo convertir los sistemas que calculamos a cents. Recordemos que un _cent_ es una centésima división de una doceava división igual de octava, es decir que la razón en cents entre una frecuencia \\(f\_1\\) y otra \\(f\_2\\) está dada por:

\\[c =  log\_2(\frac{f\_2}{f\_1})\*1200\\]

con esta formula podemos definir una función `inCents` que reciba dos frecuencias y nos dé la razón en cents entre ellas:

```python

def inCents(f1,f2):
    return math.log(f2 / f1, 2)*1200
```

y podemos mapear esta función a una lista de frecuencias:

```python

def scaleInCents(freqlist,root):
    return list(map(lambda x: inCents(root, x), freqlist))
```

En lisp podemos hacer algo similar:

```common-lisp

(defun generate-diatonic (root ratio times)
  (mapcar (lambda (x) (octavize x root))
          (stack-fifths root ratio times)))

(defun in-cents (f1 f2)
  (* 1200
     (log (/ f2 f1) 2)))

(defun diatonic-in-cents (freq-list root)
  (mapcar (lambda (x) (in-cents root x))
          freq-list))

(defun generate-in-cents (ratio times)
  (cdr
   (diatonic-in-cents
    (sort (generate-diatonic 1 ratio times) #'<) 1)))
```

Por ejemplo:

```common-lisp

CL-USER> (generate-in-cents 1.5 17)
(23.46001 113.685 203.90999 227.37 317.595 407.81998 431.27997 521.505
 611.73004 701.95496 725.41504 815.63995 905.8649 929.3251 1019.55 1109.775)
```

Ahora podemos usar scale workshop para anotar todas nuestras razones y exportarlas en formato `kontakt tuning script`

{{< figure src="/images/sw1.png" >}}

Ahora lo único que hay que hacer es poner el script en una tab vacía del editor de scripts de Kontakt como se muestra aquí:

{{< figure src="/images/skw1.gif" >}}
