#meta

Shared formatting for [[Money]], [[Monthly Budget]] and [[Emergency Fund]]. The calculations live on the pages themselves.

```space-lua
money = money or {}

function money.euros(amount)
  return string.format("€%.2f", amount)
end
```
