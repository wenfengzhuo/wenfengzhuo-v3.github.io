---
layout: post
comments: true
categories: design
title: Anycalc - make everything calculable
---
Anycalc is an extensible library I developed recently, which aims to let developers easily create calculators for caclulating
on customized objects rather than just numbers. To create a new calculator on customzied objects, the library requires developers
to define their own operands and operators, and the rest of things (evaluate the expressions etc.) will be taken care of by the
library.





### Usage
Let's define a new calculator that operates on strings. Before we use the library to implement the calculator, we have to define
some use cases for this brand-new calculator.

```
concatenation: given two strings, the calculator could concatenate the two strings.
remove: given two string s1, s2, the calculator could remove all s2 appeared in s1.
repeated: given a string s1 and a number n, the calculator could repeat s1 by n times.
... (we could define more)
```

**First Step**

With the library, we need to define our operand. Since *string* is supported by java, there is no need to create a new object.
All we need to do is use it as follows:

```
Operand<String> operand = new Operand<String>()
```

**Second Step**

Now we need to define our operators. With the use cases above, we could define the operators as follows:

```
"+": concatenate two strings s1, s2. The expression will be: s1 + s2 = s1s2
"-": given s1, s2, remove all s2 appeared in s1. The expression will be s1 - s2 = s3 (s3 will not contain any s2 substring)
"*": given s1, n, repeated s1 by n times. The expresssion will be s1 * n = s1s1...s1 (repeat s1 n times)
```

Let's take the *repeated* case as example, the code will looks like the follows:

```
public class StringMultiplyOperator implements Operator<String> {

  @Override
  public Operand<String> apply(List<Operand<String>> operands) {
    assert operands.size() == 2;
    String str = operands.get(0).getValue();
    int repeatedNum = Integer.parseInt(operands.get(1).getValue());
    StringBuilder sb = new StringBuilder();
    while (repeatedNum -- > 0) {
      sb.append(str);
    }
    return new Operand<>(sb.toString());
  }

  @Override
  public int priority() {
    return 2;
  }

  @Override
  public int numOfOperands() {
    return 2;
  }

  @Override
  public String getRep() {
    return "*";
  }

}
```

For any operators, it should implement the four functions that is required by the library. The four functions are:

```
- apply(): define how the operator operates on the given operands
- priority(): define the priority of the operator. This is similiar to number calculator. "*" has higher priority than "+".
- numberOfOperands(): define how many operands the operator requires. 2 means the operators is binary, so 1 means the operator is unary.
- getRep(): define the representation of each operators.
```

**Third Step**

Most of things are done now (actually, for current implementation, the library requires a *operator factory* and a customized
calculator instance. Later on, I will remove this part to make it more easier). Let's see a real example of how to use our newly
created calculators.

![string calculators](http://i.imgur.com/w5yKQPS.gif)

### How it works

The underline principles for the library is pretty easy and straightforward. There is no fancy algorithms or third-party tools that are used by Anycalc. The library adopts the popular [Reverse Polish Notation](https://en.wikipedia.org/wiki/Reverse_Polish_notation){:target="_blank"}, a common methodology used in computer science, to evaluate the expressions. In the library, the workflow could be represented by the following diagram:

![Anycalc workflow](http://i.imgur.com/WzcMS9p.png)

To make the library extensible and robust, the library leverages on some design patterns that are widely used in software development, for example, [Factory Design Pattern](https://en.wikipedia.org/wiki/Factory_method_pattern){:target="_blank"}. I made a diagram to show the relationship of different objects in the library.

![Anycalc design](http://i.imgur.com/KvXIw2q.png)

(note: Double Calculator is what we daily used calculator that works on numbers)


### Future work
There are still many things that should be improved in this library. The todo items for this library are:

```
- Dynamic scanner to automatically load new operators and operands
- Add more powerful tokenization logic for given expressions
- More unit tests to make the code robust
```

### Source code

[Anycalc](https://github.com/wenfengzhuo/anycalc){:target="_blank"}
