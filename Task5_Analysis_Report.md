# 5. Comparative Refactoring Analysis (Manual vs LLM vs Agentic)

In this section, we present a qualitative and empirical comparison of refactoring outcomes produced using three different approaches: **manual refactoring**, **LLM-assisted refactoring**, and **agentic refactoring**. The comparison is carried out on two design smells that were identified earlier in Task 2A and addressed in subsequent tasks. These design smells were chosen because of their architectural significance and their suitability for a meaningful comparative analysis.

The existence and location of the design smells discussed in this section are based on the findings from **Task 2A**, which we treat as the established baseline for this comparison.

---

## 5.1 Design Smell: Deficient Encapsulation

### Smell Description
Deficient Encapsulation was identified in core weblog-related components where internal data structures and responsibilities were overly exposed to other parts of the system. This weakened information hiding and increased coupling between classes, making the system more fragile to change and harder to maintain as it evolves.

---

### Manual Refactoring (Task 3A)
In the manual refactoring approach, we addressed this smell across multiple classes within the Weblog and User Management subsystems. We redistributed responsibilities by introducing clearer service boundaries and encapsulating internal logic behind well-defined interfaces. Persistence-related concerns were separated from higher-level business logic. After refactoring, all existing tests passed successfully, giving us confidence that the original system behavior was preserved.

---

### LLM-Assisted Refactoring (Single-Shot)
For LLM-assisted refactoring, we used a **single prompt** with the source code of a representative class, **`Weblog.java`**, along with a description of the Deficient Encapsulation smell. The LLM suggested improving encapsulation by restructuring responsibilities, extracting helper methods, and reducing the exposure of internal state. While the suggestions were reasonable at the class level, they were limited to the provided file and did not account for broader architectural dependencies.

---

### Agentic Refactoring (Task 4)
In the agentic refactoring approach, we used an automated LLM-powered pipeline that performed smell detection, file prioritization, refactoring planning, and refactoring execution. As part of this process, `Weblog.java` was refactored through multiple automated stages. The agent introduced additional abstractions and structural changes, applying refactoring patterns more aggressively and without direct human intervention.

---

### Comparative Analysis

| Dimension | Manual Refactoring | LLM-Assisted Refactoring | Agentic Refactoring |
|---------|------------------|------------------------|--------------------|
| Clarity | Clear intent driven by domain understanding | Readable but sometimes verbose | Reduced clarity due to added abstraction |
| Conciseness | Balanced changes with minimal disruption | Moderate redundancy in suggestions | Over-refactoring in some areas |
| Design Quality (DRY, SOLID) | Strong adherence to SRP and encapsulation | Partial adherence with limited context | Good principle usage but excessive abstraction |
| Faithfulness | Fully validated by existing tests | Not executed or validated | Not validated against test suite |
| Architectural Impact | Localized and controlled | Limited to a single class | Broader and riskier impact |
| Human vs Automation Judgment | Human judgment preserved architectural intent | Useful ideas but lacked context | Automation lacked restraint and prioritization |

---

### Reflection
Overall, we observed that manual refactoring aligned most closely with the system’s architectural intent and constraints. The LLM-assisted approach was useful for generating refactoring ideas but lacked awareness of cross-class interactions. The agentic approach provided structured and scalable refactoring suggestions but occasionally over-refactored, reinforcing the need for human judgment in design-level decisions.

---

## 5.2 Design Smell: Insufficient Modularization

### Smell Description
Insufficient Modularization was identified in content-related components where multiple responsibilities were consolidated within single classes. This resulted in large and complex classes that were difficult to understand, test, and extend, thereby violating the Single Responsibility Principle and reducing overall maintainability.

---

### Manual Refactoring (Task 3A)
To address this smell manually, we decomposed responsibilities across multiple focused services and operation-specific interfaces within the Weblog and Indexing subsystems. Our goal was to introduce clearer module boundaries while preserving existing workflows and interactions. After applying these changes, all existing tests passed successfully, confirming behavioral consistency.

---

### LLM-Assisted Refactoring (Single-Shot)
For LLM-assisted refactoring, we selected **`WeblogEntry.java`** as a representative class and provided it to the LLM using a single prompt describing the Insufficient Modularization smell. The LLM suggested splitting responsibilities into smaller methods and extracting related functionality to improve modularity. While the output was syntactically correct and logically structured, it remained limited to the scope of the single file.

---

### Agentic Refactoring (Task 4)
In the agentic approach, the automated refactoring pipeline analyzed and refactored `WeblogEntry.java` as part of a broader multi-stage process. The agent proposed extensive restructuring, including class decomposition and method relocation, with the aim of improving modular separation. Although systematic, this approach introduced additional complexity and required careful human evaluation.

---

### Comparative Analysis

| Dimension | Manual Refactoring | LLM-Assisted Refactoring | Agentic Refactoring |
|---------|------------------|------------------------|--------------------|
| Clarity | Improved readability through logical separation | Generally clear but fragmented | Reduced due to over-splitting |
| Conciseness | Maintained balance between size and responsibility | Increased class and method count | Excessive decomposition |
| Design Quality (DRY, SOLID) | Strong SRP and separation of concerns | Partial SRP enforcement | Good modular intent but weaker cohesion |
| Faithfulness | Fully validated by tests | Not executed or validated | Not validated against test suite |
| Architectural Impact | Carefully integrated into existing design | Minimal impact beyond the file | Significant structural impact |
| Human vs Automation Judgment | Humans preserved cohesion and intent | Automation generated ideas only | Automation lacked contextual restraint |

---

### Reflection
From our observations, manual refactoring was the most effective in achieving meaningful modularization while maintaining architectural coherence. LLM-assisted refactoring helped surface potential improvements but was insufficient for broader design changes. The agentic approach demonstrated scalability and structure but also highlighted the risks of over-automation without domain-aware human oversight.

---

## Overall Observations
Across both design smells, we found that manual refactoring consistently produced the most balanced and reliable outcomes due to contextual understanding and architectural awareness. LLM-assisted refactoring proved valuable as a support tool for ideation, while agentic refactoring showcased the potential of automation but underscored the continued importance of human judgment in complex software systems.
