---
tags: list
---
Birds spotted on [[Photo Walks]], one line per sighting.

* Grey heron by the old bridge [count: 3] [light: early] #bird
* Kingfisher at the weir [count: 1] [light: overcast] #bird
* Little egret in the reeds [count: 2] [light: early] #bird

Best at first light:
${query[[
  from b = index.items("bird")
  where b.light == "early"
  select {
    Bird = b.name,
    Count = b.count,
  }
]]}
