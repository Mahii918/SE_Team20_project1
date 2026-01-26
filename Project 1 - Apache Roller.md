# CS6.401 Software Engineering
## Spring 2026
## Project - 1

Welcome to the first project! Your objective is to reverse engineer, analyze, and refactor **Apache Roller**. This system is significantly more complex than a simple RSS reader; it is a mature Apache project used by major organizations (like Oracle and IBM) for enterprise-level blogging.

---

## Target System

**Apache Roller** is an open-source, Java-based blog platform. It supports:
- Multi-user blogging  
- Group blogs  
- Comment moderation  
- Content indexing via Lucene  

Repository: https://github.com/apache/roller

> **Note on LLM Usage**  
> If you use LLMs (ChatGPT, Claude, Gemini, etc.), you must include screenshots of your prompts and the generated responses in your report. Provide a qualitative analysis of the LLM outputs and highlight your own manual contributions or modifications.

---

## Task 1: Architectural Mapping and Design Recovery (30 Marks)

Apache Roller is a large system. To manage its complexity, focus on these three major functional areas:

1. **Weblog and Content Subsystem**
   - Manages the creation of blogs, entries, and comments  
   - Includes the rendering engine and content fetching  

2. **User and Role Management Subsystem**
   - Handles user registration  
   - Manages permissions (Owner, Editor, Drafter)  
   - Provides administrative controls  

3. **Search and Indexing Subsystem**
   - Centered around Lucene-based search  
   - Enables searching through blog content  

### Task Description

- **Identify Relevant Classes**  
  Discern the key Java classes and interfaces responsible for the three subsystems mentioned above.

- **Document Functionality**  
  Create detailed documentation for each identified class, explaining its role and how it interacts with the rest of the subsystem.

- **Create UML Diagrams**  
  Use PlantUML or StarUML to model relationships (inheritance, association, composition, etc.).
  - *Requirement:* Hand-drawn or general drawing tools (draw.io, etc.) will not be considered for final submission.

- **Observations and Comments**  
  Highlight strengths and weaknesses of the current design.

- **Assumptions**  
  Explicitly state any simplifications you made while modeling.

- **LLM vs Manual Analysis**  
  Perform the above steps once **without** LLM assistance and once **with** LLM assistance on a small, clearly defined part of any one subsystem.  
  Provide a brief comparative analysis highlighting differences in:
  - Completeness  
  - Correctness  
  - Effort  

> **Note:** You need to submit the PlantUML/StarUML diagram along with the code.

---

## Task 2: Smells and Metrics Analysis (30 Marks)

### Task 2A: Design Smells

Design smells are indicators of deeper architectural problems. While SonarQube detects *code smells* (fine-grained), you must use these as clues to identify *design smells* (system-level violations).

- **Task:** Detect 5–7 design smells using the tools already mentioned.
- **Tools:**  
  - **SonarQube is mandatory**  
  - You are encouraged to supplement this with Designite Java or IDE plugins.
- **Deliverable:**  
  A list of smells with supporting evidence from tool reports and UML analysis.

> **Note:** SonarQube or any automated tool is not perfect. Use your own judgment.  
> The best way to identify design smells is to first make the UML class diagram and then justify design choices or identify better alternatives.

---

### Task 2B: Code Metrics Analysis

The goal is to conduct a comprehensive code metrics analysis for the initial state of the project using tools such as:
- CodeMR  
- Checkstyle  
- PMD  
- Any other suitable tools  

Analyze **up to 6 key code metrics**, including **OOP-specific metrics** (e.g., Chidamber and Kemerer metrics).

#### Task Description

- **Code Metrics Analysis**  
  Use analysis tools to extract up to 6 relevant metrics.

- **Tools Used**  
  Clearly state the tools used and justify their reliability.

- **Implications Discussion**  
  For each metric, discuss implications related to:
  - Software quality  
  - Maintainability  
  - Potential performance issues  
  - Refactoring decision-making  

---

## Task 3: Refactoring (40 Marks)

> **Note:**  
> All refactorings must preserve the original behavior of the system.  
> - All existing (in-built) test cases must pass  
> - Core functionality must be manually verified  
> - Refactorings that break tests or behavior will be considered incorrect

### Task 3A: Manual Refactoring

Using the 5–7 design smells identified in Task 2A:

- Refactor the code to address these issues
- Do **not** discard existing code—enhance its design
- Refactorings must be **significant (class-level changes)**

