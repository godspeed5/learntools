Issues
------
- Globals binding statefulness (see comment in `globals_binder.py`)

Feature requests / Nice-to-haves
-----
- add some tests to learntools.core
- rework `bind_exercises`. Add overarching "Exercise" objects for containing Problems. See comments in `utils.py`.
- Helpers for exploiting a canonically correct implementation of a function for `FunctionProblem`s.
- add "abstract attribute" enforcement (see comment at top of problem.py)
- more factory syntax conveniences for problem creation
- Some kernel support for automagically running standard setup code in learn notebooks
- Some convenience for initializing an exercise module with standard boilerplate (imports at top, `bind_exercises` stuff at the bottom)
- Make ProblemViews (and Problems?) aware of their canonical `bind_exercises` name for more helpful messages. (See Hint "coda" for example)
- revive/complete scripts for autogenerating (/autotesting) exercise notebook skeleton 
- convenient syntax for per-variable custom checking
- some helper for testing notebooks to exec a problem's CodeSolution source and verify that checking passes?
- in RichText, replace angle brackets with html entities (this is a frequent problem when passing in strings that include the result of calling `type()` on something, which gives you something like `"<class 'str'>"`
- would be nice to extract the 'check whether empty function body has been touched as proxy for attemptedness' logic out of `FunctionProblem`. See embeddings course ex2 `RecommendFunction` for an example where this would be useful (a sort of function problem not amenable to basic test cases, need custom check fn).
    - similarly for extracting out logic for checking whether variable has its default value from static equality checking problems. 
        - could take out the stuff around _expected, and check_whether_attempted in `EqualityCheckProblem` and turn it into some kind of mixin
    - like, maybe some declarative way to specify attemptedness-checking logic as like, "all these variables have non-default values", "any of these variables have non-default values", "this function is non-empty", etc.
    - and/or some easy way to raise unattempted inside a custom checking fn? (I think, as the code is currently structured, the check function is somehow past the point of no return for declaring unattempted)
- Unified handling of effect of PlaceholderValues on `check_whether_attempted` outcome.
- For CodingProblem and its subclasses, I really like the idea of some declarative API around the variables of interest to be injected from the calling runtime.
    - Goals:
        - Abolish ugliness of parallel `_vars`, and `_default_values` attributes (and `_expected_values` any other per-var list attrs)
        - Plug holes in attemptedness-checking logic (see previous top-level bullet)
        - Right now any CodingProblem which isn't an `EqualityCheckProblem` or `FunctionProblem` requires custom `check()` method logic. And there are a lot of problems in this bucket. Would like to be rare for problem implementer to implement `check()`, because it's kind of tedious and error-prone.
    - For a given variable, could potentially specify...
        - Its default value (including unset)
        - Whether it's the user's responsibility to set the value appropriately in order to solve this problem vs. this is some auxiliary variable that's expected to have been created in the setup code or in a previous problem (in which case, if this var doesn't exist or has the wrong type or something, our reaction shouldn't be "incorrect", but rather "omg what did you do?")
        - Expected type (maybe in most cases the conditions described below will be a sufficient form of duck typing)
        - Conditions for correctness (specified in 'CNF' form - i.e. a list of clauses to be ANDed together, where a clause is either one of these conditions, or a disjunction of them, expressed via fancy or overloading). e.g.
            - this dataframe has a column named x
            - this df/array has shape x
            - this thing is equal to x (subsuming `EqualityCheckProblem`)
            - this thing is equal to one of {x, y, z}
            - fallback: some lambda to be applied to this thing, which, say, returns either True or (False, err msg)?
- emit warning when `tutorial_id` not set in `bind_exercises`
- `show_solution_on_correct` should be opt-in (rather than defaulting on for questions with non-CodeSolution solns)
    - related opt-in behaviour that would be useful: echo *value* on correct (for `EqualityCheckProblem`s)
