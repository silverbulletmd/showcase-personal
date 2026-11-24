Space configuration: SilverBullet’s own light theme with a blue accent, the page picker docked on the left, an icon and a few attributes for books, ideas and people, and clicking a tag opens the page that collects it.

```space-lua
config.set("theme", "Library/Std/Themes/SilverBullet Light")
config.set("accentColor", "#2563eb")

config.set("view.defaults", {
  ["std.pages"] = {
    dock = "lhs",
    open = true,
  },
})

-- Each page tagged #book, #idea or #person gets its icon wherever it’s listed or linked.
-- The schema turns a book’s frontmatter into a form with a status picker and a rating.
tag.define {
  name = "book",
  tagPage = "Reading List",
  icon = "book",
  schema = {
    type = "object",
    properties = {
      author = schema.string(),
      status = schema.enum { "to read", "reading", "finished" },
      rating = schema.nullable "number",
    },
  },
}

tag.define {
  name = "idea",
  tagPage = "Ideas",
  icon = "lightbulb",
}

tag.define {
  name = "person",
  icon = "user",
}
```
