# Collaborative Git Workflows: Part 1

This lab is designed for a team of five members to practice **GitHub Flow** while understanding alternative Git workflows. You will focus on **team governance**, **branch protection**, and **explicit merging** without fast-forwarding to maintain a clear audit trail.

---

## 1. Overview of Git Workflows

Before starting, review these three primary workflows used in the industry:

* **GitHub Flow (Our Focus):** Ideal for agile teams and web applications. It relies on a single `main` branch that must always be in a deployable state. Feature branches are used for all changes.
* **Git Flow:** A more structured approach with multiple long-lived branches (`main`, `dev`, `release`, `feature`, and `bug`). It is ideal for installable applications (like mobile apps) where multiple versions run simultaneously.
* **Trunk-Based Development (TBD):** The current industry standard (2026) for large-scale SaaS. It uses direct commits to `main` or very short-lived feature branches (~4 hours) and relies on "feature flags" and bulletproof automated testing.

All team members should clone the repository to their local machines:

**Clone the Repository:** `git clone <repository-url> <destination-folder>`

---

## 2. Branch Protection and Repository Setup (Member 1)

**Member 1** will act as the "team lead" for this lab.

1. **Confirm that your Team Members are Collaborators:** Go to: **Settings > Collaborators**. You should see all five members listed with "Write" access or "Admin" access.

2. **Configure Branch Protection:**
    * Go to **Settings > Branches > Add branch ruleset**.
    * Create a rule named `pr-and-1-approval-required-to-merge`.
    * Set the enforcement status to **Active**.
    * Include the default branch as the target.
    * **Enable:** "Require a pull request before merging" and set "Required approvals" to **1**.
    * This ensures that no team member can bypass the review process. At least 1 other team member must agree with the changes before they can be merged into `main`.
    * Got to **Settings > Rulesets** and confirm that the new ruleset is active and correctly configured.

---

## 3. Create a Project (Member 2)

1. Go to the **Projects** tab in the repository and click on **New Project**.
2. Select the **Iterative development** template and name it "**[GROUP NAME] Business Intelligence Labs**".
3. Ensure that your team members are added to the project as collaborators with **Admin** rights. This can be done in the project **Settings** > **Manage access**.
4. Create iterations and specify the start and end dates for each iteration. This is available under the project **Settings** > **Iterations**.

---

## 4. Create Milestones (Member 3)

1. Go to the **Issues** tab and click on **Milestones**.
2. Create 3 milestones and:
   * Assign appropriate due dates for each to align with the iterations you set up in the project.
   * Add an appropriate description for each milestone to clarify the goals and expectations
     * **50% Complete Milestone**
     * **75% Complete Milestone**
     * **100% Complete Milestone**

---

## 5. Creating the Task Backlog (All Members)

Have a discussion as a team to determine the specific technical tasks that need to be completed for the lab. Each task should be clearly defined and assigned to a specific team member. This is similar to identifying functional and non-functional requirements from the potential end users of a system. Once you have identified the tasks, follow these steps:

1. Each member must create a "GitHub Issue" to represent a specific technical task.
2. Each issue should be assigned to the respective member of the team.
3. Each issue should be assigned the label "**enhancement**".
4. Each issue should be assigned the type "**Feature**".
5. Each issue should be assigned to the "**[GROUP NAME] Business Intelligence Labs**" project.

Example issues for the lab:

* **Member 1:** Issue #1 - 50% Complete Milestone - Title: feature/lab-1/update-project-README. Description: Update the project's README with team roles.
* **Member 2:** Issue #2 - 50% Complete Milestone - Title: feature/lab-1/research-on-data-sources. Description: Create a `data_source.md` file reviewing sources of data in a business.
* **Member 3:** Issue #3 - 50% Complete Milestone - Title: feature/lab-1/research-on-star-schema. Description: Create a `warehouse_schema.md` describing a Star Schema.
* **Member 4:** Issue #4 - 75% Complete Milestone - Title: feature/lab-1/research-on-ETL-ELT-EtLT. Description: Create a `data_pipeline.md` differentiating between ETL, ELT, and EtLT in the context of compliance with the legal requirements in an industry.
* **Member 5:** Issue #5 - 100% Complete Milestone - Title: feature/lab-1/research-on-data-governance. Description: Add a `governance.md` file reviewing data governance and access to PII.

