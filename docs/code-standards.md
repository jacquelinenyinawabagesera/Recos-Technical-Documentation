# <span style="font-weight: 1; font-size: 2.1em; color: #232f3e;">Code Standards</span>

---

## <span style="font-weight: 300; color: #8645e8;">Frontend (Next.js, TypeScript, Tailwind)</span>

- <span style="color: #232f3e;"><b>Components:</b></span> Always use functional components for consistency and performance.
- <span style="color: #232f3e;"><b>Naming:</b></span>
  - camelCase for variables and functions
  - PascalCase for component names
  - SCREAMING_SNAKE_CASE for constants
- <span style="color: #232f3e;"><b>Files:</b></span> One component per file. Group files by feature/module for maintainability.
- <span style="color: #232f3e;"><b>Testing:</b></span> Use Jest and React Testing Library for unit tests; add tests for each new component or logic.
- <span style="color: #232f3e;"><b>Styling:</b></span> Tailwind CSS for all styling. Avoid inline styles unless absolutely necessary.

---

## <span style="font-weight: 300; color: #8645e8;">Backend (Django, DRF)</span>

- <span style="color: #232f3e;"><b>Naming:</b></span>
  - snake_case for variables and functions
  - PascalCase for classes and models
- <span style="color: #232f3e;"><b>Serializers/Views:</b></span> Place each in its own file unless they are closely related.
- <span style="color: #232f3e;"><b>Testing:</b></span> Use Django’s built-in test runner (`python manage.py test`) and maintain high coverage.

---

## <span style="font-weight: 300; color: #8645e8;">General Standards</span>

- <span style="color: #232f3e;"><b>Linting:</b></span>
  - Prettier + ESLint for frontend code
  - flake8 and black for backend code
- <span style="color: #232f3e;"><b>Pull Requests:</b></span> 
  - Write clear summaries for every PR
  - Link related issues (use `Fixes #issue_num`)
  - Always request at least one review before merging
- <span style="color: #232f3e;"><b>Documentation:</b></span>  
  - Document components, functions, and modules with JSDoc (frontend) or docstrings (backend)
  - Update README and feature docs for significant changes

---

## <span style="font-weight: 300; color: #8645e8;">Best Practices</span>

- Keep functions and components small and focused.
- Prefer composition over inheritance.
- Avoid commented-out code in committed files.
- Refactor and remove unused code regularly.
- Always run lint and tests before pushing or merging.

---