
${home.greeting "Sam"}

${home.actions()}

${home.tiles()}

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
