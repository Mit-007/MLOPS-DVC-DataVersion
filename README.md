# MLOps: Data Versioning with DVC

A small, hands-on project that shows how to **version datasets like code** using **Git + DVC (Data Version Control)**. Clone it, switch between commits, and watch the dataset change.

![Python](https://img.shields.io/badge/python-3.8%2B-blue)
![DVC](https://img.shields.io/badge/DVC-data%20versioning-945DD6)
![Git](https://img.shields.io/badge/Git-code%20versioning-F05032)

---

## Overview

Git is great for code but not for large or changing datasets. DVC solves this:

| Tool | Stores |
|------|--------|
| **Git** | Code, `data.dvc` pointer file, configs |
| **DVC** | The actual data (in a cache and a remote storage) |

This repo contains three versions of one CSV dataset (`data/sample_data.csv`), each saved in a separate Git commit. Checking out a commit and running `dvc pull` restores the exact data from that point in time.

| Version | Description | Rows |
|---------|-------------|------|
| v1 | Base dataset (Alice, Bob, Charlie) | 3 |
| v2 | Adds row `GF1` | 4 |
| v3 | Adds row `GF2` | 5 |

---

## How It Works

```
 mycode.py ──► data/sample_data.csv
                     │
                dvc add data/
                     │
        ┌────────────┴────────────┐
        ▼                         ▼
   data.dvc (hash)         DVC cache (.dvc/cache)
        │                         │
   git commit/push           dvc push
        ▼                         ▼
   GitHub repo              DVC remote (S3/ folder)
```

1. `dvc add` hashes the data and writes the hash to `data.dvc`.
2. Git commits `data.dvc`, a tiny text file.
3. `dvc push` uploads the real data to the remote.
4. Later, `git checkout <commit>` + `dvc pull` restores the matching data.

---

## Command Cheat Sheet

### DVC

| Command | Purpose |
|---------|---------|
| `dvc init` | Initialize DVC in a Git repo |
| `dvc remote add -d <name> <path>` | Add a default remote |
| `dvc remote list` | List configured remotes |
| `dvc add <path>` | Track or update data; creates `.dvc` file |
| `dvc status` | Compare workspace with `.dvc` files |
| `dvc status -c` | Compare local cache with remote |
| `dvc commit` | Save workspace changes to the DVC cache |
| `dvc push` | Upload data to remote |
| `dvc pull` | Download data from remote |
| `dvc fetch` | Download to cache only |
| `dvc checkout` | Sync workspace with the current `.dvc` file |
| `dvc diff [a_rev] [b_rev]` | Show data differences between versions |
| `dvc gc -w` | Remove unused cache files |

### Git

| Command | Purpose |
|---------|---------|
| `git init` | Initialize a repository |
| `git status` | Show working tree status |
| `git add <files>` | Stage files |
| `git commit -m "<msg>"` | Commit changes |
| `git push origin main` | Push commits |
| `git pull` | Fetch and merge remote changes |
| `git log --oneline` | Compact commit history |
| `git checkout <commit>` | Switch to an older version |
| `git checkout main` | Return to latest version |
| `git rm -r --cached data` | Stop Git tracking a folder (keeps files) |
| `git tag v1 <commit>` | Label a version |

---