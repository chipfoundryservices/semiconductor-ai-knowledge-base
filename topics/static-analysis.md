# Static analysis

**Keywords**: static analysis,software engineering

---

**Static analysis** is the technique of **analyzing code without executing it** — examining source code, bytecode, or intermediate representations to detect bugs, security vulnerabilities, code quality issues, and verify properties, all without running the program.

**What Is Static Analysis?**

- **Static**: Analysis performed on code at rest — no execution required.
- **Automated**: Tools automatically scan code to find issues.
- **Scalable**: Can analyze large codebases quickly.
- **Early Detection**: Finds bugs during development, before code runs.

**Why Static Analysis?**

- **Find Bugs Early**: Detect issues before code reaches production — cheaper to fix.
- **No Test Cases Needed**: Unlike testing, doesn't require writing tests or generating inputs.
- **Comprehensive**: Can analyze all code paths, including rare or hard-to-test scenarios.
- **Security**: Find vulnerabilities that could be exploited — SQL injection, buffer overflows, etc.
- **Code Quality**: Enforce coding standards, detect code smells, improve maintainability.

**Types of Static Analysis**

- **Syntactic Analysis**: Check code structure and syntax.
  - Parsing, syntax checking, style enforcement.
  - Tools: linters (ESLint, Pylint, RuboCop).

- **Type Checking**: Verify type correctness.
  - Ensure variables are used consistently with their types.
  - Tools: TypeScript, MyPy, Flow.

- **Data Flow Analysis**: Track how data flows through the program.
  - Detect uninitialized variables, unused values, null pointer dereferences.
  - Tools: FindBugs, SpotBugs, Infer.

- **Control Flow Analysis**: Analyze program control flow.
  - Detect unreachable code, infinite loops, missing return statements.

- **Taint Analysis**: Track untrusted data flow.
  - Detect when user input reaches sensitive operations without sanitization.
  - Find SQL injection, XSS, command injection vulnerabilities.

- **Abstract Interpretation**: Soundly approximate program behavior.
  - Prove absence of certain bug classes.
  - Tools: Astrée, Polyspace.

**Common Bug Types Detected**

- **Null Pointer Dereferences**: Accessing null/None objects.
- **Buffer Overflows**: Writing beyond array bounds.
- **Resource Leaks**: Not closing files, connections, or freeing memory.
- **Concurrency Bugs**: Race conditions, deadlocks, data races.
- **Security Vulnerabilities**: Injection attacks, authentication bypasses, crypto misuse.
- **Logic Errors**: Unreachable code, infinite loops, incorrect conditions.
- **Code Quality Issues**: Dead code, duplicated code, overly complex functions.

**Example: Static Analysis Detecting Bugs**

```python
# Bug 1: Null pointer dereference
def process_user(user):
    return user.name.upper()  # What if user is None?

# Static analysis warning: "user may be None"

# Bug 2: Resource leak
def read_file(filename):
    f = open(filename)
    data = f.read()
    return data  # File never closed!

# Static analysis warning: "Resource leak: file not closed"

# Bug 3: SQL injection
def get_user(username):
    query = f"SELECT * FROM users WHERE name = '{username}'"
    return execute_query(query)

# Static analysis warning: "SQL injection vulnerability: unsanitized user input"
```

**Static Analysis Techniques**

- **Pattern Matching**: Look for known bug patterns.
  - Example: `if (x = 5)` instead of `if (x == 5)` — assignment in condition.

- **Type Inference**: Infer types and check consistency.
  - Example: Detect when a function expecting int receives string.

- **Symbolic Execution**: Explore paths symbolically without concrete values.
  - Example: Determine if null check is missing on a path.

- **Abstract Interpretation**: Compute abstract values representing sets of concrete values.
  - Example: Track that a variable is "positive" or "possibly null."

- **Model Checking**: Verify properties against a model of the program.
  - Example: Prove that a lock is always released.

**Static Analysis Tools**

- **General Purpose**:
  - **SonarQube**: Multi-language code quality and security analysis.
  - **Coverity**: Commercial static analyzer for C/C++, Java, C#.
  - **Fortify**: Security-focused static analysis.

- **Language-Specific**:
  - **Pylint / Flake8 (Python)**: Style and bug detection.
  - **ESLint (JavaScript)**: Linting and bug detection.
  - **RuboCop (Ruby)**: Style and bug detection.
  - **FindBugs / SpotBugs (Java)**: Bug detection.
  - **Clang Static Analyzer (C/C++)**: Bug detection.

- **Security-Focused**:
  - **Bandit (Python)**: Security issue detection.
  - **Brakeman (Ruby on Rails)**: Security vulnerability scanner.
  - **Semgrep**: Pattern-based security and bug detection.

**Soundness vs. Completeness**

- **Sound Analysis**: Never misses bugs (no false negatives) — but may report false positives.
  - Conservative: Reports potential bugs even if uncertain.
  - Example: Abstract interpretation tools.

- **Complete Analysis**: Never reports false positives — but may miss bugs (false negatives).
  - Optimistic: Only reports definite bugs.
  - Most practical tools are incomplete.

- **Trade-Off**: Sound tools have many false positives (noise). Complete tools miss bugs. Most tools balance between the two.

**Challenges**

- **False Positives**: Reporting bugs that don't exist — developers ignore warnings if too many false positives.
- **False Negatives**: Missing real bugs — no tool finds all bugs.
- **Scalability**: Analyzing large codebases can be slow.
- **Precision**: Balancing precision (few false positives) with recall (few false negatives).
- **Undecidability**: Some properties are undecidable — perfect analysis is impossible.

**LLMs and Static Analysis**

- **Bug Detection**: LLMs can identify bug patterns in code.
- **False Positive Reduction**: LLMs can help filter false positives from static analyzers.
- **Explanation**: LLMs can explain why code is flagged and how to fix it.
- **Custom Rules**: LLMs can help developers write custom analysis rules.

**Applications**

- **Continuous Integration**: Run static analysis on every commit — catch bugs early.
- **Code Review**: Automated pre-review to catch obvious issues.
- **Security Audits**: Find vulnerabilities before deployment.
- **Compliance**: Ensure code meets standards (MISRA C, CERT C, etc.).
- **Refactoring**: Identify code smells and improvement opportunities.

**Benefits**

- **Early Bug Detection**: Find bugs before testing or deployment.
- **No Execution Needed**: Analyze code that's hard to test or run.
- **Comprehensive Coverage**: Analyze all code paths, not just tested ones.
- **Automated**: Requires minimal human effort once set up.

**Limitations**

- **Cannot Find All Bugs**: Some bugs require runtime information or complex reasoning.
- **False Positives**: Can report non-issues, leading to alert fatigue.
- **Configuration**: Requires tuning to balance precision and recall.

Static analysis is a **fundamental software engineering practice** — it provides automated, scalable bug detection that complements testing and code review, improving code quality and security throughout the development lifecycle.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/static-analysis) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
