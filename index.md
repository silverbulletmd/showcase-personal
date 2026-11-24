
${home.greeting "Sam"}

${widgets.commandButton("Quick note", "Quick Note")} ${widgets.commandButton("Today’s journal", "Journal: Today")}

${home.tiles()}

# Journal
${query[[
  from p = index.pages "journal"
  order by p.name desc
  limit 5
  select templates.pageItem(p)
]]}

# Projects
${query[[
  from p = index.contentPages "project"
  order by p.name
  select templates.pageItem(p)
]]}

# Lists
${query[[
  from p = index.contentPages "list"
  order by p.name
  select templates.pageItem(p)
]]}

# [[Money]]
My [[Monthly Budget|monthly budget]] and [[Emergency Fund|emergency fund]] are linked worksheets: change a number on one page and see the others recalculate.

# People
${query[[
  from p = index.contentPages "person"
  order by p.name select templates.pageItem(p)
]]}

# [[Ideas]]
${query[[
  from p = index.contentPages "idea"
  order by p.name
  select templates.pageItem(p)
]]}

# Up next
${query[[
  from t = index.tasks()
  where not t.done
  order by t.page desc
  limit 3
  select templates.taskItem(t)
]]}

# How this is built
Plain markdown pages, plus a few SilverBullet features:
* [Tags](https://docs.silverbullet.md/Reference/Tag): projects, lists, people, books and ideas are pages with a tag in their frontmatter. Each section above is a [query](https://docs.silverbullet.md/Reference/Space%20Lua/Integrated%20Query) over one tag.
* [Journal](https://docs.silverbullet.md/Guides/Journal): one page per day under `Journal/`, started from the built-in journal template, which tags it `#journal`. The latest five are listed above, and [[Ideas]] collects every `#idea` from journal pages.
* [Linked Mentions](https://docs.silverbullet.md/Reference/Concepts/Linked%20Mention): every person and project page shows where it was mentioned, without any extra work.
* [Page templates](https://docs.silverbullet.md/Reference/Concepts/Page%20Template): new book notes start from [[Library/Page Templates/Book]].
* [Space Lua](https://docs.silverbullet.md/Reference/Space%20Lua) and [Space Style](https://docs.silverbullet.md/Reference/Theming/Space%20Style): the greeting and tiles are small widgets in [[Library/Home]]. The quick actions are built-in [command buttons](https://docs.silverbullet.md/Reference/API/widgets).
* [Tag icons and schemas](https://docs.silverbullet.md/Reference/API/tag): books, ideas and people get an icon wherever they’re listed, and a book’s frontmatter is a form with a status picker, all set up in [[Library/Config]].
* [Theming](https://docs.silverbullet.md/Reference/Theming): SilverBullet’s own light theme, tinted blue with one accent color.
* [Page variables](https://docs.silverbullet.md/Reference/Space%20Lua#Page%20variables): [[Money]] connects two finance worksheets with page variables, formulas and buttons that update the saved Markdown.
