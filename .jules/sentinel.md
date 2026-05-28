## 2024-05-27 - Path Traversal in Agent Instructions
**Vulnerability:** Path traversal risk found in `skills/artifact-management/SKILL.md`. The instructions told the agent to create a directory `superpowers-[topic]-[date]/` using an unsanitized `[topic]` variable.
**Learning:** LLMs blindly interpolating user input into file paths in agent instruction templates creates a unique class of path traversal vulnerabilities. If the human input for `[topic]` includes `../` or absolute paths, the agent could create files or overwrite data outside the intended working directory.
**Prevention:** Explicitly instruct the agent to sanitize user-provided variables (e.g., removing slashes, keeping only alphanumeric and hyphens) before using them in file path construction.
