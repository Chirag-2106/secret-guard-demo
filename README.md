# Secret Guard: Test Your Own Pre-commit Hook

**Hands-on Security Lab | Python | Git | GitHub | Gitleaks**

Welcome, team!

Today, you will reproduce a real-world security practice: preventing hard-coded API keys from entering a Git repository.

You will configure a pre-commit hook, deliberately introduce a fake API key, observe the hook block the commit, fix the code, and submit a pull request.

## Learning Objectives

By completing this exercise, you will learn to:

- Understand why hard-coded API keys are dangerous.
- Install and configure a Git pre-commit hook.
- Use Gitleaks to detect potential secrets.
- Fix a security issue by reading the API key from an environment variable.
- Use feature branches, GitHub forks, and pull requests.

## Step 1: Fork and Clone the Repository

Repository: https://github.com/appars/secret-guard-demo

1. Open the repository and click **Fork**.
2. Create a fork under your GitHub account.
3. Open your terminal or PowerShell and clone your fork.

Replace `YOUR_USERNAME` with your GitHub username.

```powershell
git clone https://github.com/YOUR_USERNAME/secret-guard-demo.git
cd secret-guard-demo
```

## Step 2: Create Your Branch

```powershell
git checkout -b secure_bot
```

You will perform your experiment on this branch instead of working directly on `main`.

## Step 3: Install and Configure Pre-commit

Ensure Python and Git are installed.

```powershell
py -m pip install pre-commit
pre-commit install
```

The repository configuration uses Gitleaks to scan staged files for potential secrets.

The hook runs automatically when you attempt to commit changes.

## Step 4: Introduce a Fake API Key

Open `groq_buddy.py` in your editor.

Find the API key configuration and temporarily use this synthetic test value:

```python
GROQ_API_KEY = "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"
```

**Important:** This is a demonstration value, not a real API key. Do not replace it with your actual Groq API key.

Save the file.

## Step 5: Verify That the Hook Blocks the Commit

Run:

```powershell
git add groq_buddy.py
git commit -m "test hook"
```

**Expected result:** Gitleaks should detect the test value and reject the commit if it matches the configured detection rules.

If the hook blocks the commit, capture a screenshot showing the failure.

If it passes unexpectedly, do not assume detection works. Check the hook output and ask for assistance.

## Step 6: Fix the Security Issue

Do not leave the API key hard-coded. Update `groq_buddy.py` to read the key from an environment variable.

For example:

```python
import os

GROQ_API_KEY = os.environ.get("GROQ_API_KEY")
```

If the file already imports `os`, do not add the import a second time.

Save the file and stage the corrected version:

```powershell
git add groq_buddy.py
git commit -m "read API key from environment"
```

**Expected result:** The commit should pass if no other secrets are present in the staged changes.

The application will need the `GROQ_API_KEY` environment variable configured separately to make authenticated API calls. Never commit the actual key.

## Step 7: Push Your Branch

```powershell
git push -u origin secure_bot
```

Open your fork on GitHub and create a pull request:

- **Base repository:** Your fork
- **Base branch:** `main`
- **Compare branch:** `secure_bot`

Submit the pull request with a brief explanation of the security issue and your fix.

## Submission Requirements

Submit the following:

1. Your pull request URL.
2. A screenshot showing Gitleaks blocking the attempted commit.
3. A screenshot or code excerpt showing the corrected environment-variable approach.

## Deadline

Monday, 12 October 2026, at 11:59 PM.

## Reflection Questions

Be prepared to answer these questions in the next class:

1. What is a pre-commit hook?
2. At what point does Gitleaks scan for secrets?
3. Why is detecting a secret before a commit better than discovering it after pushing?
4. Why should API keys be stored in environment variables rather than source code?
5. Does a local pre-commit hook replace GitHub-side security controls?

**Remember:** Security is not just about writing secure code. It is about building security checks into the development workflow.

— Prof. Appar
