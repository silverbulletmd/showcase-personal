---
tags: finance
---
My money at a glance. The move changed my rent, so I’m checking the monthly plan and building up a cushion before the next big project. All amounts are fictional, in euros.

${widgets.grid({
  widgets.stat("Monthly spending", money.euros(budget.expenses), {
    tone = "neutral", sub = "From the monthly budget",
    onClick = function() editor.navigate("Monthly Budget") end,
  }),
  widgets.stat("Left to save", money.euros(budget.surplus), {
    tone = budget.surplus < 0 and "danger" or "success",
    sub = string.format("%.1f%% of take-home pay", budget.savingsRate),
    onClick = function() editor.navigate("Monthly Budget") end,
  }),
  widgets.stat("Emergency fund", money.euros(fund.saved), {
    tone = "accent", sub = "Target: " .. money.euros(fund.target),
    bar = math.min(1, math.max(0, fund.progress)),
    onClick = function() editor.navigate("Emergency Fund") end,
  }),
})}

# My worksheets
* [[Monthly Budget]] — income, bills and what’s left to save. Edit an expense to try a different month.
* [[Emergency Fund]] — savings, the target and a projection based on that budget.

${fund.forecast}

# Try a small win
${widgets.button("Save another €100", function()
  pageVars.set("saved", pageVars.get("saved", "Emergency Fund") + 100, "Emergency Fund")
end)}

${widgets.button("Reset example savings", function()
  pageVars.set("saved", 2400, "Emergency Fund")
end)}

Try saving €100: the fund card and forecast update, and the amount is saved on [[Emergency Fund]]. Then reset the example or change rent on [[Monthly Budget]] to see the effect across all three pages.

# How this works
These are ordinary Markdown pages with live calculations. Page Lua keeps each worksheet’s inputs and formulas in its own scope. This page reads both with `pageVars.all()`; the buttons write to another page with `pageVars.set()`. Space Lua in [[Library/Money]] supplies one shared currency formatter.

```page-lua
budget = pageVars.all("Monthly Budget")
fund = pageVars.all("Emergency Fund")
```
