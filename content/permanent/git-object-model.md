---
title: "Git Object Model"
aliases: ["git internals", "git objects", "git plumbing"]
tags: [Git, Internals, VCS]
created: 2026-04-10
updated: 2026-04-10
---

Git stores everything as **objects** in `.git/objects/`. Every object — whether a file snapshot, directory listing, or commit — is content-addressed by its SHA-1 hash (40 hex characters). The first 2 characters become the subdirectory name; the remaining 38 are the filename. This is why, as long as `.git/objects/` is intact, you haven't truly lost anything you committed.

**Three object types that build on each other**:

- **blob** — stores raw file content. One blob per file version.
- **tree** — stores a directory listing: maps filenames to blob hashes (and nested tree hashes for subdirectories). One tree per directory snapshot.
- **commit** — stores metadata (author, timestamp, message) plus a pointer to the root tree. Each commit also points to its parent commit(s), forming the DAG.

To inspect any object: `git cat-file -p <hash>`.

**How `refs/` and `logs/` fit in**: Objects alone are just a DAG of content — you'd need to know a commit hash to start navigating. `refs/` solves this by storing the latest commit hash per branch (e.g., `.git/refs/heads/main` is just a file containing a 40-char hash). `logs/` goes further: it stores the full history of every branch pointer move, so even after `git reset --hard` you can find the old commit hash and recover.

**Repository recovery pattern**: If `.git` is corrupted but `objects/` is intact, the recovery path is:

1. `git clone` a fresh copy from remote (or create a fresh `git init`)
2. Copy the `objects/` directory from the corrupted repo into the fresh clone's `.git/objects/`
3. Find the commit hash you want via `refs/` or `logs/`
4. `git checkout <commit-hash>` to restore working tree

The insight: commits are immutable and content-addressed, so "corruption" almost always means the index or ref pointers are broken — not that the data is gone. As long as you committed, you have the objects.

> **The golden rule**: never delete `.git/`. Everything recoverable lives there.

## Connections

- [[permanent/devops]] — Git is the foundation of source control in any DevOps pipeline
- [[permanent/cicd]] — CI/CD systems trigger on git events; understanding refs helps debug webhook and pipeline issues

## Sources

- [[journal/how_to_recover_corrupted_git_repository]] — Original post on recovering after a power outage (2018)
