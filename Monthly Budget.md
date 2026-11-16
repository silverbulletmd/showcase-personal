---
tags: finance
---
My monthly plan after the move. Rent is higher, but I can still put something aside. All figures in this showcase are fictional and in euros.

[[Money|Overview]] · [[Emergency Fund]]

# What comes in
Take-home pay: €${salary = 3200}

# What goes out
Edit a number below and pause for autosave. The totals and the emergency fund projection update together.

${rent = 1150} 
${utilities = 180}
${groceries = 320}
${transport = 90}
${insurance = 60}
${eatingOut = 160}
${hobbies = 120}
${subscriptions = 35}

# Where that leaves me
Monthly spending: ${money.euros(expenses)}

Left to save: ${money.euros(surplus)}

Savings rate: ${string.format("%.1f%%", savingsRate)}

${surplus < 0 and "Over budget: reduce spending before planning a savings contribution." or "I’m putting the monthly surplus toward my emergency fund."}

# How this works
The inputs above are page variables. The `local` ones below calculate derived values without showing them; the sections above format them, and [[Emergency Fund]] reads them with `pageVars.all()`. The euro formatter comes from Space Lua in [[Library/Money]].

${local expenses = rent + utilities + groceries + transport + insurance + eatingOut + hobbies + subscriptions}
${local essentials = rent + utilities + groceries + transport + insurance}
${local surplus = salary - expenses}
${local savingsRate = salary > 0 and surplus / salary * 100 or 0}
