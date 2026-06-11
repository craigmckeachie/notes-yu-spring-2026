# Excluding your application.properties file from source control


### Q: Have you already committed your code locally to a git repository or shared it on GitHub (which does a local commit)?

### A: Yes

**1. Run this command**

```bash
git rm --cached src/main/resources/application.properties

```

> **What this does:** This tells Git to stop tracking the file and removes it from the index. The `--cached` flag ensures the file remains intact on your local hard drive so you don't lose your local configurations.

**2. Add the file to your `.gitignore**`**

Open the `.gitignore` file at the root of your project and add the filename on a new line:

```text
application.properties

```

> **Why this is needed:** Running `git rm --cached` only untracks the file for the moment. Without this step, Git will flag it as an "untracked file" during your next `git status`, leaving it exposed to being accidentally restaged. By using just the filename, Git will ignore it at any depth in your project structure.

**3. Stage, commit, and push your changes**

```bash
git add .gitignore
git commit -m "Remove application.properties from tracking and add to gitignore"
git push origin <your-branch-name>

```

> ⚠️ **Security Warning:** Because the file was already committed, its contents are still saved in your **Git commit history**. If it contained sensitive production passwords or API keys, you should rotate those credentials immediately or use a tool like `git filter-repo` to purge it from your history.

---

### A: No

**1. Run this command**

```bash
git rm --cached src/main/resources/application.properties

```

> **What this does:** Even though you haven't committed yet, if IntelliJ automatically added the file to your staging area, this command safely pulls it back out of Git's tracking index without touching your local file.

**2. Add the file to your `.gitignore**`**

Open your `.gitignore` file and add the filename to a new line:

```text
application.properties

```

Because the file never made it into a local commit or onto GitHub, simply stopping Git from tracking it and adding this rule completely resolves the issue. Your very next commit will include the updated `.gitignore`, and the properties file will remain entirely invisible to Git.