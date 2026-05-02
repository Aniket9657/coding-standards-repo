# coding-standards-repo
I took this from nasa guidelines for myself , I find this very cool
The 10 Coding Rules for Safety-Critical Systems
Restrict Control Flow (4:52): Avoid goto, setjump, longjump, and all forms of recursion to ensure an acyclic call graph that is easier to verify.
Fixed Upper Bound Loops (9:19): Every loop must have a statically provable upper bound to prevent infinite or runaway execution.
No Dynamic Memory Allocation (10:37): Avoid malloc and similar functions after initialization to eliminate unpredictable memory behavior and potential leaks.
Function Length Limits (14:06): Functions should be limited to about 60 lines, fitting on a single sheet of paper to ensure clarity and logical unit structure.
Assertion Density (15:49): Use at least two assertions per function to catch anomalous conditions early.
Scope Limitation (19:28): Declare data objects at the smallest possible scope to improve fault diagnosis.
Return Value Checking (21:10): Force explicit handling or explicit ignoring (via casting to void) of return values from non-void functions.
Limited Preprocessor Use (24:29): Strictly limit the preprocessor to header inclusion and simple macros, avoiding complex token pasting or conditional compilation.
Restricted Pointers (29:53): Limit to one level of pointer dereferencing and forbid function pointers to keep data flow predictable.
Mandatory Compilation and Analysis (30:13): Code must be compiled daily with strict warnings enabled and must pass daily checks by static analysis tools with zero warnings.
Key Takeaways
Automation Over Opinion: The video highlights how these rules favor tools that can mechanically verify code rather than human-subjective style guides.
Practicality: While initially appearing "Draconian," these rules are compared to wearing seatbelts—uncomfortable at first, but essential for safety in critical systems where a failure could lead to catastrophic results.
Code Quality: The Primeagen expresses great appreciation for the coherence and pragmatism of the document, noting that while he doesn't personally follow every rule, the philosophy of rigorous, analyzable code is a masterclass in engineering
