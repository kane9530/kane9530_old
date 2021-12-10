---
layout: post
title: 5 tips to setting up a well-organised bioinformatics project
tags: bioinformatics, bds 
comments: true
analytics: true
---

# Resource:
Adapted from Chapter 2 of [Bioinformatics Data Skills (bds)](https://www.amazon.com/Bioinformatics-Data-Skills-Reproducible-Research/dp/1449367372){:target="_blank"}

# TL;DR:
1. Have a well-organised directory structure
2. Use relative paths in scripts
3. Maintain good project documentation
4. Name the files consistently
5. Use Markdown as our computational notebook

# Tip 1: Have a well-organised directory structure.

E.g. 
1. A *data/* directory that contains all raw and intermediate data
2. A *scripts/* directory that contains project-wide scripts, with further subdirectories for modules.
3. An *analysis/* directory

```bash
# Note the use of brace expansion.
# The -p flag builds the necessary subdirectories as needed.

$ mkdir -p myProject/{data/seqs, scripts, analysis}

```

## Create subdirectories 
Do not crowd all output in the same project directory. Instead, use *subdirectories* to logically partition subprojects (e.g. separate the data quality improvement from alignment from downstream analyses etc.)

# Tip 2: Use relative paths in scripts.

Absolute paths will differ from one machine to another, whereas relative paths will always work as long as we don't change the project directory structure.

Hence, for reproducibility purposes, always use relative paths.

# Tip 3: Maintain good project documentation.
Document:

1. All data (raw/intermediate). Do include
- *how* (MySQL vs SRA-toolkit) 
- *when* (in case of version updates)
- *where* (website?) these data were accessed.

2. All command-line options, parameters etc. (if not explicitly indicated in the .sh scripts)
3. All software versions used.

These information can be recorded within plain-text README files in each directory. For instance, to create a README file in the data/ directory if it doesn't already exist:

```bash

$ touch README data/README
```

# Tip 4: Name the files consistently.

Consistent file naming assists us in *programmatically accessing* files with wildcards (*globbing*) or brace expansions.

# Tip 5: Use Markdown as our computational notebook

I used the markdown editor **Ghostwriter** to write my posts for my Jekyll blog. This [blog](https://www.oberlo.com/blog/markdown-editors){:target="_blank"} reviews a full list of free and paid markdown editors for various OS.

