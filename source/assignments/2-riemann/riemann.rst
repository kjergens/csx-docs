.. highlight:: java

Riemann Sum Assignment
======================

.. figure:: fig1.svg
   :width: 25 %
   :align: center

Approximating Area Under a Curve
--------------------------------

The focus of this assignment will be defining and calculating
the area under a curve. The following slides, created by Dr. Gomprecht,
contain an introduction to the concept of **Riemann sums**, which
provide a way of approximating this area:

:download:`Area Under a Curve Slides </_static/RiemannSumSlides.pdf>`

We have seen that as the width of the rectangles in a Riemann sum gets smaller
the approximation to the area under the curve improves. In some cases, we’ve
been able to use this fact to compute the exact value of the area under the
curve. In many cases, though, the sum is too complicated to be computed by
hand.

This is where computers can be of help. Calculating a Riemann sum requires
adding up many small areas to get an approximation of the total area under the
curve. Computers are good at this kind of repetitive task: while there are many
steps, the calculation involved in each step is simple. In Compsci 1 and 2,
you learned how to tell a computer to do the same thing over and over using
``for`` and ``while``-loops. Now, you will apply this knowledge to create a
Java class which evaluates a given Riemann sum.

.. figure:: fig2.svg
   :width: 80 %
   :align: center

   The individual rectangles' areas can be added up using a ``for``-loop.
   The more iterations (steps) of the loop, the better the approximation.

.. admonition:: Optional Exercise

   The syntax of ``for``-loops in Java can be hard to remember.

   * Use a ``for``-loop to print the first 100 positive integers.
   * Use a ``for``-loop to add up the first 100 positive integers.
   * Use a ``for``-loop and an array to find the mean of the following ten numbers:
     ``28.2, 14.7, 10.3, -2.0, 55.8, 10.3, 0.2, 1.0, 0.0, 25.1``

Classes and Methods
-------------------

You will create several classes for this assignment: a base class called
``AbstractRiemann`` and then child classes for each of the Riemann sum rules.

The AbstractRiemann Class
^^^^^^^^^^^^^^^^^^^^^^^^^

The first class which you will create for this assignment, ``AbstractRiemann``, will
contain the majority of your code for calculating Riemann sums. Start by
`opening up the documentation
<https://kjergens.github.io/csxdocs-build/_static/riemann-javadoc/riemannsum/Riemann.html>`_ for ``AbstractRiemann``. The
linked page, known as a **JavaDoc**, has information about each of the methods
of the ``AbstractRiemann`` class. This includes the methods' **parameters** (inputs)
and their **return values** (outputs). Your job will be to create a class
which conforms to the given JavaDoc---the ``AbstractRiemann`` class which you create
should contain each of the listed methods, and each method should behave as
described in the JavaDoc, taking in the same parameters and outputting the
same type of return value.


.. note::
   Java programmers frequently use **JavaDocs** to document their code so that
   other people can understand what it does. JavaDocs are created by
   writing comments in your source code using a specific format; you can find a good introduction to documenting your code in this way at https://alvinalexander.com/java/edu/pj/pj010014.

Abstract Classes and Methods
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ``AbstractRiemann`` class, as shown in the JavaDoc, contains a keyword which you most likely have not yet encountered: ``abstract``. This keyword will allow you to use to use **object-oriented programming (OOP)** to organize your code in a more logical way.

You have learned that there are several different rules which can be used to
calculate Riemann sums, such as the left hand rule, right hand rule, and
trapezoid rule. Thinking of a Riemann sum as the sum of many small slices of
the total area, these rules correspond to different ways of defining the
slices. However, the overall method for calculating a Riemann sum remains the
same; given the endpoints of the interval on which to calculate the sum and
the number of slices, the calculation can always be divided into the following
steps:

#. Calculate :math:`\Delta x` (the width of each subinterval) from the
   endpoints of the interval and the number of slices.
#. Determine the endpoints of each subinterval.
#. Calculate the area of each slice.
#. Add up the areas to find the total area.

Notice that only the third step---calculating the area of each slice---depends upon the specific rule being used; the others are the same regardless of the rule.

.. figure:: fig3.svg
   :width: 95 %
   :align: center

   Here, three different rules are being used to calculate the same Riemann sum. While the slices' shapes are different, they exist over the same subintervals in each diagram.

