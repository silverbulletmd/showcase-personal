Ideas noticed on walks and saved in the journal. Each one still lives on the day it was written.

${query[[
  from o = index.items "idea"
  where table.includes(o.itags, "journal")
  order by o.page desc
  select "* " .. o.text .. " — [[" .. o.ref .. "|" .. string.sub(o.page, 9) .. "]" .. "]"
]]}