---

## 6. Assigning Issues to Iterations and Managing the Status of Issues

1. Go to the **Projects** tab and open the "**[GROUP NAME] Business Intelligence Labs**" project. Navigate to the **My items** view where you can see all the issues in the backlog.
2. For each issue, click on the issue title and assign it to the appropriate iteration based on its milestone. Example:
   * Issues #1, #2, and #3 are planned for Iteration 1, which corresponds to the **50% Complete Milestone**.
   * Issue #4 is planned for Iteration 2, which corresponds to the **75% Complete Milestone**.
   * Issue #5 is planned for Iteration 3, which corresponds to the **100% Complete Milestone**.

You should see the issue under the **"current iteration"** tab of the project listed in the Backlog.

Issues can move across 5 states by default: Backlog, Ready, In Progress, In Review, and Done.

* **Backlog**: The issue is created but not yet started.
* **Ready**: The issue is ready to be worked on but has not been started yet.
* **In Progress**: The issue is currently being worked on.
* **In Review**: The issue has been worked on and is being reviewed by others, i.e., there is a code review before the Pull Request is approved and merged.
* **Done**: The issue has been completed, approved, and merged into the main branch through the Pull Request process.

The team members should have daily brief meetings to discuss the status of their assigned issues and update the status accordingly in the project board. This helps in tracking the progress of each task and ensures that everyone is on the same page regarding the project's timeline and deliverables.

Such meetings are similar to "stand-up meetings" in agile development, where team members quickly share updates on their work, any blockers they are facing, and their plans for the day. This promotes transparency and collaboration within the team. It helps you to avoid surprises a few hours to the deadline.

---

## 7. The GitHub Flow Cycle

Each member must follow these steps for their respective issue. **Member 1** will follow these steps last.

### Step A: Branching

On your local machine, create and switch to a new branch for your task.

*Note: The branch name should follow the format `feature/lab-number/description` to maintain clarity and consistency across the team. For example, if you are working on an Issue in Lab 3, then your branch name could be `feature/lab-3/data-source`. This naming convention helps everyone in the team quickly understand the purpose of the branch and its connection to the corresponding lab.*

```bash
git checkout -b feature/lab-number/description
```

*Note: A branch is an independent line of development used for team governance*.

### Step B: Working and Committing

Create your assigned file and confirm that there is a modification ready to be committed.

```bash
git status
```

You can configure the default commit message editor to be **VS Code** for better formatting of the commit message body. Run the following command:

```bash
git config --global core.editor "code --wait"
```

Once ready, stage and commit it:

```bash
git add .
git commit
```

**Important:** What constitutes an **academically sound** commit message?

A commit message is a permanent, public record of **why** a change was made — not merely what changed (the diff already shows that). In an academic context, it serves the same function as a lab notebook entry: it must be comprehensible to a reader who has no prior context and who may be reviewing the work weeks later.

Your `git commit` command (without the `-m` flag) will open a text editor. Structure your message as follows:

```text
<Verb in imperative mood> <concise description of the change> (≤72 characters)

<Blank line — mandatory>

Why this change was made:
<One to three sentences explaining the academic or technical motivation.
What problem does this address?
What would happen without this change?>

Related issue: #<issue-number>
```

Why the **imperative mood**? Git itself uses it — "Merge branch", "Revert commit", "Add file". Reading your message alongside Git's own messages should feel consistent. Write "Add feature" rather than "*Added* feature" or "*Adding* feature".

Example:

```text
Add data_source.md listing primary BI source types

This file documents the four primary data source categories used
in Business Intelligence architectures: transactional databases,
flat files, APIs, and streaming sources from various departments.

It is important to understand these categories as they inform the
design of data pipelines and the choice of tools for extraction
and transformation.

Related issue: #2
```

