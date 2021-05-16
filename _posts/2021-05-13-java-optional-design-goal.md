---
title: "How we should extract the value from a Java Optional"
tags: [java-8, optional]
date: 2021-05-13
slug: java-optional-design-goal
description: "Main goal design goal of Java Optional."
---

## What is the main design goal Java Optional?
// TODO add design of an optional image

Optional was designed for a limited uses, but frequently it is misused. In this post we recap the original intention of Optional, review misuses and abuses of Optional, and analyze best practices.

Optional is a library method return type that may or may not be present. When we need to represent in API that the return value may be "no result".

Optional<T> represents 2 states: empty or present (it contains a non null reference of type T).

Don't use null as return value of a library method where there is need to represent "no result".

never, ever, use null for an optional variable or return value.

## How we should instantiate an Optional?
[Misuse] initilize an optional with a null reference easily will end in NPE.

[Best-practice] Initilize the optional by .ofNullable(T value) or if you are sure T is not null .of(T value).

## Not as class field
A class field is inside your scope you can controll the null references. It s worthy the pain to control the null reference and avoid created redundant objects. Dont use optional as fields, better use 'null object' pattern of nullable fields.

## Not as method parameter
- it is a code smell that your method code is going to do more than once thing.
and u are forcing every caller to create a optional. Resources!!
Better 2 methods overload

## Dont use chain methods
Xq no usar cadenas de optional y lanzar la exc al final. Al final no sabes q fallo

Abuse
It is generally a bad idea to create an Optional for the specific purpose of chaining methods from it to get a value.
avoding if statments
creating optional for just chaining method and extract the the value is a bad idea.
Optinal.ofNullable(string).orElseGet(this::getDefault);
ternary-if better

 it sgenrally a bad idea to create an optional for the specifc purpose of chaingin methods form it to get a values5.

## Dont use it in Collections, Stream or inside other Optional
optional is a wrapper object with a bare reference. It can urn in performance/ memory spaces problems
space issues

## dont use as method variable
dont replace every null for optional, if the null are well controller. null for private fields can be Easilychecked.

## dont reemplace every nullable
it is not a replacemtn for null
narrowly it is a replacemnet for return type might or might not have a value

- dont avoid null to at all cost.

if u have a filed that may or mya not contain a value, just use a nullable reference.
And in you API in the point you are return it create the optional.


Think about your internal representation different than your API. Optional is a API return value.

## null safe dereference(elvis) operator instead of optional

## as return value consume memory is very temporal

## In a nutshell
this API has very limited uses and normally is extensively use in wrong places.

you are calling someone else code and this return mya or may to return a value
