# Responsible Managers (X4)
Station managers diligently report profits, again. Logical continuation of another mod from X: Rebirth with the same name (Responsible Managers https://github.com/Vectorial1024/responsible_managers)

The logical difference compared with the XR version would be that all stations, regardless of whether they are energy stations, would send in their profits reliably.

## The Problem

Apparently, your stations are never profitable. I say "apparently" because you will often see station managers rolling in credits, credits that somehow are not transferred up the chain of command to you with no apparent reason. This is the same situation as the meme/skit depicted in the original X Rebirth Responsible Managers Steam page:

![Meme/skit of the original Responsible Managers](https://steamuserimages-a.akamaihd.net/ugc/964220282100047391/9333B1D32713A85A871E1C60C82A82A3286E1D4D/?imw=637&imh=358&ima=fit&impolicy=Letterbox&imcolor=%23000000&letterbox=true)

> In case the image failed to load:
> 
> ```
[Energy Array] At your service Sir.
[Ren Otani] Show me your account, please.
[Betty] Account of manager: 19562318 Cr (Translator Note: XR Energy Cell ave price 5 Cr @; ave revenue of full station without secondary resources (8 * 8400 * 5) approx 336000 Cr/hour; this indicates approx 58 hours of unreported revenue)
[Yisha Tarren] Why aren't you sending in Credits? You know we are a little bit short -
[Energy Array] I can reassure you that I am working according to the Manual here.
[Ren Otani] bruh
[Energy Array] I'll let you get on Sir.
```

In X Rebirth, player-to-player trading will actually move credits between Manager accounts, but such trading will not trigger the budget self-check procedure: whenever the money floats above 150% of the budget, transfer the surplus (compared to the budget) back to the player. The original Responsible Managers fixed that, with a bit of extra effort made on Energy Array stations since they have 0 budget (they somehow do not consider secondary wares).

In X4 Foundations, the same situation happens again, yet the cause is currently unknown. This is perhaps the reason why mods like [Standalone Station Manager Credit Transfer](https://www.nexusmods.com/x4foundations/mods/270/) exist in the first place.

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
