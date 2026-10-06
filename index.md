
${home.greeting "Sam"}

${home.actions()}

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
* [Tags](https://docs.silverbullet.md/Tag): projects, lists, people, books and ideas are pages with a tag in their frontmatter. Each section above is a [query](https://docs.silverbullet.md/Space%20Lua/Integrated%20Query) over one tag.
* [Journal](https://docs.silverbullet.md/Journal): one page per day under `Journal/`, started from the built-in journal template, which tags it `#journal`. The latest five are listed above, and [[Ideas]] collects every `#idea` from journal pages.
* [Linked Mentions](https://docs.silverbullet.md/Linked%20Mention): every person and project page shows where it was mentioned, without any extra work.
* [Page templates](https://docs.silverbullet.md/Page%20Template): new book notes start from [[Library/Page Templates/Book]].
* [Space Lua](https://docs.silverbullet.md/Space%20Lua) and [Space Style](https://docs.silverbullet.md/Space%20Style): the greeting, quick actions and tiles are small widgets in [[Library/Home]].
