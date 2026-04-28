# Invariant detection

**Keywords**: invariant detection,software engineering

---

**Invariant detection** is the process of **automatically discovering properties that always hold during program execution** — identifying relationships between variables, data structures, and program states that remain true across all observed executions, providing insights into program behavior and enabling verification, testing, and debugging.

**What Is an Invariant?**

- **Invariant**: A property or condition that is always true at a specific program point.
- **Examples**:
  - `array_size >= 0` — size is never negative
  - `balance >= 0` — bank balance is non-negative
  - `left <= right` — in binary search, left pointer never exceeds right
  - `size == elements.length` — size field matches actual array length

**Why Detect Invariants?**

- **Program Understanding**: Invariants reveal implicit assumptions and constraints in code.
- **Documentation**: Automatically document program properties without manual effort.
- **Bug Detection**: Violations of invariants indicate bugs.
- **Verification**: Invariants are essential for formal verification — proving correctness requires knowing what properties should hold.
- **Test Generation**: Use invariants to generate valid test inputs and check test outputs.

**How Invariant Detection Works**

1. **Instrumentation**: Insert probes into the program to log variable values at key points (function entry/exit, loop headers, etc.).

2. **Execution**: Run the program on test inputs, collecting traces of variable values.

3. **Candidate Generation**: Generate candidate invariants — hypotheses about properties that might hold.

4. **Filtering**: Check candidates against observed traces — discard those that are violated.

5. **Reporting**: Present likely invariants to developers — those that held in all observed executions.

**Types of Invariants**

- **Unary Invariants**: Properties of single variables.
  - `x > 0` — x is always positive
  - `x != null` — x is never null
  - `x in {1, 2, 3}` — x is always one of these values

- **Binary Invariants**: Relationships between two variables.
  - `x < y` — x is always less than y
  - `x == y + 1` — x is always one more than y
  - `x == y * 2` — x is always twice y

- **Array Invariants**: Properties of arrays or sequences.
  - `arr[i] <= arr[i+1]` — array is sorted
  - `all elements are non-negative`
  - `no duplicates`

- **Object Invariants**: Properties of object state.
  - `this.size == this.elements.length`
  - `this.head != null implies this.size > 0`

- **Temporal Invariants**: Properties about execution order.
  - `open() always called before read()`
  - `lock() and unlock() are balanced`

**Daikon: The Classic Invariant Detector**

- **Daikon** is the most well-known invariant detection tool.
- **Process**:
  1. Instrument Java/C/C++ programs to log variable values.
  2. Run instrumented program on test suite.
  3. Analyze traces to find invariants.

**Example: Daikon in Action**

```java
public class BankAccount {
    private double balance;
    private int transactionCount;
    
    public void deposit(double amount) {
        balance += amount;
        transactionCount++;
    }
    
    public void withdraw(double amount) {
        if (balance >= amount) {
            balance -= amount;
            transactionCount++;
        }
    }
}

// Daikon detects invariants:
// - balance >= 0  (always non-negative)
// - transactionCount >= 0  (always non-negative)
// - transactionCount increases monotonically
// - After deposit: balance == old(balance) + amount
// - After withdraw: balance == old(balance) - amount OR balance == old(balance)
```

**Invariant Templates**

- Daikon uses **templates** to generate candidate invariants:
  - `x == a` (constant)
  - `x > a`, `x >= a`, `x < a`, `x <= a` (bounds)
  - `x == y`, `x != y` (equality)
  - `x < y`, `x <= y` (ordering)
  - `x == y + a` (linear relationship)
  - `x == y * a` (multiplicative relationship)
  - `x in {a, b, c}` (enumeration)

**Statistical Confidence**

- **Problem**: Some properties may hold by chance in observed executions but not be true invariants.
- **Solution**: Report confidence levels — how likely is this a true invariant vs. coincidence?
- **Heuristics**: Properties that hold across diverse inputs are more likely to be true invariants.

**Applications**

- **Program Comprehension**: Understand what properties the code maintains.
- **Regression Testing**: Check that invariants still hold after code changes.
- **Bug Finding**: Invariant violations indicate bugs.
  ```python
  # Detected invariant: balance >= 0
  # Test case: withdraw(1000) when balance = 500
  # Invariant violated! Bug found: insufficient funds check missing
  ```

- **Formal Verification**: Use detected invariants as loop invariants or function contracts for verification tools.
- **Test Oracle Generation**: Use invariants to check test outputs — if invariant is violated, test failed.

**LLM-Based Invariant Detection**

- **Code Analysis**: LLMs analyze code to hypothesize likely invariants without execution.
- **Trace Analysis**: LLMs analyze execution traces to identify patterns.
- **Natural Language**: LLMs express invariants in human-readable form.
- **Refinement**: LLMs refine detected invariants based on developer feedback.

**Example: LLM Detecting Invariants**

```python
# Code:
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1

# LLM-detected invariants:
"""
Precondition:
  - arr is sorted in ascending order

Loop invariants:
  - 0 <= left <= len(arr)
  - -1 <= right < len(arr)
  - left <= right + 1
  - If target is in arr, it's in arr[left:right+1]

Postcondition:
  - If target found: 0 <= return < len(arr) and arr[return] == target
  - If target not found: return == -1
"""
```

**Challenges**

- **False Positives**: Properties that hold by chance but aren't true invariants.
- **Incomplete Coverage**: Only detects invariants evident in observed executions — may miss invariants that require rare inputs.
- **Scalability**: Analyzing large programs with many variables generates many candidate invariants.
- **Noise**: Too many reported invariants can overwhelm developers.
- **Validation**: Determining which detected invariants are meaningful requires human judgment.

**Evaluation Metrics**

- **Precision**: What percentage of reported invariants are true?
- **Recall**: What percentage of actual invariants are detected?
- **Usefulness**: Do detected invariants help developers understand or verify code?

**Tools**

- **Daikon**: The classic invariant detection tool for Java, C, C++.
- **Agitator**: Commercial tool with invariant detection for Java.
- **DySy**: Dynamic symbolic execution with invariant inference.

Invariant detection is a **powerful program analysis technique** — it automatically discovers implicit properties that govern program behavior, providing valuable insights for understanding, testing, and verifying software.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/invariant-detection) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
