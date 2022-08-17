---
title: generating a maths worksheet
description: describing how to write a simple maths worksheet generator
section: technology
publication_date: 2022-02-19T16:53:33
keywords: [maths, problem generator, worksheet generator]
---
## Inspiration

When you get a maths worksheet, it usually comes with the answers to the
problems from each section, so you can check your work - pretty nifty.
However, it turns out that's not the way they do it at my university.
Passing semester, I had to trudge through *a ton* of answerless
worksheets, which made studying much more time-consuming, since I had to
spend extra minute or two *per example* inputting the equation into
[Symbolab](https://www.symbolab.com/), or googling for
the solution. Let for minor annoyances, when I couldn't input some
problem into Symbolab or when it [spouted out
gibberish](https://www.symbolab.com/solver/step-by-step/%5Clim_%7Bx%5Cto0%7D%5Cleft(%5Cfrac%7Bsin%5Cleft(x%5Cright)%7D%7Bx%7D%5Cright)?or=input),
I finished the semester less than unharmed - I got inspired. Leaving the
question of whether not handing out answer-sheets was just a big
[psyop](https://www.urbandictionary.com/define.php?term=psyop)
by my instructors to to force students into creating study groups for
another time, I decided I would generate my own worksheets (without
blackjack and hookers\... sadly).

## How?

[Wolfram's Problem
Generator](https://www.wolframalpha.com/problem-generator/)
claims to be using AI to generate the problem sheets,
[mathsbot](https://mathsbot.com/generators/textbook)
probably uses some database of predefined questions. Shouldn't writing
a maths worksheet generator be complicated? or at least time consuming?
Also, don't you need someone to handout answers? With the right
software and mindset - no, no and no. If we think about it, maths
worksheets are just permutations of different symbols. We can generate
these just fine, for example with python's
[itertools](https://docs.python.org/3/library/itertools.html#itertools.product)
or
[random](https://docs.python.org/3/library/random.html)
module. Now, since a worksheet usually comes with an answers section, we
will also be needing a way of evaluating the generated expressions,
luckily there's a [whole bunch of software that dose
this](https://en.wikipedia.org/wiki/Computer_algebra_system),
for example, [sympy](https://www.sympy.org/). To
present the worksheets we can generate a
[LaTeX](https://www.latex-project.org/) document via
[jinja](https://jinja.palletsprojects.com/) and then
compile it with, for example,
[pdflatex](https://man.archlinux.org/man/pdftex.1).

## Implementation

For simplicity, we'll write a quadratics worksheet generator. First,
we'll need a function that spouts out quadratics, we would like the
generator to:

1.  differentiate quadratics on basis of the number of solutions,
    because a worksheet with, for example, only two solutions quadratics
    would be boring/incomprehensive,
2.  ensure that the quadratic's coefficients are not to crazy - in the
    end it's humans that will be solving the equations.

To achieve the first goal, we'll split the function into three. Second
one will require making some simple observations:

1.  it's more difficult to randomize fraction coefficents, so random
    coefficients will only be integers,
2.  coefficients should be bounded,
3.  it's easier to generate positive integers and then to pick a sign
4.  we know that a quadratic has two solutions, if and only if its
    [discriminant is
    positive](https://en.wikipedia.org/wiki/Discriminant#Degree_2),
    so given a quadratic of the form: *ax^2^ + bx + c ∧ a ≠ 0*, with two
    different solutions, we can write: *b^2^⁄~4a~ \> c*. Since we are
    looking for an integer upper-bound for *a* and *c*, lets set *c \>
    2*, therefore *b^2^⁄~8~ \> a*,
5.  for a quadratic to have one solution, it's discriminant must equal
    zero, so if we pick *a* and *c*, *b =
    √[4ac]{style="text-decoration: overline"}*, since this could produe
    complicated coefficients, let's require *a* and *c* to be perfect
    squares.

Figuring out bounds for no-solutions quadratics is left as an exercise
for the reader. Translating the observations into python code:

```python
    import math
    import sympy
    import random

    SIGN = [-1, 1]

    def generate_quadratic_two_solutions(max_coefficient=20):
        b = random.randint(3, max_coefficient)
        a = random.randint(1, (b**2)//8)
        c = random.randint(1, (b**2)//(4*a))
        return sympy.sympify(f'{a}*x**2 + {b}*x + {c}')

    def generate_quadratic_one_solution(max_coefficient=20):
        sign = random.choice(SIGN)
        a, c = sign * random.randint(1, max_coefficient)**2, sign * random.randint(1, max_coefficient)**2
        b = int(math.sqrt(4*a*c))
        return sympy.sympify(f'{a}*x**2 + {b}*x + {c}')

    def generate_quadratic_zero_solutions(max_coefficient=20):
        sign = random.choice(SIGN)
        a, c = sign * random.randint(1, max_coefficient), sign * random.randint(1, max_coefficient)
        b = math.sqrt(random.randint(1, math.floor(math.sqrt(4*a*c))))
        b = sympy.Rational(str(math.trunc(b * 10)/10))
        return sympy.sympify(f'{a}*x**2 + {b}*x + {c}')
```

Wait a minute, problems in a worksheet are usually ordered by
difficulty - the further the problem, the more difficult it is. I
couldn't come up with any **solid** way of evaluating a quadratic's
difficulty, based on it's coefficients, so I went with:

```python
    def quadratic_difficulty_comperator(x):
        return sum(x.as_coefficients_dict().values())
```

Now onto generating a LaTeX document. Luckily, [sympy has a built-in
LaTeX
converter](https://docs.sympy.org/latest/tutorial/printing.html#mathrm-latex),
so we'll only need to instiate
[jinja](https://jinja2docs.readthedocs.io/en/stable/api.html#basics)
and put toghether a
[template](https://raw.githubusercontent.com/jacadzaca/zadanko/master/zadanko/templates/problem_sheet.jinja.tex):

```python
    #!/usr/bin/env python3
    import math
    import random
    import collections

    import sympy
    from jinja2 import Environment, FileSystemLoader

    #...quadratic generating code ommited for breviety...#


    Task = collections.namedtuple('Task', ['instruction', 'problems', 'awnsers'])

    ENV = Environment(
        loader=FileSystemLoader('.'),
        trim_blocks=True,
        lstrip_blocks=True)

    def main():
        # generate diffrent kinds of quadratic equations
        zero_solutions_quadratics = [quadratics.generate_quadratic_zero_solutions() for _ in range(10)]
        one_solution_quadratics = [quadratics.generate_quadratic_one_solution() for _ in range(10)]
        two_solutions_quadratics = [quadratics.generate_quadratic_two_solutions() for _ in range(6)]
        # sort them according to 'difficulty' - the greater the sum of quadratic's coefficients, the more difficult it is
        zero_solutions_quadratics.sort(key=quadratics.quadratic_difficulty_comperator)
        one_solution_quadratics.sort(key=quadratics.quadratic_difficulty_comperator)
        two_solutions_quadratics.sort(key=quadratics.quadratic_difficulty_comperator)

        # arrange the problems in random order, but try to keep the problems' difficulty incremental
        problem_lists = [zero_solutions_quadratics, one_solution_quadratics, two_solutions_quadratics]
        problems = []
        for _ in range(len(zero_solutions_quadratics) + len(one_solution_quadratics) + len(two_solutions_quadratics)):
            choice = random.choice(problem_lists)
            problems.append(choice.pop())
            if not choice:
                problem_lists.remove(choice)

        # generate the awnsers with sympy https://docs.sympy.org/latest/index.html
        awnsers = (sympy.printing.latex((sympy.solvers.solveset(quadratic, domain=sympy.S.Reals))) for quadratic in problems)
        # convert into LaTeX
        problems = map(sympy.printing.latex, problems)

        #output latex code
        latex = ENV.get_template('problem_sheet.jinja.tex').render(tasks=[Task('Find the roots of function $f$, given by expression:', problems, awnsers)])
        print(latex)

    if __name__ == '__main__':
        main()
```

To compile and view the document, we can use:

```shell
    ./worksheet.py | pdflatex --jobname worksheet --output-directory /tmp && zathura /tmp/worksheet.pdf
```

## More

Slightly modifed version of this script can be found
[here](https://github.com/jacadzaca/zadanko), along
with scripts generating worksheets on
[differentiation](https://raw.githubusercontent.com/jacadzaca/zadanko/master/examples/generate_differentiation_worksheet.py),
[integration](https://raw.githubusercontent.com/jacadzaca/zadanko/master/examples/generate_integration_worksheet.py)
and
[limits](https://raw.githubusercontent.com/jacadzaca/zadanko/master/examples/generate_limit_worksheet.py).
Example worksheet generated with the script can be found
[here](https://file.enum.run/e8d4b8c0bdfa9554/example_worksheet.pdf).
If you write a simillar script on some other math topic, for exmaple
function graphing or trig equations, feel free to issue a [pull request
:)](https://github.com/jacadzaca/zadanko/pulls). I will
also gladly accept
[links](mailto:vitouejj@gmail.com?subject=Worksheet%20generator%20script%20-%20%5BINSERT%20NAME%5D)
to simillar repositories and include them in [zadanko's
README](https://github.com/jacadzaca/zadanko#readme).

