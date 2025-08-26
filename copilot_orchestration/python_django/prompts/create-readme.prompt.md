---
mode: agent
tools: ['codebase', 'fileSystem', 'gitLog']
description: "Generates a comprehensive README.md file for the current project, analyzing the codebase and existing documentation."
---
# README.md Generator

**Your Role:** You are an expert Technical Writer and Project Documenter. Your task is to create a detailed, engaging, and accurate `README.md` file for this repository.

**Context Analysis:**
*   **Analyze the entire codebase** (`#codebase`) to understand the project's purpose, functionality, technology stack, and structure.
*   **Scan existing documentation files** (e.g., in `.github/copilot/` directory, `copilot-instructions.md`, or other `*.md` files) to extract information about the project's architecture, development workflow, coding standards, and testing approaches [3, 4].
*   **Review `git log`** for recent commit messages to understand key features or changes implemented.

**README Structure Requirements:**
Your generated `README.md` should be in Markdown format and include the following sections, ordered logically. Use clear, concise language and professional tone.

1.  **Project Title & Description:**
    *   A compelling, concise title.
    *   A clear, high-level overview of what the project does, its primary goals, and its value proposition.

2.  **Features:**
    *   A bulleted list of the core functionalities or capabilities of the project.

3.  **Technology Stack:**
    *   List the main programming languages, frameworks, libraries, and tools used (e.g., Python/Django, JavaScript/React, PostgreSQL, Docker, etc.).

4.  **Installation Guide:**
    *   **Prerequisites:** List any software, tools, or dependencies users need to install beforehand.
    *   **Steps:** Provide clear, step-by-step instructions for setting up the project locally. Include commands for cloning, installing dependencies (e.g., using `pip` for Python, `npm`/`pnpm` for Node.js), and database setup if applicable. Refer to project-specific setup scripts if found (e.g., `setup.sh`, `dev.sh`) [5].

5.  **Usage:**
    *   How to run the application or use the core functionalities.
    *   Basic examples or command lines if it's a CLI tool.

6.  **Testing:**
    *   Briefly describe the testing framework used (e.g., `pytest`, `Jest`) [6].
    *   Instructions on how to run the project's unit and integration tests.

7.  **Contributing:**
    *   Guidelines for how other developers can contribute to the project (e.g., "Follow a feature-branching workflow" [7], "Write clear and descriptive commit messages" [7]).
    *   Reference a `CONTRIBUTING.md` file if one exists.

8.  **License:**
    *   State the project's license (e.g., MIT, Apache 2.0).

9.  **Contact/Support (Optional):**
    *   Information on how to get help or contact the maintainers.

**Formatting & Style:**
*   Use appropriate Markdown headings (`#`, `##`, `###`).
*   Employ code blocks for code snippets and commands.
*   Use bullet points and numbered lists for readability.
*   Ensure **consistent formatting** and style throughout. Refer to any existing style guides or `copilot-instructions.md` for specifics [4].

**Output:**
**Provide only the Markdown content for the `README.md` file.** Do not include any conversational text or explanations outside the Markdown block.