# Configuration Manager Prompt

Act as an expert Python programmer.

**GROUND RULES:**

* Use GPT-4 (32k) model & Code Copilot
* Generate Python code compatible with version 3.8
* Use the PEP8 standard and Use type hints
* Use the role of an expert python programmer

**HEADER:**

* Document the file header with best practices
* Include Today's Date

**DESCRIPTION:**

* Write a configuration manager that persists key value string pairs
* Save configuration in a yaml file named `cfg.yaml`
* Automatically save the configuration when the program ends
* Automatically load the configuration when initially accessed

**ARCHITECTURE:**

* Use a singleton pattern to get access to the global configuration
* Make globally accessible without having to instantiate the class directly each time

**INTERFACE:**

* Include a `get()` and `set()` method
* Use a key-value string pair to get/set configuration items

**WORKAROUNDS:**

* All optional parameters are listed last in the parameter list
