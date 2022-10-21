---
toc: true
layout: post
description: "I figure out how to do some basic calculation operations in J that I've needed for my mathematics study."
categories: [j, mathematics, q31, mu123]
comments: true
author: Alex Strick van Linschoten
title: "Using J for simple calculation"
image: images/mu123-unit1/jblue.png
---

I'm curious to see if I can improve my command of [some J idioms and syntax](https://www.jsoftware.com) while working on [my mathematics study](https://www.open.ac.uk/courses/maths/degrees/bsc-mathematics-q31) at the Open University. People sometimes (pejoratively) say that J is just a calculator on steroids. I happen to need a calculator from time to time doing [MU123](https://www.open.ac.uk/courses/modules/mu123), so I figure I'll just learn as I go.

I [wrote last week](https://mlops.systems/j/mathematics/mu123/q31/notation/2022/10/16/notational-precedence.html) about orders of precedence and notation in J (and for mathematics in general) so I won't repeat myself here. The summary of that all is that J evaluates from right to left in the order that expressions are encountered.

## Some ultra basics

Negative numbers are denoted by the underscore (`_`) symbol:

```shell
   3-4
_1
```

Note that there can't be any space in between the underscore and the number that's negative.

Multiplication is handled by the asterisk (`*`) symbol, much like elsewhere in the world of computers:

```shell
   3*5
15
```

Division is handled by the percentage (`%`) symbol:

```shell
   15 % 5
3
```

While we're here at simple operators, we can specify power operations with the caret:

```shell
   3^2
9
```

The square root is calculated by using `%:` as in the following calculation:

```shell
   %:9
3
```

## Fractions and Rational Numbers

In J, the letter `r` is used for the notation of rational numbers (i.e. the numbers which represent a ratio of the two integers). For example, to represent two-thirds, you would write `2r3`. J is smart about interactions between rationals (i.e. fractions), so you can use them in calculations:

```shell
   1r2 * 6r4
3r4
```

If you want to turn a decimal number into a fraction / rational number, use `x:` as in the following example:

```shell
   x:0.3
3r10
   x:0.97
97r100
```

## Assigning variables

Variables and algebra hasn't come up too much so far in MU123, but as a sneak peek, in J you assign variables using `=.` as in:

```shell
a=.4
b=.0.6
c=._0.3
```

## Open Questions

I'm still looking for the J way to do rounding (i.e. decimal places and significant figures). I did see [one example on the J wiki](https://code.jsoftware.com/wiki/Fifty_Shades_of_J/Chapter_28#Grouping_and_Rounding) which went like this:

```shell
   R=: <.@(0.5&+)
   R 9.5
10
```

So that rounds numbers up. On the first line some kind of function is defined and then on the second line we're applying it to the number 9.5 which rounds up to 10. I assume the logic of that function is 'round down to the nearest integer if the decimal value is less than 0.5, but round up if it's greater than 0.5'.

I'm also trying to figure out how exactly to use J in Jupyter notebooks. I use [fastpages](https://fastpages.fast.ai) for this blog and one feature is that you can publish your notebooks and they get converted into blog pages. If I had a way to write J-backed notebooks, that'd be great. (At the moment, it seems [these files](https://github.com/tmcguirefl/J-Jupyter) are the main options and guides.)

## Acknowledgements

I learned a lot from the 'Numbers' section in [Learning J](https://www.jsoftware.com/help/learning/19.htm) while figuring out how to do these things.