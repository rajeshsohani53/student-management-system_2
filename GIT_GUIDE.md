# Git Guide for Beginners — How to Push Code

This guide is for Rajesh and Prajakta. Follow these steps every time you write code.

---

## One-Time Setup (Do This Only Once)

### Step 1 — Install Git
Download and install Git from https://git-scm.com/downloads  
After installing, open a terminal (Command Prompt / Git Bash on Windows).

### Step 2 — Configure Git with your name and email
```bash
git config --global user.name "Your Name"
git config --global user.email "your-email@gmail.com"
```
> Rajesh: use `Rajesh Sohani` and `hagosohani53@gmail.com`  
> Prajakta: use `Prajakta Takbhate` and your GitHub email

### Step 3 — Clone the repo (download it to your computer)
```bash
git clone https://github.com/rajeshsohani53/student-management-system_2.git
```
This creates a folder called `student-management-system_2` on your computer.

### Step 4 — Go into the project folder
```bash
cd student-management-system_2
```

---

## Every Day Workflow (Do This Every Time You Code)

### Step 1 — Get the latest code from GitHub
Always do this before starting work so you have the latest version.
```bash
git pull origin main
```

### Step 2 — Create your own branch
Never code directly on `main`. Always create a branch for your work.
```bash
git checkout -b feature/your-feature-name
```
**Examples:**
```bash
git checkout -b feature/student-registration
git checkout -b feature/course-creation
git checkout -b feature/seat-allocation
```

### Step 3 — Write your code
Open the project in your IDE (Eclipse / IntelliJ) and write your code normally.

### Step 4 — Check what files you changed
```bash
git status
```
This shows you which files were added or modified.

### Step 5 — Stage your changes (select files to save)
```bash
git add .
```
This stages all changed files. Or add a specific file:
```bash
git add src/StudentServlet.java
```

### Step 6 — Commit your changes (save a snapshot)
```bash
git commit -m "Add student registration form"
```
> Write a short message describing what you did. Be specific.

**Good commit messages:**
- `Add student registration servlet`
- `Fix null pointer in course DAO`
- `Create JSP page for seat allocation`

**Bad commit messages:**
- `changes`
- `fix`
- `update`

### Step 7 — Push your branch to GitHub
```bash
git push origin feature/your-feature-name
```
Example:
```bash
git push origin feature/student-registration
```

### Step 8 — Open a Pull Request (PR) on GitHub
1. Go to https://github.com/rajeshsohani53/student-management-system_2
2. You'll see a yellow banner saying **"Compare & pull request"** — click it
3. Add a title and short description of what you built
4. Click **"Create pull request"**
5. The other person will review and approve it
6. Once approved, click **"Merge pull request"**

---

## Quick Reference — Commands Cheat Sheet

| What you want to do | Command |
|---|---|
| Get latest code | `git pull origin main` |
| Create a new branch | `git checkout -b feature/name` |
| See changed files | `git status` |
| Stage all changes | `git add .` |
| Save a snapshot | `git commit -m "your message"` |
| Push to GitHub | `git push origin feature/name` |
| Switch to an existing branch | `git checkout branch-name` |
| See all branches | `git branch` |

---

## Golden Rules

1. **Never push directly to `main`** — always use a branch + PR
2. **Always `git pull` before starting work** — avoids conflicts
3. **Commit often** — small commits are easier to review and fix
4. **Write clear commit messages** — your teammate needs to understand what you did
5. **If you get an error you don't understand — message the other person immediately**

---

## Common Errors and Fixes

**Error:** `failed to push — updates were rejected`  
**Fix:** Run `git pull origin main` first, then push again

**Error:** `merge conflict`  
**Fix:** Open the conflicting file, look for `<<<<<<`, `======`, `>>>>>>` markers, fix the code manually, then `git add .` and `git commit`

**Error:** `not a git repository`  
**Fix:** You're not inside the project folder. Run `cd student-management-system_2` first

---

For any issue, message Rajesh on WhatsApp immediately. Don't struggle alone for more than 15 minutes.
