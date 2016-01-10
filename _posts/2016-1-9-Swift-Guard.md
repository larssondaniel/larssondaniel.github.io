---
layout: post
title: Swift guard
---

<p class="lead">When Swift 2.0 was released a few months ago, one of the new features that were added was the guard statement.</p>

Ever since, I have had quite a few discussions with other developers on how this can make our lives easier, and this are my thoughts on it.

## What is it?
A guard statement works a little bit like the opposite of an if statement. Instead of entering a block if the conditions are met, the block is exited. Constants and variables that have been assigned a variable in the guard statement can be used for the rest of the guard statement's block.

The else clause is mandatory, and must exit the scope by either calling a @noreturn function, or by using any of the following:

* return
* break
* continue
* throw

## How do I use it?

{% highlight swift %}

func someCheck(foo: String?) {
    if foo == nil {
        // Requirement not met
        return
    }

    // foo is fine, go ahead and use it
    foo!.count
}

{% endhighlight %}

This has been the way to do it for quite some time, until Apple gave us an alternative when they introduced optional binding. With optionals, you could instead do this:

{% highlight swift %}

func someCheck(foo: String?) {
    if let foo = foo {
        // foo is fine, go ahead and use it
        foo.count
    }

    // Requirement not met
}

{% endhighlight %}

Which removes some issues from the first snippet. First of all, we no longer have to force unwrap the optional when we actually want to use it. Also, we now check for success instead of looking for an error, which I believe makes for better code. The problem that occurs with this is, however, that we now need to put the code that we want to run within the conditions, instead of putting it after the condition checks have passed.

Now, with the guard statement, we can simply do this (notice how it's used in the complete opposite way of an if statement):

{% highlight swift %}

func someCheck(foo: String?) {
    guard let foo = foo else {
        // Requirement not met
        return
    }

    // foo is fine, go ahead and use it
    foo.count
}

{% endhighlight %}

Success! Another great thing about the guard statement is that you no longer end up with your checks and your error throwing spread all over your code. Instead of:

{% highlight swift %}

func someThrowableCheck(foo: String?) {
    if let _ = foo.characters.contains("bar") {

        // 20+ lines of working with foo

    } else {
        throw SomeError
    }
}

{% endhighlight %}

We can throw the error at the same place in the code as where we find it:

{% highlight swift %}

func someThrowableCheck(foo: String?) throws {
    guard let _ = foo.characters.contains("bar") else {
        throw SomeError }

    // 20+ lines of working with foo
}

{% endhighlight %}
