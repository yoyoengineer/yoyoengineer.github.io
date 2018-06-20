---
layout: post
title:  "Python Notes"
date:   2018-06-16 14:56:06 +0800
featured-img: python
categories: python
---

# Python Notes

## Chapter 1 Elementary Programming

1. You can display any number of items in a `print` statement using the following syntax:

   ```
   print(item1, item2, ..., itemk)
   ```

   If an item is a number, the number is automatically converted to a string for displaying. 

2. The following statement prompts the user to enter a value, and then it assigns the value to the
   variable:

   ```python
   variable = input("Enter a value: ")
   ```

   The value entered is a string. You can use the function `eval` to evaluate and convert it to a numeric value.

   ```python
   radius = eval(input("Enter a value for radius: "))
   ```

3. In some cases, the Python interpreter cannot determine the end of the statement written in multiple lines. You can place the line continuation symbol (\) at the end of a line to tell the interpreter that the statement is continued on the next line. For example, the following statement

   ```python
   sum = 1+ 2+ 3+ 4+ \
   5+ 6
   ```

   is equivalent to

   ```python
   sum = 1+ 2+ 3+ 4+ 5+ 6
   ```

4. Python also supports simultaneous assignment in syntax like this:

   ```python
   var1, var2, ..., varn = exp1, exp2, ..., expn
   ```

   It tells Python to evaluate all the expressions on the right and assign them to the corresponding variable on the left simultaneously. Swapping variable values is a common operation in programming and simultaneous assignment is very useful to perform this operation. you can simplify the task using the following statement to swap the values of x and y.

   ```python
   x, y = y, x # Swap x with y
   ```

   Simultaneous assignment can also be used to obtain multiple input in one statement.

   ```python
   # Prompt the user to enter three numbers
   number1, number2, number3 = eval(input("Enter three numbers separated by commas: "))
   # Compute average
   average = (number1 + number2 + number3) / 3
   ```

5. Python does not have a special syntax for naming constants. You can simply create a variable to denote a constant. However, to distinguish a constant from a variable, use all uppercase letters to name a constant. For example, you can use a named constant for p, as follows:

   ```python
   PI = 3.14159
   ```

6. Numeric Operators

   | Name | Meaning          | Example    | Result |
   | ---- | ---------------- | ---------- | ------ |
   | +    | Addition         | 34 + 1     | 35     |
   | -    | Subtraction      | 34.0 - 0.1 | 33.9   |
   | *    | Multiplication   | 300 * 30   | 9000   |
   | /    | Float Division   | 1 / 2      | 0.5    |
   | //   | Integer Division | 1 // 2     | 0      |
   | **   | Exponentiation   | 4 ** 0.5   | 2.0    |
   | %    | Remainder        | 20 % 3     | 2      |

7. Floating-point values can be written in scientific notation in the form of  For example, the scientific notation for 123.456 is  1.23456 * 10<sup>2</sup> and for 0.0123456 is  1.23456 * 10<sup>-2</sup> Python uses a special syntax to write scientific notation numbers. For example,  1.23456 * 10<sup>2</sup> is written as 1.23456E2or 1.23456E+2, and 1.23456 * 10<sup>-2</sup>  as 1.23456E-2. The letter E (or e) represents an exponent and can be in either lowercase or uppercase.

   The float type is used to represent numbers with a decimal point. Why are they called floating-point numbers? These numbers are stored in scientific notation in memory. When a number such as 50.534is converted into scientific notation, such as 5.0534E+1, its decimal point is moved (floated) to a new position.

8. Augmented Assignment Operators

   | Operator | Name                        | Example | Equivalent |
   | -------- | --------------------------- | ------- | ---------- |
   | +=       | Addition assignment         | i += 8  | i = i + 8  |
   | -=       | Subtraction assignment      | i -= 8  | i = i - 8  |
   | *=       | Multiplication assignment   | i *= 8  | i = i * 8  |
   | /=       | Float division assignment   | i /= 8  | i = i / 8  |
   | //=      | Integer division assignment | i //= 8 | i = i // 8 |
   | %=       | Remainder assignmen         | i %= 8  | i = i % 8  |
   | **=      | Exponent assignment         | i **= 8 | i = i ** 8 |

9. You can use the `int(value)` function to return the integer part of a float value. For example,

   ```python
   >>> value = 5.6
   >>> int(value)
   5
   >>>
   ```

   Note that the fractional part of the number is truncated, not rounded up.

   You can also use the round function to round a number to the nearest whole value. For example,

   ```python
   >>> value = 5.6
   >>> round(value)
   6
   >>>
   ```

   The functions `int` and `round` do not change the variable being converted. For example, value is not changed after invoking the function in the following code:

   ```python
   >>> value = 5.6
   >>> round(value)
   6
   >>> value
   5.6
   >>>
   ```

10. The `int` function can also be used to convert an integer string into an integer. For example, `int("34")` returns 34. So you can use the `eval` or `int` function to convert a string into an integer. Which one is better? The `int` function performs a simple conversion. It does not work for a non-integer string. For example, `int("3.4")` will cause an error. The `eval` function does more than a simple conversion. It can be used to evaluate an expression. For example, `eval("3 + 4")` returns 7. However, there is a subtle “gotcha” for using the `eval` function. The `eval` function will produce an error for a numeric string that contains leading zeros. In contrast, the `int` function works fine for this case. For example, `eval("003")` causes an error, but `int("003")` returns 3.