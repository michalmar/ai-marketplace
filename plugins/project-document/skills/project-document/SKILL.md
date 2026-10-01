---
name: project-document
description: Create or format a project Markdown document with its name, owner, creation date, and description.
---

# Project document

Use this skill when the user asks to create, initialize, or reformat a project
Markdown document.

## Required information

Collect these values from the request or existing project files:

- Project name
- Project owner or author
- Project description

Ask the user only for values that cannot be determined confidently. Use the
current local date as the creation date unless the user provides another date.

## Output

Create the document as `PROJECT.md` unless the user specifies a different
path. Preserve the following structure exactly:

```markdown
# <name>

## project

## owner: <author for the project>

## date: <date of creation>

## description: <actual description>
```

Replace every angle-bracket placeholder with its actual value. Format the date
as `YYYY-MM-DD`. Do not leave placeholder text in the finished document.

If formatting an existing document, preserve useful description content while
normalizing it to this structure. Do not overwrite an unrelated file without
first confirming with the user.
