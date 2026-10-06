---
tags: list
---
Birds spotted on [[Photo Walks]], one line per sighting.

* Grey heron by the old bridge [count: 3] [light: early] #bird
* Kingfisher at the weir [count: 1] [light: overcast] #bird
* Little egret in the reeds [count: 2] [light: early] #bird

All birds:
${query[[
  from b = index.items "bird"
  select {
    Bird = b.name,
    Count = b.count,
  }
]]}