Fourtunately, Java provides a convenient means of structuring classes which
are mostly the same but differ with respect to certain functions:
**inheritance**. You will discuss this concept in class, and the following
pages are recommended for reference:

* `Oracle - Inheritance Tutorial <https://docs.oracle.com/javase/tutorial/java/IandI/subclasses.html>`_
* `Oracle - Abstract Tutorial <https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html>`_

As shown in the `JavaDoc <https://kjergens.github.io/csxdocs-build/_static/riemann-javadoc/riemannsum/Riemann.html>`_, the ``AbstractRiemann`` class which you will create will be an **abstract class**. As such, you will never directly construct a ``new AbstractRiemann()``; instead, you will create **child classes** (also known as **subclasses**) of ``AbstractRiemann`` for each Riemann sum rule. In this way, you will end up with a structure where ``RightHandRule`` and ``LeftHandRule``, both child classes of ``AbstractRiemann``, share most methods, differing only in their implementations of ``slice()`` and ``slicePlot()``, since these are the only methods whose functionality should depend on the rule. For example, this is what a fictional rule called ``OvalRule`` could look like::

    public class OvalRule extends AbstractRiemann {
        @Override
        public double slice(Polynomial poly, double sleft, double sright) {
            // return the area of an ellipse whose width is (sright - sleft)
            // and whose height is the polynomial evaluated at sleft
        }

        @Override
        public void slicePlot(PlotFrame pframe, Polynomial poly, double sleft, double sright) {
            // draw an ellipse whose width is (sright - sleft)
            // and whose height is the polynomial evaluated at sleft
        }
    }

As shown in ``OvalRule``, you do not have to reimplement all of the
methods in ``AbstractRiemann``. Only the abstract methods should be
written out in the subclasses.

.. figure:: riemann-diagram.png
   :width: 85 %
   :align: center

   This class diagram shows the relationship between ``AbstractRiemann`` and
   its child classes.

Assignment
-----------

Remember to **document as you go.** Each method you write should
have a documentation comment (ideally in the JavaDoc format)
before it::

    /**
     * [DESCRIPTION OF WHAT THE METHOD DOES]
     *
     * @param left [DESCRIPTION OF THE 'left' PARAMETER]
     * @param right [DESCRIPTION OF THE 'right' PARAMETER]
     * @param subintervals [DESCRIPION OF THE 'subintervals' PARAMETER]
     * @return [DESCRIPTION OF WHAT THE METHOD RETURNS]
     */
    public double calculateDeltaX(double left, double right, int subintervals) {
        // the actual method
    }


Base Assignment
----------------

You will write a total of **eight** Java classes for the base assignment. Together, they will demonstrate three Riemann variations: Righthand Rule, Lefthand Rule, and Trapezoid Rule.

1. Create a Package Namespace
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
A good way to organize all the projects you will do this year is by creating a separate package namespace for each.

.. admonition:: Exercise

  **Summary**: Create a package namespace to hold your project.

  #. In src, right-click to get the option menu.
  #. Select New...Package
  #. Name it ``com.[yourname]`` (E.g. if your name is Kim Cheng, name it ``com.kimcheng``)
  #. Inside your the package you just created (the folder named ``com.[yourname]``) create another package called ``riemann``.

  When your folder looks like the following you are done with this exercise:

  .. figure:: packagenamespace.png
   :align: center

2. AbstractRiemann Class
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. admonition:: Exercise

  **Summary**: Create an abstract class that has logic common to all Riemann rules.

  #. In ``riemann`` create the ``AbstractRiemann`` abstract class based on the `JavaDoc <https://kjergens.github.io/csxdocs-build/_static/riemann-javadoc/riemannsum/Riemann.html>`_ .
  #. Write ``calculateDeltaX()``.
  #. Add the abstract methods ``slice()`` and ``slicePlot()``. Make sure to mark them as ``abstract`` and end the line with a semicolon instead of implementing the method.
  #. Write ``rs()``.
  #. Write ``rsPlot()``.
  #. Write ``rsAcc()`` (see :download:`Area Under a Curve Slides </_static/RiemannSumSlides.pdf>` for an explanation of the accumulation function).


