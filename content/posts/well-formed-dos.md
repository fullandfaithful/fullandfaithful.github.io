+++
title = "Pseudoescalas y polytuning"
author = ["Abraham Aguilar"]
publishDate = 2022-05-22T00:00:00-05:00
tags = ["música"]
draft = false
series = "Escalas bien formadas"
+++

En [la nota anterior](https://fullandfaithful.com/posts/well-formed-scales/) vimos qué era un sistema de afinación pitagórico generalizado. Para recapitular, toda quinta formal \\(\mu\\) tiene un sistema de afinación asociado. Este sistema de afinación asociado tiene escalas que representamos con los conjuntos \\(\mathbb{Z}\_n\\). Nos gustaría, con el fin de crear recursos para componer música con estos sitemas, ver si existe algúna manera matemáticamente significativa de relacionar sistemas de afinación. Mientras que la elección de una quinta formal puede resultar algo arbitrario o trabajo para un estudio psicoacústico, el concepto de una escala bien formada nos da una generalización muy natural. Vamos a ver cuál es.

Primero, podemos observar que dado cualquier sistema de afinación, los datos que constituyen a las escalas bien formadas son:

1.  un conjunto de \\(n\\) notas, representado por \\(\mathbb{Z}\_n\\)
2.  un automorfismo que ordena las notas en orden de escala \\(\mathbb{Z}\_n \to \mathbb{Z}\_n\\)

como recurso musical, podemos perseguir la siguiente generalización: que el automorfismo no sea necesariamente el que ordena a las notas en orden de escala. En otras palabras, podemos debilitar los datos para conseguir _escalas generalizadas_ o _pseudoescalas_ cuyos constituyentes sean:

1.  un conjunto de \\(n\\) notas, representado por \\(\mathbb{Z}\_n\\)
2.  un automorfismo arbitrario \\(x:\mathbb{Z}\_n \to \mathbb{Z}\_n\\)

Es un hecho conocido que el grupo de automorfismos de \\(Z\_n\\) es \\(Aut(Z\_n)=\\{\sigma\_a \mid a \in U(n)\\}\\) en donde \\(U(n)\\) es el conjunto (el grupo) de los \\(k \in \mathbb{Z}\_n\\) tales que el máximo común divisor \\(mcd(n,k) = 1\\). Esto quiere decir que para cada conjunto \\(\mathbb{Z}\_n\\) hay tantas escala o pseudoescalas como números primos relativos a \\(n\\). Visualmente, los automorfismos de \\(Z\_n\\) son "ordenamientos" de sus elementos. Por ejemplo en este visual las flechas indican el orden de los elementos:

{{< figure src="/images/Image006.png" >}}

Vista desde esta perspectiva, una escala bien formada es una escala generalizada \\((Z\_n, x)\\) para la cual existe un sistema de afinación \\(P\_{}\mu\\) en el que \\(x\\) ordena a \\(Z\_n\\) en orden de escala. Se dice, entonces que una pseudoescala \\((Z\_n, x)\\) es bien formada respecto a \\(\mu\\). Utilizando los ejemplos de la nota pasada podemos decir que \\((Z\_5, 2z)\\) es bien formada respecto al sistema de afinación \\(\mu=\frac{3}{2}\\). La escala \\((Z\_7, 2z)\\) es bien formada respecto al mismo sistema.

Hay que notar que, en la nota pasada, en el ejemplo 2, calculamos que \\((Z\_5,2z)\\) también es bien formada en el sistema de \\(9\\) EDO. Esto nos lleva al concepto de polytuning. Dada una pseudoescala \\(X\\), un **sistema de polytuning**, o **sistema de poliafinación** para tal escala, es una colección de sistemas de afinación \\(\mathcal{A}=\\{P\_{\mu\_1},\ldots,P\_{\mu\_k}\\}\\) en donde la escala \\(X\\) es bien formada respecto a cada sistema de afinación \\(P\_{\mu\_i}\\). Como ejemplo tenemos el sistema de polituning formado por los sistemas pitagórico y de 9 EDO que forman un sistema de polytuning para la escala \\((Z\_5,2z)\\).


## Escalas de transición {#escalas-de-transición}

Otro recurso que de inmediato podemos reconocer es el de morfismos entre escalas. Dadas dos escalas como en el diagrama:

{{< figure src="/images/Image007.png" >}}

una **(pseudo)escala de transición** es cualquiér automorfismo que haga conmutar el diagrama:

{{< figure src="/images/Image008.png" >}}

Por ejemplo, las escalas \\(A=(Z\_7,2z)\\) y \\(B=(Z\_7,5z)\\), tienen una escala de transición \\(C = (Z\_7,6z)\\), puesto que \\(2( 5^{-1})z=2(3)z\_{mod\ 7}=6(z)\_{mod\ 7}\\).
