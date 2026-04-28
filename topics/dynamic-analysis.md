# Dynamic analysis

**Keywords**: dynamic analysis,software engineering

---

**Dynamic analysis** is the technique of **analyzing program behavior during execution** — monitoring running programs to detect bugs, performance issues, security vulnerabilities, and verify properties by observing actual runtime behavior with concrete inputs.

**What Is Dynamic Analysis?**

- **Dynamic**: Analysis performed while the program runs — requires execution.
- **Runtime Monitoring**: Instruments code to collect data during execution.
- **Concrete Inputs**: Uses actual test inputs, not symbolic or abstract values.
- **Behavioral**: Observes what the program actually does, not what it might do.

**Why Dynamic Analysis?**

- **Precise**: Observes actual behavior — no false positives from conservative approximations.
- **Runtime Issues**: Detects bugs that only manifest during execution — memory leaks, race conditions, performance bottlenecks.
- **Real Inputs**: Tests with actual data — finds bugs that static analysis might miss.
- **Validation**: Confirms that code behaves correctly on specific inputs.

**Types of Dynamic Analysis**

- **Memory Analysis**: Detect memory errors.
  - Buffer overflows, use-after-free, memory leaks, uninitialized memory.
  - Tools: Valgrind, AddressSanitizer, MemorySanitizer.

- **Concurrency Analysis**: Detect threading bugs.
  - Race conditions, deadlocks, data races, atomicity violations.
  - Tools: ThreadSanitizer, Helgrind, Intel Inspector.

- **Performance Profiling**: Identify performance bottlenecks.
  - CPU hotspots, memory usage, cache misses, I/O wait.
  - Tools: gprof, perf, VTune, Chrome DevTools.

- **Coverage Analysis**: Measure code coverage.
  - Which statements, branches, paths are executed.
  - Tools: gcov, coverage.py, Istanbul.

- **Taint Tracking**: Track untrusted data flow at runtime.
  - Detect when user input reaches sensitive operations.
  - Tools: TaintDroid, libdft.

- **Invariant Detection**: Discover properties that hold during execution.
  - Tools: Daikon.

**Common Bug Types Detected**

- **Memory Errors**: Buffer overflows, use-after-free, double-free, memory leaks.
- **Concurrency Bugs**: Data races, deadlocks, race conditions.
- **Undefined Behavior**: Integer overflow, null pointer dereference, division by zero.
- **Resource Leaks**: Unclosed files, sockets, database connections.
- **Performance Issues**: Slow functions, excessive memory allocation, cache thrashing.
- **Security Vulnerabilities**: Exploitable memory corruption, information leaks.

**Example: Dynamic Analysis Detecting Bugs**

```c
// Bug 1: Buffer overflow
char buffer[10];
strcpy(buffer, "This is a very long string");  // Overflow!

// AddressSanitizer detects at runtime:
// "heap-buffer-overflow: write of size 27 at address 0x..."

// Bug 2: Use-after-free
int *ptr = malloc(sizeof(int));
free(ptr);
*ptr = 42;  // Use after free!

// AddressSanitizer detects:
// "heap-use-after-free: write of size 4 at address 0x..."

// Bug 3: Data race
int counter = 0;
// Thread 1: counter++;
// Thread 2: counter++;
// No synchronization!

// ThreadSanitizer detects:
// "WARNING: ThreadSanitizer: data race on counter"
```

**Dynamic Analysis Techniques**

- **Instrumentation**: Insert monitoring code into the program.
  - **Source Instrumentation**: Modify source code before compilation.
  - **Binary Instrumentation**: Modify compiled binary.
  - **Compiler Instrumentation**: Compiler inserts checks (sanitizers).

- **Shadow Memory**: Maintain metadata about memory.
  - Track allocation status, initialization, access permissions.
  - Example: AddressSanitizer uses shadow memory to detect out-of-bounds access.

- **Happens-Before Analysis**: Track synchronization to detect races.
  - Build happens-before graph from lock/unlock, signal/wait operations.
  - Detect conflicting accesses not ordered by happens-before.