3. RightHandRule, LeftHandRule and TrapezoidRule Classes
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. admonition:: Exercise

  **Summary**: Create specific Object classes for various Riemann rules.

  #. Also in the ``riemann`` package, create three child classes: ``RightHandRule``, ``LeftHandRule``, and ``TrapezoidRule``, each extending ``AbstractRiemann`` class.
  #. Each rule should implement the abstract methods ``slice()`` and ``slicePlot()``. Do not include implementations of any other methods from ``AbstractRiemann`` in these classes; they will be automatically inherited.
  #. For ``slicePlot()`` , make sure the plots correspond to the specific rules. You don't need to fill in the trapezoids for ``TrapezoidRule``.


4. Test Classes
^^^^^^^^^^^^^^^^
.. admonition:: Exercise

  **Summary**: Test the Riemann ``slice()`` methods.

  #. In the ``test`` folder, create a class called ``RightHandRuleTest`` and add a test method called ``slice`` that does the following:

    - Creates a Polynomial (first import ``org.dalton.polyfun.Polynomial``). E.g.,

    .. code-block:: java

      Polynomial poly = new Polynomial(new double[]{3, 4, 2});

    - Creates a ``RightHandRule`` object. E.g.,

    .. code-block:: java

      RightHandRule rightHandRule = new RightHandRule();

    - Asserts that the object's ``slice()`` returns the correct area of the rectangle under the given Polynomial between two ``x`` values. You can check what the Riemann sum should be using a `Riemann Sum Calculator <https://www.emathhelp.net/calculators/calculus-2/riemann-sum-calculator/>`_.

  You may add more test methods as you see fit. When you are certain ``RightHandRule``'s ``slice()`` method works, test the other rules: 

  #. In the ``test`` folder, create a class called ``LeftHandRuleTest`` that contains at least one method to test ``LeftHandRule``'s ``slice()``.
  #. In the ``test`` folder, create a class called ``TrapezoidRuleTest`` that contains at least one method to test ``TrapezoidRule``'s ``slice()``.

  When all the test methods pass you are done with this exercise.


5. RiemannApp
^^^^^^^^^^^^^^^
.. admonition:: Exercise

  **Summary**: Plot the Riemann rules.

  #. Back in the ``riemann`` package, create ``RiemannApp``, which will have a ``main`` method and be responsible for plotting example Polynomials, Riemann rectangles, and printing the estimated area. 
  #. Create an example Polynomial to find the area under. E.g., 3x^2-6x+3.
  #. Create one PlotFrame for each rule. E.g.,

    .. code-block:: java

      PlotFrame rightHandPlot = new PlotFrame(...);

  #. Create a ``RightHandRule`` object, a ``LeftHandRule`` object, and a ``TrapezoidRule`` object. E.g.,

    .. code-block:: java

      RightHandRule rightHandRule = new RightHandRule();

  #. For each rule object use ``rsPlot()`` to plot rectangles under the example Polynomial onto the cooresponding PlotFrame. E.g.,

    .. code-block:: java
    
      rightHandRule.rsPlot(rightHandPlot, polynomial, 1, 2, 10);

  #. Also on each PlotFrame, plot the example Polynomial so you can see the line in relation to the rectangles.
  #. Finally, for each rule, print the estimated area under the curve.

  When your RiemannApp prints estimated areas and launches three PlotFrames similar to the following, you are done with this exercise.

  .. figure:: areas.png
   :width: 80 %
   :align: center

  .. figure:: rightRule.png
   :width: 50 %
   :align: center

  .. figure:: leftRule.png
   :width: 50 %
   :align: center

  .. figure:: trapRule.png
   :width: 50 %
   :align: center


6. Analysis
^^^^^^^^^^^^^

Use your program to answer the following question: **which of the three rules is the most accurate?** This should compare the results of the Riemann sums with the actual area under the curve (use this `Integral Calculator <https://www.integral-calculator.com>`__ to get the actual value).

  .. warning:: Remember to account for the following edge cases:

     * The value of the polynomial for a given :math:`x` is negative.
     * The left endpoint is greater than the right endpoint.

Extension
----------

The three Riemann sum rules which you have seen so far (the right hand rule,
left hand rule, and trapezoid rule) tend to yield good approximations of the
area under a curve provided that :math:`\Delta x` is small enough. However,
they are not the only rules.

