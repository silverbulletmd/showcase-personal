Every book I’m reading, have read, or want to read, collected from pages tagged `#book`.

${query[[
  from b = index.contentPages "book"
  order by b.rating desc nulls last
  select { Book = "[[" .. b.name .. "]]", Author = b.author, Rating = b.rating and string.rep("★", b.rating) or b.status }
]]}
