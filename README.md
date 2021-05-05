# Responsible Managers (X4)
Station managers diligently report profits. Logical continuation of another mod from X: Rebirth with the same name (Responsible Managers https://github.com/Vectorial1024/responsible_managers)

The logical difference compared with the XR version would be that all stations, regardless of whether they are energy stations, would send in their profits reliably.

## Logic and Pseudo code
The logic of this mod is adapted from the similar feature from X: Rebirth. All credits of the algorithm should go back to EgoSoft. I am here only to show you that such algorithm exists.

### Profit Reporter
The Profit Reporter sends in station profits directly to your account. Basically it looks something like this:
```
event PlayerFactionTradeCompleted:
  station = tradeParties.where({type: station, faction: factions.player})
  if station.account >= station.maxBudget * 1.5:
    station.transferCreditsToPlayer(station.account - station.maxBudget)
  endif
endevent
```

In X: Rebirth, station budget rarely go beyond 10M, so the 1.5 multiplier made sense even for the more expensive stations. But in X4, if your stations are large enough, it is not surprising to see budgets up to 400M (or more!), where the 1.5 multiplier would simply be very difficult to reach.

I am still determining the threshold of station budget where I would apply a lower budget multiplier (or even just a flat difference) instead of the standard 1.5 multiplier for profit retrieval.

### Budget Replenisher
The Budget Replenisher is the opposite of the profit Reporter; it extracts money from your account to fill the budget needs of station accounts.

Because randomly extracting money from the player can bankrupt the player themselves, this feature is likely to be disabled at game start, but eventually and softly unlocked as the player becomes more and more wealthy. But still, the necessary criteria to trigger the Replenisher would be when the station has less than 50% of their budget remaining. Details are still being determined.

Perhaps I could make it such that the Replenisher can only move funds when the amount to be moved is less than a certain percentage of current player cash.

There should also be a control to temporarily disable the Replenisher so that the player could save up money for something else and not have their money "stolen" by managers. Or I could make it so that players need to enable Replenishment to certain stations aside the global control. Or something something.
