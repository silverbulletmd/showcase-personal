---
tags: finance
---
A cushion for the unexpected. My example goal is three months of essential bills. I’m leaving eating out, hobbies and subscriptions out of that target.

[[Money|Overview]] · [[Monthly Budget]]

# The plan
Saved so far: €${saved = 2400}

Months of essential expenses to cover: ${targetMonths = 3}

# Progress
${widgets.stat("Emergency fund", money.euros(saved), {
  tone = remaining == 0 and "success" or "accent",
  sub = string.format("%.0f%% of %s", progress * 100, money.euros(target)),
  bar = math.min(1, math.max(0, progress)),
})}

Essential monthly bills from [[Monthly Budget]]: ${money.euros(budget.essentials)}

Still to save: ${money.euros(remaining)}

Planned monthly contribution: ${money.euros(contribution)}

${forecast}

This projection assumes the budget stays the same and ignores interest. Change the budget or target to try another plan.

# Try it
${widgets.button("Save another €100", function()
  pageVars.set("saved", pageVars.get("saved") + 100)
end)}

${widgets.button("Reset example savings", function()
  pageVars.set("saved", 2400)
end)}

The buttons update the saved amount in this page’s Markdown. They are a way to try the worksheet.

# How this works
The budget is read from another page. Its essential expenses set the target, and its surplus sets the contribution. Editing either page recalculates this projection. The buttons use `pageVars.set()` to rewrite the plain `saved` input above. The `local` values below do the math without showing it.

${local budget = pageVars.all("Monthly Budget")}
${local target = math.max(0, targetMonths * budget.essentials)}
${local remaining = math.max(0, target - saved)}
${local contribution = math.max(0, budget.surplus)}
${local progress = target > 0 and saved / target or 1}
${local forecast = remaining == 0 and "Target reached — the cushion is in place."
  or contribution > 0 and "At this pace: " .. math.ceil(remaining / contribution) .. " more months to reach the target."
  or "No monthly surplus available. Adjust the budget to make room for saving."}
