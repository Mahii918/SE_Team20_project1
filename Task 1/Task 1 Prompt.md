INPUT
## Basic Information
- Objective is to reverse engineer, analyze, and refactor **Apache Roller**. This system is significantly more complex than a simple RSS reader; it is a mature Apache project used by major organizations (like Oracle and IBM) for enterprise-level blogging.
- I am working with a 5 member team for this project

### Architectural Mapping and Design Recovery

#### User and Role Management Subsystem
- Handles user registration
- Manages permissions (Owner, Editor, Drafter)
- Provides administrative controls 
#### Weblog & Content Subsystem
- Manages the creation of blogs, entries, and comments
- Includes the rendering engine and content fetching 
#### Search and Indexing Subsystem
- Centered around Lucene-based search
- Enables searching through blog content

### Task Description
##### Identify Relevant Classes: 
- Discern the key Java classes and interfaces responsible for the three subsystems mentioned above.
##### Document Functionality: 
- Create detailed documentation for each identified class, explaining its role and how it interacts with the rest of the subsystem.
##### Create UML Diagrams:
- Use PlantUML to model relationships (inheritance, association, composition, etc.).
- You need to submit the PlantUML diagram along with the code.
##### Observations and Comments:
- Highlight strengths and weaknesses of the current design.
##### Assumptions:
- Explicitly state any simplifications you made while modeling.
##### LLM vs Manual Analysis:
- Perform the above steps once **without** LLM assistance and once **with** LLM assistance on a small, clearly defined part of any one subsystem.
	- Completeness
	- Correctness
	- Effort

OUTPUT
- Give me a step by step process of how to accomplish this task. Answer to the point and make clear actionable tasks for us to accomplish.
- Outline what folders and files in specific do we focus on.

AVOID
- Suggesting the next steps. I will tell you exactly what needs to be done later