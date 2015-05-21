---
layout: post
title: Polinômios II
---
Para praticar o uso de folds, vamos desenvolver a função derivative, na qual tem como resultado a derivada da função passada como parâmetro. Inicialmente, vamos declará-la com recursão e em seguida com foldr.
{% highlight haskell %}
derivative :: Polynomial -> Polynomial
derivative (x:xs) 
		|grade (x:xs) /= 0 = grade (x:xs) * x : derivative xs
		|otherwise = []
{% endhighlight %}  
A função grade retorna o grau da potência do polinômio. Basicamente, na função derivative, cada elemento é multiplicado pelo retorno de grade associado ao tamanho da lista.A seguir, o uso do foldr.
{% highlight haskell %}
derivative' :: Polynomial -> Polynomial
derivative' = snd . foldr f (1,[]) . init 
			where f x (e,l) = (e+1,x*e:l)
{% endhighlight %}  
Nesta, foi utilizado a função snd que retorna o segundo elemento de uma tupla, e a init que devolve uma lista sem o último elemento, já que a derivada deste é zero pois trata-se de uma constante. O resultado de foldr é uma tupla tendo como parâmetros o grau de potência e a lista formada pela derivada do polinômio. Note que a cada dobra, incrementa-se o parâmetro e, em seguida multiplica-se o elemento da lista ao mesmo. 
<br>
Exemplo:
{% highlight haskell %}
f(x) = 3x3 + 2x2 + 5x + 1 
<br>
derivative' [3,2,5,1] 
[9,4,5]
{% endhighlight %}  
Agora vamos fazer uma função que retorna o valor de f(x) dado um valor para x. 
<br>
Usando recursão:
{% highlight haskell %}
eval :: Int -> Polynomial -> Int
eval _ [] = 0
eval a (x:xs) = x * multExp (grade (x:xs)) a + eval a xs  
{% endhighlight %}  
A função multExp retorna o valor de um número elevado a um outro dado como parâmetro. Em eval, utilizamos esta para retornar o valor de x elevado ao valor do retorno da função grade. Logo, somamos todas os produtos de x e multExp associado a cada tamanho da lista passada como parâmetro.
<br>
Usando  foldr:
{% highlight haskell %}
eval' :: Int -> Polynomial -> Int
eval' v = snd . foldr g (0,0) 
		where g x (e,ac) = (e+1,x*multExp e v + ac)
{% endhighlight %}  
Neste caso, o foldr também retorna uma tupla, todavia, possui o expoente e o valor da soma, isto é, o valor de f(x), como elementos. Observa-se que a cada dobra, incrementamos a variável e, em seguida a utilizamos como parâmetro de multExp, multiplicando o resultado por x.
<br>
Exemplo:
{% highlight haskell %}
x = 2, f(x) = 3x3 + 2x2 + 5x + 1
<br>
eval' 2 [3,2,5,1] 
43
{% endhighlight %}  
