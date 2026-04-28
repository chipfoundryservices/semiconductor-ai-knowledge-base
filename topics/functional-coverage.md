# Coverage-Driven Verification (CDV)

**Keywords**: functional coverage,assertion,svunitest,uvm testbench,coverage driven,covergroup

---

**Coverage-Driven Verification (CDV)** is the **systematic verification using functional coverage (covergroups, coverpoints) and assertions (immediate and concurrent) — measuring verification completeness, guiding test creation, and ensuring design intent is tested — achieving >90-98% coverage on all nodes as requirement for tapeout**. CDV is modern verification best practice.

**Functional Coverage (Covergroups, Coverpoints)**
Functional coverage measures what design behavior has been exercised: (1) coverpoint — monitors specific signal or condition (e.g., if opcode ranges 0-255, coverpoint covers all 256 values), (2) covergroup — collection of coverpoints and their interactions (crosses). Example covergroup for ALU: coverpoints are {opcode, operand_a_sign, operand_b_sign, overflow}, crosses is {opcode × overflow}, measuring coverage of all opcode-overflow combinations. Coverage metric: (number of covered items) / (total items). Target: >90% for block-level, >95% for subsystem, >98% for full-chip (practical limit due to unreachable corners).

**Assertion-Based Verification**
Assertions are formal statements about design behavior: (1) immediate assertion — combinational check, evaluated immediately, (2) concurrent assertion (SVA, system verilog assertions) — temporal check over multiple cycles. Example immediate: always assert (ready == 1 || busy == 0) else $error("Invalid state"). Concurrent: assert property ( @(posedge clk) ready |=> ack ) — if ready, then ack must come next cycle. Assertions catch bugs (detected as assertion failures during simulation). Benefits: (1) early detection (failures visible during simulation), (2) self-documentation (assertions specify expected behavior), (3) automated checking (no manual verification needed).

**UVM (Universal Verification Methodology)**
UVM is an industry-standard framework for building testbenches: (1) agent — encapsulates stimulus/monitor for a protocol (e.g., AXI agent), (2) sequencer — generates sequences of transactions (legal sequences per protocol spec), (3) driver — converts transactions to physical signals, (4) monitor — observes signals, converts back to transactions, (5) scoreboard — checks transactions against expected behavior (golden model), (6) coverage collector — gathers functional coverage. UVM architecture is hierarchical and reusable: agents for different interfaces can be combined; scoreboard can be plugged in independently. UVM is a library (SystemVerilog classes, base classes providing methodology).

**Coverage Closure Methodology**
Coverage-driven verification: (1) write testbench (UVM-based with coverage), (2) run simulations (random tests, directed tests), (3) measure coverage (identify uncovered items), (4) analyze gaps (why not covered? unreachable or test not exercising?), (5) write directed tests (target uncovered items), (6) repeat until coverage target met. Directed tests target specific corner cases (corner cases often have low random-hit probability, requiring explicit tests). Example: if opcode=X never covered by random tests (rare opcode), write directed test forcing opcode=X.

**Regression Suite Management**
Verification suite is large (100s to 1000s of tests), run regularly (regression) to check for regressions (newly-introduced bugs). Regression flow: (1) source code change (logic fix, optimization), (2) run full regression suite on new code, (3) check if new failures appear (regression), (4) if failures, identify and fix. Regression is time-consuming (hours to days for full-chip regressions on 10M+ test cases). Optimization: (1) reduced smoke-test suite (subset of full, faster, catches most issues), (2) incremental regression (only run tests affected by change), (3) parallel execution (split across compute cluster). Industry trend: shift left (verification earlier in design cycle, catch bugs before high-level design complete).

**SVA (SystemVerilog Assertions)**
SystemVerilog Assertions (SVA) are a formal language for writing temporal properties: (1) properties combine: antecedent (trigger condition) and consequent (expected behavior), (2) examples: "if request, then grant within 2 cycles", "signal must not stay high for >10 cycles", (3) assertions are checked in simulation and in formal verification (property checking). SVA is more expressive than immediate assertions, enabling specification of complex temporal behaviors. Learn curve for SVA is moderate (not as complex as formal methods, but more than simple if-then).

**Scoreboards and Golden Models**
Scoreboards compare actual design outputs to expected outputs (golden model). Golden model is reference implementation (often written in high-level language like C, or behavioral Verilog). For each input, golden model computes expected output; actual design computes output; scoreboard compares. Mismatch indicates bug. Advantages: (1) testbench independent (scoreboard works with any testbench), (2) bugs in testbench logic separated from bugs in design, (3) golden model often debugged separately (lower risk of scoreboard bugs). Disadvantage: golden model takes effort (parallel implementation).

**Coverage-Driven Closing of Verification**
Late in verification, achieving last percentage points (95% → 98% coverage) is expensive (many tests, low-hit probability for remaining items). Strategies: (1) analyze uncovered items (identify if unreachable or rare), (2) if unreachable, analyze design (is feature disabled? dead code? remove from coverage goal), (3) if rare, write heavy directed tests (multiple runs targeting same item, increase probability), (4) increase testbench complexity (add constraints, scenarios making item more likely), (5) accept lower coverage (if >90% achieved and remaining uncovered, may not be worth effort, get approval from management). Final coverage: typically 95-98%, difficult to push higher.

**Formal Verification Integration**
Formal property checking (FPV) complements simulation-based verification: (1) FPV exhaustively checks properties (all inputs, all states), (2) discovers corner cases that random simulation misses, (3) provides proof of correctness for specific properties, (4) slow for large circuits (limited to blocks), (5) requires property specification (manual, effort-intensive). Verification flow often uses: simulation for comprehensive coverage (fast, broad), formal for specific critical properties (slower, deeper). Example: formal FPV on ARB (arbiter) to prove fairness and no starvation.

**Summary**
Coverage-driven verification is industry best practice, ensuring comprehensive design verification and high confidence for tapeout. Continued advances in coverage analysis, UVM refinement, and formal integration drive improved efficiency and quality.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/functional-coverage) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
