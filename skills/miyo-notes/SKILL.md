---
name: miyo-notes
description: Search and work with the user's notes, documents, and saved AI chats through Miyo. Use when the user asks about content in their Miyo library or explicitly asks to create or edit a file there.
---

# Work with Miyo notes

Use Miyo's MCP tools to search and read the user's connected content. Write
only when the user requests it and the target folder explicitly allows remote
writes.

## Choose the search corpus

Miyo keeps documents and saved AI chats in separate search corpora. One
`search` call searches exactly one corpus.

- Use `source: "documents"` for notes and files the user wrote or saved. This
  is the default.
- Use `source: "chats"` for the user's saved ChatGPT and Claude conversations.
- If the request spans both, search each corpus separately and identify which
  corpus each result came from. Do not imply that one call searched both.

Keep the user's question in `query`. For broad, short, or jargon-heavy
requests, add up to two complete rephrasings in `variants` and raise `limit`
to about 30. If an ordinary first search is thin, retry with variants.

## Find and read content

- Use `list_folders` when folder availability or write permission matters.
- Use `list_files` to browse or filter by title, path, or modification date.
- Use `search` to find relevant passages, then `read_file` when the full file
  is needed.
- Preserve the folder-relative paths returned by Miyo when calling
  `read_file` or `edit_file` and when citing a source to the user.

## Write safely

Before `create_file` or `edit_file`, call `list_folders` and inspect the target
folder's `allow_writes` field.

- Proceed only when `allow_writes` is `true` for that exact folder.
- If it is `false` or the folder is unavailable, explain that remote writes
  must be enabled in Miyo. Do not choose a different writable folder without
  the user's direction.
- Use `create_file` only for a new file in an existing folder or subfolder.
- Before `edit_file`, read the current file. Choose `old_text` that occurs
  exactly once and preserve surrounding content.

Never ask the user for a Miyo API key. If authentication fails, ask them to
complete Miyo OAuth. If the connector is unavailable, ask them to confirm that
Miyo Connect is enabled and the desktop app is online.
