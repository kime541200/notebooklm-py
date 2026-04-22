[English](ROADMAP.md) | [中文](docs/ROADMAP_zh.md)

# NotebookLM MCP Server Roadmap

This document outlines the currently implemented features of `notebooklm-mcp-server` and plans future extensions based on the capabilities of `notebooklm-py`.

## 🟢 Phase 1: Basic Core (Implemented)

The MCP Server currently has the fundamental capabilities to interact with NotebookLM:

- **Notebook Management**
  - [x] List all Notebooks (`list_notebooks`)
  - [x] Create a new Notebook (`create_notebook`)
- **Source Management**
  - [x] Add source via URL (`add_source`)
  - [x] List sources in a Notebook (`list_sources`)
- **Chat Interaction**
  - [x] Ask questions based on a Notebook and return citation info (`ask_notebook`)

---

## 🟡 Phase 2: Advanced Resource & Chat Management (Implemented)

Expand granular control over chat and resources, allowing AI Agents to interact with Notebooks more flexibly.

### Resource Management Expansion
- [x] **Delete Resources**: Provide tools to delete a Notebook or specific sources.
- [x] **Get Source Fulltext** (`get_source_fulltext`): Allow Agents to directly read the complete plain text content of a source after system indexing, which is highly useful for local analysis.
- [x] **Multiple Source Types Import**:
  - [x] Support adding YouTube videos as sources.
  - [x] Support creating sources directly from plain text (Pasted Text).
  - [ ] (Optional) Support uploading local files or Google Drive files.

### Chat Control Expansion
- [x] **Scope Chat to Specific Sources**: Expand `ask_notebook` parameters to restrict retrieval and chat to specific Source IDs.
- [x] **Chat History Management**:
  - [x] Retrieve past chat history.
  - [x] Support passing `conversation_id` to continue previous chat context.
- [x] **Note Management**: Add a tool to save specific answers as internal Notes in the Notebook.

---

## 🔵 Phase 3: Research Agent Integration (Mid-term Plan)

Introduce NotebookLM's powerful built-in research capabilities.

- [ ] **Trigger Deep Research** (`start_research`): Provide a tool for the MCP Server to call the NotebookLM research agent (supporting `fast` or `deep` modes) to collect data on a specific topic from the Web or Google Drive.
- [ ] **Auto-import Research Results**: Support automatically converting research results into Notebook sources upon completion.
- *Note: These operations are time-consuming and require designing tools for asynchronous waiting or status querying (`get_task_status`).*

---

## 🟣 Phase 4: Content Generation & Export (Long-term Plan)

Unlock the full multimedia generation capabilities of NotebookLM Studio. This allows AI Agents to "produce final deliverables".

### Generation Trigger Tools
- [ ] **Audio/Podcast Generation** (`generate_audio_overview`)
- [ ] **Slide Generation** (`generate_slides`)
- [ ] **Quiz & Flashcard Generation** (`generate_quiz`, `generate_flashcards`)
- [ ] **Document & Chart Generation** (Report, Data Table, Mind Map)
- [ ] **Global Language Setting** (`set_output_language`): Control the generation language.

### Export & Download Tools
- [ ] **Download Multimedia Artifacts**: Support downloading generated `.mp3`, `.mp4`, `.pptx` back to the local machine.
- [ ] **Get Structured Data**: Directly return generated JSON (e.g., quiz data, mind map structure) or CSV (data tables) for further Agent processing.

---

## 💡 Architecture & Deployment Optimization

- [x] Docker Deployment Support
- [ ] **Token & Auth Auto-refresh**: Explore mechanisms to handle Session expiration or re-authentication in long-running Docker containers.
- [ ] **Concurrency & Queueing Mechanism**: If time-consuming generation tasks are supported in the future, consider adding a simple task queue state management.
