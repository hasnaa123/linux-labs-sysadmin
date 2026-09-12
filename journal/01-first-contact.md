### 1\. Build a Structure

```
practice/

├── projectA/

│   ├── notes.txt

│   └── drafts/

├── projectB/

│   └── notes.txt

└── archive/

```
![Build Structure](assets/build-structure.png)

 ### 2\. Populate and Edit

 Put one line of text into both `notes.txt` files using an editor.

 Linux text editors are **programs used to create, open, and modify plain text files, configuration files, and source code**.

 **Nano:** A simple, beginner-friendly terminal editor. It displays keyboard shortcuts at the bottom of the screen.

 **Vim (and Vi):** A powerful, highly efficient terminal editor. It operates using distinct operating modes.

![Text editing](assets/text_editing.png)

 ### 3\. Rename

 Rename `projectB` → `projectB-old` without losing its contents.

```
mv projectB projectB-old

```

 ### 4\. Copy vs Move

 Copy `projectA/notes.txt` → `archive/projectA-notes-backup.txt` (original stays).

```
cp filename.txt /path/to/destination/

```

 Copy multiple files:

```
cp file1.txt file2.txt file3.txt /path/to/destination/

```

 Copy an entire directory:

```
cp -r /path/to/source_directory/ /path/to/destination/

```

 Avoid accidental overwrites if two files have the same name:

```
cp -i filename.txt /path/to/destination/

```

 Move `projectB-old/notes.txt` → `archive/notes.txt` (original disappears).

```

mv projectB-old/notes.txt archive/

```

 ### 5\. Clean Deletion

 Delete `projectA/drafts/` (empty directory) and `archive/notes.txt` (file) separately.

 Test what happens if you try deleting a non-empty directory without a special flag — note the error, don't avoid it.

 Delete the empty directory:

```
rmdir projectA/drafts

```
 Delete the file:

```
rm archive/notes.txt

```

 Attempting to remove a non-empty directory with `rmdir`:

```
rmdir archive

```

 Example error:

```
rmdir: failed to remove 'archive': Directory not empty

```

 ### 6\. Full Teardown

 Remove all of `practice/` in one recursive operation.

 Before running it: **write down exactly what path you're deleting.**

 This is the most dangerous command pattern in Linux — build the double-check habit now.

```

rm -r practice

```