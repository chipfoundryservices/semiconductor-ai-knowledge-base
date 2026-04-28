# Specification mining

**Keywords**: specification mining,software engineering

---

**Specification mining** is the process of **automatically extracting formal specifications from code, execution traces, or documentation** — discovering implicit rules, protocols, invariants, and contracts that govern how software components should behave, without requiring manual specification writing.

**Why Specification Mining?**

- **Specifications Are Rare**: Most code lacks formal specifications — developers don't write them due to time constraints or lack of expertise.
- **Implicit Knowledge**: Specifications exist implicitly in code behavior, comments, and developer knowledge.
- **Documentation Drift**: Written specifications often become outdated as code evolves.
- **Automated Discovery**: Mining specifications from code ensures they reflect actual behavior.

**What Can Be Mined?**

- **API Usage Protocols**: Correct sequences of API calls — "open before read," "lock before access."
- **Invariants**: Properties that always hold — "balance >= 0," "size == elements.length."
- **Pre/Postconditions**: Function contracts — what must be true before/after execution.
- **Temporal Properties**: Ordering constraints — "request always followed by response."
- **Type Specifications**: Refined types — "positive integers," "non-null strings."
- **Error Handling**: Exception specifications — which functions throw which exceptions.

**Specification Mining Approaches**

- **Static Analysis**: Analyze code structure without execution.
  - **Pattern Matching**: Find common code patterns that suggest specifications.
  - **Data Flow Analysis**: Track how data flows through the program.
  - **Type Inference**: Infer more precise types than declared.

- **Dynamic Analysis**: Learn from program execution.
  - **Trace Mining**: Observe execution traces, extract patterns.
  - **Invariant Detection**: Monitor variable values, find properties that always hold.
  - **Temporal Mining**: Observe event sequences, extract ordering constraints.

- **Machine Learning**: Train models on code and execution data.
  - **Clustering**: Group similar behaviors, extract specifications for each cluster.
  - **Classification**: Learn to classify correct vs. incorrect behaviors.
  - **Sequence Learning**: Learn valid sequences of operations.

- **LLM-Based**: Use language models to extract specifications from code and documentation.

**Example: API Protocol Mining**

```java
// Observed code patterns:
File f = new File("data.txt");
f.open();
f.read();
f.close();

File g = new File("log.txt");
g.open();
g.write("...");
g.close();

// Mined specification:
// Protocol: open() must be called before read() or write()
// Protocol: close() should be called after open()
// Finite State Machine:
//   State: CLOSED -> open() -> OPEN
//   State: OPEN -> read()/write() -> OPEN
//   State: OPEN -> close() -> CLOSED
```

**Daikon: Invariant Detection**

- **Daikon** is a famous tool for mining likely invariants from execution traces.
- **Process**:
  1. Instrument program to log variable values at function entry/exit.
  2. Run program on test inputs, collect traces.
  3. Analyze traces to find properties that always hold.

```python
# Function:
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

# Daikon mines invariants:
# - arr is sorted (arr[i] <= arr[i+1] for all i)
# - 0 <= left <= len(arr)
# - -1 <= right < len(arr)
# - left <= right + 1
# - If found, return value is in [0, len(arr))
# - If not found, return value is -1
```

**Temporal Specification Mining**

- **Goal**: Discover ordering constraints on events or API calls.
- **Techniques**:
  - **Frequent Sequence Mining**: Find common sequences in execution traces.
  - **Finite State Machine Learning**: Infer FSM from observed transitions.
  - **Linear Temporal Logic (LTL)**: Mine LTL formulas describing temporal properties.

**Example: Temporal Specification**

```
// Observed traces:
lock() → access() → unlock()
lock() → access() → access() → unlock()
lock() → unlock()

// Mined temporal specification:
// - lock() must precede access()
// - unlock() must follow lock()
// - access() only allowed between lock() and unlock()
// LTL: G(access() → (lock() S true) ∧ ¬(unlock() S lock()))
```

**Applications**

- **Documentation Generation**: Automatically document API usage patterns and constraints.
- **Bug Detection**: Compare actual behavior against mined specifications — violations indicate bugs.
- **Test Generation**: Use mined specifications to generate valid test inputs.
- **Program Verification**: Use mined specifications as input to formal verification tools.
- **Code Review**: Help reviewers understand implicit contracts and protocols.
- **API Migration**: Mine specifications from old API to guide migration to new API.

**LLM-Based Specification Mining**

- **Code Analysis**: LLMs analyze code to extract implicit specifications.
- **Documentation Mining**: LLMs extract specifications from comments, documentation, and commit messages.
- **Natural Language Specs**: LLMs generate human-readable specifications from code.
- **Refinement**: LLMs refine mined specifications based on developer feedback.

**Example: LLM Mining Specifications**

```python
# Code:
def withdraw(account, amount):
    if amount <= 0:
        raise ValueError("Amount must be positive")
    if account.balance < amount:
        raise InsufficientFundsError()
    account.balance -= amount
    return account.balance

# LLM-mined specification:
"""
Preconditions:
  - amount > 0
  - account.balance >= amount

Postconditions:
  - account.balance == old(account.balance) - amount
  - return value == new account.balance

Exceptions:
  - ValueError if amount <= 0
  - InsufficientFundsError if balance < amount

Invariants:
  - account.balance >= 0 (maintained)
"""
```

**Challenges**

- **Noise**: Mined specifications may include spurious patterns that don't represent true requirements.
- **Incompleteness**: Mining only discovers specifications evident in observed behavior — may miss rare cases.
- **Overfitting**: Specifications may be too specific to the training data.
- **Validation**: Determining whether mined specifications are correct requires human judgment.
- **Scalability**: Analyzing large codebases and execution traces is computationally expensive.

**Evaluation**

- **Precision**: What percentage of mined specifications are correct?
- **Recall**: What percentage of actual specifications are discovered?
- **Usefulness**: Do mined specifications help developers understand or verify code?

**Tools**

- **Daikon**: Invariant detection from execution traces.
- **JADET**: Mines temporal specifications from Java programs.
- **Synoptic**: Infers FSMs from system logs.
- **Texada**: Mines LTL properties from execution traces.

Specification mining is a **powerful technique for recovering implicit knowledge** — it makes hidden specifications explicit, improving code understanding, documentation, and verification without requiring manual specification writing.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/specification-mining) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
