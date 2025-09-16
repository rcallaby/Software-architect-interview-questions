# Contributing to Software Architect Interview Questions

Thank you for your interest in contributing to **Software Architect Interview Questions**!  
This repository is a collaborative effort to create a high-quality, community-driven collection of interview questions, answers, and preparation materials for aspiring and practicing software architects.

We welcome your ideas, improvements, and corrections.  
To keep contributions consistent and useful for the community, please follow the guidelines below.

---

## How to Contribute

### 1. Reporting Issues
- Use the [Issues tab](../../issues) to report:
  - Errors, typos, or formatting problems
  - Duplicate or outdated questions
  - Suggestions for new topics or categories
- Before opening a new issue, please search existing ones to avoid duplication.

### 2. Submitting Changes
- Fork the repository.
- Create a feature branch for your contribution:
  ```bash
  git checkout -b feature/add-architecture-patterns
  ```

* Commit changes with clear, concise messages:

  ```
  Add: Questions on event-driven architecture
  Fix: Typo in system design trade-offs section
  ```
* Push to your fork and submit a Pull Request (PR).
* Reference related issues (if any) in your PR description.

---

## Contribution Guidelines

Since this repository is primarily **Markdown documentation**, please follow these rules:

### Content Standards

* **Clarity:** Questions should be direct, with optional brief context if needed.
* **Neutrality:** Avoid opinionated answers. If perspectives vary, mention alternatives with pros/cons.
* **Accuracy:** Verify technical correctness before submission.
* **Structure:** Group related questions under the right section (e.g., *Architecture Patterns*, *Scalability*, *Evaluation & Metrics*).

### Writing Style

* Use **clear, professional English**.
* Avoid jargon unless it is industry-standard.
* Prefer concise questions and answers (bullet points where appropriate).
* Use proper Markdown formatting:

  * Headings: `#`, `##`, `###` for hierarchy
  * Lists: `-` or `1.`
  * Code blocks for examples:

    ```csharp
    public interface IRepository<T> {
        T GetById(Guid id);
    }
    ```

### Examples of Good Questions

* *"How would you evaluate the scalability of a microservices-based system?"*
* *"What are the trade-offs between layered architecture and hexagonal architecture?"*

### Examples of Good Answers

* Balanced, concise explanation
* Links to further reading (if applicable)
* Structured with bullet points or short paragraphs

---

## Review Process

* Each Pull Request will be reviewed by maintainers for:

  * **Accuracy** (technical correctness)
  * **Readability** (clear, concise, professional language)
  * **Organization** (proper categorization and formatting)
* Minor edits (grammar, formatting) may be adjusted directly by maintainers.
* Larger discussions (new categories, restructuring) will be handled in issues before merging.

---

## Repository Structure

* Do not create new files unless you are proposing a **new category** (open an issue first).
* Place new questions under the appropriate file.
* Maintain consistency in formatting across all documents.

---

## Community Guidelines

* Be respectful and constructive in discussions.
* Assume good intent—this is a collaborative knowledge-sharing project.
* Use inclusive language suitable for a professional and global audience.

---

## License

By contributing, you agree that your work will be licensed under the repository’s [LICENSE](./LICENSE).

---

Thank you for helping build a resource that empowers current and future software architects!

