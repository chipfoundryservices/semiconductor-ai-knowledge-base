# Postcondition inference

**Keywords**: postcondition inference,software engineering

---

**Postcondition inference** is the process of **automatically determining the guaranteed outcomes and effects of a function after it executes** — discovering what properties hold about return values, modified state, and side effects, without requiring manual specification writing.

**What Is a Postcondition?**

- **Postcondition**: A condition that is guaranteed to hold after a function executes successfully.
- **Examples**:
  - `return value >= 0` — function always returns non-negative value
  - `array is sorted` — function sorts the array
  - `balance == old(balance) - amount` — balance is reduced by amount
  - `file.isClosed()` — function closes the file

**Why Infer Postconditions?**

- **Documentation**: Automatically document function guarantees.
- **Verification**: Postconditions are essential for proving correctness.
- **Testing**: Use postconditions as test oracles — check that they hold after execution.
- **Debugging**: Postcondition violations indicate bugs.
- **API Understanding**: Help developers understand what functions do.

**How Postcondition Inference Works**

- **Static Analysis**: Analyze code to determine what properties must hold after execution.
  - Track assignments, state changes, return statements.
  - Compute relationships between inputs and outputs.

- **Dynamic Analysis**: Observe executions to learn postconditions.
  - Run function with various inputs, observe outputs and state changes.
  - Infer properties that always hold after execution.

- **Symbolic Execution**: Symbolically execute function to derive postconditions.
  - Compute symbolic expressions for outputs in terms of inputs.
  - Extract postconditions from symbolic results.

- **Machine Learning**: Learn postconditions from examples.
  - Train models on (input, output, state change) tuples.
  - Extract patterns as postconditions.

**Example: Postcondition Inference**

```python
def abs_value(x):
    if x < 0:
        return -x
    else:
        return x

# Inferred postconditions:
# - return value >= 0 (always non-negative)
# - return value == x OR return value == -x
# - return value == abs(x)

def sort_array(arr):
    arr.sort()
    return arr

# Inferred postconditions:
# - arr is sorted in ascending order
# - arr[i] <= arr[i+1] for all valid i
# - len(arr) == len(old(arr)) (length unchanged)
# - set(arr) == set(old(arr)) (same elements)
# - return value == arr (returns the sorted array)

def deposit(account, amount):
    account.balance += amount
    account.transaction_count += 1

# Inferred postconditions:
# - account.balance == old(account.balance) + amount
# - account.transaction_count == old(account.transaction_count) + 1
```

**Static Postcondition Inference**

- **Approach**: Analyze code to determine what must be true after execution.

```python
def increment(x):
    return x + 1

# Inferred postcondition: return value == x + 1

def max_of_two(a, b):
    if a > b:
        return a
    else:
        return b

# Inferred postconditions:
# - return value >= a
# - return value >= b
# - return value == a OR return value == b
# - return value == max(a, b)
```

**Dynamic Postcondition Inference (Daikon-Style)**

- **Approach**: Run function with many inputs, observe outputs, find properties that always hold.

```python
# Function:
def square(x):
    return x * x

# Observed executions:
square(0) → 0
square(1) → 1
square(2) → 4
square(3) → 9
square(-2) → 4

# Inferred postconditions:
# - return value >= 0 (always non-negative)
# - return value == x * x
# - If x >= 0: return value >= x
```

**Symbolic Postcondition Inference**

- **Approach**: Symbolically execute function, derive symbolic expressions for outputs.

```python
def compute(x, y):
    z = x + y
    w = z * 2
    return w

# Symbolic execution:
# z = x + y
# w = (x + y) * 2
# return = (x + y) * 2

# Inferred postcondition: return value == (x + y) * 2
```

**LLM-Based Postcondition Inference**

- **Code Analysis**: LLMs analyze function code to identify guaranteed outcomes.
- **Natural Language**: LLMs express postconditions in human-readable form.
- **Documentation Mining**: LLMs extract postconditions from comments and documentation.

**Example: LLM Inferring Postconditions**

```python
def withdraw(account, amount):
    if amount <= 0:
        raise ValueError("Amount must be positive")
    if account.balance < amount:
        raise InsufficientFundsError()
    account.balance -= amount
    return account.balance

# LLM-inferred postconditions:
"""
Postconditions (if function succeeds):
  - account.balance == old(account.balance) - amount
  - return value == new account.balance
  - account.balance >= 0 (invariant maintained)
  
Exceptions:
  - ValueError if amount <= 0
  - InsufficientFundsError if old(account.balance) < amount
  
Note: Function only succeeds if preconditions are met:
  - amount > 0
  - account.balance >= amount
"""
```

**Relational Postconditions**

- **Relate outputs to inputs**: Express how outputs depend on inputs.
  - `return == input + 1`
  - `output_array == sorted(input_array)`
  - `new_balance == old_balance - amount`

- **Relate multiple outputs**: Express relationships between different outputs or state changes.
  - `return_value == modified_array[0]`
  - `size_field == array.length`

**Applications**

- **Test Oracle Generation**: Use postconditions to check test outputs.
  ```python
  result = sort_array([3, 1, 2])
  assert is_sorted(result)  # Check postcondition
  assert len(result) == 3  # Check postcondition
  ```

- **Formal Verification**: Use postconditions in verification tools to prove correctness.
- **Documentation**: Automatically document function guarantees.
- **Regression Testing**: Check that postconditions still hold after code changes.
- **Debugging**: Postcondition violations indicate bugs.

**Challenges**

- **Completeness**: May not discover all postconditions, especially complex ones.
- **Precision**: May infer postconditions that are too weak (don't capture all guarantees) or too strong (claim more than actually guaranteed).
- **Side Effects**: Tracking all side effects (file I/O, network, global state) is difficult.
- **Validation**: Determining whether inferred postconditions are correct requires human judgment.

**Evaluation**

- **Soundness**: Are inferred postconditions actually guaranteed?
- **Completeness**: Are all important guarantees discovered?
- **Usefulness**: Do inferred postconditions help developers?

Postcondition inference is a **powerful program analysis technique** — it automatically discovers function guarantees, improving documentation, enabling verification, and providing test oracles for validating correctness.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/postcondition-inference) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
