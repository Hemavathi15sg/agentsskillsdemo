Below is a **simple demo example using an existing skill** from the `github/awesome-copilot` skills collection, and then we’ll **create our own custom skill for the same demo.**
*(Agent Skills are folders of instructions that teach GitHub Copilot how to perform specialized coding tasks automatically)([GitHub Docs][1])*

---

## 📌 **1. Simple Example Using an Existing Skill**

Let’s pick the **GitHub Issues Skill** (this is a real skill inside the repo) — this teaches Copilot how to create and manage GitHub Issues.([GitHub][2])

### 📁 Folder Structure

```
.github/skills/github-issues/
└── SKILL.md
```

### ⭐ Typical Prompt You Can Use

When this Skill is in your repository and loaded by Copilot, you can ask:

```
Create a GitHub issue for this bug:
The login API returns 500 when the password is blank.
```

### ✔ What Copilot Will Do

Instead of just suggesting code, Copilot *uses the skill instructions* to produce a full issue description such as:

```
Title: Login API returns 500 when password is blank
Body:
Steps to Reproduce:
1. Call login API with empty password
2. Observe server responds with HTTP 500

Expected:
API should return validation error 400 with message “Password required”.

Actual:
Server crashes with 500 response.

Environment:
Node.js v18, Express 4.18
```

This makes issue creation faster and consistent. (This behavior matches how Skills enhance Copilot responses)([GitHub][2])

---

## 🛠️ **2. Create Our Own Skill for the Same Example**

Now let’s make a **custom Agent Skill** that teaches Copilot how to draft a GitHub issue following our own template.

### 📂 Step-by-Step: Create the Skill Files

**1. Create a new folder in your repo:**

```
.github/skills/create-github-issue/
```

**2. Inside it, create a file named:**

```
SKILL.md
```

**3. Put the following content inside `SKILL.md`:**

> This text teaches the Copilot agent how to format GitHub issues.

```md
# Create GitHub Issue Skill

When asked to create a GitHub issue, follow this template:

Title: <one–sentence summary of the problem>

Body:
**Description:**
<Explain what the bug or task is>

**Steps to Reproduce:**
1. <step one>
2. <step two>
...

**Expected Behavior:**
<What should have happened?>

**Actual Behavior:**
<What actually happened?>

**Environment:**
<OS, language, framework versions, tools etc.>

Use clear, descriptive language and include any relevant code snippets or logs.
```

---

## 🤖 **How to Use Your Skill (Demo Prompt)**

Once this installed skill is in `.github/skills/create-github-issue`, open Copilot agent or Copilot CLI and ask:

```
Create a GitHub issue for this problem:
The search API returns incorrect results for query “AI”.
```

Copilot will apply your Skill instructions and output something like:

```
Title: Search API returns incorrect results for query “AI”

Body:
**Description:**
The search API returns unrelated items when the query is “AI”. Expected only entries containing AI keywords.

**Steps to Reproduce:**
1. Call GET /search?q=AI
2. Observe returned items

**Expected Behavior:**
Results should be relevant to “AI”.

**Actual Behavior:**
Results include unrelated entries.

**Environment:**
Python Flask API v1.2, Postman
```

This shows Copilot *automatically following the Skill template* you created. ([GitHub Docs][1])

---

## 🧠 What Just Happened?

* Skills teach Copilot *how* to respond to your prompts (not just *what* code to write). ([NathanNellans.com][3])
* The example shows how to automate *GitHub issue creation* with a consistent structure.

---

If you want, **next we can extend this Skill** to include auto-labeling, issue templates for enhancements, or integrate with GitHub Actions!

[1]: https://docs.github.com/copilot/concepts/agents/about-agent-skills?utm_source=chatgpt.com "About Agent Skills"
[2]: https://github.com/github/awesome-copilot/blob/main/skills/github-issues/SKILL.md?utm_source=chatgpt.com "awesome-copilot/skills/github-issues/SKILL.md at main"
[3]: https://www.nathannellans.com/post/all-about-github-copilot-custom-instructions?utm_source=chatgpt.com "All About GitHub Copilot Custom Instructions - Nathan Nellans"
