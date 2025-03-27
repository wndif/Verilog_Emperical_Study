# Verilog_Emperical_Study

This repository contains empirical data from our analysis of 300+ open-source Verilog hardware design projects. The dataset is divided into two main parts, each supporting different aspects of hardware design reliability research.

## Dataset Overview

### Part 1: Repository Metadata
**File:** `basic_info_of_verilog_repositories.csv`  
**Format:** CSV with header row  
**Columns:**
- `NO.`: Sequence number
- `Project`: Repository name
- `All Commits`: Total commits in project history
- `All collected patch-related Commits`: Valid commits after filtering (see criteria below)
- `LOC`: Lines of code

**Commit Filtering Criteria:**
1. Keyword-based filtering: Retain commits containing 'bug', 'fix', 'error', 'repair', 'patch', or 'fault' in changelogs
2. Code relevance filtering: Exclude commits targeting:
   - Test files (`*Testbench.v`, `TB/tb.v`, `test.v`)
   - Documentation files
   - Comment-only changes

**Statistics:**
- Projects range: 63 - 900,000+ LOC
- Total commits analyzed: >15,000
- Sample Data:
  ```
  NO.,Project,All Commits,All collected patch-related Commits,LOC
  1,oscilloscope,3,0,322
  2,i2crepeater,2,0,185
  3,usb_phy,10,4,1091
  ```

### Part 2: Bug Pattern Analysis
**Location:** `Patches/` directory  
**Per-project Files:**
1. **elements.csv** - Expression-level bug patterns  
   - Rows: Bug types (e.g., HdlOp-MemberAccessing)
   - Columns: Fix types (update/insert/move/delete)
   - Values: Occurrence counts

2. **stmt.csv** - Statement-level bug patterns  
   - Rows: Bug types (e.g., HdlStmAssign)
   - Columns: Fix types (update/insert/move/delete)
   - Values: Occurrence counts

3. **fixpatterns.csv** - Aggregation of statement/expression-level bug patterns and repair methods  
   - Rows: Defect location descriptor (ChildNodeType-ParentNodeType)  
     *Example: `HdlIdDef-HdlModuleDef` indicates a defect located at a child node of type `HdlIdDef` under a parent node of type `HdlModuleDef`*  
   - Columns: Repair actions (update, insert, delete, move)  
     *Specific code modification operations*  
   - Values: Pattern frequency  
     *Count of repair actions applied to defects at the specified hierarchical location*  

4. **patchesFile.txt** - AST diff analysis of bug-fix commits  
   Format:
   ```
   CommitId: [SHA]
   Commit message
   --- Diff paths
   @@ Diff chunks
   ParseResult: AST change operations (UPD/INS/DEL/MOV)
   ```

## File Structure
```
├── repository_metadata.csv
└── Patches/
    ├── Project1/
    │   ├── elements.csv
    │   ├── stmt.csv
    │   ├── fixpatterns.csv
    │   └── patchesFile.txt
    ├── Project2/
    └── .../
```

## Usage Notes
1. CSV files contain empty cells where no corresponding fix operation exists
2. AST diffs in patchesFile.txt use the following notation:
   - `UPD`: Node update
   - `INS`: Node insertion 
   - `DEL`: Node deletion
   - `MOV`: Node movement
3. HDL types follow Verilog syntax hierarchy (HdlOp, HdlStm, etc.)


---

*For questions or collaboration requests, please contact wujadeon@outlook.com.*
