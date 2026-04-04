---
name: md-learning-stddzr
description: Standardizes Markdown files by applying consistent formatting, structure, and style conventions. Use this skill when you need to normalize MD file content for consistency across your project.
license: MIT
---

# Markdown Standardizer Skill

## Purpose
This skill guides standardization of Markdown files to ensure consistency across the project.

## When to Use
- Normalizing inconsistent heading levels
- Ensuring consistent list formatting
- Standardizing code block annotations
- Formatting tables consistently
- Applying consistent link formats
- Normalizing spacing and line breaks

## Standardization Guidelines

### 1. Heading Structure
- Use `#` for H1, `##` for H2, etc. (no skipping levels)
- Add blank lines before and after headings
- Use sentence case for headings

### 2. Lists
- Use `-` for unordered lists (not `*` or `+`)
- Use `1.` for ordered lists
- Add blank lines between list items only if they contain multiple lines

### 3. Code Blocks
- Always include language identifier: ` ```language `
- Add blank lines before and after code blocks

### 4. Links and References
- Use `[text](url)` format
- For images: `![alt text](path)`
- Reference-style links for repeated URLs

### 5. Spacing
- Maximum 2 consecutive blank lines
- Single blank line between sections
- No trailing whitespace

### 6. Special Elements
- **Bold**: use `**text**` (not `__text__`)
- *Italic*: use `*text*` (not `_text_`)
- Inline code: use backticks

### 7. Tables
- Use `|` to separate columns
- Align headers with `:---`, `:---:`, or `---:`
- Add blank lines before and after tables 
- Use consistent spacing around `|` for readability
- Prefer using tables for command lists and structured data

## Process
1. Analyze the current MD file structure
2. Identify areas that don't meet guidelines
3. Apply transformations consistently
4. Preserve all content and meaning
5. Provide summary of changes made
6. In case of having more than 8 chapters, create a table of contents at the top with links to each section