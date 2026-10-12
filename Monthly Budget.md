---
tags: finance
---
My monthly plan after the move. Rent is higher, but I can still put something aside. All figures in this showcase are fictional and in euros.

[[Money|Overview]] · [[Emergency Fund]]

# What comes in
Take-home pay: €${salary = 3200}

# What goes out
Edit a number below and pause for autosave. The totals and the emergency fund projection update together.

| Expense | Monthly amount (€) |
| --- | ---: |
| Rent | ${rent = 1150} |
| Utilities | ${utilities = 180} |
| Groceries | ${groceries = 320} |
| Transport | ${transport = 90} |
| Insurance | ${insurance = 60} |
| Eating out | ${eatingOut = 160} |
| Hobbies | ${hobbies = 120} |
| Subscriptions | ${subscriptions = 35} |

# Where that leaves me
Monthly spending: ${money.euros(expenses)}

Left to save: ${money.euros(surplus)}

Savings rate: ${string.format("%.1f%%", savingsRate)}

${surplus < 0 and "Over budget: reduce spending before planning a savings contribution." or "I’m putting the monthly surplus toward my emergency fund."}

# How this works
The inputs above are page variables. This `page-lua` block calculates derived values on this page; [[Emergency Fund]] reads them with `pageVars.get()`. The euro formatter comes from Space Lua in [[Library/Money]].

```page-lua
expenses = rent + utilities + groceries + transport + insurance + eatingOut + hobbies + subscriptions
essentials = rent + utilities + groceries + transport + insurance
surplus = salary - expenses
savingsRate = salary > 0 and surplus / salary * 100 or 0
```
