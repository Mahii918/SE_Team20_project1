INPUT
## Architectural Mapping and Design Recovery

### User and Role Management Subsystem
- Handles user registration
- Manages permissions (Owner, Editor, Drafter)
- Provides administrative controls

### Files to go through
- **Core Domain Models (POJOs)**
  - User.java
  - UserRole.java
  - WeblogPermission.java
  - GlobalPermission.java
  - RollerPermission.java
  - ObjectPermission.java
- **Business Logic (Managers)**
  - UserManager.java
  - JPAUserManagerImpl.java
  - Weblogger.java
  - WebloggerFactory.java
- **UI Layer (Admin)**
  - UserAdmin.java
  - UserEdit.java
  - Register.java
  - Profile.java

OUTPUT
Step 1 - Identify Relevant Classes: 
- Discern the key Java classes and interfaces responsible.
Step 2 - Document Functionality: 
- Create detailed documentation for each identified class, explaining its role.
- The documentation should be in a plural first person, and human like.
- Ensure that the documentation is in markdown format.
Step 3 - Create UML Diagrams:
- Use PlantUML to model relationships.
- Understand the Classes, Interfaces and their uses first, then add the appropriate relationships between them.
Step 4 - Observations and Comments:
- Highlight strengths and weaknesses of the current design in the documentation as a separate section.
Step 5 - Assumptions:
- Explicitly state any simplifications you made while modeling in the documentation, as a separate section.

AVOID
- Using emojis in the documentation.
- Using tables or other diagrammatic representation in the documentation