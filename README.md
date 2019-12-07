# transfinite
Small library for working with ordinals and transfinite numbers.

**This repository is being merged into [`komiamiko/doodads`](https://github.com/komiamiko/doodads). Please go there for recent code. This repository will go down sometime in the future.**

Background
---

This is good for emerging mathematicians who want to explore ordinals and experienced mathematicians who want to do heavy lifting with ordinals. It is based on some serious high level set theory, so do not feel bad if some things look strange!

This started as just a small project to occupy my time, though it does indeed fill a niche - a software package for ordinal numbers.

The project is currently in early development, and no expert mathematicians have reviewed it to make sure all the math is correct.

Mathematical Limitations
---
If you aren't familiar with ordinals, skip this section, because it won't make sense.

No matter what system we choose to represent and work with ordinals in, it always becomes possible to construct an ordinal that is outside that system. If your system is the natural numbers, we can build *ω* which is outside of it. If your system is the Cantor normal form, we can build *ε_0* which is outside of it. If your system is the Veblen normal form, we can build *Γ_0* which is outside of it.

The current implementation is based on the Cantor normal form. These representations look like a polynomial in *ω*: *ω^β_1 c_1 + ω^β_2 c_2 + ... + ω^β_n c_n*, where *β* are ordinal and *c* are positive integers. Where this breaks down is at *ε_0*, which is the limit of *ω^ω^ω^...*, and has the special property that *ε_0 = ω^ε_0*. Written as tetration, *ε_0 = ω↑↑ω*. You can imagine this special ordinal *ε_0* causes problems for the Cantor normal form. Thus this implementation works great for everything below it but has no support at and above *ε_0*.

Each smaller hierarchy is able to take advantage of higher level strangeness that does not apply to it. For example, below *ω*, we have *a+b=b+a*, and below *ε_0*, we have *ω^a > a*. This can be used to take shortcuts, and in fact, it is necessary to use rules from the lower hierarchies in working with the higher ones. A later version of this library will use a unified `ordinal` class which keeps ordinals represented in the smallest hierarchy that can contain it.

Ordinals? Transfinite?
---
We should begin our journey at the concept of transfinite numbers. In short, they are not quite finite, and not quite infinite.

Finite numbers are great and all, but only for describing finite things. What is the size of the natural numbers? Well, we can count: *0, 1, 2, 3, ...* (we start at 0 here) and it goes on forever. There's clearly infinity (∞) of them, right?

The issue with infinity is that it causes all sorts of weird issues and paradoxes. A simple demonstration of this is comparing the size of the even natural numbers and the size of the natural numbers.

The natural numbers go like *0, 1, 2, 3, 4, 5, 6, 7, 8, ...*

The even natural numbers go like *0, 2, 4, 6, 8, ...*

Suppose there are *N* natural numbers. Well then, clearly the even natural numbers take up half of the natural numbers, so the number of even natural numbers is *N/2*. But wait, if we divide the even natural numbers in half, suddenly the sequence is exactly the same, so it should be *N*. So we have *N = N/2*. We let this pass because it's infinite. Sometimes, however, we want to quantify the size of the infinitys. How is the size of the rational numbers related to the size of the natural numbers? Saying they're both infinite isn't very helpful, and clearly there's more rational numbers than natural numbers.

What we're looking for is something that is larger than all the finite numbers, yet still behaves somewhat like a finite number and not an infinity. Voila, we have invented transfinite numbers.

As it turns out, in transfinite land, we must differentiate ordinals and cardinals. Ordinals are the order of things, like using the number 4 to say something is 4th in line. Cardinals are the size of things, like using the number 4 to say there are 4 things in a bag. The size of the natural numbers we call aleph 0. The order of the natural numbers we call omega (ω).

To help us understand ordinals, we'll need to grasp the concept of an order type. As the name suggests, an order type describes ordering. For any natural number *N*, this is the finite sequence *0, 1 < 2 < 3 < ... < N-1*. Notice, it has a start and an end, and contains *N* elements, and every element is less than *N*. For ω, this is instead the natural numbers: *0 < 1 < 2 < 3 < ...*. Notice, it has a start but no end, and its size is not finite. Every element here is less than ω. Reworded to be more useful, ω is the smallest ordinal larger than every natural number. We will say 2 order types are equivalent if we can relabel one and get the same properties of the other.

We can start with perhaps one of the most basic operations: addition. Addition of ordinals means to concatenate their order types.

As an example, we will show that 2+5 = 7 = 5+2.

```
2 + 5 = 0  < 1  < 0' < 1' < 2' < 3' < 4'
    7 = 0  < 1  < 2  < 3  < 4  < 5  < 6
5 + 2 = 0  < 1  < 2  < 3  < 4  < 0' < 1'
```

In fact, addition is commutative in the natural numbers, meaning you can always change the order and the result never changes.

The first strangeness comes at ω. Let us see that 3+ω = ω. This sounds strange indeed, so we should take the time to see why.

```
3 + ω = 0  < 1  < 2  < 3  < 0  < 1' < 2' < 3' < 4' < ...
    ω = 0  < 1  < 2  < 3  < 4  < 5  < 6  < 7  < 8  < ...
```

Relabeling aside, we can see from the properties that 3+ω = ω. Notice that both sequences have a start but no end, and their size is not finite.

It gets stranger. ω+3 is not the same as ω.

```
ω + 3 = 0  < 1  < 2  < 3  < 4  < 5  < 6  < 7  < 8  < ... < 0' < 1' < 2'
    ω = 0  < 1  < 2  < 3  < 4  < 5  < 6  < 7  < 8  < ...
```

We know all those natural numbers would come before 0'. What comes directly before 0' though? What is the previous item? It would be the largest natural number, which is not defined. Thus there is no item directly before 0'. Also interestingly, this sequence ω+3 has an end: there is nothing after 2'.

And so, when transfinite ordinals are involved, addition is no longer commutative.

When delving deeper, you'll need to understand the set theoretic view of ordinals. When it makes sense why we define the order sequence to start at *0* and go up to but not including the ordinal, you're at a good point. For multiplication, exponentiation, and other interesting things you can do with transfinite ordinals, go explore on your own. There are plenty of resources online.
