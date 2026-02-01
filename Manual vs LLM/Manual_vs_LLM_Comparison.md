# Comparison of Manual Analysis vs LLM Analysis

This document compares the **Manual Analysis** and **LLM Analysis** approaches used to study the Apache Roller core domain model, focusing on completeness, correctness, effort, diagram quality, and depth of insights.

## Comparative Analysis Table

| Dimension | Manual Analysis | LLM Analysis | Conclusion |
|--------|----------------|-------------|------------|
| **Completeness** | Covered all core domain classes and key relationships with an architectural focus. | Covered the same core classes plus additional low-level details such as method behavior and defensive patterns. | LLM more complete in raw detail, manual complete at architectural level |
| **Correctness** | Highly accurate in representing design intent and relationships without speculation. | Structurally correct, but some interpretations required human validation. | Manual analysis more accurate|
| **Effort** | Required careful reading, reasoning, and manual UML creation. <br> **Time:** 6 hours | Faster generation using prompts and review. <br> Time: 2.5 hours | LLM required less effort |
| **Diagram Quality** | Clean, minimal, and focused on core domain concepts. | Very detailed, including methods and internal attributes. | Manual better for clarity, LLM better for detail |
| **Depth of Insights** | Identified deeper design and maintainability issues. | Identified common patterns but mostly descriptive insights. | Manual analysis deeper |

## Qualitative Summary: Manual Analysis vs LLM Analysis

### When Manual Analysis Was Better

- Better at understanding **design intent and architectural trade-offs**
- Clearly explained why design choices were made (e.g., composition in `ObjectPermission`)
- Identified deeper design issues such as:
  - Tight coupling between `User` and `ObjectPermission`
  - Risks of bidirectional associations
  - Rigid composition constraints
  - Inconsistencies between `GlobalPermission` and `WeblogPermission`
- Required contextual reasoning about **long-term maintainability**
- Produced a **conceptually clear UML diagram** focused on core domain relationships

### When LLM Analysis Was Better

- Faster and more **systematic coverage** of classes and relationships
- Strong at **pattern recognition**, such as:
  - Hierarchical permission logic
  - Centralized action handling
  - Defensive programming practices
- Generated a **highly detailed UML diagram**, useful as a low-level reference
- Reduced overall analysis effort and time


