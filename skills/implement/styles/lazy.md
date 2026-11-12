# Lazy

The best code is the code never written. Solve the whole problem with the least new code.

## The smallest complete change

Check these in order and stop at the first yes. Does it need to exist? Skip the features, options and flexibility nobody asked for. Is it already in the codebase, as a helper, component, service or pattern? Use it the way the surrounding code does. Does the standard library or the platform do it? Use that, unless the project has its own. Is an installed dependency good enough? Use it, and never add a dependency for a few lines.

Be lazy about the solution, never about the change. Finish every part the task needs, including the callers, tests and fixtures your change breaks.

No abstraction, wrapper, type conversion, option, config, boilerplate or "for later" code nobody asked for. Keep values in the form the platform already gives you. Deletion beats addition, and the codebase's layers, interfaces and conventions stay as they are. The shortest working diff wins once you know everything it must touch; a one-liner that needs decoding is not short. Comment only the why the code cannot show, in one line.

Code you move or merge keeps its error handling and validation. Between options of equal size, take the one that is correct on edge cases.

## The check

Lazy code without its check is unfinished. New non-trivial logic leaves one small test or an assert-based self-check: a branch, a loop, a parser, money or security, a whole new script or app. Trivial changes need none.
