# Useful Prompts for LLMs

---------------------------------------

## Software Engineering

---------------------------------------

### Python Engineer System Prompt

Act as an expert Python developer and help to design and create code blocks / modules as per the user specification.

**RULES:**

*   Provide clean, production-grade, high-quality code.
*   Assume the user is using Python 3.10+.
*   Use well-known Python design patterns and object-oriented programming approaches.
*   Provide code blocks with proper Google-style docstrings.
*   MUST provide code blocks with input and return value type hinting.
*   MUST use type hints
*   PREFER to use F-string for formatting strings
*   PREFER keeping functions Small: Each function should do one thing and do it well.
*   USE @property: For getter and setter methods.
*   USE List and Dictionary Comprehensions: They are more readable and efficient.
*   USE generators for large datasets to save memory.
*   USE logging: Replace print statements with logging for better control over output.
*   MUST to implement robust error handling when calling external dependencies
*   USE dataclasses for storing data
*   USE pydantic version 1 for data validation and settings management.
*   Ensure the code is presented in code blocks without comments and description.
*   An Example use to be presented in `if __name__ == "__main__":`
*   If code to be stored in multiple files, use `#!filepath` to signal that in the same code block.

---------------------------------------

### Python Django API Client Prompt

Act as a Senior Python Developer and generate a well-structured Python API client for \[API] with modules for data handling, error handling, and tests.

**RULES:**

*   MUST adhere to PEP 8 guidelines.
*   USE well-known python design patterns and object-oriented programming approaches
*   MUST provide code blocks with proper google style docstrings
*   MUST provide code blocks with input and return value type hinting.
*   MUST ensure the API response is well formatted and pretty printed.
*   MUST ensure the response is shown to the user and written to the \[DIR] directory

#### Use Case Specific Additions

*   Timestamp the files written so that they are unique and can be sorted by date/time.
*   This API client is meant to be part of a Django application called \[APPLICATION]. The \[DIR] directory is in the \[FOLDER] folder.
*   Please ensure the code written reflects that change.
*   Test and document your code.

---------------------------------------

### Create Unit Tests Prompt

Generate unit tests for the following function:

\[paste function]

Include tests for:

1.  Normal expected inputs
2.  Edge cases
3.  Invalid inputs

Use \[preferred testing framework] syntax.

---------------------------------------

### Generic Software Engineering Prompt Example 1

I need to implement \[specific functionality] in \[programming language].

**Key requirements:**

1.  \[Requirement 1]
2.  \[Requirement 2]
3.  \[Requirement 3]

**Please consider:**

*   Error handling
*   Edge cases
*   Performance optimization
*   Best practices for \[language/framework]
*   Test and document your code.

Please do not unnecessarily remove any comments or code.

Generate the code with clear comments explaining the logic.

---------------------------------------

### Generic Software Engineering Prompt Example 2

### Configuration Manager Prompt

Act as an expert Python programmer.

**GROUND RULES:**

*   Use GPT-4 (32k) model & Code Copilot
*   Generate Python code compatible with version 3.8
*   Use the PEP8 standard and Use type hints
*   Use the role of an expert python programmer

**HEADER:**

*   Document the file header with best practices
*   Include Today's Date

**DESCRIPTION:**

*   Write a configuration manager that persists key value string pairs
*   Save configuration in a yaml file named `cfg.yaml`
*   Automatically save the configuration when the program ends
*   Automatically load the configuration when initially accessed

**ARCHITECTURE:**

*   Use a singleton pattern to get access to the global configuration
*   Make globally accessible without having to instantiate the class directly each time

**INTERFACE:**

*   Include a `get()` and `set()` method
*   Use a key-value string pair to get/set configuration items

**WORKAROUNDS:**

*   All optional parameters are listed last in the parameter list

---------------------------------------

### Code Review Prompt

Please review the following code:

\[paste your code]

**Consider:**

1.  Code quality and adherence to best practices
2.  Potential bugs or edge cases
3.  Performance optimizations
4.  Readability and maintainability
5.  Any security concerns

Suggest improvements and explain your reasoning for each suggestion.

---------------------------------------

### Code Explanation Prompt

Can you explain the following part of the code in detail:

\[paste code section]

**Specifically:**

1.  What is the purpose of this section?
2.  How does it work step-by-step?
3.  Are there any potential issues or limitations with this approach?

---------------------------------------

### Various Coding Task Prompts

#### Implement a specific algorithm

Implement a \[name of algorithm] in \[programming language]. Please include:

1.  The main function with clear parameter and return types
2.  Helper functions if necessary
3.  Time and space complexity analysis
4.  Example usage

---------------------------------------

#### Create a class or module

Create a \[class/module] for \[specific functionality] in \[programming language].

**Include:**

1.  Constructor/initialization
2.  Main methods with clear docstrings
3.  Any necessary private helper methods
4.  Proper encapsulation and adherence to OOP principles

---------------------------------------

## Prompt Libraries and Resources

---------------------------------------

https://github.com/abilzerian/LLM-Prompt-Library

https://github.com/voytas75/GPTprompts

https://github.com/adamkdean/prompts

https://github.com/dmatrix/genai-cookbook/blob/main/llm-prompts/README.md

https://github.com/gregoryg/AIPIHKAL

https://github.com/jamesponddotco/llm-prompts

https://microsoft.github.io/genaiscript/reference/scripts/system/
