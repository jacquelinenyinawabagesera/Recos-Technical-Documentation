
# Code Standards

## Frontend (Next.js, TypeScript, Tailwind)

- **Components:** Always use functional components.
- **Naming:** camelCase for variables, PascalCase for components, SCREAMING_SNAKE_CASE for constants.
- **Files:** One component per file. Group by feature.
- **Testing:** Use Jest + React Testing Library for unit tests.
- **Styling:** Tailwind for all styling.

## Backend (Django, DRF)

- **Naming:** snake_case for variables and functions, PascalCase for classes.
- **Serializers/Views:** One per file unless very related.
- **Testing:** Use Django's built-in test runner.

## General

- **Linting:** Prettier + ESLint (frontend), flake8 (backend).
- **PRs:** Clear summaries, link related issues, request review.

---