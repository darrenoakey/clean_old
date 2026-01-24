![](banner.jpg)

# clean_old

A utility that automatically archives old files and directories based on configurable age thresholds.

## Purpose

`clean_old` helps keep your common directories tidy by moving files and folders that haven't been modified recently to an archive location. It organizes archived items by year and month based on their last modification date.

## Installation

No installation required. The script is a standalone Python executable.

### Requirements

- Python 3.10 or later
- `rsync` (pre-installed on macOS)

## Configuration

Edit the script to customize behavior:

```python
# Where archived files are moved to
TARGET_PATH = "/Volumes/big1/old"

# Directories to clean and their age thresholds (in days)
DIRECTORIES_TO_CLEAN = [
    ("Downloads", 30),
    ("Desktop", 30),
    ("Documents", 30),
    ("src", 120),
]
```

## Usage

Run the script directly:

```bash
./clean_old
```

Or with Python:

```bash
python3 clean_old
```

## Examples

### Example Output

```
Cleaning Downloads (threshold: 30 days)...
  [15/42] Moving: old-presentation.pdf
  [23/42] Moving: archive.zip
  Done: 2 items moved out of 42 checked

Cleaning Desktop (threshold: 30 days)...
  Done: 0 items moved out of 8 checked

Cleaning Documents (threshold: 30 days)...
  [3/12] Moving: 2024-notes/
  Done: 1 items moved out of 12 checked

Cleaning src (threshold: 120 days)...
  [45/89] Moving: abandoned-project/
  Done: 1 items moved out of 89 checked
```

### Archive Structure

Files are organized by their last modification date:

```
/Volumes/big1/old/
├── Downloads/
│   ├── 2024/
│   │   ├── 10 - October/
│   │   │   └── old-presentation.pdf
│   │   └── 11 - November/
│   │       └── archive.zip
│   └── 2025/
│       └── 01 - January/
│           └── report.docx
├── Desktop/
│   └── ...
└── src/
    └── ...
```

### Automating with cron

Run weekly on Sundays at 2 AM:

```bash
0 2 * * 0 /path/to/clean_old
```