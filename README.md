# Hebrew RTL Instructions for GitHub Copilot Chat

> [עברית](README.he.md)

A drop-in instructions file that fixes bidirectional text corruption in GitHub Copilot Chat
responses written in Hebrew inside Visual Studio Code.

**Scope:** the GitHub Copilot Chat extension inside the VS Code editor. Not Microsoft 365
Copilot, and not the chat on github.com.

## The problem

When Copilot answers in Hebrew and the sentence contains an English term, a file name or a
code snippet, the punctuation between them breaks:

- The period at the end of the sentence jumps to the beginning of the line.
- Parentheses flip.
- Hyphens and dashes land on the wrong side.

This is not a Hebrew bug. Punctuation characters are *neutral* under the Unicode
Bidirectional Algorithm: they have no direction of their own. When a neutral character sits
between a Hebrew run and a Latin run, it is resolved against the surrounding paragraph
direction and gets rendered on the opposite end of the line.

## The fix

The instructions file tells the model to wrap the text content of every paragraph, heading,
list item and table cell in a right-to-left embedding, so the entire line is resolved as one
RTL run and the punctuation stays put. It also adds complementary writing rules:

- Start every line with a Hebrew word, never with a Latin term or inline code.
- Move long commands, paths and signatures into their own fenced code block.
- Replace an em dash between Hebrew and English with a colon.
- Handle parentheses, numbers, units and markdown file links explicitly.

## Installation

Pick one of the two locations.

**1. User level, applies to every project**

Copy the file into your VS Code user prompts folder:

| OS | Path |
| --- | --- |
| Windows | `%APPDATA%\Code\User\prompts` |
| macOS | `~/Library/Application Support/Code/User/prompts` |
| Linux | `~/.config/Code/User/prompts` |

**2. Project level, applies to everyone working on the repository**

Copy the file into `.github/instructions/` in your repository and commit it. Everyone who
clones the repo gets the same behavior automatically.

No further configuration is needed. The file declares `applyTo: '**'` in its front matter,
and VS Code loads it into every chat session on its own.

## What this does not do

It fixes character **order**, not **alignment**. The VS Code chat view is laid out
left-to-right, and the `dir` attribute is stripped by the content sanitizer, so short lines
will still hug the left edge. Full right alignment requires injecting CSS into the editor,
which is outside the scope of this file.

## Notes

- The file itself contains no invisible control characters, so it is safe to copy as is.
- It is encoded as UTF-8 without BOM. Prefer downloading the raw file over copying from a
  browser, since some browsers drop control characters on copy.
- The rules are not specific to Hebrew. The same structure applies to Arabic, Persian, Urdu
  and any other right-to-left script; only the examples are in Hebrew.

## Feedback

Issues and pull requests are welcome, especially edge cases the rules do not cover yet.
When reporting one, please include the Copilot output you got and the output you expected.