For your extension, research different Riemann sum rules and write classes for
them in the same style as the base assignment. Below are some suggested
extensions that students have done in the past:

* **Maximum rule** - Use the polynomial's value at the left endpoint or at the
  right endpoint, whichever is greater.
* **Minimum rule** - Use the polynomial's value at the left endpoint or at the
  right endpoint, whichever is lesser.
* **Random rule** - Randomly choose :math:`x` within the subinterval at which
  to evaluate the polynomial.
* **Midpoint rule** - Evaluate the polynomial at the mean of the endpoints.
* **Simpson's rule** - This is more involved than the other options but
  is also the most interesting---and often gives better approximations. It
  will take some outside research.

There is also the option to create a command-line **user interface** which
makes it easier to learn from your program. Even if you decide not to dedicate
a lot of time to making an interface, you should at least have some way for a
user to run your program with desired parameters without having to directly
edit the code first.

Advanced Extensions
--------------------

The following possible (optional) extensions are more advanced, either from a
mathematics or a computer science perspective.

**Calculate an approximation of pi**. Hint: use the equation for a
circle in cartesian coordinates to calculate the area under a semicircle.


**Write a class which approximates arc length**: if, when graphed, a function
produces a curve, then calculate the length of that curve in a given
subinterval Hint: instead of breaking up an area into rectangles, break up the
curve into line segments. You will need the distance formula :math:`r =
\sqrt{\Delta x ^ 2 + \Delta y ^ 2}` and the Java function ``Math.sqrt()`` to
calculate the length of each segment.

.. figure:: fig4.svg
   :width: 60 %
   :align: center

   Arc length can be approximated by dividing the curve and replacing the
   smaller arcs with segments.

**Write a version of** ``AbstractRiemann`` **called** ``AbstractRiemannExtended``
**which supports arbitrary non-polynomial
functions**. So far, we have only worked with polynomials. However, it is
possible to calculate the area under other functions as well---calculating
the area of a semicircle is an example of this. The hardest part will be
representing arbitrary real-valued functions as Java objects:

* One option is to write an abstract class called ``Function`` with a
  single abstract method called ``evaluate()`` which takes a ``double`` and
  returns a ``double``. Subclasses of ``Function`` will contain
  implementations of ``evaluate()`` which calculate the value of the function
  for a given :math:`x`. Replace ``Polynomial`` with ``Function`` throughout
  ``AbstractRiemannExtended`` and its subclasses.

  .. figure:: function-diagram.png
     :width: 95%
     :align: center

* A cleaner but more advanced way of representing functions is to use
  Java 8 **lambda expressions** and ``DoubleUnaryOperator``.
  Replace ``Polynomial`` with ``DoubleUnaryOperator`` throughout
  ``AbstractRiemannExtended`` and its subclasses.
  This is an example of how those features could be used::

    // f(x) = sin(x) + cos(x) / 2
    DoubleUnaryOperator f = (x) -> Math.sin(x) + Math.cos(x) / 2;

    // Print f(4.9)
    System.out.println(f.applyAsDouble(4.9));

    // g(x) = poly.evaluateToNumber(x)
    DoubleUnaryOperator g = (x) -> poly.evaluateToNumber(x);

    // Print g(-2)
    System.out.println(g.applyAsDouble(-2));


Further Resources
-----------------

Java/Computer Science
^^^^^^^^^^^^^^^^^^^^^

* `Oracle - Inheritance <https://docs.oracle.com/javase/tutorial/java/IandI/subclasses.html>`_
* `Oracle - Abstract Classes and Methods <https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html>`_
* `Oracle - Interfaces <https://docs.oracle.com/javase/tutorial/java/IandI/createinterface.html>`_
* `freeCodeCamp.org - Lambda Expressions <https://www.freecodecamp.org/news/learn-these-4-things-and-working-with-lambda-expressions-b0ab36e0fffc/>`_
* `JavaDoc <https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/lang/Math.html>`_
  for the ``Math`` class - contains useful mathematical functions such as
  ``Math.sin()`` and ``Math.sqrt()``.

Math
^^^^

* `Wolfram Alpha <https://www.wolframalpha.com/>`__ - Can be used to calculate exact values of
  Riemann sums including arc lengths.
