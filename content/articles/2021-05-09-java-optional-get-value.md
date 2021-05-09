---
title: "How we should extract the value from a Java Optional"
tags: [java-8, java-10, optional, clean-code]
date: 2021-05-09
slug: java-optional-get-value
description: "Best practices for extracting the containing value from a Java Optional."
---

Optional has been with us since Java 8, and it is extensively used in modern Java. In order to extract the containing value the Optional API provides the self-explanatory method `get()`. This is a tricky method that uses to cause cluttered and buggy code. In this post we are going show why we should avoid it, while spotlighting their downsides and showing better alternatives.

## We should not use `get()` unless we prove the containing value is present.
Sooner than later this code will cause a `NoSuchElementException`.

``` java
String getGreeting(String email) {
   Optional<User> optionalUser = userFinder.findByEmail(email);
   User user = optional.get(); // potential NoSuchElementException!!!
   return user.getSalutation();
}
```

## And neither use the `isPresent()` / `get()` block
We can prove the presence and extract the containing value by `isPresent()` / `get()` block.

``` java
String getGreeting(String email) {
   Optional<User> user = userFinder.findByEmail(email);
   if (user.isPresent()) {
      return user.get().getSalutation();
   }
   return "Hello";
}
```    

This is an effective approach, it protects our code from `NoSuchElementException`, but it is a poor solution. It is so cluttered as legacy Java code was when we were checking constantly for null references.

``` java
String getGreeting(String email) {
   User user = userFinder.findByEmail(email);
   if (user != null) {
      return user.getSalutation();
   }
   return "Hello";
}
```

## The best practice is to use a method of the `orElse(...)` family
The Optional API provides a much less cluttered and defensive methods to get the containing value. In case the containing value is not present they give us different approachs to get over:
* `public T orElse​(T other)`
* `public T orElseGet​(Supplier<? extends T> supplier)`
* `public T orElseThrow()`
* `public <X extends Throwable> T orElseThrow​(Supplier<? extends X> exceptionSupplier) throws X`

In our sample if there is no user with the given email, the default salutation is a compile constant. The way to go seems to be `orElse(T other)`.

``` java
String getGreeting(String email) {
   Optional<String> salutation = userFinder.findByEmail(email).map(User::getSalutation);
   return salutation.orElse("Hello");
}
```

## We should not use `orElse(T other)` if `other` is generated only for being used in the or else case.
If the evaluation of or else case requires any computation work, we should not use `orElse(T other)`. Even we could face a much worse scenario, where the operation is extremely costly, ie. db query. Overuse `orElse(T other)` may result in a performance impact.

``` java
String getGreeting(String email) {
   Optional<String> salutation = userFinder.findByEmail(email).map(User::getSalutation);
   // getSalutation() is evaluated even when the user is present!!!
   return salutation.orElse(getSalutation());
}

String getSalutation() {
   // computation work!!!
   // or even worse very costly operation, ie. db query !!!
   return ...;
}
```

## Best practice, we should use `orElseGet​(Supplier<? extends T> supplier)` instead of `orElse(T other)` if `other` is generated only for being used in the or else case.
The parameter of `orElse(T other)` is evaluated even when the optional containing value is present. Instead the supplier parameter of `orElseGet​(Supplier<? extends T> supplier)` is applied **only** when the optional value is absent.

``` java
String getGreeting(String email) {
   Optional<String> salutation = userFinder.findByEmail(email).map(User::getSalutation);
   return salutation.orElseGet(() -> getSalutation());
}

String getSalutation() {
   return // very cost operation, ie db query
}
```

In our sample the costly operation in `getSalutation()` is evaluated only when it is needed, when the user is absent.

## Lastly, consider use `orElseThrow​(Supplier<T>)` over `orElseThrow()`
While `orElseThrow​(Supplier<>)` is in Optional API since the beginning, `orElseThrow()` was introduced in Java 10 (non-LTS). `orElseThrow()` throws a `NoSuchElementException` if the value is not present.

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

While `orElseThrow()` ends up in a very clean code, the stack trace lacks the missed email that cause this exception. Unless the given parameter is sensitive data that we should not be logged, it is preferible used `orElseThrow​(Supplier<>)`, because we can provide a richer message to the exception that we will help us in future analysis of buggy issues.

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

# Conclusion
We should hardly ever use `get()` because it results in cluttered code with high chances of throwing a `NoSuchElementException`. Alternatively we should choose the `orElse(...)` alternative method that best suits our purposes.

It is not unreasonable that `get()` would be deprecated in future releases.

We should be aware when `orElse(T other)` could cause unnecessary potential performance impacts, that could be fixed just by replacing it by `orElseGet​(Supplier<? extends T> supplier)`.

We should provide messages to our thrown exception with enough information to clear up the reason of our buggy scenarios.
