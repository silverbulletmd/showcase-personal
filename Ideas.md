Ideas noticed on walks and saved in the journal. Each one still lives on the day it was written.

${query[[
  from o = index.paragraphs("idea")
  where string.startsWith(o.page, "Journal/")
  order by o.page desc
  select "* " .. o.text .. " — [[" .. o.ref .. "|" .. string.sub(o.page, 9) .. "]" .. "]"
]]}
