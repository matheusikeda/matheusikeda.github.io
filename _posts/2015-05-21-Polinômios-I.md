---
layout: post
title: Polinômios I
---

Para praticar utilizaremos como tema polinômios. A representação destes basicamente é dado por uma lista na qual cada valor desta representa um coeficiente, e o grau da potência, o tamanho da mesma menos 1, dado que o último valor da lista representa o termo independente.
<br>
Exemplo:
Função: f(x) = \\(3x^4 + x^2 + 2x -5\\)
<br>
Lista: [3,0,1,2,-5]
 
Algumas operações com polinômios podem ser representadas por funções em Haskell. Primeiramente vamos verificar como a operação soma pode ser declarada. A soma de dois polinômios se dá pela soma dos coeficientes com coerência, logo, a dos elementos de uma lista a outra. Caso a soma seja de dois polinômios de potências diferentes torna-se necessário modificar a lista representada pelo o de menor potência, ou seja, normalizá-lo.
{% highlight haskell %}
normalize :: Polynomial -> Polynomial -> (Polynomial, Polynomial)
normalize a b
		|length a < length b = normalize ([0]++a) b
		|length a > length b = normalize a ([0]++b) 
		|otherwise = (a,b)
{% endhighlight %}  
A função acima recebe como parâmetro dois Polynomial, que nada mais é uma lista de inteiros [Int], e retorna uma tupla.
Exemplo
{% highlight haskell %}
normalize [1,2,0,4,3] [3,1,5]
([1,2,0,4,3], [0,0,3,1,5])
{% endhighlight %}  
Com esta função, a função soma pode ser desenvolvida. 
{% highlight haskell %}
(.+.) :: Polynomial -> Polynomial -> Polynomial
(.+.) a = uncurry (zipWith (+)) . normalize a
{% endhighlight %}  
A função acima utiliza a função zipWith já apresentada anteriormente, a uncurry na qual recebe como parâmetro um função e um tupla e retorna o resultado de uma função tendo como parâmetros os elementos da tupla, e a normalize.
A seguir um pequeno rastreio do que acontece na função.
![i](https://cloud.githubusercontent.com/assets/10578368/7751840/688a69ba-ffb2-11e4-8744-499933bd30d5.png)
<br>
Dado um polinômio, vamos realizar uma função que retorna o grau.
{% highlight haskell %}
type Polynomial = [Int]
grade :: Polynomial -> Int
grade [] = 0
grade (a:x) = length (a:x) - 1
{% endhighlight %}  
Como o tamanho da lista representa o grau do polinômio mais um, já que o último valor  representa o termo independente, somente subtraímos um do tamanho da lista calculado por length.


