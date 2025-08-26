---
mode: agent
tools: ['codebase', 'gitDiff', 'gitLog']
description: "Generate a conventional commit message based on recent changes."
---
# Conventional Commit Message Generator

Your task is to generate a concise, conventional commit message for the pending changes in the current Git working directory. Adhere strictly to the Conventional Commits specification (https://www.conventionalcommits.org/en/v1.0.0/).

**Format:** `<type>[optional scope]: <description>`

**Body (optional):** A more detailed explanation of the commit.

**Footer (optional):** References to issues (e.g., `Fixes #123`, `Closes #456`).

**Rules:**
*   **<type>**: Must be one of `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, or `revert`.
    *   `feat`: A new feature
    *   `fix`: A bug fix
    *   `docs`: Documentation only changes
    *   `style`: Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc.)
    *   `refactor`: A code change that neither fixes a bug nor adds a feature
    *   `perf`: A code change that improves performance
    *   `test`: Adding missing tests or correcting existing tests
    *   `build`: Changes that affect the build system or external dependencies (example scopes: gulp, broccoli, npm)
    *   `ci`: Changes to our CI configuration files and scripts (example scopes: Travis, Circle, BrowserStack, SauceLabs)
    *   `chore`: Other changes that don't modify src or test files
    *   `revert`: Reverts a previous commit
*   **[optional scope]**: A noun describing the section of the codebase affected, enclosed in parentheses (e.g., `feat(parser): add ability to parse arrays`).
*   **<description>**: A succinct, imperative, present-tense description of the change. Start with a lowercase letter.
*   **Body**: Explain *why* the change was made and *how* it addresses the problem.
*   **Footer**: For breaking changes, use `BREAKING CHANGE:` followed by a description.

## Generate Semantic Commit Messages

### Context
Generate clear, descriptive commit messages following semantic commit conventions for Python/Django projects.

### Commit Message Format
```
<type>(<scope>): <description>

[optional body]

[optional footer(s)]
```

#### Commit Types
- **feat**: New features or functionality
- **fix**: Bug fixes
- **docs**: Documentation changes
- **style**: Code style changes (formatting, missing semicolons, etc.)
- **refactor**: Code refactoring without functional changes
- **perf**: Performance improvements
- **test**: Adding or modifying tests
- **chore**: Build process, dependencies, or maintenance tasks
- **ci**: CI/CD configuration changes

#### Django-Specific Scopes
- **models**: Database models and migrations
- **views**: Views and URL configurations
- **api**: REST API endpoints and serializers
- **admin**: Django admin customizations
- **auth**: Authentication and authorization
- **forms**: Form classes and validation
- **templates**: HTML templates and static files
- **settings**: Configuration and environment settings
- **migrations**: Database migrations
- **management**: Management commands
- **tests**: Test files and testing utilities

### Message Guidelines

#### Description Requirements
- Use imperative mood ("add" not "added" or "adds")
- Keep under 50 characters
- Don't end with a period
- Be specific and descriptive

#### Body Guidelines (when needed)
- Explain what changed and why
- Include breaking changes
- Reference issue numbers
- Provide context for complex changes

#### Examples
```
feat(api): add user profile endpoints with pagination

Implements GET /api/users/{id}/profile and PUT endpoints
with proper serialization and permission checking.

Closes #123
```

```
fix(models): resolve user email uniqueness constraint

- Add case-insensitive email validation
- Update existing migration to handle duplicates
- Add database index for performance

Fixes #456
```

```
refactor(views): extract common pagination logic

Move pagination logic from individual views to a base
mixin class to reduce code duplication.
```

### Analysis Instructions
When analyzing code changes:
1. Identify the primary type of change (feat, fix, refactor, etc.)
2. Determine the appropriate scope based on files modified
3. Summarize the change concisely
4. Note any breaking changes or important context
5. Reference related issues or pull requests if mentioned

### Output Format
Provide the commit message in the specified format, including explanation of why the chosen type and scope are appropriate.