### Step C: Pushing and Pull Request

Push your branch to GitHub:

```bash
git push origin feature/lab-number/description
```

1. Go to GitHub (the website) and open a **Pull Request (PR)** from your branch to `main`. There should be a green button prompting you to "Compare & pull request" after pushing. If not, then you can navigate to the "**Pull requests**" tab and click on "New pull request" to select your branch and create the PR.
2. Name the PR appropriately, e.g., "*Merge feature/lab-1/update-project-readme into main*" and add a detailed description of the changes made. This description should provide context for the reviewer, explaining the motivation behind the changes and any relevant details that would help them understand the purpose of the PR.
3. Link the PR to the corresponding issue by including `#issue-number` in the PR description. This creates a connection between the changes and the issue it addresses. A common way to include the issue number is to use the text "`Closes #issue-number`" in the PR description, e.g., "`Closes #2`". This not only links the PR to the issue but also automatically closes the issue when the PR is merged.
4. Assign a teammate to perform a **Code Review**. If this was your research/project, then your research supervisor would be the assigned reviewer. The author of the PR should not merge their own PR. This is a critical aspect of team governance and ensures that all changes are reviewed by at least one other team member before being integrated into the main branch.
5. The assignees can be anyone who contributed to the commits in the branch, the label can be "**enhancement**" for a new feature, the project should be the **[GROUP NAME] Business Intelligence Labs**, and the milestone should correspond to the one assigned to the issue that the feature branch addresses.

### Step D: The Code Review

* **The Reviewer:** Check the **Files changed** for clarity and accuracy based on the source material. Approve the PR if it meets requirements. If not, request changes and provide specific feedback in the comments.
* **The Author:** Address any feedback provided during the review. If it is approved, then the author can proceed to merge the PR. If changes are requested, make the necessary updates and push them to the same branch. The PR will automatically update with the new commits, and the reviewer can re-review until it is approved.

### Step E: Merging **WITHOUT** Fast-Forward (`--no-ff`)

Once approved, the merge must be performed. To ensure the process is visible for audit purposes, we will avoid "Fast-Forward" merging. Click the green "Merge pull request" on the GitHub web interface, then select "Create a merge commit" to ensure that the merge is explicitly recorded in the history. This should achieve the same result as running the following command in the terminal:

```bash
git merge --no-ff feature/lab-number/description
```

After successfully merging the pull request in the web browser, team members should pull the latest changes to their local `main` branch to stay up to date:

```bash
git checkout main
git pull origin main
```

**Why `--no-ff`?** This option always creates a "merge commit," documenting the integration as a deliberate event in history. This is ideal in academic contexts so that research supervisors can see the branch evolution and your collaborative process. It also enables the lecturer to see the history of changes in a more structured way, which is beneficial for grading and feedback.

---

## 8. Handling Merge Conflicts (Member 4 and 5)

A merge conflict occurs when two branches have each made different changes to the **same lines** in **the same file**. Git cannot decide which version is correct, so it pauses and asks you to resolve it manually.

This section engineers a guaranteed conflict so that every team member experiences the resolution process.

**Setup** — creating the conflict (Members 4 and 5, working simultaneously).

Both members must edit the same line in README.md before either has merged.
Member 4 adds the following line to README.md on their feature branch and commits it:

```bash
echo "Project lead: Member 4 — responsible for overall coordination." >> README.md
git add README.md
git commit -m "Add project lead attribution to README"
```

Member 5 adds a different line at the exact same position on their own feature branch and commits it:

```bash
echo "Project lead: Member 5 — responsible for governance and audit." >> README.md
git add README.md
git commit -m "Add governance author attribution to README"
```

Member 4 merges first through the normal PR process. By the time Member 5 attempts to merge, main has moved forward and the two branches have diverged on the same line.

### Resolution process (Member 5)

#### Option 1

