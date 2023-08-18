---
share: true
catagory: "Query Language"
---

> Have some use case in Obsidian notes, this note is for reference that what I used

## Query structure

Or LIST/TABLE can be used to render the content

```dql
TABLE WITHOUT ID "[[" + file.name + "|" + file.name + "]]" as "Problem", <column>
FROM <file/folder/tag>
WHERE <expression>
SORT <column> (DESC)
LIMIT <number>
```

## Metadata:

- `file.mtime` : File modify time
- `file.tags` : File tags as array
- `<meta-inlines>` : reference to metadata

## Adding metadata:

### Frontmatter

```yaml
---
tags:
    - <anything>
---
```

### Inline

- `<meta-inlines>:: <anything>`

## Function

- `econtain(<array/string>)` : Check for exact contain of element in list array

