# Notebook style (Jupyter)

Opinionated defaults for Jupyter notebooks. Apply these unless the project records a different choice.

## 1. Kernel

- Pin one project kernel per notebook and record its name and interpreter path in the project's tech-stack doc. Do not rely on a shared global kernel.

## 2. Required header

Every notebook starts with three cells, in this order:

1. Markdown cell — title and description of what the notebook does.
2. Markdown cell — exactly:

   ```
   ## Preparation

   Ensure the cwd is the project root. This part is the same for all notebooks.
   ```

3. Code cell — locate the project root by walking up the tree until a root marker file (`root.txt`) is found, then `chdir` there:

   ```python
   import os
   import sys


   def find_root_directory():
       current_dir = os.getcwd()
       # root.txt lives at the root of the project
       root_indicator = "root.txt"

       while True:
           if root_indicator in os.listdir(current_dir):
               # Root directory found
               return
           parent_dir = os.path.dirname(current_dir)
           if parent_dir == current_dir:
               print("Error: Root directory not found. Exiting.")
               sys.exit(1)
           current_dir = parent_dir
           os.chdir(current_dir)

   # Ensure the working directory is the root directory
   find_root_directory()
   ```

This makes every notebook's working directory the project root, so relative paths resolve consistently regardless of where the kernel was launched.
