#meta

Space configuration: the page picker docked on the left, an icon for books, ideas and people, clicking a tag opens the page that collects it, and a command to add a book.

```space-lua
config.set("view.defaults", {
  ["std.pages"] = {
    dock = "lhs",
    open = true,
  },
})

-- Each page tagged #book, #idea or #person gets an icon wherever it’s listed or linked
local function icon(i)
  return function(o)
    if o.tag == "page" then
      o.pageDecoration = { icon = i }
    end
    return o
  end
end

local LIGHTBULB = [[<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 14c.2-1 .7-1.7 1.5-2.5 1-.9 1.5-2.2 1.5-3.5A6 6 0 0 0 6 8c0 1 .2 2.2 1.5 3.5.7.7 1.3 1.5 1.5 2.5"/><path d="M9 18h6"/><path d="M10 22h4"/></svg>]]

tag.define {
  name = "book",
  tagPage = "Reading List",
  transform = icon "book",
}

tag.define {
  name = "idea",
  tagPage = "Ideas",
  transform = icon(LIGHTBULB),
}

tag.define {
  name = "person",
  transform = icon "user",
}

-- Ask for a title and author, and start a page for the book; the Reading List picks it up
command.define {
  name = "Book: Add",
  run = function()
    local title = editor.prompt "Which book?"
    if not title or title == "" then
      return
    end
    local author = editor.prompt "Who wrote it?" or ""
    space.writePage(title, table.concat({
      "---",
      "tags: book",
      "author: " .. author,
      "status: to read",
      "---",
    }, "\n"))
    editor.navigate(title)
  end,
}
```
