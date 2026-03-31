# Notehub

Riley's knowledge repository, in Markdown.

## Folder Structure

- **01-inbox** — Quick captures and raw notes awaiting organization
- **02-projects** — Notes tied to specific projects or active learning tracks
- **03-notes** — Organized, topic-based notes
- **04-resources** — Cheatsheets, scripts, prompts, tooling references
- **05-archive** — Completed or superseded learning materials

### 03-notes Topics

| Directory | Contents |
|---|---|
| `ai/` | AI concepts, tools, MCP |
| `architecture/` | Onion Architecture, OOP principles |
| `azure/` | Azure AD, AI Search, Monitor, Bastion, Graph API |
| `c/` | C language notes |
| `code-generation/` | Code generation tooling |
| `data/` | Big O, data structures, SQL vs NoSQL |
| `design-patterns/` | Creational, structural, behavioural patterns |
| `dll/` | Creating and using DLLs |
| `dotnet-csharp/` | C# types, threading, async, auth, web APIs |
| `git/` | Git basics, branching, mac tips |
| `maui/` | .NET MAUI UI, MVVM, platform implementations |
| `mqtt-messaging/` | MQTT protocol notes |
| `power-bi/` | Power BI notes |
| `privacy/` | Privacy best practices |
| `programming-principals/` | SOLID, APIs, general concepts |
| `rust/` | Rust ownership, structs, enums, crates, macros |
| `security/` | API auth, CVE tracking |
| `shell-scripting/` | Shell scripting patterns |
| `software-architecture/` | CAP theorem, events vs messaging |
| `swift/` | Swift iOS, navigation, error handling |
| `testing/` | Regression testing |
| `vite/` | Vite build tool |
| `virtualisation-and-containerisation/` | Podman, Dev Containers |
| `visualisation/` | Data visualisation |
| `vim/` | Buffers, windows, tabs |

### 04-resources

| Directory/File | Contents |
|---|---|
| `cli-commands.md` | CLI cheatsheet (git, docker, az) |
| `common-lint-warnings.md` | Linter warnings reference |
| `conventions.md` | Coding conventions |
| `dotnet-methods/StringMethods.md` | .NET string methods |
| `null-handling.md` | Null handling patterns |
| `powershell/` | Scripts, AD management, profiles |
| `prompts/` | LLM prompt templates |
| `scripts/docker.md` | Docker scripts |
| `shortcuts.md` | Keyboard shortcuts |
| `useful-links.md` | Bookmarks |
| `vba-macros.md` | VBA macro tips |
| `versioning.md` | Semantic versioning |

## Recommended Plugins & Tools

- **render-markdown** — .md renderer for Neovim
- **cweijan.vscode-office** — WYSIWYG .md editing in VS Code

## Things to Learn

- [ ] Different flags that can be applied to classes — static, ref, etc.
- [ ] How does git AutoMerge work? What are its limitations?

## Things to Experiment With

- [ ] Code snippets for markdown: https://mambusskruj.github.io/posts/pub-neovim-for-markdown/
- [ ] markdown.nvim: https://neovimcraft.com/plugin/tadmccorkle/markdown.nvim/

## Things to Remember

- When executing Neovim editor commands with Lua, prefix with `:lua`:
  - `:lua {command}`