> **Note:**  
> Trivial changes include:
> - Defining variables for repeated constants  
> - Wrapping code in try-catch blocks  
> - Excessive method extraction  

These are **not sufficient**.

- **Workflow Requirement:**  
  For each design issue, open a **GitHub Issue** in your team’s repository before refactoring.  
  Your grade depends on this documented roadmap.

---

### Task 3B: Post-Refactoring Metrics

- Re-run metrics analysis
- Discuss:
  - Whether metrics improved or deteriorated  
  - Whether one metric can improve while another worsens  

---

### Task 3C: Automating the Refactoring Pipeline using LLMs

> **Note:**  
> You only need to **document** LLM-suggested changes.  
> The actual codebase should contain **only manually implemented changes**.

Design and implement a pipeline (using OpenAI/Gemini APIs) that:

1. **Detects**  
   Periodically scans the GitHub repository for potential design smells.

2. **Refactors**  
   Generates refactored code using LLMs while preserving behavior.

3. **PR Generation**  
   Automatically creates a Pull Request containing:
   - Detected design smells  
   - Refactoring techniques used  
   - Relevant metrics  

Also provide a **flowchart** explaining:
- Pipeline stages  
- How context of large files is handled  

---

## Task 4: Agentic Refactoring on a Single File (10 Marks)

> **Note:**  
> - Only document agentic refactorings  
> - Actual codebase must include **only manual changes**

Use an existing **agentic refactoring tool/system** (e.g., Claude Code, Cursor, Antigravity).

- Apply to **at least 3–4 files**
- Focus is on **agentic reasoning**, not building agents

### Task Description

The agentic workflow should demonstrate:

1. **Smell Identification**
   - Takes a single Java file  
   - Optionally uses static analysis  
   - Identifies *design-level* smells  

2. **Refactoring Planning**
   - Proposes refactoring strategies  
   - Explains why they are appropriate  
   - Identifies affected methods/classes  

3. **Refactoring Execution**
   - Produces refactored code snippets  
   - Ensures syntactic correctness  
   - Clearly distinguishes original vs refactored code  

### Deliverables

- Name of agentic tool/system used  
- Prompts/commands used  
- Documentation of suggested refactorings (snippets/diff allowed, not applied repo-wide)  

---

## Task 5: Comparative Refactoring Analysis (Manual vs LLM vs Agentic) (10 Marks)

Perform a unified empirical analysis by selecting **2–3 design smells** and refactoring them using:

1. **Manual Refactoring**  
   - No LLM assistance  

2. **LLM-Assisted Refactoring**
   - Exactly **one prompt per smell**
   - No iteration, memory, tools, or agents  

3. **Agentic Refactoring**
   - Uses the agent-based setup from Task 4  

### Analysis Requirements (Qualitative / Empirical)

Compare across:

- Clarity  
- Conciseness  
- Design Quality (DRY, SOLID)  
- Faithfulness to original behavior  
- Architectural Impact  
- Human vs Automation Judgment  
  - Where humans performed better  
  - Where LLMs/agents were advantageous  
  - Where automation failed or needed correction  

---

## Bonus: Benchmarking (5 Marks)

> **Note:**  
> Bonus can only recover lost marks.  
> Final marks = `min(120, project marks + bonus)`

Compare **two different LLMs** (e.g., GPT-4o vs Claude 3.5 Sonnet) in:
- Non-agentic settings  
- Agentic settings  

Evaluate:
- Ability to identify the same design smells as manual analysis  
- Quality and compliability of refactored code  

---

## Submission Instructions

- Submission via **GitHub Classroom**
- Repository will be automatically downloaded at the deadline
- No extensions granted

### Required Documents

- Main report:  
  `docs/project1_<team_number>.pdf`

- Bonus report (if applicable):  
  `docs/project1_bonus_<team_number>.pdf`

- Accurately report contribution of each team member

---

## Deadlines

- **Soft Deadline:**  
  10th February 2026, 11:59 PM IST

- **Hard Deadline:**  
  17th February 2026, 11:59 PM IST

> **Note:**  
> Bonus can be submitted till the hard deadline without penalty.  
> Late days are not applicable for bonus components.

### Commit Requirement

- At least **50% of commits** must be made **before 3rd February 2026, 11:59 PM**
- Repositories with most work near the deadline may be penalized

---

**Best of luck (start early)!**

© 2025 Karthik Vaidhyanathan — Distributed under the MIT License
