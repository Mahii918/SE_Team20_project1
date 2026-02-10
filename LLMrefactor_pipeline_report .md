# 🔧 LLM-Powered Code Refactoring Pipeline

An automated pipeline that detects design smells in Java code and generates refactored code using Large Language Models (Groq + LangChain).

## 📋 Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [File Structure](#file-structure)
- [Pipeline Flowchart](#pipeline-flowchart)
- [Handling Large Files](#handling-large-files)
- [Refactored Files](#refactored-files)
- [Suggestions for Future Refactoring](#suggestions-for-future-refactoring)
- [Usage](#usage)
- [Configuration](#configuration)

---

## 🎯 Overview

This pipeline automates the process of:
1. **Detecting** design smells from a CSV report or by analyzing source code
2. **Prioritizing** files based on the number of smells detected
3. **Refactoring** code using Groq's LLM (llama-3.3-70b-versatile) via LangChain
4. **Creating Pull Requests** on GitHub with detailed descriptions

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    LLM Refactoring Pipeline                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐       │
│  │   GitHub     │───▶│    Clone     │───▶│   Analyze    │       │
│  │   Repo       │    │    Repo      │    │   Smells     │       │
│  └──────────────┘    └──────────────┘    └──────────────┘       │
│                                                 │                │
│                                                 ▼                │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐       │
│  │   Create     │◀───│   Groq LLM   │◀───│  Prioritize  │       │
│  │   PR         │    │   Refactor   │    │   Files      │       │
│  └──────────────┘    └──────────────┘    └──────────────┘       │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📁 File Structure

| File | Description |
|------|-------------|
| **`main.py`** | Entry point and orchestrator. Handles CLI arguments, clones repository, coordinates the pipeline steps, and manages the overall workflow. |
| **`smell_detector.py`** | Detects design smells by parsing Java source code using `javalang`. Also includes `CSVSmellLoader` to load pre-detected smells from DesigniteJava CSV reports. |
| **`refactor_engine.py`** | Uses Groq LLM via LangChain to generate refactored code. Contains prompt templates, smell-to-technique mappings, and response parsing logic. |
| **`pr_creator.py`** | Creates Pull Requests on GitHub using the GitHub API. Handles branch creation, file commits, and generates detailed PR descriptions. |
| **`requirements.txt`** | Python dependencies for the project. |
| **`.env`** | Environment variables (API keys). |
| **`designCodeSmells.csv`** | Input CSV file containing detected design smells (from DesigniteJava or similar tools). |

### Detailed File Descriptions

#### 1. `main.py` - Pipeline Orchestrator
```
Functions:
├── clone_repository()      - Clones GitHub repo with token authentication
├── get_top_smells_from_csv() - Parses CSV and returns top N smelly classes
├── add_custom_file()       - Adds specific files to refactoring queue
├── run_pipeline()          - Main pipeline execution
└── main()                  - CLI entry point with argument parsing
```

#### 2. `smell_detector.py` - Smell Detection
```
Classes:
├── DetectedSmell          - Dataclass for smell information
├── SmellDetector          - Analyzes Java files for design smells
│   ├── scan_repository()  - Scans all Java files
│   ├── _analyze_java_file() - Parses individual Java files
│   ├── _detect_smells()   - Applies smell detection rules
│   └── get_priority_smells() - Returns prioritized list
└── CSVSmellLoader         - Loads smells from DesigniteJava CSV
    └── parse_report()     - Parses CSV format
```

#### 3. `refactor_engine.py` - LLM Refactoring
```
Class: RefactoringEngine
├── __init__()             - Initializes Groq client via LangChain
├── _create_prompt_template() - Creates structured prompt
├── refactor()             - Main refactoring method
├── _parse_response()      - Extracts code from LLM response
└── validate_refactoring() - Validates refactored code

Smell-to-Technique Mapping:
├── Insufficient Modularization → Extract Method, Extract Class
├── Deficient Encapsulation → Encapsulate Field, Getters/Setters
├── Hub-like Modularization → Extract Class, Introduce Facade
├── God Class → Extract Class, Move Method
└── ... (14 smell types total)
```

#### 4. `pr_creator.py` - GitHub PR Creation
```
Class: PRCreator
├── create_refactoring_pr() - Main PR creation method
├── _clean_file_path()     - Normalizes file paths for GitHub API
├── _get_branch_sha()      - Gets SHA of base branch
├── _create_branch()       - Creates new branch
├── _update_file()         - Commits file changes
├── _create_pull_request() - Creates the PR
├── _generate_pr_title()   - Creates descriptive title
└── _generate_pr_body()    - Generates detailed PR description
```

---

## 🔄 Pipeline Flowchart

```
                              ┌─────────────────┐
                              │     START       │
                              └────────┬────────┘
                                       │
                                       ▼
                    ┌─────────────────────────────────────┐
                    │  1. ENVIRONMENT CHECK               │
                    │  • Verify GROQ_API_KEY              │
                    │  • Verify GITHUB_TOKEN              │
                    └────────────────┬────────────────────┘
                                     │
                                     ▼
                    ┌─────────────────────────────────────┐
                    │  2. CLONE REPOSITORY                │
                    │  • Clone from GitHub URL            │
                    │  • Or use local repo path           │
                    └────────────────┬────────────────────┘
                                     │
                                     ▼
                    ┌─────────────────────────────────────┐
                    │  3. LOAD DESIGN SMELLS              │
                    │  • Parse CSV file                   │
                    │  • Group by class                   │
                    │  • Sort by smell count (desc)       │
                    │  • Exclude already processed        │
                    └────────────────┬────────────────────┘
                                     │
                                     ▼
                    ┌─────────────────────────────────────┐
                    │  4. PRIORITIZE FILES                │
                    │  • Select top N files               │
                    │  • Add custom files if specified    │
                    └────────────────┬────────────────────┘
                                     │
                                     ▼
                    ┌─────────────────────────────────────┐
                    │  5. FOR EACH FILE:                  │
                    │  ┌───────────────────────────────┐  │
                    │  │ a. Read source code           │  │
                    │  │ b. Build LLM prompt           │  │
                    │  │ c. Call Groq API              │  │
                    │  │ d. Parse refactored code      │  │
                    │  │ e. Store results              │  │
                    │  └───────────────────────────────┘  │
                    └────────────────┬────────────────────┘
                                     │
                                     ▼
                          ┌──────────────────────┐
                          │   DRY RUN MODE?      │
                          └──────────┬───────────┘
                                     │
                    ┌────────────────┴────────────────┐
                    │                                 │
                    ▼                                 ▼
           ┌──────────────┐                 ┌──────────────────┐
           │   YES        │                 │      NO          │
           │   Show what  │                 │   Create PR      │
           │   would be   │                 │   on GitHub      │
           │   changed    │                 └────────┬─────────┘
           └──────────────┘                          │
                                                     ▼
                                    ┌─────────────────────────────┐
                                    │  6. CREATE PULL REQUEST     │
                                    │  • Create/reset branch      │
                                    │  • Commit each file         │
                                    │  • Generate PR description  │
                                    │  • Create PR                │
                                    └────────────────┬────────────┘
                                                     │
                                                     ▼
                                              ┌─────────────┐
                                              │    END      │
                                              └─────────────┘
```

---

## 📦 Handling Large Files

The pipeline handles large Java files through several strategies:

### 1. Token Limit Management

```
┌─────────────────────────────────────────────────────────────────┐
│                   LARGE FILE HANDLING                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌──────────────┐                                               │
│  │ Input File   │ ─── Size Check ───┐                           │
│  │ (Java Code)  │                   │                           │
│  └──────────────┘                   ▼                           │
│                           ┌─────────────────┐                   │
│                           │  < 8000 tokens? │                   │
│                           └────────┬────────┘                   │
│                                    │                            │
│              ┌─────────────────────┴─────────────────────┐      │
│              │                                           │      │
│              ▼                                           ▼      │
│     ┌────────────────┐                         ┌────────────────┐│
│     │ YES: Process   │                         │ NO: Truncate/  ││
│     │ entire file    │                         │ Summarize      ││
│     └────────────────┘                         └────────────────┘│
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### 2. Groq Configuration

```python
# In refactor_engine.py
self.llm = ChatGroq(
    api_key=self.groq_key,
    model_name="llama-3.3-70b-versatile",
    temperature=0.3,      # Lower for consistent output
    max_tokens=8000       # Maximum output tokens
)
```

### 3. Strategies Used

| Strategy | Description |
|----------|-------------|
| **Prioritization** | Process files with most smells first (higher impact) |
| **Batch Processing** | Process N files at a time to avoid rate limits |
| **Error Recovery** | Continue with next file if one fails |
| **Response Parsing** | Multiple regex patterns to extract code from various LLM response formats |

### 4. Context Preservation

The prompt includes:
- Full class code (up to token limit)
- Package information
- List of detected smells
- Suggested refactoring techniques
- Clear output format requirements

---

## ✅ Refactored Files

### PR: LLMGeneratedPR Branch

| # | File | Class | Smells Addressed | Techniques Applied |
|---|------|-------|------------------|-------------------|
| 1 | `app/src/main/java/org/apache/roller/weblogger/pojos/Weblog.java` | Weblog | Insufficient Modularization, Deficient Encapsulation, Hub-like Modularization, Cyclic-Dependent Modularization | Extract Class, Extract Method, Introduce Facade |
| 2 | `app/src/main/java/org/apache/roller/weblogger/pojos/WeblogEntry.java` | WeblogEntry | Insufficient Modularization, Deficient Encapsulation, Hub-like Modularization, Cyclic-Dependent Modularization | Extract Class, Move Method |
| 3 | `app/src/main/java/org/apache/roller/weblogger/ui/struts2/util/UISecurityInterceptor.java` | UISecurityInterceptor | Missing Hierarchy, Unexploited Encapsulation, Unutilized Abstraction | Extract Method, Single Responsibility |

### Summary

| Metric | Value |
|--------|-------|
| **Total Files Refactored** | 3 |
| **Total Smells Addressed** | 11 |
| **Refactoring Techniques Used** | 6 |
| **PR Branch** | `LLMGeneratedPR` |

---

## 💡 Suggestions for Future Refactoring

Based on the CSV analysis, here are high-priority files recommended for future refactoring:

### High Priority (Multiple Smells)

| Rank | Class | Package | Smells | Recommended Techniques |
|------|-------|---------|--------|----------------------|
| 1 | `WeblogEntryManager` | `org.apache.roller.weblogger.business` | God Class, Hub-like Modularization, Insufficient Modularization | Extract Class, Introduce Service Layer, Apply Repository Pattern |
| 2 | `WeblogManager` | `org.apache.roller.weblogger.business` | Hub-like Modularization, Cyclic Dependencies | Extract Interface, Dependency Injection |
| 3 | `UserManager` | `org.apache.roller.weblogger.business` | Insufficient Modularization, Feature Envy | Extract Method, Move Method |
| 4 | `ThemeManager` | `org.apache.roller.weblogger.business` | Deficient Encapsulation, Wide Hierarchy | Encapsulate Field, Collapse Hierarchy |
| 5 | `MediaFileManager` | `org.apache.roller.weblogger.business` | God Class, Long Method | Extract Class, Extract Method |

### Medium Priority

| Class | Smells | Recommended Action |
|-------|--------|-------------------|
| `PlanetManager` | Cyclic-Dependent Modularization | Break circular dependencies with interfaces |
| `BookmarkManager` | Unutilized Abstraction | Remove or repurpose unused abstractions |
| `PingTargetManager` | Deficient Encapsulation | Add proper access modifiers and getters/setters |
| `PropertiesManager` | Insufficient Modularization | Split into configuration-specific classes |

### Smell Distribution in Codebase

```
Smell Type                          Count    Priority
─────────────────────────────────────────────────────
Deficient Encapsulation             45       🔴 High
Insufficient Modularization         38       🔴 High
Hub-like Modularization             22       🟡 Medium
Cyclic-Dependent Modularization     18       🟡 Medium
Unutilized Abstraction              15       🟢 Low
Missing Hierarchy                   12       🟢 Low
Unexploited Encapsulation           10       🟢 Low
```

### Refactoring Roadmap

```
Phase 1 (Completed ✅)
├── Weblog
├── WeblogEntry
└── UISecurityInterceptor

Phase 2 (Recommended)
├── WeblogEntryManager
├── WeblogManager
├── UserManager
├── ThemeManager
└── MediaFileManager

Phase 3 (Future)
├── PlanetManager
├── BookmarkManager
├── PingTargetManager
├── PropertiesManager
└── Other utility classes
```

---

## 🚀 Usage

### Basic Usage

```bash
# Dry run (see what would be refactored)
python main.py --csv-path designCodeSmells.csv --max-files 5 --dry-run

# Create PR with top 5 files
python main.py --csv-path designCodeSmells.csv --max-files 5 --branch LLMGeneratedPR

# Include specific custom file
python main.py --csv-path designCodeSmells.csv --max-files 5 --include-weblog
```

### Command Line Arguments

| Argument | Default | Description |
|----------|---------|-------------|
| `--repo-url` | `https://github.com/serc-courses/project-1-team-20` | GitHub repository URL |
| `--repo-path` | None | Local path (skips cloning) |
| `--csv-path` | `designCodeSmells.csv` | Path to smells CSV |
| `--max-files` | `5` | Max files to refactor |
| `--branch` | `LLMGeneratedPR` | Target branch name |
| `--dry-run` | False | Preview without creating PR |
| `--include-weblog` | False | Include Weblog.java |

---

## ⚙️ Configuration

### Environment Variables (`.env`)

```bash
GROQ_API_KEY=your_groq_api_key_here
GITHUB_TOKEN=your_github_token_here
```

### Dependencies (`requirements.txt`)

```
python-dotenv>=1.0.0
requests>=2.31.0
langchain>=0.1.0
langchain-groq>=0.1.0
langchain-core>=0.1.0
schedule>=1.2.0
javalang>=0.13.0
```

### Installation

```bash
# Clone this repository
git clone <this-repo-url>
cd LLM_Refactor

# Install dependencies
pip install -r requirements.txt

# Set up environment variables
cp .env.example .env
# Edit .env with your API keys

# Run the pipeline
python main.py --csv-path designCodeSmells.csv --dry-run
```

---

## 📊 Metrics & Results

### Pipeline Performance

| Metric | Value |
|--------|-------|
| Average refactoring time per file | ~10-15 seconds |
| Success rate | ~60-80% (depends on file complexity) |
| LLM Model | llama-3.3-70b-versatile |
| Max tokens per request | 8000 |

### Quality Improvements

The refactored code shows improvements in:
- ✅ **Encapsulation**: Private fields with proper getters/setters
- ✅ **Modularity**: Smaller, focused methods
- ✅ **Single Responsibility**: Each class/method has one purpose
- ✅ **Reduced Dependencies**: Fewer circular dependencies
- ✅ **Code Readability**: Better naming and structure

---

## 🤝 Contributing

To add support for new smell types or refactoring techniques:

1. Update `SMELL_TO_TECHNIQUE` mapping in `refactor_engine.py`
2. Add detection logic in `smell_detector.py`
3. Update prompt template if needed

---

## 📝 License

This project is for educational purposes as part of the SERC course project.

---

*Generated by LLM Refactoring Pipeline - Powered by Groq + LangChain*