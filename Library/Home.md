#meta

Sam’s home page: a greeting, buttons for the things he does every day, and a few numbers about his space.

# Implementation
```space-lua
home = {}

-- Widget links open pages through editor.navigate, so they work wherever the space is served
local function go(ref)
  return function()
    editor.navigate(ref)
  end
end

function home.greeting(name)
  local hour = tonumber(os.date "%H")
  local part = hour < 12 and "morning" or hour < 18 and "afternoon" or "evening"
  local date = os.date "%A" .. " " .. tonumber(os.date "%d") .. " " .. os.date "%B"
  return widget.htmlBlock(dom.div {
    class = "home home-greeting",
    dom.h1 { "Good " .. part .. ", " .. name },
    dom.p { date },
  })
end

local function action(label, command)
  return dom.button {
    class = "home-action",
    onclick = function()
      editor.invokeCommand(command)
    end,
    label,
  }
end

function home.actions()
  return widget.htmlBlock(dom.div {
    class = "home home-actions",
    action("Quick note", "Quick Note"),
    action("Today’s journal", "Journal: Today"),
  })
end

local function tile(value, label, page)
  return dom.a {
    class = "home-tile",
    onclick = go(page),
    dom.strong { tostring(value) },
    dom.span { label },
  }
end

function home.tiles()
  local books = query[[
    from p = index.contentPages "book"
    select p
  ]]
  local reading, finished = "–", 0
  for _, b in ipairs(books) do
    if b.status == "reading" then
      reading = b.name
    end
    if b.status == "finished" then
      finished = finished + 1
    end
  end
  local openTasks = query[[
    from t = index.tasks()
    where not t.done
  ]]
  local journalDays = query[[
    from p = index.pages()
    where p.name:startsWith "Journal/"
    order by p.name desc
  ]]
  return widget.htmlBlock(dom.div {
    class = "home home-tiles",
    tile(#openTasks, "open tasks", "index"),
    tile(finished, "books finished", "Reading List"),
    tile(#journalDays, "journal days", journalDays[1].name),
    dom.a {
      class = "home-tile home-tile-wide",
      onclick = go(reading),
      dom.span { "Reading now" },
      dom.em { reading },
    },
  })
end
```

# Style
```space-style
/* Sam’s accent colour, used by SilverBullet’s own UI too */
html:root {
  --ui-accent-color: #2563eb;
}

/* Let the home widgets sit on the page without the usual widget frame */
.sb-lua-directive-block:has(.home) {
  border: none !important;
  background: none !important;
}

/* Greeting and date */
.home-greeting h1 {
  font-size: 2.2em;
  font-weight: 600;
  margin: 0.2em 0 0;
  color: #172554;
}
.home-greeting p {
  margin: 0.1em 0 0;
  color: #78716c;
}

/* The row of buttons */
.home-actions {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  margin: 4px 0;
}
.home-action {
  font: inherit;
  font-size: 0.85em;
  padding: 6px 14px;
  border-radius: 999px;
  border: 1px solid #bfdbfe;
  background: #eff6ff;
  color: #1e40af;
  cursor: pointer;
}

/* Stat tiles, with a wide “Reading now” tile below them */
.home-tiles {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
}
.home-tile {
  display: flex;
  flex-direction: column;
  gap: 2px;
  padding: 12px 14px;
  border-radius: 12px;
  background: linear-gradient(135deg, #eff6ff, #dbeafe);
  color: #1e3a8a !important;
  text-decoration: none !important;
}
.home-tile strong {
  font-size: 2em;
  line-height: 1;
}
.home-tile span {
  font-size: 0.8em;
  color: #1d4ed8;
}
.home-tile-wide {
  grid-column: 1 / -1;
  flex-direction: row;
  align-items: baseline;
  gap: 10px;
  background: linear-gradient(135deg, #2563eb, #3b82f6);
}
.home-tile-wide span,
.home-tile-wide em {
  color: #fff !important;
}
.home-tile-wide em {
  font-size: 1.05em;
  font-weight: 600;
}

/* Widget links navigate on click, so give them a pointer */
.home a {
  cursor: pointer;
}
```
