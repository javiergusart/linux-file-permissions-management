# Managing Linux File Permissions

## Table of Contents
1. [Overview](#overview)
2. [Scenario](#scenario)
3. [Step 1: Check Current Permissions](#step-1-check-current-permissions)
4. [Step 2: Understand Permission Strings](#step-2-understand-permission-strings)
5. [Step 3: Remove Write Permission for Others](#step-3-remove-write-permission-for-others)
6. [Step 4: Secure a Hidden File](#step-4-secure-a-hidden-file)
7. [Step 5: Restrict Directory Access](#step-5-restrict-directory-access)
8. [Summary](#summary)

---

## Overview

This report was developed as part of the *Google Cybersecurity Professional Certificate* program. The objective is to demonstrate how to manage file and directory permissions in a Linux environment to maintain proper access control and system security.

In this scenario, I acted as a security professional supporting a research team within a large organization. I was tasked with auditing existing permissions, identifying misconfigurations, and applying the necessary changes to restrict unauthorized access and authorize appropriate users.

This activity outlines the commands used, the rationale behind each action, and the resulting permission updates to ensure compliance with security best practices.

---

## Scenario

My organization tasked me with reviewing the current permissions for files and directories used by the research team. I needed to verify that the configured permissions matched the required authorization levels and adjust them if they didn’t. The goal was to ensure that only authorized users had the appropriate level of access and to remove any unnecessary permissions that could pose a security risk.

---

## Step 1: Check Current Permissions

I started by reviewing the current permissions using the `ls -la` command. This allowed me to view detailed information about all files and directories, including hidden files.

```bash
ls -la ~/projects/
```

Example output:

```
drwxr-xr-x 2 researcher2 team 4096 May 11 10:00 drafts
-rw-rw-r-- 1 researcher1 team  512 May 11 10:00 project_t.txt
-rw-rw-rw- 1 researcher1 team 1024 May 11 10:00 project_k.txt
-rw-r--r-- 1 researcher1 team  256 May 11 10:00 .project_x.txt
```

---

## Step 2: Understand Permission Strings

To make informed decisions, I analyzed the permission strings displayed in the output. Each permission string is composed of 10 characters:

```
-rw-rw-r--
```

| Position  | Description                             |
|-----------|-----------------------------------------|
| 1st       | File type: `-` (file) or `d` (directory) |
| 2nd-4th   | User permissions: `r` (read), `w` (write), `x` (execute) |
| 5th-7th   | Group permissions                       |
| 8th-10th  | Others permissions                      |

For example, `-rw-rw-r--` indicates:
- This is a regular file.
- The user and group have read and write permissions.
- Others have read-only access.

---

## Step 3: Remove Write Permission for Others

While reviewing permissions, I found that `project_k.txt` allowed write access for all users. This was a clear security risk. I removed write permissions for others using the following command:

```bash
chmod o-w ~/projects/project_k.txt
```

I verified that the permissions were updated correctly:

```bash
ls -l ~/projects/project_k.txt
```

---

## Step 4: Secure a Hidden File

The hidden file `.project_x.txt` had been archived and should no longer be modified. However, I needed to ensure that the user and group could still read the file. I applied the following changes:

```bash
chmod u-w ~/projects/.project_x.txt
chmod g-w ~/projects/.project_x.txt
chmod g+r ~/projects/.project_x.txt
```

I confirmed the changes:

```bash
ls -l ~/projects/.project_x.txt
```

---

## Step 5: Restrict Directory Access

Next, I reviewed the `drafts` directory. Only the `researcher2` user should have access to it. I removed execute permissions for both the group and others:

```bash
chmod g-x ~/projects/drafts/
chmod o-x ~/projects/drafts/
```

I verified that only `researcher2` retained access:

```bash
ls -ld ~/projects/drafts/
```

Expected output:

```
drwx------ 2 researcher2 team 4096 May 11 10:00 drafts
```

---

## Summary

In this permissions audit, I ensured that file and directory access within the research team’s environment adhered to our security policies by:

- Reviewing permissions using `ls -la`.
- Interpreting permission strings to understand current access levels.
- Removing unnecessary write permissions for unauthorized users.
- Securing archived hidden files against modification.
- Restricting directory access to authorized users only.

These changes help protect sensitive data and reinforce a secure file management environment. I will continue to perform regular audits to maintain these standards.
