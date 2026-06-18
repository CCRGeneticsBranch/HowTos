# AI Tools and Best Practices for Researchers

A training series for the **CCR Genetics Branch** on using AI in a reproducible and responsible way — specifically, using **GitHub Copilot inside VS Code** to analyze genomic datasets and support day-to-day research tasks.

The series is open to all Genetics Branch members. Sessions build on each other: we start on your local machine and work up to running Copilot against larger datasets on Biowulf.

!!! info "Session 1 — Tuesday, June 23, 2026"

    **In person**, with a virtual WebEx option for anyone who can't join on-site.

    *Bringing AI into your workspace: VS Code + GitHub Copilot on your local machine.*

## Session 1 overview

For most people, "AI" means a web chatbot. Those are useful, but analyzing real data means copying files and code in and out of a browser — which loses context, raises data-security concerns, and breaks reproducibility.

This session flips that around: instead of bringing your data to the AI, you bring the AI to your workspace. Copilot runs inside VS Code, on the file system where your data already lives. It reads your files, writes scripts, and runs them in place — nothing gets uploaded anywhere.

We'll get your environment set up end to end and finish with a hands-on exercise. By the end you'll have a working Copilot setup and a sense of where it helps and where it doesn't.

**What we'll cover**

- **Safety & data security** — keeping your files safe and your analyses reproducible
- **Set up VS Code** on your local machine
- **Connect GitHub Copilot** — link your GitHub account and sign in inside VS Code
- **Install the Copilot Chat extension**
- **Understand models** — which are available and how requests/tokens are billed
- **Hands-on** — prompt Copilot to write and run an R script that builds a volcano plot from a DESeq2 results table

!!! warning "The guiding principle"

    AI is a fast assistant, not an oracle — a "stochastic parrot" that predicts plausible code, it doesn't understand your biology. **Never** expose PHI/PII or blindly accept generated code; **always** review commands and outputs before you run them.

## Session materials

**Session 1 slides**

[View slides](AI_Training_Session1_v2.pdf){ .md-button .md-button--primary target="_blank" }
[Download (PDF)](AI_Training_Session1_v2.pdf){ .md-button download="AI_Training_Session1_v2.pdf" }

**VS Code setup guide**

[View guide](VS_Code_Setup_Guide.pdf){ .md-button .md-button--primary target="_blank" }
[Download (PDF)](VS_Code_Setup_Guide.pdf){ .md-button download="VS_Code_Setup_Guide.pdf" }

**Test dataset**

[Download (XLSX)](test_dataset.xlsx){ .md-button download="test_dataset.xlsx" }



## Coming up

**Session 2 — Biowulf and git.** GitHub for version control and collaboration, custom instructions and skills to steer AI, and running VS Code on Biowulf to reach larger datasets and more compute.

---

*Questions? Reach out to Erica Pehrsson or Vineela Gangalapudi*
