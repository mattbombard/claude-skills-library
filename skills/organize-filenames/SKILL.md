---
name: organize-filenames
description: Reviews files in a directory and renames them using consistent naming conventions (kebab-case for folders, snake_case for files)
---

# Organize Filenames

## Trigger
User wants to review and fix file/folder naming conventions in a directory, or explicitly invokes this skill.

## Workflow

### Step 1: Determine Target Directory
- If user specifies a path, use that directory
- If no path specified, use the current working directory
- Ask user to confirm the directory to review

### Step 2: Scan Directory Contents
Recursively list all files and folders in the target directory:
```bash
find [directory] -depth
```

### Step 3: Identify Naming Violations

Check each item against naming conventions:

**Folders** should use kebab-case (lowercase with hyphens):
- ✅ `_master-files/`, `td-bank/`, `senior-audit-manager-us/`
- ❌ `Master Files/`, `TD_Bank/`, `SeniorAuditManager/`

**Files** should use snake_case (lowercase with underscores):
- ✅ `matthew_bombard_resume.pdf`, `cover_letter.docx`, `phone_screen_notes.docx`
- ❌ `Matthew Bombard Resume.pdf`, `CoverLetter.docx`, `phone-screen-notes.docx`

**Common violations to detect**:
- Spaces in names (both files and folders)
- Capital letters (CamelCase, PascalCase)
- Incorrect separators (hyphens in files, underscores in folders)
- Mixed case inconsistencies

### Step 4: Generate Rename Plan

Create a table of proposed changes:

```
Current Name                          → Proposed Name                      Type
================================================================================
Master Files/                         → _master-files/                    Folder
TD Bank/                              → td-bank/                          Folder
Matthew Bombard Resume.pdf            → matthew_bombard_resume.pdf        File
CoverLetter.docx                      → cover_letter.docx                 File
phone-screen-notes.docx               → phone_screen_notes.docx           File
```

### Step 5: User Confirmation

Present the rename plan to the user with:
- Total number of files/folders to rename
- List of all proposed changes
- Ask for confirmation before proceeding

**Important**: Always get explicit user approval before renaming anything.

### Step 6: Execute Renames

If user confirms, rename files/folders in order (deepest paths first to avoid conflicts):

```bash
# Rename from deepest to shallowest to avoid path issues
# Use mv command for each rename
mv "old-name" "new-name"
```

**Safety checks**:
- Skip if target name already exists
- Handle nested renames (rename children before parents)
- Report any errors or conflicts

### Step 7: Summary Report

After completion, report:
- ✅ Successfully renamed items (count)
- ⚠️ Skipped items (already compliant or conflicts)
- ❌ Failed renames (with error messages)
- List all changes made

## Naming Convention Reference

**Folders**: kebab-case (lowercase-with-hyphens)
- Root folders: `_master-files/`, `_templates/`, `applications/`
- Company folders: `td-bank/`, `wells-fargo/`, `fastbreak-ai/`
- Role folders: `senior-audit-manager-us/`, `product-manager/`

**Files**: snake_case (lowercase_with_underscores)
- Resume files: `matthew_bombard_resume.pdf`, `matthew_bombard_resume.docx`
- Cover letters: `cover_letter.docx`, `cover_letter_matthew_bombard.docx`
- Notes/misc: `phone_screen_notes.docx`, `jeff_call_notes_2025_01_16.docx`

**Benefits**:
- Terminal-friendly (no need to escape spaces)
- Consistent and predictable
- Easy to script and automate
- Professional appearance

## Example Usage

```
User: "organize filenames in applications/TD Bank/"
Claude: [Scans directory, finds violations, presents rename plan]
User: "yes, proceed"
Claude: [Executes renames, reports results]
```

## Notes

- Preserve file extensions
- Don't rename hidden files (starting with `.`) unless explicitly requested
- Don't rename system or configuration files
- Be careful with git-tracked files (warn user if in git repo)
