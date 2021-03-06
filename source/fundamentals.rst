Testing Fundamentals
=========================

This chapter is focused on the essentials of testing. Because we know that most readers are likely to come to Scala from a Java (or similar) environment based on JUnit, which is one of the one of the xUnit variants. we will begin this discussion with an example of how to use JUnit within Scala, since Java can be used and mixed into Scala programs. We’ll then take a look at a couple of introductory (and integrated) Scala examples and how to write them using JUnit and ScalaTest.


Dependencies
----------------------

You need to install the Scala Build Tool [#SBT]_ - The details of this tool are covered in the :doc:`environment`. There is no need to install Scala separately. What we'll be doing in this chapter uses SBT to install Scala on demand.

You must also install the Git distributed version control system (DVCS) but it is already on your computer if you are running OS X or Linux. Git is available for all major platforms [#GIT]_. Git is one of the preeminent version control systems and is used by GitHub [#GitHub]_, one of the most popular--and we think, best--sites for hosting free and open source projects. We use it for almost all of our public-facing projects.

.. note::

   Many folks often think of Git (the tool) and GitHub (a social coding service built around Git) as being one in the same. You can use Git with or without a hosted service and might feel the need to do so, especially when your project isn't open source or you wish to keep it private. We take advantage of one such service in our own work, Bitbucket [#Bitbucket]_, which offers a number of features for corporate customers and academic software projects. For example, we write our papers in Markdown and LaTeX and often keep them private--at least until they are accepted for publication. Our book is also an example of where we use Git on two systems--one for the book chapters (our publisher wants to sell books) and several for the code examples.

To do the examples in this chapter, you must install both of these. Installation is not covered as it is platform-specific. However, we point you to web materials.

Cloning the Exemplar
----------------------

An important defining principle of our book is that the text and examples should remain relevant long after you have decided the book is no longer needed. We keep all of our examples under version control. In addition to our dear readers, countless students, research collaborators, and we, ourselves, depend on the examples continuing to work long after we create them. It might go without saying, but we think there's nothing worse than getting a book where there is a typo or compilation error and having to waste time trying to convince the author to cough up the latest code (or fix it).

So without further ado, let's get the code for this chapter.
   
.. code-block:: shell

   $ git clone
   https://github.com/LoyolaChicagoCode/scala-tdd-fundamentals.git

   $ cd scala-tdd-fundamentals
   $ ls

At this point, it is worth noting that we do most of our work in a Unix style environment. You can also checkout our project using IntelliJ (covered int he next chapter) but we're mostly going to operate at the command line in this chapter. It really does help to illuminate the principles.

.. note:: 

   It is also worth noting at this point that we are command-line junkies. When it comes to things we are doing in a terminal session (e.g. entering commands and seeing output), at times we will need to break lines up for the purpose of formatting the book. Notably, the ``git clone`` command is entered entirely on one line without hitting the return (a.k.a. enter) key. Similarly, when we should you the output of a command, it is entirely possible the output you observe will be slightly different.

The Testing Mindset - Running Tests on Checkout
--------------------------------------------------

.. |sbt| replace:: ``sbt``

.. |git| replace:: ``git``

As you now have the source code, let's make sure we can compile and run the tests using |sbt|. We consider it a sign of a healthy project where you can immediately test the code as soon as you've downloaded it. We also think it can be part of a growth mindset when it comes to coding. If you think of testing as fun, you're more likely to do it. Not to sound hokey, but when you see others having fun when they checkout your code and gleefully observe the tests passing, you are multiplying the fun!

The following *compiles* the code we just checked out via |git|. |sbt|
generates a lot of output, mainly aimed at *helping* you when something goes wrong. Because dependencies are being pulled from the internet for the unit testing frameworks (JUnit and ScalaText) there is a chance you could see an error message but it is extremely unlikely. We're written the build file in such a way that it will evolve with Scala and its many moving parts.

.. literalinclude:: images/fundamentals/sbt-compile-wrap.txt

The most important line to observe in this output is the one beginning with ``[success]``. If for any reason you don't see this line when running it yourself, it is likely that something failed, more than likely a network connectivity problem (or some repository became unavailable, we hope, temporarily).

If you've indeed encountered success in building our code with |sbt|, the next step is to run the tests. This is achieved by running ``sbt test``.

.. literalinclude:: images/fundamentals/sbt-test-wrap.txt

Again there is a *lot* of output generated, and what you see may differ from what you see in this console output, owing to changes that occur between the time a book is published and other changes that we or the Scala developers may make.

You'll see a number of lines that begin with ``[info]``. This indicates that informative messages about tests are being written to the console. For our example, there are various tests being performed on our domain model (rational numbers). We have organized the tests according to the different methods in our implementation. Here, you can observe that we test everything from an internal helper method (the greatest common divisor), to initializer (construction) methods, and the each related groups of methods (for arithmetic, helpers, and equality/hashcode for use with other Scala classes).

While the approach to checking out code and testing it without knowing any details might appear a bit odd at first, it is deliberate on our part. There is a *mindset* to testing. When you do testing right, you should be able to check out someone's code, compile it, and run the tests. We'd also like to hope that when we checkout your code, the first thing we can do is to run tests and see evidence of good software engineering!

Without further ado, let's get started by looking at some code.

Assertions
-------------

A key notion of test-driven development and behavior-driven development is the ability to make a logical assertion about something that generally must hold *true* if the test is to pass. Because this is so fundamental to a *testing mindset*, we cover it separately before diving into various testing approaches.

.. todo::

	Joe - In my experience, the built in language assertions are usually a debug-build only sanity check that wouldn’t make sense for the cases where you’d normally use exceptions. There is a strong case for being able to run unit tests in debug, release, and obfuscated release modes to validate your product. Sometimes these language level asserts don’t work in these cases.

Assertions are not a standard language feature in Scala. (They are in other languages but often without the other good stuff.) Instead, there are a number of classes that provide functions for assertion handling. In the framework we are using to introduce unit testing (JUnit), a class named Assert supports assertion testing (class) methods. When we move to ScalaTest (for good), we'll find that the need for explicit assertions is significantly reduced but they can still be useful.

In our tests, we make use of assertion method, e.g. ``Assert.assert()``, to determine whether an assertion is successful. If the variable or expression passed to this method is *false*, the assertion fails.

Here are some examples of assertions, JUnit style. Readers should consult [#junit]_ for details of all supported methods. Although these are Java, we will be using them in Scala (Scala can use any Java class, a salient feature of the language).

.. |sbt-console| replace:: ``sbt test:console``

Let's take a look at how to test drive the API via |sbt-console|. The details of |sbt| are covered in the :doc:`environment` chapter.

Let's fire up the Scala test console:

.. code-block:: text

   $ sbt test:console
   [info] Set current project to SimpleTesting (in build
   file:/Users/gkt/Dropbox/Work/LUC/scala-tdd-fundamentals/)
   [info] Starting scala interpreter...
   [info]
   Welcome to Scala version 2.11.4 (Java HotSpot(TM) 64-Bit
   Server VM, Java 1.8.0_25).
   Type in expressions to have them evaluated.
   Type :help for more information.
   
Now let's import JUnit and try out the assertion methods.

.. code-block:: text

   scala> import org.junit.Assert._
   import org.junit.Assert._

   scala> org.junit.Assert.<tab>
   asInstanceOf        assertNotEquals   assertSame   isInstanceOf
   assertArrayEquals   assertNotNull     assertThat   toString
   assertEquals        assertNotSame     assertTrue
   assertFalse         assertNull        fail


Perhaps one of the most awesome things in Scala's test console is that you can *discover* the available methods by using tab completion (a long-time favorite of Linux/Unix geeks like us).

Let's try an assertion that we know would be successful.

.. code-block:: text

   scala> assertTrue(true)
   scala> assertFalse(false)

When an assertion is *successful*, you will see *no output*. On the other hand, when an assertion is *not successful* or *fails*, you see the dreaded *stack trace*.

.. code-block:: text

   scala> assertTrue(false)
   java.lang.AssertionError
   at org.junit.Assert.fail(Assert.java:86)
   at org.junit.Assert.assertTrue(Assert.java:41)
   at org.junit.Assert.assertTrue(Assert.java:52)
   ... 43 elided   

You can see more information about the exception as follows:

.. code-block:: text

   scala> lastException.printStackTrace

Let's look at some of the other assertions. assertArrayEquals() is an interesting one!

.. code-block:: text

   scala> val v1 = Array(1, 2, 3)
   v1: Array[Int] = Array(1, 2, 3)

   scala> val v2 = Array(1, 2)
   v2: Array[Int] = Array(1, 2)

   scala> assertArrayEquals(v1, v2)
   java.lang.AssertionError: array lengths differed,
   expected.length=3 actual.length=2
     at org.junit.Assert.fail(Assert.java:88)
     at org.junit.internal.ComparisonCriteria.assertArraysAreSame
     Length(ComparisonCriteria.java:71)
     at
   org.junit.internal.ComparisonCriteria.arrayEquals(Comparison
   Criteria.java:32)
     at
   org.junit.Assert.internalArrayEquals(Assert.java:473)
     at org.junit.Assert.assertArrayEquals(Assert.java:369)
     at org.junit.Assert.assertArrayEquals(Assert.java:380)
     ... 43 elided

   scala> val v3 = Array(1, 2, 3)
   v3: Array[Int] = Array(1, 2, 3)

   scala> assertArrayEquals(v1, v3)


For arrays to be considered equal, they must match in type, length, and content. For this reason, only ``v1`` and ``v3`` compare equal as arrays. 

If we try to compare two arrays of *different* types, we won't get very far. Consider:

.. code-block:: text

   scala> val v4 = Array(1.0, 2.0)
   v4: Array[Double] = Array(1.0, 2.0)

   scala> assertArrayEquals(v1, v4)
   <console>:13: error: overloaded method value assertArrayEquals
   with alternatives:
     (x$1: Array[Long],x$2: Array[Long])Unit <and>
     (x$1: Array[Int],x$2: Array[Int])Unit <and>
     (x$1: Array[Short],x$2: Array[Short])Unit <and>
     (x$1: Array[Char],x$2: Array[Char])Unit <and>
     (x$1: Array[Byte],x$2: Array[Byte])Unit <and>
     (x$1: Array[Object],x$2: Array[Object])Unit
    cannot be applied to (Array[Int], Array[Double])
                 assertArrayEquals(v1, v4)
                 ^

Remember that we're trying out Scala's assertion methods *interactively* in the REPL. The carat symbol just before ``assertArrayEquals(v1, v4)`` means that this is a *syntax error*. Why? The problem is that ``v1`` is a Scala ``Array[Int]``, while v4 is a Scala ``Array[Double]``--a problem! So this assertion neither passes nor fails. Luckily, when you write your unit tests normally, they will be within a Scala module (often within a class) and full syntactic and type checking will be performed before any test is run. The lowdown is that Scala requires your assertions to compile cleanly before they are actually executed--even within the REPL.

So we know that ``assertArrayEquals(v1, v3)`` is successful. Let's look at what happens when we use ``assertEquals(v1, v3)``. The results may surprise you!

.. code-block:: text

   scala> assertEquals(v1, v3)
   java.lang.AssertionError: expected:<[I@3533e8ba> but
   was:<[I@386e9a04>
     at org.junit.Assert.fail(Assert.java:88)
     at org.junit.Assert.failNotEquals(Assert.java:743)
     at org.junit.Assert.assertEquals(Assert.java:118)
     at org.junit.Assert.assertEquals(Assert.java:144)
     ... 43 elided

What's going on here?

Well, if you look carefully, there are two different object identifiers here. Although ``v1`` and ``v3`` have the same array values, they are by no means equal as in object equality. Further examination reveals what might be going on:

.. code-block:: text

   scala> v1.hashCode
   res13: Int = 892594362

   scala> v2.hashCode
   res14: Int = 1561484803

   scala> v3.hashCode
   res15: Int = 946772484

Aha! ``v1`` and ``v3`` refer to identical arrays, at least in terms of their content, but their hash codes don't match. This is often a sign of *not being equal*, especially in object-oriented thinking.

Luckily, a unit-testing framework allows you to avoid looking at the dreaded stack trace. It will cheerfully intercept the ``AssertionError`` in the test runner. (This chapter doesn't use the IDE so everyone will have a common basis for thinking about tests that relies only on Scala's standard toolset.)

.. todo::

   Add demonstrations of a few others here, especially those that are useful to our actual test cases.

A Basic Example: Rational Arithmetic
---------------------------------------------

We begin with a *guiding example* that has been conceived with the following objectives in mind:

- Easy to explain and (likely) familiar to most readers. It relies on mathematical ideas that are taught to us in childhood.
- Has sufficient complexity to make testing necessary.
- Plays to Scala's strengths as a language.
- Allows us to introduce all of the testing styles without getting bogged down by domain-specific details.

This example is also featured in Martin Odersky's seminal introduction to Scala, a.k.a. Scala by Example [#SBE]_. Harrington and Thiruvathukal also present a version with are more complete API and robust unit testing in their introductory CS1 course [#IntroCS]_.

While we're reasonably certain you already know what a rational number is, it is helpful to understand its requirements. Later, we shall see that these requirements can be expressed in the various testing styles--a form of documentation.

1. A rational number is expressed as a quotient of integers with a *numerator* and a *denominator*.

2. The *denominator* must not be zero.

3. The *numerator* and *denominator* are always kept in a reduced form. That is, if the *numerator* is 2 and the *denominator* is 4, you would expect the reduced form to be *numerator* = 1 and *denominator* = 2.

4. Rational numbers must be able to perform the usual rules of arithmetic, including binary operations such as +, -, \*, and / (explained below) and various *convenience* operations such as negation and reciprocal.

5. Any two rational numbers that are the same number should compare equally and be treated as the same number anywhere they might be used. For example, if you used a Scala set to keep a set of rational numbers (e.g. 1/2, 2/3, 2/4) you would expect this set to contain 1/2 and 2/3. 2/4 wouldn't be expected to appear in the set.


Implementation
~~~~~~~~~~~~~~~~~

Central to making rational numbers work is a famous algorithm, attributable to Euclid, that computes the greatest common divisor. While one of the most important algorithms, it isn’t provided as a standard Scala function. Even so, it makes for kind of an interesting testing example, because in the case of Rational numbers, it really needs to work for positive and negative values (and all possible combinations in the numerator and denominator) and do what is expected. 


.. literalinclude:: ../examples/scala-tdd-fundamentals/src/main/scala/Rational.scala
   :language: scala
   :dedent: 2
   :start-after: begin-RationalMathUtility-gcd
   :end-before: end-RationalMathUtility-gcd

We won’t rehash the details of how Euclid’s algorithm actually does what it does. This is covered extensively in online resources. The following is the complete implementation of the Rational class. We will explore this in a bit of detail to ensure the ensuing discussion of unit testing makes complete sense.

The following is the complete implementation of the Rational class. We will explore this in a bit of detail to ensure the ensuing discussion of unit testing makes complete sense.

We'll present this in pieces. Here is a general outline of how the Rational class is organized:

class Rational(n: Int, d: Int) extends Ordered[Rational] {

.. code-block:: scala

   def gcd(x: Int, y: Int) = ...

   // initialization
   private val g = gcd(n, d)
   val numerator: Int = n / g
   val denominator: Int = d / g

   // arithmetic
   def +(that: Rational) = ...
   def -(that: Rational) = ...
   def *(that: Rational) = ...
   def /(that: Rational) = ...

   // important unary operations
   def reciprocal() = ...
   def negate() = ...

   // comparisons

   def compare(that: Rational) = ...

   // objects
   override def equals(o: Any) = ...
   override def hashCode = ...

   // companion object

   object Rational ...

We think the Rational class is a great example that plays to Scala's strengths, especially when it comes to clarity, conciseness, and correctness. Let's take a look at the implementation of this class in detail.

For readers new to Scala, you can start by thinking of Scala as "Java done more concisely". The class definition basically gives us all we need to construct instances. A Rational number has a numerator and a denominator, *n* and *d*, respectively.

.. literalinclude:: ../examples/scala-tdd-fundamentals/src/main/scala/Rational.scala
   :language: scala
   :dedent: 4
   :start-after: RationalClass.Initialization
   :end-before: RationalClass

Initializing *members* is a matter of creating Scala ``val`` definitions. Our Rational implementation is intended to be *side-effect free*. We initialize the *numerator* and *denominator* by dividing out the greatest-common divisor. This has the effect of reducing the fraction 

Let's take a look at the methods that deal with basic arithmetic.


Rational number arithmetic is interesting. One of the things that is fascinating about arithmetic involving rational numbers is that the addition (and subtraction) operations are in some ways *harder* than the multiplication (and division) operations. Why? Well, for one thing, when you do addition, you need to compute the product of the first rational's numerator and the second rational's denominator. Then you add this result to the product of second rational's numerator and the first rational's denominator. Once done, you divide this result by the product of the two rationals' demoninators. Of course, having seen children actually get confused by this (including myself), suffice it to say, you're going to want to test it to believe it--even if you understand it.

So more formally, when we have :math:`\frac{a}{b} + \frac{c}{d}`, addition is achieved by doing :math:`\frac{a d + b c}{b d}` and then using the normal methods (prayer, guessing factors, or using Euclid's algorithm) to reduce the fraction to a final result.

Now let's look at it in Scala.

.. literalinclude:: ../examples/scala-tdd-fundamentals/src/main/scala/Rational.scala
   :language: scala
   :dedent: 4
   :start-after: RationalClass.Arithmetic
   :end-before: RationalClass

Scala makes this particularly beautiful, given its robust support for *operator overloading* (a concept you may have missed when moving from C++ to Java as we did). In Scala, it's just a matter of defining an operator function (+), which we are defining to work *only* with Rational on the left and right hand side in this example (again, with the intent of not driving to far from Martin Odersky's example). 

We won't go into the explanation of every operator. Subtraction (-) is essentially the same as addition (+). 

Multiplication of rational numbers is super simple by comparision. Multiply the numerators and denominators of each rational number. Reduce. Division follows similarly, where you basically are multiplying the first fraction by the reciprocal of the second fraction. Reduce. Done.

To support the ``Ordered[Rational]`` trait, we need to implement comparison. This is shown in the following:


.. literalinclude:: ../examples/scala-tdd-fundamentals/src/main/scala/Rational.scala
   :language: scala
   :dedent: 4
   :start-after: RationalClass.Comparisons
   :end-before: RationalClass

It is interesting to think about how rational numbers are properly compared. It is *essentially* the same as subtraction. The diference, however, is that we really don't need to know the denominator. Why is this? A simple example is helpful. For example, if you had 1/4 and 1/4, subtraction gives 0/16. Of course, we only need the numerator of the subtraction result to know that the two fractions compared equally. More importantly, however, it is about not doing work that doesn't need to be done.

This

.. code-block:: scala

   def compare(that: Rational) = numerator * that.denominator - that.numerator * denominator

is way more efficient than

.. code-block:: scala

   def compare(that: Rational) = (this - that).numerator

Nevertheless, because it is taking advantage of a bit of mathematical cleverness, there is great value to testing whether compare does what we expect. In our forthcoming JUnit and ScalaTest examples, it will be apparent that we took great care to test this, despite our confidence in the underlying mathematical thinking behind comparison.

Lastly, we have some methods that allow Rational instances to be used properly in Scala collections. We'll keep this simple for now by saying that we define object equality on Rational numbers as follows:

- If a Rational is compared to another non-Rational object, the objects are not equal.
- Two Rationals are equal objects if (and only if) they compare equal using the compare method we just discussed. That is, this.compare(that) == 0.


.. literalinclude:: ../examples/scala-tdd-fundamentals/src/main/scala/Rational.scala
   :language: scala
   :dedent: 4
   :start-after: RationalClass.Object
   :end-before: RationalClass
   

We also provide a definition of hashcode() by taking the numerator and denominator and placing them in a Scala tuple and then punting the actual hash code computation to the tuple. Although a simple idea, this little session in |sbt-console| shows how it works:

.. code-block:: scala

   scala> (1, 4).hashCode
   res1: Int = -1116984814
   scala> (1, 4).hashCode
   res2: Int = -1116984814

Two different tuples containing the same integers (numerator and denominator) do, indeed, result in the same hash code! Whew!

Test-driven development with JUnit and Scala
----------------------------------------------

In this section, we're going to take a look at the first of two general ways of thinking about testing: test driven development, or TDD [#TDDvsBDD]_. In test-driven development, you typically think in terms of the tests you need to write and then write the implementation code expressly with the idea of making the tests pass. (In the interests of disclosure, the author wrote the test code and the implementation code somewhat simultaneously after starting with a spartan implementation of the Rational class.) TDD is a valuable way of thinking, because for some classes, it is virtually impossible to envision the entire set of interfaces that are needed. If you look at many class libraries (e.g. collections), it is clear that there was often an organic process that led to their creation.

Java programmers nowadays write JUnit tests by creating a test class and using the @Test annotation for each intended test method. Each method is written by using the various and sundry assertion methods covered earlier in this chapter. 

As the intention of this section is to show how you can take what you already know in Java and apply it immediately to Scala, we are assuming that you have previous experience working with JUnit and Java.

Let's start by looking at the tests for our greatest common divisor method.

.. literalinclude:: ../examples/scala-tdd-fundamentals/src/test/scala/RationalJUnitTests.scala
   :language: scala
   :dedent: 2
   :start-after: RationalJUnitTests.gcd
   :end-before: RationalJUnitTests

Our GCD tests are basically all encoded in one test method, because they're all similar and very closely related. What we're primarily trying to test here is whether various combinations of positive and negative numbers result in the expected behavior. It is worth pondering, however, what exactly is *expected* when it comes to Euclid's algorithm. If you've never looked at it closely, the results may surprise (or scare) you.

Let's start with some easy examples. :math:`gcd(3, 6)` should produce 3. It's easy to see why, because 3 divides into both 3 and 6. You'd expect these two numbers, when they appear in a Rational number (fraction) to be reduced to 1 and 2, respectively.

What about when either or both of the numbers is/are negative? :math:`gcd(-3, -6)` should do what? Let's start by thinking about what we *hope* it does. Ideally, we'd like a rational with -3 numerator and -6 denominator to be *reduced* to 1/2. So we *hope* that the greatest common divisor gives us -3. (Rest assured, it does, but we need to test this. It will only help to cure a bad headache later.)

When either of the numbers is negative (but not both), we expect the greatest common divisor to be positive, though it doesn't really matter in this case, because it will only affect whether the sign is negative in the numerator or the denominator. (Cosmetically, we prefer the sign to be in the numerator when writing fractions, but it doesn't much matter to a computer as long as it works properly when doing arithmetic.

Lastly, we need to look at what happens when zero appears in a greatest common divisor calculation. The last two tests :math:`gcd(0,5)` and :math:`gcd(5,0)` both check that the GCD is 5. Is this what we expect? Yes. If I had a rational number :math:`\frac{0}{5}`, it should be reduced to :math:`\frac{0}{1}`.

So assuming all of our greatest common divisor tests work properly, it becomes much easier to think about whether the rest of our class Rational's methods are working.

.. literalinclude:: ../examples/scala-tdd-fundamentals/src/test/scala/RationalJUnitTests.scala
   :language: scala
   :dedent: 2
   :start-after: RationalJUnitTests.Initialization
   :end-before: RationalJUnitTests

Testing whether rational number arithmetic works as expected.

.. literalinclude:: ../examples/scala-tdd-fundamentals/src/test/scala/RationalJUnitTests.scala
   :language: scala
   :dedent: 2
   :start-after: RationalJUnitTests.Arithmetic
   :end-before: RationalJUnitTests

Testing whether the comparisons involving rational numbers work as expected.

.. literalinclude:: ../examples/scala-tdd-fundamentals/src/test/scala/RationalJUnitTests.scala
   :language: scala
   :dedent: 2
   :start-after: RationalJUnitTests.Comparisons
   :end-before: RationalJUnitTest

Behavior-Driven Testing using ScalaTest 
------------------------------------------

The following is an example of one of the ScalaTest styles, known as FlatSpec. FlatSpec is an example of *behavior-driven development* or BDD. You write FlatSpec tests with the idea of *describing* the *requirements* and/or expected behavior. As software engineers, behavior-driven development allows us to ensure that the stated requirements match the expected behavior. When we think in terms of BDD as opposed to TDD, we can express our tests at a much higher level, often in plain (English) language. As we consider each of the of the following groups of tests, therefore, we'll go back to what we *said* a rational number should *do*.

One thing we *said* that rational numbers should *do* is to maintain their representation in a reduced form. When combined with other rational numbers (via arithmetic), we also expect this behavior to be observed.

So let's start with looking at how this style applies to the greatest-common divisor itself.

What do we expect of the greatest common divisor algorithm? Well, our friend Euclid had something to say about this. 

- When 0 is involved, e.g. :math:`gcd(x, 0)` or :math:`gcd(0, y)`, we expect :math:`x` or :math:`y` to be the result.
- When some multiple of a reduced fraction's numerator and denominator are given, we expect that multiple to be the result. A good example is :math:`\frac{1}{3} \cdot \frac{3}{3}`. We'd expect the greatest common divisor to be this multiple.

In a technical sense, this might be more of a pure testing style than behavior driven. However, the way we have expressed this is in terms of how we expect GCD to behave.

As this is the first example of a FlatSpec style test, let's introduce a few things. To create a FlatSpec style test that uses the features we'll be showing in the remaining discussion, you need to put your tests in a Scala class:

.. code-block:: scala

   import org.scalatest._

   class RationalScalaTestFlatSpecMatchers extends FlatSpec 
     with Matchers {
      // Tests go here
   }

You then write your tests (behavioral examples):

Each test is written as follows:

.. code-block:: scala

   "Example" should "do something" in {
      // details of what it should do
   }


or

.. code-block:: scala

   it sould "do something" in {
      // details of what it should do
   }

When you write something like "Example", the idea is to indicate that we are describing the behavior of a particular aspect under testing. When we use *it*, we are referring to the same aspect but breaking out a separate case. After seeing a few examples, it will become apparent how cool this is.

Back to the actual GCD, here it is:

.. literalinclude:: ../examples/scala-tdd-fundamentals/src/test/scala/RationalScalaTestFlatSpecMatchers.scala
   :language: scala
   :dedent: 2
   :start-after: RationalFlatSpec.GCD
   :end-before: RationalFlatSpec

As you can see, the code more or less follows the descriptions already given. We basically look at the different cases as written (GCD involving 0 and GCD not involving 0).

Behavior-driven development, as mentioned earlier, tends to eschew (but not prohibit) explicit assertions. So we see this:

.. code-block::scala

   gcd(0, 5) should be (5)

instead of:

.. code-block::scala

   assert(gcd(0, 5) == 5)

We think both have value, but it is clear that one has a more *literate* style than the other.

.. literalinclude:: ../examples/scala-tdd-fundamentals/src/test/scala/RationalScalaTestFlatSpecMatchers.scala
   :language: scala
   :dedent: 2
   :start-after: RationalFlatSpec.Initializing
   :end-before: RationalFlatSpec

Armed with the knowledge the GCD is working, testing initialization is fairly straightforward. We basically want to ensure that initialization of rational instances with any combination of positive and negative denominators results in a reduced fraction.

.. todo::

   We could also ensure that whole numbers represented as fractions have a one in the denominator. We could also ensure that 0 with any denominator reduces to 0/1. Need to add this to the code. Might also be a good exercise for the reader.

One of the interesting tests is to "not allow a zero denominator". This shows easy it is to test for exceptions in Scala sans the familiar (dreaded?) try/catch syntax found in Java and other languages. The test fails if the ``ArithmeticException`` (actually, ``java.lang.ArithmeticException``) is not successfully intercepted.

The tests of Rational arithmetic are largely what we'd expect.

.. literalinclude:: ../examples/scala-tdd-fundamentals/src/test/scala/RationalScalaTestFlatSpecMatchers.scala
   :language: scala
   :dedent: 2
   :start-after: RationalFlatSpec.Arithmetic
   :end-before: RationalFlatSpec

We won't go through every single one of these as they are largely similar, but let's take a look at what is possible with the be-matching logic, thanks to our support (in Rational) for proper object equality.

.. code-block::scala

   r1 + r2 should be (new Rational(36, 64))

Clearly, ``r1 + r2`` do not result in exactly the same object as ``new Rational(36, 64)``. In Scala, as in Java, and many other object-oriented language, it is important to know that equality is not a given. It depends on having defined equality. Because we've done this in the implementation of our Rational class, we are able to use the be matcher to write the test rather concisely. (In fact, we can use it in any situation where we want to assert equality, but having proper equality really comes in handy for be-matchers in ScalaTest FlatSpec style.)

Let's look at how we test the comparison operations.

.. literalinclude:: ../examples/scala-tdd-fundamentals/src/test/scala/RationalScalaTestFlatSpecMatchers.scala
   :language: scala
   :dedent: 2
   :start-after: RationalFlatSpec.Comparisons
   :end-before: RationalFlatSpec

For testing equality, we test a number of different dimensions:

- Does == work as expected? We must always be able to compare two rational numbers, even when we are not thinking about object-oriented programming?
- Does object equality, ``equals()``, work as expected?
- Does the object hash, ``hashcode()``, work as expected?

Knowing the answer to the first is particularly important to scientific programming types (one of us being among them) who want to know whether Rational behaves largely like a built-in datatype. Knowing the answer to the second and third questions is important for using Rational in non-computational situations, e.g. within a Scala collection (more on that shortly).

This also shows another aspect of how BDD style testing goes *beyond* basic TDD testing. The ``info()`` method can be called to show how a test is breaking out different cases (the three cases above, in fact). The setup is largely the same for each, so we don't want to repeat ourselves, because all three of these questions are addressing different ways of looking at equality. Taken as a group, we want all of them to work so equality is meaningful in both a numeric and object-oriented sense.

For the rest of our tests, we expect them to work, because we defined our Rational class to extend Ordered[Rational]. Nevertheless, a characteristic of good testing is to *test the obvious*. As we did with the equality tests, the ``<=`` and ``>=`` are tested to ensure that they work for ``<`` and ``=`` (for ``<=``) and ``>`` and ``=`` (for ``>=``). 

Recalling the points we made about collections, the following shows how we test whether Rational works properly in a collection. We chose a Scala Set, because this set uses equality to determine whether or not a member should be included.

.. literalinclude:: ../examples/scala-tdd-fundamentals/src/test/scala/RationalScalaTestFlatSpecMatchers.scala
   :language: scala
   :dedent: 2
   :start-after: RationalFlatSpec.Collections
   :end-before: RationalFlatSpec

If this test works as expected, ``Rational(2, 4)`` and ``Rational(1, 2)`` (two different objects, but both of which will have the numerator and denominator by virtue of having been reduced) will result in only one entry being added to the set. The second entry will be ``Rational(-3, 6)``. So the cardinality of the resulting Set[Rationa] should be two (2). 

.. todo::

   Add pattern-matching case? I'm still a bit concerned that this is making the example too complicated for a first chapter.



Discussion
---------------

In the interests of following the tradition of Packt books and providing highly hands-on treatment from the beginning, we focused our energy in this chapter on introducing the basics of testing by introducting a fairly well-known OO example, Rational numbers, and elaborating it completely by writing tests in TDD style (using JUnit, a fairly low-level testing framework) and BDD style (using one of the ScalaTest frameworks, known as FlatSpec). While TDD will continue to have a place in your toolset, we expect that you'll find BDD the more compelling alternative, especially when you want to show your customers that you have incorporated direct testing their requirements in your code.

It is worth taking a few moments to consider what sort of things we might want to test in practice. Three rather general questions tend to guide the way we think of testing:

- Does the code work?

- Does the code work well?

- How could the code be improved?

In the general theory of testing, these three questions tend to be informed by some general ideas of software engineering, some of which can get rather theoretical.

Correctness
~~~~~~~~~~~~~~~~~~~

The first of our three questions definitely falls into this category. When we talk about correctness, we are fundamentally talking about whether the code (algorithms) are correct in a formal sense. While there is still hope that formal verification systems will eventually be able to prove any piece of code correct, testing is a *pragmatic* approach to correctness. With the work we have put into testing the Rational number class, we are confident that it is correct. We've tested every dimension that a math-inclined person would expect us to test.

But it is interesting to ponder: Have we *exhaustively* tested Rational yet? The answer is firmly in the negative. We've tested various combinations of things that should (and should not) appear in the numerator and denominator. But we haven't thrown random data both to know whether anything breaks. More importantly, we haven't done anything that appears to break performance. 

Performance
~~~~~~~~~~~~~~~~~~~

We all are authors with interests in high-performance computing and related genres (e.g. embedded systems, mobile computing, covered later in this book). One thing our tests in this chapter haven't covered is related to our second question, "Does the code run well?" For something as simple as Rational, we hope the answer is yes, but these tests do nothing to inform us of same.

Performance testing is a huge challenge in general and something beyond the scope of this book to cover in depth.  But we still need to know in many situations. If we were implementing a collection class, we'd like to know if the running time of certain methods tracks the known theoretical running time. For example, if we were inserting an item into a (mutable) set, we'd expect the performance to track the running time of the underlying structure. If this were a red/black tree, we'd hope to see time proportional to :math:`O(log(n))`. We'll look at these aspects later in the book by testing and exploring built-in collection classes, which provide a great case study for performance testing that doesn't become unwieldy or overly domain-specific.

Lifecycle Testing
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

At present, we are taking a rather limited view of testing in this first chapter. We're very much focused on unit testing and some aspects of acceptance testing. Complex software engineering includes other in-process testing, including integration testing (do all pieces of code work when put together?) to system and acceptance testing. When testing web and mobile applications, you interact with forms that accept data and interact in a black-box way with domain objects. We're going to do a bit of user-interface testing in this book, especially when it comes to Android examples, but there are presentation-level tiers that we're unlikely to cover, owing to the fact that we're not writing those tiers in Scala (e.g. a web app, which is probably using some sort of JavaScript library).

Code Coverage
~~~~~~~~~~~~~~~~

The third of our general questions about testing is to know "How could the code be improved?" For this question, we may need to look at code coverage tools. This can help us to understand whether the tests we have written really coverage all of the code in our domain. For example, do we honestly know that every single method and line of code in the Rational class has been *touched* by the tests we have written. We don't know that by looking at JUnit or FlatSpec output directly. We'll look at tools that can help us with this later in the book.

Coupling
~~~~~~~~~~~~~~~~~~

Test driven development is an important part of incremental development. Because code is continuously changing as new requirements are introduced or refined, refactoring is also an important part of incremental development. 

Two important metrics to determine how easy it is to refactor code are afferent and efferent coupling. Afferent coupling describes the number of types that refer to a class. Efferent coupling describes the number of types a class refers to. Stable classes have a high ratio of afferent coupling to efferent coupling. These types of classes are considered ‘stable’. A ratio of 10:1 or 15:1 is commonly considered stable. When refactoring, it is simpler to refactor less stable classes and more challenging to refactor more stable classes. 

When writing unit tests, be sure to pay attention to how they effect efferent coupling. In our experience we’ve seen cases where tests will raise the stability from a 20:1 ratio to over a 100:1 ratio. It is our recommendation that if unit tests are introducing excessive efferent coupling that you introduce a factory or builder pattern for your tests to make use of. This will allow you to preserve the production stability of your code and make refactoring and incremental development more productive.


.. note::

   The role of automated testing in the development process (see Fowler)
   - testing
   - refactoring
   - CI/CD

.. [#SBE] Martin Odersky, *Scala by Example*, http://www.scala-lang.org/docu/files/ScalaByExample.pdf.

.. [#IntroCS] Andrew N. Harrington and George K. Thiruvathukal, *Introduction to Computer Science with C# and Mono*, http://introcs.cs.luc.edu.

.. [#BDDSeparatingConcerns] http://stackoverflow.com/questions/15389490/bdd-in-scala-does-it-have-to-be-ugly

.. [#ATDDCucumber] http://blog.knoldus.com/2013/01/15/atdd-cucumber-and-scala/

.. [#TDDvsBDD] http://blog.andolasoft.com/2014/06/rails-things-you-must-know-about-tdd-and-bdd.html

.. [#SBT] Scala Build Tool, http://www.scala-sbt.org/

.. [#Git] Git, http://git-scm.com

.. [#GitHub] GitHub, http://github.com

.. [#Bitbucket] http://bitbucket.org

.. [#junit] http://junit.org
