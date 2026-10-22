# Linux Commands Practice Notes

## 1. Find the Number of Files/Entries in a Directory

```bash
ls -1 | wc -l
```

- `ls -1` — displays each directory entry on a separate line. Here `1` is the number one.
- `wc -l` — counts lines. Here `l` is a lowercase letter L.

**Note:** This counts visible entries, including directories, not just regular files. Hidden entries are excluded.

To count all entries, including hidden ones (excluding `.` and `..`):

```bash
ls -1A | wc -l
```

## 2. Combine and Sort Data from Two Files

```bash
cat name.txt list.txt | sort
```

- `cat` — combines and displays the contents of both files.
- `|` — passes the combined output to the next command.
- `sort` — sorts the lines alphabetically by default.

## 3. Sort Data and Get Unique Values

Display sorted data:

```bash
cat name.txt | sort
```

Remove duplicate lines:

```bash
cat name.txt | sort | uniq
```

- `sort` — sorts the lines.
- `uniq` — removes adjacent duplicate lines.

**Note:** Sorting first ensures identical lines are adjacent, so `uniq` can remove duplicates wherever they originally appeared.

Alternative:

```bash
sort -u name.txt
```

## 4. Display Specific Lines Using `head` and `tail`

Display the first 20 lines:

```bash
head -20 file.txt
```

Display the last 5 lines:

```bash
tail -5 file.txt
```

Display lines 16–20:

```bash
head -20 file.txt | tail -5
```

**How it works:**
1. `head -20` selects the first 20 lines.
2. `tail -5` selects the last 5 lines from those 20.

Therefore, the result contains lines 16, 17, 18, 19 and 20.

You can also use `head -21`, but that selects 21 lines, not 20.

## 5. View Output Page by Page Using `more`

```bash
netstat | more
```

- `netstat` — displays network connections and statistics.
- `|` — passes output to `more`.
- `more` — displays output one screen at a time.

**Useful keys:**

- `Space` — next screen
- `Enter` — next line
- `q` — quit

## 6. View Output Using `less`

```bash
netstat | less
```

`less` allows forward and backward navigation.

**Useful keys:**

- `Space` — next screen
- `b` — previous screen
- `↑` / `↓` — move up or down
- `/word` — search forward for a word
- `n` — next search match
- `q` — quit

**Remember:** `less` is more flexible than `more`.

## 7. Display and Save Output Using `tee`

Display the directory listing and save it to a file:

```bash
ls | tee files.txt
```

- `ls` — generates the directory listing.
- `tee files.txt` — displays the output in the terminal and writes it to `files.txt`.

Count the directory entries while saving and displaying the listing:

```bash
ls | tee files.txt | wc -l
```

The output is displayed in the terminal, saved to `files.txt`, and counted by `wc -l`.

**Note:** `tee` overwrites the destination file by default. To append instead, use `tee -a files.txt`.

## 8. Understand `echo`, Pipes and `xargs`

### A. `ls | echo`

```bash
ls | echo
```

This normally produces a blank line because `echo` does not read standard input; it prints the arguments supplied to it.

### B. `ls | xargs echo "hi"`

```bash
ls | xargs echo "hi"
```

- `xargs` — reads input and passes it as arguments to another command.
- `echo` — prints `hi` followed by the names supplied by `xargs`.

The output will include `hi` and the directory entry names, subject to `xargs` parsing.

### C. Create files from the contents of a text file

```bash
cat country.txt | xargs touch
```

- `cat country.txt` — displays the file contents.
- `xargs touch` — passes the input as filenames to `touch`.
- `touch` — creates the files if they do not exist.

**Important:** Each country name should be a valid filename and should not contain spaces unless handled carefully. Do not run this on untrusted input because it may create unintended files.

A simpler equivalent for ordinary text input is:

```bash
xargs touch < country.txt
```

---

## Quick Revision

| Command | Purpose |
|---|---|
| `ls -1 \| wc -l` | Count visible directory entries |
| `cat a.txt b.txt \| sort` | Combine and sort two files |
| `sort name.txt \| uniq`
