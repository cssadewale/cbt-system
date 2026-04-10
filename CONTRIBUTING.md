# Contributing to CBT Pro

Thank you for your interest. This project was built by a tutor to solve a real problem in Nigerian education. Every contribution — a bug fix, a feature idea, a typo correction — is valued.

---

## Who Should Contribute

- Nigerian educators who want features that reflect actual classroom workflows
- Developers who want to improve performance, security, or accessibility
- Data scientists who want to add better analytics or reporting
- Anyone who found a bug or has a well-defined improvement idea

---

## How to Contribute

### Reporting a Bug
1. Open an **Issue** on GitHub
2. Describe what you expected to happen
3. Describe what actually happened
4. Include the browser and device you were using
5. A screenshot helps if the problem is visual

### Suggesting a Feature
1. Open an **Issue** with the label `enhancement`
2. Describe the problem the feature solves (not just the solution)
3. Describe how a teacher or student would use it in a real Nigerian school setting

### Submitting Code
1. Fork this repository
2. Create a branch: `git checkout -b feature/your-feature-name`
3. Make your changes — keep them focused (one thing per PR)
4. Test on both desktop and mobile before submitting
5. Open a **Pull Request** with:
   - A clear title
   - A description of what changed and why
   - Screenshots if the UI changed

---

## Code Style

This project uses no framework and no build tools intentionally — it must remain deployable as two plain HTML files on GitHub Pages.

- Keep JavaScript vanilla — no npm packages, no bundlers
- All logic for each page stays inside that page's HTML file
- Use the existing CSS variables for all colours — do not introduce new colour systems
- Comment any logic that is not immediately obvious
- Test CSV parsing with edge cases: UTF-8 BOM, quoted commas, empty columns, missing explanation column

---

## Project Values

These are non-negotiable for any contribution:

- **Free first** — no feature should require a paid service to work
- **Mobile-friendly** — many Nigerian students access exams on phones, not laptops
- **Simple** — the code should be readable by someone learning web development, not just experts
- **Backward compatible** — new CSV columns must remain optional so existing question banks still work

---

## Questions?

Reach the author directly:

- 📧 [buildingmyictcareer@gmail.com](mailto:buildingmyictcareer@gmail.com)
- 💼 [linkedin.com/in/adewalesamsonadeagbo](https://linkedin.com/in/adewalesamsonadeagbo)
