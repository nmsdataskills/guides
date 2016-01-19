# Best Practices for Python Projects
**Building Tools to Last**

### About Us
North Mississippi Data Skills (NMSDS) provides accessible training in professional, open source data workflows and best practices to researchers from various backgrounds.
- Our workshop materials are available at [github.com/nmsdataskills](github.com/nmsdataskills).
- Contributions are welcome! (see workshop guidelines)

### Prerequisites
This workshop does have some prerequisites.

For those following along at home, click through the links below to see another one of our guides if you need to get up to speed.

I'm going to assume that you at least know how to do the following:
- Use [git and GitHub]() for revision control.
- Use a package manager and virtual environments to handle requirements.
  * See `pip` and `virtualenv` for the native Python solutions ([pip](https://pip.pypa.io/en/stable/), [virtualenv](https://virtualenv.readthedocs.org/en/latest/))
  * See `conda` to get both (and much more) in one package ([Documentation](http://conda.pydata.org/docs/index.html), [Cheatsheet](http://conda.pydata.org/docs/_downloads/conda-cheatsheet.pdf))

## Part 1: What belongs in a project repository?
Legible code that (preferably) does what a reader expects it does.

A well-maintained, up-to-date [readme file](), including the following:
- Maybe a style guide
- A brief guide for new contributors in your repository.
- A link to online documentation.
  * GitHub Pages (can be generated)
  * GitHub Wiki
  * [Readthedocs.org](https://readthedocs.org) (can be generated)

Finally, you should include everything you need to **automate your build process**:
- A manifest for your package manager (See prerequisites)
- Tests and test runner (Part 2 of this talk)
- Scripts for compiling documentation (Part 3)
- A master script that does the above, pushes to your remote, and handles any other loose ends when you're ready to commit. (Part 3)

### Style Requirements
- Describe how any code submitted in a pull request should look.
- Not so common with Python, but it could help maintain order if you tend to have new coders contributing.

If you're not familiar with general Python coding style, read through the following to get familiar with some of the norms of the community at large:
- Keep [PEP8](https://www.python.org/dev/peps/pep-0008/) in mind.
- [Google's style guide](https://google.github.io/styleguide/pyguide.html) is a good example.
- The [code style](http://docs.python-guide.org/en/latest/writing/style/) section of the Hitchhiker's Guide to Python

### Contributing Guide
- If you have one, "Follow our style guide."
- Explain the expected workflow for your project's revision control (e.g. "We use GitHub Flow.")
- Be explicit about expectations regarding things like test coverage ("...and we use Coveralls.io.")
- List areas where new contributors can be the most helpful!

<hr>
Okay, so we just discussed some useful information you can include in your repository.

For the rest of the talk, we'll be talking about best practices that you can apply in managing your project as it grows.

We'll come back to this question, so keep it in mind.

To kick things off, let's consider a very simple way of planning out your code.

## Comment-Driven Development
Basic idea:
- Get any boilerplate code out of the way.
- Write comments, not code, to outline the high-level structure of your document.
- Not a "best practice" so much as a quick way to get coding

Benefits:
- Requires you to think about your program or function as a whole before writing any code.
- Allows you to stub out parts of the code that you aren't sure how to implement yet.

This isn't really news - Donald Knuth came up with [literate programming](https://en.wikipedia.org/wiki/Literate_programming) quite some time ago.

### Doing nothing with the `pass` statement
- `pass` is a flow control statement that's provided as a built-in in Python.
- Allows you to exit a function without doing anything (an empty function would be illegal)

<hr>
Simple case:
```python
def do_it_later():
    pass
```
<hr>
Applying our "CDD" approach:
```python
def f(splines):
    # Prime the flux capacitor

    for spline in splines:
        # Reticulate spline
        pass
```

### Docstrings

## Recap & Break
So far, we've covered...

Next...

## Part 2: Test-Driven Development
We just talked about designing code by first specifying what the code should do in prose.

Test-driven development (TDD) is a similar practice related to automated testing,
which is another important best practice.

When writing code that anyone will depend on,
one should be sure it works as intended.

To be sure that changes to the code base don't break it,
we write extra code to immediately detect new bugs.

Ideally, this process is automated with a script (see also [test harness]()),
and all new builds must pass testing for acceptance into the main development branch.

## Unit Testing
Good software design usually involves breaking down processes into isolated parts:
- Use a function to define a process.
- A class defines a collection of data and operations (methods) that can be carried out on that data.

<hr>
Software that encapsulates its functions and methods well is easier to test.

A **unit test** will provide a function with a specific input and check that the output is appropriate.

Alternatively, the test might perform a specific manipulation on an object,
and then check that the state of the object is appropriate.

## Unit Test Example
Just use `unittest`

## Functional Testing

## Functional Test Example
Selenium, I guess

## TDD in a Nutshell
In TDD, test code is written **before** new business code is written.

Then, the business code is written and iterated on until all tests pass.

The tests, then, provide both documentation and QA.

## Further Reading
Check out [Test-Driven Development with Python](http://www.obeythetestinggoat.com/).

I've greatly enjoyed my print copy,
but the author, Harry Percival, has been kind enough to host the digital copy at the above address for free.

## Part 3: Automation
"What's so hard about `git commit -am 'Message' && git push origin branchname`?" you ask.

Nothing, really, but there's much more work to be done in any nontrivial commit:
- Run your tests
- Rebuild your documentation
- Deploy your app to production / deploy your library to your package manager

## Sphinx
- Extract docs from docstring
- Compile docs pages with [autodocs]()
- [RTD]() integration
- We can write tests in our docstrings with [Doctest](), and Sphinx will use them!

### When (not) to use Doctest
- Doctest can be great if you're writing a relatively small collection of scripts that you'd nonetheless like to share
- Don't use Doctest to exhaustively test each corner case!
  * Too much comment text makes your code a pain to read.
  * Too many texts makes the docstring more confusing than helpful.

### Doctest, Reinterpreted
> I use doctest for cases where the test is giving an example of usage that is actually useful as documentation. Generally I don't make these tests comprehensive, aiming solely for informative. I'm effectively using doctest in reverse: not to test my code is correct based on my doctest, but to check that my documentation is correct based on the code.

[Brian](http://stackoverflow.com/users/9493/brian) in this [accepted SO answer](http://stackoverflow.com/a/362394)

## Continuous Integration With Travis-CI
TODO

tl;dr: automate testing, deployment, etc. for each build

Track and report test coverage of code

