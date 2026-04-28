# Metamorphic testing

**Keywords**: metamorphic testing,software testing

---

**Metamorphic testing** is a software testing technique that **tests programs using input transformations and expected output relationships** — instead of requiring a test oracle that knows the correct output for each input, metamorphic testing checks whether related inputs produce appropriately related outputs, based on metamorphic relations.

**The Oracle Problem**

- **Traditional Testing**: Requires knowing the expected output for each input — the "oracle problem."
- **Challenge**: For many programs, determining correct output is difficult or impossible.
  - **Example**: Search engines — what is the "correct" ranking for a query?
  - **Example**: Machine learning models — what is the "correct" prediction?
  - **Example**: Scientific simulations — correct output may be unknown.

**Metamorphic Testing Solution**

- **Key Idea**: Instead of checking absolute correctness, check **relationships between inputs and outputs**.
- **Metamorphic Relation (MR)**: A property that relates multiple executions of the program.
  - If input is transformed in a certain way, output should transform in a predictable way.
  - Example: `sin(x) = -sin(-x)` — sine is an odd function.

**How Metamorphic Testing Works**

1. **Identify Metamorphic Relations**: Determine properties that should hold for the program.

2. **Generate Source Input**: Create an initial test input.

3. **Execute Program**: Run program on source input, get source output.

4. **Transform Input**: Apply transformation to create follow-up input.

5. **Execute Again**: Run program on follow-up input, get follow-up output.

6. **Check Relation**: Verify that source and follow-up outputs satisfy the metamorphic relation.

7. **Report Violation**: If relation is violated, a bug is detected.

**Example: Testing a Search Engine**

```python
# Metamorphic Relation: Adding a document containing the query
# should not decrease the number of results.

# Source test:
query = "machine learning"
results1 = search_engine.search(query)
count1 = len(results1)

# Follow-up test:
# Add a new document containing "machine learning"
search_engine.add_document("New ML paper about machine learning")
results2 = search_engine.search(query)
count2 = len(results2)

# Check metamorphic relation:
assert count2 >= count1, "Adding relevant document decreased results!"
# If this fails, bug detected!
```

**Common Metamorphic Relations**

- **Permutation**: Changing input order shouldn't affect output (for commutative operations).
  - `sort([3,1,2]) == sort([1,2,3])`

- **Addition**: Adding elements should increase or maintain output.
  - `sum([1,2,3,4]) > sum([1,2,3])`

- **Scaling**: Scaling input should scale output proportionally.
  - `f(2*x) == 2*f(x)` for linear functions

- **Symmetry**: Symmetric transformations should produce symmetric outputs.
  - `sin(-x) == -sin(x)`

- **Consistency**: Multiple paths to the same result should agree.
  - `(a + b) + c == a + (b + c)`

- **Inverse**: Applying inverse operation should return to original.
  - `decrypt(encrypt(x)) == x`

**Example: Testing a Sorting Function**

```python
def test_sort_metamorphic():
    # Source input:
    source = [5, 2, 8, 1, 9]
    source_output = sort(source)
    
    # MR1: Permutation invariance
    # Shuffling input shouldn't change sorted output
    follow_up1 = [1, 9, 2, 5, 8]  # Same elements, different order
    follow_up_output1 = sort(follow_up1)
    assert source_output == follow_up_output1
    
    # MR2: Adding element
    # Adding an element should result in sorted list containing that element
    follow_up2 = source + [3]
    follow_up_output2 = sort(follow_up2)
    assert 3 in follow_up_output2
    assert len(follow_up_output2) == len(source) + 1
    
    # MR3: Removing element
    # Removing an element should result in sorted list without that element
    follow_up3 = [x for x in source if x != 5]
    follow_up_output3 = sort(follow_up3)
    assert 5 not in follow_up_output3
```

**Applications**

- **Machine Learning**: Test ML models without knowing correct predictions.
  - MR: Slightly perturbing input shouldn't drastically change prediction.
  - MR: Adding irrelevant features shouldn't change prediction.

- **Scientific Computing**: Test simulations without knowing exact results.
  - MR: Doubling all masses in physics simulation should produce predictable changes.

- **Compilers**: Test without knowing exact assembly output.
  - MR: Optimized and unoptimized code should produce same results.

- **Search Engines**: Test without knowing ideal rankings.
  - MR: Adding relevant documents shouldn't decrease result count.

- **Image Processing**: Test filters and transformations.
  - MR: Applying filter twice should equal applying stronger filter once (for some filters).

**Metamorphic Testing with LLMs**

- **Relation Discovery**: LLMs can suggest metamorphic relations for a given program.
- **Test Generation**: LLMs generate source inputs and appropriate transformations.
- **Violation Analysis**: LLMs analyze metamorphic relation violations to identify bugs.
- **Relation Validation**: LLMs verify that proposed metamorphic relations are valid.

**Benefits**

- **No Oracle Required**: Solves the oracle problem — don't need to know correct outputs.
- **Applicable to Complex Systems**: Works for programs where correct behavior is hard to specify.
- **Finds Real Bugs**: Metamorphic relation violations indicate actual bugs.
- **Complements Traditional Testing**: Can be used alongside oracle-based testing.

**Challenges**

- **Identifying Relations**: Finding good metamorphic relations requires domain knowledge and creativity.
- **Weak Relations**: Some relations are too weak — satisfied even by buggy programs.
- **False Positives**: Some violations may be due to floating-point precision or acceptable differences.
- **Computational Cost**: Requires multiple executions per test — more expensive than single-execution tests.

**Evaluation**

- **Effectiveness**: How many bugs does metamorphic testing find?
- **Efficiency**: How many tests are needed to find bugs?
- **Relation Quality**: Are the metamorphic relations strong enough to detect bugs?

Metamorphic testing is a **powerful technique for testing programs without test oracles** — it enables testing of complex systems like machine learning models, search engines, and scientific simulations where determining correct output is difficult or impossible.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/metamorphic-testing) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