- **Watchpoints**: Monitor specific memory locations or variables.
  - Trigger when watched location is accessed.
  - Useful for debugging and root cause analysis.

**Sanitizers (Compiler-Based Dynamic Analysis)**

- **AddressSanitizer (ASan)**: Detects memory errors.
  - Buffer overflows, use-after-free, use-after-return, memory leaks.
  - ~2x slowdown, widely used in development and CI.

- **MemorySanitizer (MSan)**: Detects uninitialized memory reads.
  - Tracks initialization status of every byte.

- **ThreadSanitizer (TSan)**: Detects data races.
  - Monitors memory accesses and synchronization.
  - ~5-15x slowdown.

- **UndefinedBehaviorSanitizer (UBSan)**: Detects undefined behavior.
  - Integer overflow, null pointer dereference, misaligned access.

**Example: Using Sanitizers**

```bash
# Compile with AddressSanitizer:
gcc -fsanitize=address -g program.c -o program

# Run program:
./program

# If bug exists, ASan reports:
# =================================================================
# ==12345==ERROR: AddressSanitizer: heap-buffer-overflow
# WRITE of size 4 at 0x60300000eff4 thread T0
#     #0 0x4a2f3c in main program.c:10
# ...
```

**Dynamic Analysis Tools**

- **Memory**:
  - **Valgrind**: Memory error detection, leak detection, profiling.
  - **AddressSanitizer**: Fast memory error detection.
  - **Dr. Memory**: Memory debugger for Windows and Linux.

- **Concurrency**:
  - **ThreadSanitizer**: Data race detection.
  - **Helgrind**: Race condition and deadlock detection (Valgrind tool).
  - **Intel Inspector**: Commercial concurrency analyzer.

- **Performance**:
  - **gprof**: CPU profiling.
  - **perf**: Linux performance analysis.
  - **VTune**: Intel's performance profiler.

**Dynamic vs. Static Analysis**

- **Static Analysis**:
  - Pros: No execution needed, analyzes all paths, fast.
  - Cons: False positives, may miss runtime-specific bugs.

- **Dynamic Analysis**:
  - Pros: Precise (no false positives), detects runtime bugs, real behavior.
  - Cons: Requires execution, only tests exercised paths, slower.

- **Complementary**: Use both — static analysis for broad coverage, dynamic analysis for precision.

**Challenges**

- **Coverage**: Only detects bugs in executed code paths — need good test suite.
- **Performance Overhead**: Instrumentation slows execution — may be impractical for production.
- **Non-Determinism**: Concurrency bugs may not reproduce consistently.
- **Test Input Quality**: Effectiveness depends on test inputs — poor inputs miss bugs.

**LLMs and Dynamic Analysis**

- **Test Generation**: LLMs generate test inputs to trigger bugs detected by dynamic analysis.
- **Bug Explanation**: LLMs explain dynamic analysis reports in natural language.
- **Fix Suggestion**: LLMs suggest fixes for bugs found by dynamic analysis.

**Applications**

- **Development**: Run sanitizers during development to catch bugs early.
- **Testing**: Use dynamic analysis in test suites to detect bugs.
- **Continuous Integration**: Run dynamic analysis on every commit.
- **Debugging**: Use dynamic analysis tools to diagnose specific bugs.
- **Performance Optimization**: Profile to find and fix performance bottlenecks.

**Benefits**

- **Precision**: No false positives — reports real bugs.
- **Runtime Bugs**: Detects bugs that only manifest during execution.
- **Validation**: Confirms code works correctly on specific inputs.
- **Debugging Aid**: Provides detailed information about bug location and cause.

**Limitations**

- **Incomplete Coverage**: Only analyzes executed paths — may miss bugs in untested code.
- **Performance Cost**: Instrumentation overhead can be significant.
- **Requires Execution**: Need to run the program — may be difficult for some systems.

Dynamic analysis is a **powerful complement to static analysis** — it provides precise bug detection by observing actual program behavior, making it essential for finding memory errors, concurrency bugs, and performance issues that are difficult to detect statically.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/dynamic-analysis) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
