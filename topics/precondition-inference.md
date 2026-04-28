# Precondition inference

**Keywords**: precondition inference,software engineering

---

**Precondition inference** is the process of **automatically determining the required conditions that must be true before a function executes correctly** — discovering input constraints, state requirements, and assumptions that functions depend on, without requiring manual specification writing.

**What Is a Precondition?**

- **Precondition**: A condition that must hold when a function is called for it to behave correctly.
- **Examples**:
  - `array != null` — array must not be null
  - `index >= 0 && index < array.length` — index must be valid
  - `amount > 0` — amount must be positive
  - `file.isOpen()` — file must be open before reading

**Why Infer Preconditions?**

- **Documentation**: Automatically document function requirements.
- **Bug Prevention**: Callers can check preconditions before calling — prevent crashes and errors.
- **Verification**: Preconditions are essential for formal verification.
- **Test Generation**: Generate valid test inputs that satisfy preconditions.
- **API Understanding**: Help developers understand how to correctly use functions.

**How Precondition Inference Works**

- **Static Analysis**: Analyze code to identify conditions that must hold.
  - Look for assertions, exceptions, null checks, bounds checks.
  - Trace backward from error conditions to find required preconditions.

- **Dynamic Analysis**: Observe executions to learn preconditions.
  - Run function with various inputs, observe which succeed and which fail.
  - Infer preconditions that distinguish successful from failing executions.

- **Symbolic Execution**: Explore paths symbolically to derive preconditions.
  - Compute path conditions for successful execution.
  - Negate conditions leading to errors to get preconditions.

- **Machine Learning**: Learn preconditions from examples.
  - Train models on (input, success/failure) pairs.
  - Extract decision boundaries as preconditions.

**Example: Precondition Inference**

```python
def divide(a, b):
    return a / b

# Inferred precondition: b != 0
# (Otherwise ZeroDivisionError)

def get_element(arr, index):
    return arr[index]

# Inferred preconditions:
# - arr != null
# - 0 <= index < len(arr)
# (Otherwise IndexError)

def withdraw(account, amount):
    if amount <= 0:
        raise ValueError("Amount must be positive")
    if account.balance < amount:
        raise InsufficientFundsError()
    account.balance -= amount

# Inferred preconditions:
# - amount > 0
# - account.balance >= amount
```

**Static Precondition Inference**

- **Approach**: Analyze code to find conditions that prevent errors.

```python
def process_user(user):
    # Code checks user.age
    if user.age < 18:
        return "Minor"
    else:
        return "Adult"

# Inferred precondition: user != null AND user.age is defined
# (Otherwise AttributeError)
```

- **Techniques**:
  - **Null Pointer Analysis**: Identify where null checks are needed.
  - **Bounds Analysis**: Determine valid ranges for array indices and numeric values.
  - **Exception Analysis**: Trace back from exception throws to find preventing conditions.

**Dynamic Precondition Inference**

- **Approach**: Run function with many inputs, observe successes and failures.

```python
# Function:
def sqrt(x):
    return x ** 0.5

# Test inputs:
sqrt(4) → 2.0 (success)
sqrt(0) → 0.0 (success)
sqrt(-1) → complex number or error (failure)

# Inferred precondition: x >= 0
```

- **Daikon-Style**: Collect traces of successful executions, find properties that always hold for inputs.

**Symbolic Execution for Preconditions**

- **Approach**: Symbolically execute function, collect path conditions.

```python
def abs_value(x):
    if x < 0:
        return -x
    else:
        return x

# Symbolic execution:
# Path 1: x < 0 → return -x (requires x < 0)
# Path 2: x >= 0 → return x (requires x >= 0)
# Combined precondition: true (no restriction, works for all x)

def safe_divide(a, b):
    if b == 0:
        raise ValueError()
    return a / b

# Symbolic execution:
# Path 1: b == 0 → exception
# Path 2: b != 0 → return a/b (success)
# Precondition for success: b != 0
```

**LLM-Based Precondition Inference**

- **Code Analysis**: LLMs analyze function code to identify implicit preconditions.
- **Natural Language**: LLMs express preconditions in human-readable form.
- **Documentation Mining**: LLMs extract preconditions from comments and documentation.

**Example: LLM Inferring Preconditions**

```python
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

# LLM-inferred preconditions:
"""
Preconditions:
  - arr is not null/None
  - arr is sorted in ascending order
  - target is comparable with elements of arr
  
Without these preconditions:
  - If arr is None: AttributeError
  - If arr is unsorted: incorrect result (not an error, but wrong answer)
  - If target is incomparable: TypeError
"""
```

**Applications**

- **API Documentation**: Automatically document function requirements.
- **Defensive Programming**: Insert precondition checks at function entry.
  ```python
  def withdraw(account, amount):
      assert amount > 0, "Amount must be positive"
      assert account.balance >= amount, "Insufficient funds"
      # ... rest of function
  ```

- **Contract-Based Programming**: Generate contracts for design-by-contract systems.
- **Test Input Generation**: Generate test inputs that satisfy preconditions.
- **Static Analysis**: Use preconditions to improve precision of static analyzers.

**Challenges**

- **Completeness**: May not discover all preconditions, especially complex ones.
- **Precision**: May infer preconditions that are too strong (overly restrictive) or too weak (insufficient).
- **Implicit Preconditions**: Some preconditions are implicit in the domain — hard to infer from code alone.
- **Validation**: Determining whether inferred preconditions are correct requires human judgment.

**Evaluation**

- **Soundness**: Are inferred preconditions actually required?
- **Completeness**: Are all necessary preconditions discovered?
- **Usefulness**: Do inferred preconditions help developers?

Precondition inference is a **valuable program analysis technique** — it automatically discovers function requirements, improving documentation, enabling verification, and helping developers use APIs correctly.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/precondition-inference) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
