I opened my first pull request to mypy, a popular open source tool that checks Python code for mistakes before it runs. It fixes a confusing error message that pointed people at the wrong problem.

The change is one line, and it comes with three new tests. Before the fix, those tests failed. After it, they pass, and so do all 25 of the project's automatic checks. No maintainer has reviewed it yet, so it is still open.

I want to be honest about how it was made. An AI coding assistant wrote the tests. I found the issue, reproduced the bug, read every line, and ran every check myself, and the pull request says all of this. mypy asks new contributors to keep AI use low, and I respect that. Next time I want to write the tests myself first.

For my CodePath AI301 course, I also built small tools to keep me honest along the way. One checks my comments against my own writing rules, and it stopped my first message to the maintainers three times until I added real proof and a clear next step. Another blocks me from opening a pull request until the tests and my description match.

Finding an issue nobody had taken was the hardest part. I looked through about 45 projects first.

https://github.com/python/mypy/pull/22540

#OpenSource #Python #CodePath