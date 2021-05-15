---
title: "How to get value from Optional in Java"
tags: [java-8, java-10, optional, clean-code]
date: 2021-05-09
slug: java-optional-get-value
description: "Best practices for extracting the contained value from a Java Optional."
---

Optional is with us since Java 8, and it is extensively used in modern Java. Optional API provides in order to extract the contained value a self-explanatory method `get()`. This is a tricky method that uses to cause cluttered and buggy code. In this post we are going show why we should avoid it, while spotlighting their downsides and showing better alternatives.

## We should never use `get()` unless we prove the contained value is present
Sooner than later this code will cause a `NoSuchElementException`.

``` java
String getGreeting(String email) {
   Optional<User> optionalUser = userFinder.findByEmail(email);
   User user = optional.get(); // potential NoSuchElementException!!!
   return user.getSalutation();
}
```

## Neither use `isPresent()` / `get()` block is a good idea
We can prove the presence and extract the contained value by `isPresent()` / `get()` block.

``` java
String getGreeting(String email) {
   Optional<User> user = userFinder.findByEmail(email);
   if (user.isPresent()) {
      return user.get().getSalutation();
   }
   return "Hello";
}
```    

This is certainly an effective approach. Our piece of code is protected against `NoSuchElementException`, but this is still a poor solution. It is so cluttered as legacy Java code was when we were checking constantly for null references.

``` java
String getGreeting(String email) {
   User user = userFinder.findByEmail(email);
   if (user != null) {
      return user.getSalutation();
   }
   return "Hello";
}
```

## Generally a better practice is to use one of the given `orElse...`
The Optional API provides a much less cluttered and defensive methods to get the contained value. In case the contained value is not present they give us different approaches to get over:
* `public T orElse(T other)`
* `public T orElseGet(Supplier<? extends T> supplier)`
* `public T orElseThrow()`
* `public <X extends Throwable> T orElseThrow(Supplier<? extends X> exceptionSupplier) throws X`

Our previous sample could be refactored succinctly using the overloaded method `public T orElse(T other)`. We choose this because the fallback variable `"Hello"` does not require any computational work for being generated.

``` java
String getGreeting(String email) {
   Optional<String> salutation = userFinder.findByEmail(email).map(User::getSalutation);
   return salutation.orElse("Hello");
}
```

## `orElse()` vs `orElseGet()`
We should be carefully when the fallback value is not already known and has to be generated. Always in this case we should select `T orElseGet(Supplier<? extends T> supplier)`, doing so we are saving the computation work needed to generate it when it is not going to be used.

Otherwise, if the fallback operation is extremely costly, i.e. db query, we could face a high performance impact.

``` java
String getGreeting(String email) {
   Optional<String> salutation = userFinder.findByEmail(email).map(User::getSalutation);
   // getSalutation() is computed even when the user is present!!!
   return salutation.orElse(getSalutation());
}

String getSalutation() {
   // computation work!!!
   // be very careful with very costly operations, i.e. db query !!!
   return ...;
}
```

## Best practice, we should use `orElseGet(Supplier<? extends T> supplier)` instead of `orElse(T other)` if `other` is generated only for being used in the or else case.
The parameter of `orElse(T other)` is evaluated even when the optional contained value is present. Instead the supplier parameter of `orElseGet(Supplier<? extends T> supplier)` is applied **only** when the optional value is absent.

``` java
String getGreeting(String email) {
   Optional<String> salutation = userFinder
         .findByEmail(email)
         .map(User::getSalutation);
   return salutation.orElseGet(() -> getSalutation());
}

String getSalutation() {
   return // very cost operation, ie db query
}
```

In our sample the costly operation in `getSalutation()` is evaluated only when it is needed, when the user is absent.

## Lastly, consider use `orElseThrow(Supplier<T>)` over `orElseThrow()`
While `orElseThrow(Supplier<>)` is in Optional API since the beginning, `orElseThrow()` was introduced in Java 10 (non-LTS). Both throw a `NoSuchElementException` if the value is not present.

``` java
User fetchUser(String email) {
   Optional<User> user = userFinder.findByEmail(email);
   return user.orElseThrow();
}
```

In case of absent user the resulted stack trace looks like this.

``` console
java.util.NoSuchElementException: No value present
   at java.base/java.util.Optional.orElseThrow(Optional.java:382)
   at Main.fetchUser(Main.java:20)
   at Main.main(Main.java:15)
```

While `orElseThrow()` ends up in a very clean code, the stack trace lacks the missed email that cause this exception. Unless the given parameter is sensitive data that we should not be logged, it is preferable used `orElseThrow(Supplier<>)`, because we can provide a richer message to the exception that we will help us in future analysis of buggy issues.

``` java
User fetchUser(String email) {
   return user.orElseThrow(() -> new NoSuchElementException("No email ''" + email + "' present'"));
}
```    

With the custom message we get a more helpful stack trace with the email that caused the exception.

``` console
java.util.NoSuchElementException: No email 'dude@dude.com' present
   at Main.lambda$fetchUser$1(Main.java:20)
   at java.base/java.util.Optional.orElseThrow(Optional.java:408)
   at Main.fetchUser(Main.java:20)
   at Main.main(Main.java:15)
```        

If the exception message ends up very long, don't fall into this.

``` java
User fetchUser(String email) {
   var msg = "No email ''" + email + "' present'";
   return user.orElseThrow(() -> new NoSuchElementException(msg));
}
```  

As we are creating a new `msg` object every time `fetchUser(String)` is called, not just when the user optional is empty.

## In a nutshell
We should hardly ever use `get()` because it results in cluttered code with high chances of throwing a `NoSuchElementException`. Alternatively we should choose the `orElse(...)` alternative method that best suits our purposes.

It is not unreasonable that `get()` would be deprecated in future releases.

We should be aware when `orElse(T other)` could cause unnecessary potential performance impacts, that could be fixed just by replacing it by `orElseGet(Supplier<? extends T> supplier)`.

We should provide messages to our thrown exception with enough information to clear up the reason of our buggy scenarios.