**IMPORTANT:** The standard practice **before opening any PR** is to update your feature branch with the latest changes from main. This is one of the places where conflicts can be detected.

Before opening a PR to merge `feature/lab-number/description` into `main`, Member 5 updates their local feature branch with the latest state of main:

```bash
git checkout feature/lab-number/description
git fetch origin
git merge origin/main
```

Git will halt and report a conflict. The terminal output will resemble:

```text
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts then commit the result.
```

Open the file with the merge conflict (README.md in this case) in your IDE (VS Code). Git marks the conflict zone as follows:

```text
<<<<<<< HEAD
Project lead: Member 5 — responsible for governance and audit.
=======
Project lead: Member 4 — responsible for overall coordination.
>>>>>>> origin/main
```

The section between **<<<<<<< HEAD** and **=======** is your version

The section between **=======** and **>>>>>>> origin/main** is the version already in main.

---

#### Option 2

Member 5 can proceed to push the changes to their branch and then open the PR without updating their local branch with the changes in main.

To open a PR without updating the branch with changes from main:

```bash
git push origin feature/lab-number/description
```

When they attempt to merge, GitHub will detect the conflict and prevent the merge until the conflict is resolved. The process of resolving the conflict in the web interface in the origin (on [GitHub.com](https://github.com)) is similar to the local resolution process described below, but it may be more cumbersome for complex conflicts with multiple merge conflicts at a time.

**The 'GitHub Flow' recommends not having your branches opened for too long because it increases the chances of you changing the same lines as other team members, which leads to conflicts.**

---

Member 5 must decide what the final file should say. In this case, both attributions are valid — combine them as follows:

```text
Project lead: Member 4 (coordination) and Member 5 (governance and audit).
```

Delete all three conflict markers (<<<<<<<, =======, >>>>>>>) and save the file.

**If you were using Option 1**, stage the resolved file and complete the merge:

```bash
git add README.md
git commit -m "Resolve merge conflict: consolidate dual attribution in README"
```

**If you were using Option 2**, click the "Resolve conflicts" button in the GitHub web interface, edit the file to resolve the conflict as described above, and then commit the resolution directly in the web interface.

---

Member 5's branch now contains a clean merge commit. Proceed to push and open your PR as normal.

---

**Key principle:** A conflict is not an error — it is Git asking a human to make a decision that a machine cannot. The discipline lies in reading both versions carefully before choosing, not in simply accepting one side and discarding the other.

The quality of the resolution matters as much as the resolution itself. In a professional codebase, a careless conflict resolution that silently discards one team member's valid change is far more dangerous than the conflict itself, precisely because it leaves no trace.

The actual discipline is to understand both sides of the conflict well enough to make an informed editorial decision.

---

## 9. Summary of Merging Techniques

While we used **Git Merge (`--no-ff`)**, be aware of these alternatives:

* **Git Rebase:** Rewrites history by moving the base of your branch to the tip of `main`. It creates a clean, linear history but reduces traceability.
* **Squash Commits:** Combines all commits from a feature branch into one single commit on `main`. This is useful for cleaning up "messy" intermediate commits but discards individual commit details.

---

## 10. Auditing Repository History (`git log`)

**All Members** should practice viewing the "technical lab notes" of the project to ensure the process is transparent.

* Go back to the main branch: `git checkout main`
* Fetch the latest changes: `git pull origin main`
* Confirm that the remote branch is up to date with the branch in the origin by running `git status`
* Then run `git log --oneline --graph` to view the repository history. You should see a series of "knots" representing the deliberate merge commits made by each team member.

* **View a Graph of Merges:** To see how branches have evolved and joined, run:

    ```bash
    git log --merges
    ```

    *Note: This provides a quick overview of the branch evolution and is ideal for supervisors to see your collaborative process.*

* **Inspect Recent Changes:** To see the full details of the most recent commit, including the reasoning in the commit body, use:

    ```bash
    git show HEAD
    ```

    *Note: The **HEAD** pointer identifies your current location in the repository's history.*
