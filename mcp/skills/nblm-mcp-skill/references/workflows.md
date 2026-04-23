# NotebookLM MCP Workflows & Best Practices

## Tool Usage Guide

### `ask_notebook`
This is your primary tool for extracting knowledge.
- **Parameters**: 
  - `notebook_id` (required): The ID of the target notebook.
  - `query` (required): Your question or instruction.
  - `conversation_id` (optional): To maintain conversation history, always pass this if you are continuing a multi-turn interaction.
  - `source_ids` (optional): A list of source IDs if you want NotebookLM to only consult specific documents.
- **Output**: Returns the text response along with citations. Always surface the citations in your final response to the user to build trust.

### Source Management
When adding sources, be mindful of the content type:
- `add_source`: Best for static web pages and articles.
- `add_youtube_source`: Extracts transcripts from YouTube. Extremely useful for video research.
- `add_text_source`: Use this if you have raw text to inject directly.

After adding sources, you can use `get_source_fulltext` to retrieve the processed, raw text if you need to perform client-side analysis that NotebookLM's chat might miss.

### State Management
NotebookLM's MCP server operates statelessly from your perspective, but maintains state via Notebook and Conversation IDs.
- **Always keep track of the `notebook_id`** during a session.
- **Always keep track of the `conversation_id`** if the user is asking follow-up questions.

## Example Flow

1. User: "Can you summarize the latest news about AI from these two URLs?"
2. Agent uses `create_notebook(title="AI News Research")` -> gets `notebook_id: "123"`
3. Agent uses `add_source(notebook_id="123", url="...")` for URL 1.
4. Agent uses `add_source(notebook_id="123", url="...")` for URL 2.
5. Agent uses `ask_notebook(notebook_id="123", query="Summarize the latest news from the sources")` -> gets response and `conversation_id: "abc"`
6. Agent presents summary to user.
7. User: "What did the first article say about model sizes?"
8. Agent uses `ask_notebook(notebook_id="123", query="What did the first article say about model sizes?", conversation_id="abc")`
