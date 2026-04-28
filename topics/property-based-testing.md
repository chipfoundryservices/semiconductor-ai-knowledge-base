# Property-based testing

**Keywords**: property-based testing,software testing

---

**Property-based testing** is a software testing approach that **tests general properties or invariants that should hold for all inputs** rather than testing specific input-output examples — automatically generating diverse test cases and checking whether the specified properties are satisfied, providing more comprehensive testing than example-based tests.

**Traditional vs. Property-Based Testing**

- **Example-Based Testing**: Write specific test cases with known inputs and expected outputs.
  ```python
  assert add(2, 3) == 5
  assert add(0, 0) == 0
  assert add(-1, 1) == 0
  ```

- **Property-Based Testing**: Specify general properties that should always hold.
  ```python
  # Property: Addition is commutative
  for all x, y: add(x, y) == add(y, x)
  
  # Property: Adding zero is identity
  for all x: add(x, 0) == x
  
  # Property: Addition is associative
  for all x, y, z: add(add(x, y), z) == add(x, add(y, z))
  ```

**How Property-Based Testing Works**

1. **Define Properties**: Specify invariants or properties that should hold for the function.

2. **Generate Inputs**: Testing framework automatically generates diverse test inputs.

3. **Execute Tests**: Run the function with generated inputs.

4. **Check Properties**: Verify that properties hold for all generated inputs.

5. **Shrinking**: If a property fails, automatically minimize the failing input to find the simplest counterexample.

6. **Report**: Present the minimal failing case to the developer.

**Example: Property-Based Testing**

```python
from hypothesis import given
import hypothesis.strategies as st

# Function to test:
def reverse_list(lst):
    return lst[::-1]

# Property 1: Reversing twice returns original
@given(st.lists(st.integers()))
def test_reverse_twice(lst):
    assert reverse_list(reverse_list(lst)) == lst

# Property 2: Length is preserved
@given(st.lists(st.integers()))
def test_reverse_length(lst):
    assert len(reverse_list(lst)) == len(lst)

# Property 3: First element becomes last
@given(st.lists(st.integers(), min_size=1))
def test_reverse_first_last(lst):
    reversed_lst = reverse_list(lst)
    assert lst[0] == reversed_lst[-1]
    assert lst[-1] == reversed_lst[0]

# Framework generates hundreds of test cases automatically:
# [], [1], [1,2,3], [-5, 0, 100], [1,1,1,1], etc.
```

**Common Properties to Test**

- **Idempotence**: Applying operation twice has same effect as once.
  - `sort(sort(x)) == sort(x)`

- **Commutativity**: Order of operands doesn't matter.
  - `add(x, y) == add(y, x)`

- **Associativity**: Grouping doesn't matter.
  - `(x + y) + z == x + (y + z)`

- **Identity**: Identity element leaves value unchanged.
  - `x + 0 == x`, `x * 1 == x`

- **Inverse**: Inverse operation cancels out.
  - `decrypt(encrypt(x)) == x`

- **Invariants**: Certain properties remain constant.
  - `len(filter(predicate, lst)) <= len(lst)`

- **Monotonicity**: Output changes predictably with input.
  - `x < y implies f(x) <= f(y)` (for monotonic functions)

**Input Generation Strategies**

- **Random Generation**: Generate random values within type constraints.
- **Edge Cases**: Automatically include boundary values — 0, -1, MAX_INT, empty lists, etc.
- **Structured Generation**: Generate complex data structures — trees, graphs, nested objects.
- **Constrained Generation**: Generate inputs satisfying specific constraints.

**Shrinking**

- **Problem**: When a property fails on a complex input, it's hard to understand why.
- **Solution**: Automatically simplify the failing input to find the minimal counterexample.

```python
# Property fails on: [42, -17, 0, 999, -3, 18, 7, -100, 55]
# After shrinking: [0]  # Minimal failing case

# This makes debugging much easier!
```

**Property-Based Testing Frameworks**

- **QuickCheck (Haskell)**: The original property-based testing framework.
- **Hypothesis (Python)**: Powerful property-based testing for Python.
- **fast-check (JavaScript)**: Property-based testing for JavaScript/TypeScript.
- **PropEr (Erlang)**: Property-based testing for Erlang.
- **ScalaCheck (Scala)**: Property-based testing for Scala.
- **FsCheck (F#/.NET)**: Property-based testing for .NET languages.

**Applications**

- **Algorithm Testing**: Verify algorithmic properties — sorting, searching, graph algorithms.
- **Data Structure Testing**: Test invariants — balanced trees, heap property, set uniqueness.
- **Parser Testing**: Verify that parsing and unparsing are inverses.
- **Serialization**: Test that serialize/deserialize round-trips correctly.
- **API Testing**: Verify API contracts and invariants.
- **Compiler Testing**: Test that optimizations preserve semantics.

**Example: Testing a Stack**

```python
from hypothesis import given
from hypothesis.stateful import RuleBasedStateMachine, rule
import hypothesis.strategies as st

class StackMachine(RuleBasedStateMachine):
    def __init__(self):
        super().__init__()
        self.stack = []
    
    @rule(value=st.integers())
    def push(self, value):
        self.stack.append(value)
    
    @rule()
    def pop(self):
        if self.stack:
            self.stack.pop()
    
    @rule()
    def check_invariants(self):
        # Property: Stack size is non-negative
        assert len(self.stack) >= 0
        
        # Property: If we push then pop, we get back to original state
        if self.stack:
            original = self.stack.copy()
            value = self.stack[-1]
            self.stack.pop()
            self.stack.append(value)
            assert self.stack == original

# Framework generates random sequences of operations
# and checks properties after each operation
```

**Benefits**

- **Comprehensive Testing**: Tests many more cases than manually written examples.
- **Finds Edge Cases**: Automatically discovers boundary conditions and corner cases.
- **Specification**: Properties serve as executable specifications.
- **Regression Prevention**: Properties continue to hold as code evolves.
- **Minimal Counterexamples**: Shrinking provides clear, simple failing cases.

**Challenges**

- **Property Discovery**: Identifying good properties requires thought and domain knowledge.
- **Performance**: Generating and testing many inputs can be slow.
- **Flaky Tests**: Random generation can lead to non-deterministic test failures.
- **Complex Properties**: Some properties are hard to express or check efficiently.

**LLMs and Property-Based Testing**

- **Property Generation**: LLMs can suggest properties to test for a given function.
- **Test Case Generation**: LLMs can generate diverse test inputs.
- **Property Validation**: LLMs can verify that proposed properties are correct.
- **Counterexample Analysis**: LLMs can explain why a property fails on a specific input.

Property-based testing is a **powerful complement to example-based testing** — it provides broader coverage, finds edge cases automatically, and serves as executable documentation of program properties, leading to more robust and reliable software.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/property-based-testing) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
