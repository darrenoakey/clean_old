# clean_old

Automated file cleanup utility that moves old files/directories to an external archive organized by date.

## Project Structure

```
~/src/clean_old/
├── src/
│   └── clean_old      # Main Python script (executable)
└── claude.md

~/bin/clean-old        # Shell wrapper that calls the Python script
```

## Key Design Decisions

### ExFAT Compatibility
Target drive (`/Volumes/big1/old`) is ExFAT. Uses `rsync` with `--no-perms --no-owner --no-group` instead of `shutil.move` to avoid extended attribute errors (`[Errno 22] Invalid argument`).

### Early Exit Optimization
When checking if a directory should be moved, stops scanning as soon as any file newer than the cutoff is found. Critical for performance with large directories like `~/src` (368GB+).

### Configuration
All configuration is at the top of `src/clean_old`:
- `TARGET_PATH`: Where to move old items
- `DIRECTORIES_TO_CLEAN`: List of (directory, days_threshold) tuples

### Directory Handling
- Recursively scans directories to find the **newest** mtime of any file within
- Uses the newest mtime to determine the YYYY/MM folder for archiving
- Follows symlinks to get real paths
- Only operates on items in the root of each source directory (no recursive cleanup)

## Gotchas

- Large directories with many files take time to scan and copy to external drives
- Items that already exist at destination are skipped (logged as "Skipped (exists)")
- The shell wrapper passes all arguments through with `"$@"`
