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
    from p = index.pages "journal"
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
/* Let the home widgets sit on the page without the usual widget frame */
.sb-lua-directive-block:has(.home) {
  border: none !important;
  background: none !important;
}

/* Greeting and date */
.home-greeting h1 {
  font-size: 2em;
  font-weight: 700;
  letter-spacing: -0.02em;
  margin: 0.2em 0 0;
}
.home-greeting p {
  margin: 0.1em 0 0;
  color: var(--subtle-color);
}

/* The row of buttons: styled by the built-in look */
.home-actions {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  margin: 4px 0;
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
  gap: 4px;
  padding: 14px 16px;
  border: 1px solid var(--ui-surface-border-color);
  border-radius: 12px;
  background: var(--ui-accent-surface-color);
  color: var(--root-color) !important;
  text-decoration: none !important;
  cursor: pointer;
}
.home-tile:hover {
  border-color: var(--tone-accent-soft);
  background: var(--tone-accent-soft);
}
.home-tile strong {
  font-size: 1.9em;
  font-weight: 700;
  letter-spacing: -0.02em;
  line-height: 1;
  color: var(--ui-accent-text-color);
  font-variant-numeric: tabular-nums;
}
.home-tile > span {
  font-size: 13px;
  font-weight: 500;
  color: var(--subtle-color);
}
.home-tile-wide {
  grid-column: 1 / -1;
  flex-direction: row;
  align-items: baseline;
  gap: 10px;
  border-color: transparent;
  background: var(--ui-accent-color);
}
.home-tile-wide:hover {
  border-color: transparent;
  background: var(--ui-accent-strong-color);
}
.home-tile-wide > span,
.home-tile-wide > em {
  color: var(--ui-accent-contrast-color) !important;
}
.home-tile-wide > span {
  opacity: 0.8;
}
.home-tile-wide em {
  font-size: 1.05em;
  font-style: normal;
  font-weight: 600;
}
```
