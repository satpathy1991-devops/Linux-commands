# Linux VI Editor – Complete Command Reference

## 1. Introduction

The `vi` editor is a text editor available on Linux and Unix systems. It is commonly used by DevOps engineers and system administrators to edit configuration files, scripts, YAML manifests, and application files on remote servers.

```bash
vi filename
```

Example:

```bash
vi data.csv
```

## 2. Navigation Commands

| Command | Description |
|---|---|
| `G` | Move to the bottom of the file |
| `gg` | Move to the top of the file |
| `75G` | Jump to line 75 |
| `:set number` | Display line numbers |
| `:set nonumber` | Hide line numbers |

## 3. Search Commands

| Command | Description |
|---|---|
| `/word` | Search forward for a word |
| `?word` | Search backward for a word |
| `n` | Repeat the search in the same direction |
| `N` | Repeat the search in the opposite direction |
| `*` | Search forward for the word under the cursor |
| `#` | Search backward for the word under the cursor |

### Examples

```vim
/DevOps
```

Search forward for `DevOps`.

```vim
?Linux
```

Search backward for `Linux`.

Press `n` to continue searching in the original direction.

## 4. Find and Replace

### Replace All Occurrences in the Entire File

```vim
:%s/old/new/g
```

Example:

```vim
:%s/Active/Completed/g
```

Explanation:

- `%` — Search all lines in the file.
- `s` — Substitute text.
- `Active` — Original text.
- `Completed` — Replacement text.
- `g` — Replace all matches on each line.

### Replace with Confirmation

```vim
:%s/Active/Completed/gc
```

The `c` flag asks for confirmation before each replacement.

## 5. Linux `sed` Command

The `sed` command can also replace text in files.

### Preview Changes Without Modifying the File

```bash
sed 's/Active/Completed/g' /root/data.csv
```

### Modify the File Directly

```bash
sed -i 's/Active/Completed/g' /root/data.csv
```

Explanation:

- `sed` — Stream editor.
- `-i` — Edit the file in place.
- `s` — Substitute.
- `g` — Replace all matching occurrences on each line.
- `/root/data.csv` — Path to the target file.

**Best practice:** Back up important files before modifying production configuration files.

```bash
cp /root/data.csv /root/data.csv.bak
```

## 6. Undo and Redo Commands

| Command | Description |
|---|---|
| `u` | Undo the last change |
| `Ctrl + r` | Redo an undone change |
| `:e!` | Reload the current file, discarding unsaved changes |
| `:q!` | Quit without saving unsaved changes |
| `:w` | Save the file |
| `:wq` | Save and quit |

**Important:** Use `:e!` and `:q!` carefully because they can discard unsaved work.

## 7. Insert and Edit Commands

| Command | Description |
|---|---|
| `i` | Insert text before the cursor |
| `a` | Insert text after the cursor |
| `o` | Open a new line below and enter Insert mode |
| `O` | Open a new line above and enter Insert mode |
| `I` | Insert at the first non-blank character of the line |
| `A` | Insert at the end of the line |
| `Esc` | Return to Normal mode |

**Note:** These commands are case-sensitive.

## 8. Delete Commands

| Command | Description |
|---|---|
| `x` | Delete the character under the cursor |
| `r` | Replace one character |
| `dd` | Delete the current line |
| `5dd` | Delete five lines starting from the current line |
| `12dd` | Delete twelve lines starting from the current line |
| `:%d` | Delete all lines in the file |

## 9. Copy, Cut, and Paste

| Command | Description |
|---|---|
| `yy` | Copy the current line |
| `5yy` | Copy five lines |
| `dd` | Delete (cut) the current line |
| `p` | Paste below the cursor line |
| `P` | Paste above the cursor line |

### Example: Copy and Paste

1. Move the cursor to the required line.
2. Press `yy` to copy the line.
3. Move to the destination.
4. Press `p` to paste below the cursor line.

### Example: Cut and Paste

1. Move the cursor to the required line.
2. Press `dd` to delete the line into the register.
3. Move to the destination.
4. Press `p` to paste the line.

## 10. The Three Main Modes of VI

### Normal Mode

Used for navigation and editing commands.

Examples:

```text
gg
G
dd
yy
p
u
```

Press `Esc` to return to Normal mode.

### Insert Mode

Used to type or modify text.

Enter this mode using:

```text
i
a
o
O
I
A
```

### Command-Line Mode

Used to execute commands beginning with `:`.

Examples:

```vim
:w
:wq
:q!
:e!
:set number
:%d
:%s/old/new/g
```

## 11. Practical Linux and DevOps Use Cases

VI is useful for editing:

- Linux configuration files.
- Shell scripts.
- Dockerfiles.
- Kubernetes YAML manifests.
- Terraform configuration files.
- Nginx configuration files.
- Application configuration files.

Example:

```bash
vi deployment.yaml
```

This opens a Kubernetes deployment manifest for editing.

## 12. Quick Revision Cheat Sheet

| Task | Command |
|---|---|
| Open a file | `vi filename` |
| Go to top | `gg` |
| Go to bottom | `G` |
| Jump to line 75 | `75G` |
| Search forward | `/word` |
| Search backward | `?word` |
| Delete a line | `dd` |
| Delete 12 lines | `12dd` |
| Copy a line | `yy` |
| Paste below | `p` |
| Paste above | `P` |
| Undo | `u` |
| Redo | `Ctrl + r` |
| Save | `:w` |
| Save and exit | `:wq` |
| Exit without saving | `:q!` |
| Delete all lines | `:%d` |
| Replace throughout the file | `:%s/old/new/g` |
| Show line numbers | `:set number` |

## 13. Practice Exercises

1. Create a file named `practice.txt`.
2. Add at least 20 lines of text.
3. Navigate to the first and last lines.
4. Search for a particular word.
5. Replace a word throughout the file.
6. Copy and paste five lines.
7. Delete three lines and undo the deletion.
8. Display line numbers.
9. Save and exit.
10. Reopen the file and verify the changes.



## Conclusion

Learning the VI editor is an important part of Linux administration and DevOps engineering. Regular hands-on practice helps improve speed and confidence when managing files on remote Linux servers.

**My learning goal:** Become comfortable using VI commands while working with Linux, Docker, Kubernetes, Terraform, and CI/CD projects.

