
Ah, good one! A **stacked diff** (or **stacked PR**) is a way of organizing **multiple related pull requests (PRs)** where **each PR builds on top of the previous one**, like a stack.

---

### 🧱 What Is a Stacked Diff / PR?

Instead of putting all your changes in one big PR, you break them into **smaller, focused PRs**, where each one:

- Depends on the previous PR,
    
- Introduces one logical unit of work.
    

Think of it like chapters in a book — each one builds on the last.

---

### 📦 Real Example

Let’s say you're adding a new feature:

#### Without stacked PRs:

- One giant PR with: UI, API changes, DB changes — messy to review.
    

#### With stacked PRs:

1. PR 1: Add DB schema
    
2. PR 2: Add backend API
    
3. PR 3: Add frontend call to API
    
4. PR 4: Add UI component
    

Each PR:

- Is easier to review,
    
- Can be tested/merged independently (ideally),
    
- Keeps the history clean.
    

---

### 🔄 Workflow (usually with Git)

```bash
# PR 1: base branch (main)
git checkout -b feat/db-schema

# PR 2: based on previous PR
git checkout -b feat/backend-api feat/db-schema

# PR 3: based on backend
git checkout -b feat/frontend feat/backend-api
```

You open a PR for each, targeting the branch before it.

---

### 🧠 Tools That Help

- **GitHub:** supports stacked PRs, just make sure base branches are correct.
    
- **Graphite.dev**, **ghstack**, **Phabricator**: tools that help manage stacked diffs visually.
    

---

### ✅ TL;DR

> A **stacked diff** is a series of small pull requests where each builds on the last.  
> It makes code easier to review, test, and merge in steps — especially for big features.

---

Let me know if you want a visual or a Git cheat sheet for managing stacked PRs!

https://medium.com/@alexanderjukes/stacked-diffs-vs-trunk-based-development-f15c6c601f4b

https://newsletter.pragmaticengineer.com/p/stacked-diffs

https://newsletter.pragmaticengineer.com/p/stacked-diffs-and-tooling-at-meta

