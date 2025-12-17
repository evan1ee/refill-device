# CLAUDE.md - Patent Summary CSV Generator

## Project Overview
Summarize patent analysis markdown files into a single consolidated CSV table for quick comparison and review.

---

## File Structure
```
/output/               # Input: Patent analysis .md files
/summary/              # Output: CSV summary file
```

---

## Output CSV Format

**Filename:** `patent_summary.csv`

**Columns:**
| Column | Description |
|--------|-------------|
| Patent_Number | Patent identifier |
| Technical_Problem | What problem does the patent solve? |
| Claim_1_Type | Device / System / Method |
| Claim_1_Elements | Key elements from Claim 1 decomposition |
| How_To_Achieve | Core technical approach/mechanism |
| Infringement_Risk | Literal risk level (High/Medium/Low) |
| Design_Around | Top 1-2 strategies summarized |

---

## Workflow

### Step 1: Initialize
```
1. List all .md files in /output/ folder (exclude _analysis_progress.txt)
2. Create /summary/ folder if not exists
3. Initialize empty CSV data structure
4. Count total files to process
```

### Step 2: Extract Data from ALL .md Files

For EACH patent analysis file:
```
1. READ the .md file
2. EXTRACT:
   - Patent Number (from "## 1. Patent Identification")
   - Technical Problem (from "## 2. Patent Purpose & Technical Problem")
   - Claim 1 Type & Elements (from "## 4. Independent Claims" + "## 5. Broadest Independent Claim Decomposition")
   - How to Achieve (from "## 3. Core Invention Summary")
   - Infringement Risk (from "## 7. Infringement Risk Evaluation")
   - Design-Around (from "## 8. Design-Around Strategies" - first 1-2 items)
3. APPEND row to CSV data structure
4. Continue to next file
```

### Step 3: Write Single CSV
```
1. Write header row ONCE
2. Write ALL patent rows to SAME file
3. Save to /summary/patent_summary.csv
```

### Step 4: Completion
```
✓ Summary complete: patent_summary.csv
  Total patents: X
  Location: /summary/patent_summary.csv
```

---

## Extraction Rules

### Technical Problem
- Extract first 1-2 sentences from section 2
- Max 200 characters
- If longer, truncate with "..."

### Claim 1 Elements
- Combine Elements A, B, C from section 5
- Format: "A: <desc>; B: <desc>; C: <desc>"
- Max 300 characters total

### How to Achieve
- Extract core mechanism from section 3
- Focus on the "Key inventive contribution"
- Max 200 characters

### Design-Around
- Take first 2 strategies from section 8
- Summarize each to ~50 characters
- Format: "1) <strategy>; 2) <strategy>"

---

## CSV Formatting Rules

- Delimiter: `,` (comma)
- Text qualifier: `"` (double quote)
- Escape quotes inside text: `""`
- Line ending: `\n`
- Encoding: UTF-8
- **Single file output** - all patents in one table

**Example output:**
```csv
"Patent_Number","Technical_Problem","Claim_1_Type","Claim_1_Elements","How_To_Achieve","Infringement_Risk","Design_Around"
"US12345678","Solves heat dissipation in compact devices","Device","A: housing; B: heat sink; C: airflow channel","Uses passive convection with structured fins","Medium","1) Use active cooling; 2) Different fin geometry"
"US87654321","Addresses signal interference in wireless systems","Method","A: receive signal; B: filter noise; C: amplify output","Adaptive filtering with real-time calibration","High","1) Different frequency band; 2) Alternative modulation"
```

---

## Commands

| Command | Action |
|---------|--------|
| `Generate patent summary` | Process all .md files → single CSV |
| `Preview summary` | Show first 5 rows before saving |
| `Add column: <name>` | Add custom column to extraction |

---

## Rules

- Tone: **Technical, neutral, design-oriented**
- **DO NOT modify** source .md files
- **ALL patents in ONE CSV file** - do not create separate files
- Skip files that don't match expected format
- If field missing: use `"N/A"`
- If patent number unreadable: use filename as identifier
- **Overwrite** existing CSV on regeneration
- Output language: **English only**