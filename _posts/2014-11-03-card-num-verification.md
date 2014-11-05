---
layout: post
title: Credit Card Numbers
---

{% highlight erlang %}

-module(cc).
-import(lists,[reverse/1,filter/2]).
-export([is_valid/1]).

% converts base 10 integer to list of digits

to_gigits(0) -> [0].
to_digits(I) ->
	Base = 10,  
	{R,M} = {I div Base,I rem Base},
	if 
		R == 0 -> [M];
	   	  true -> to_digits(R) ++ [M]
	end.
	
to_digits_rev(I) -> reverse( to_digits(I) ).

% doubles every next element in list
% when indexing from 0 => 1,3,5 ... odd 

double_second([]) -> [];
double_second([X]) -> [X];
double_second( [X0,X1 | XS]  ) -> [X0 |  [ 2*X1 | double_second(XS) ] ].

% sums digits of list elements
% for example: [1,12,3]  ->  1 + 1 + 2 + 3 

sum_digits([]) -> 0;
sum_digits([X]) when (0 =< X) and (X =< 9) -> X;
sum_digits([X]) -> sum_digits( to_digits(X) );

sum_digits([X|XS]) -> sum_digits([X]) + sum_digits(XS).

is_valid(X) -> 
	sum_digits( double_second( to_digits_rev(X) ) ) rem 10 == 0.

{% endhighlight %}

Assuming Nums is list of cc numbers, to pick the valid ones, do:

{% highlight erlang %}

> filter( fun is_valid/1, Nums ).

{% endhighlight %}
