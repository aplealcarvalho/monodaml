
---

# Daml Fundamentals Certification Mandatory Project

Small project, mandatory for completing the DAML Fundamentals Certification.

This is a first approach of implementing a monopoly game using DAML language. Though not every feature of the game is implemented (in fact, only the player movement along the board and the rent/buy properties were included yet), it was a theme that I enjoyed to abord and that I hope to continue to develop on my free time. 

# Requirements

* Java 11 or greater
* Daml SDK version 2.6.0-snapshot.20221218.11169.0.9d007b31

For more info about setting up a proper environment to run a DAML project, check out [this](https://docs.daml.com/ops/requirements.html#).

# Start the Project
On the root of the project simply:
```
daml start
```

Wait for a few minutes until navigator is up

# Important details and usage
Some tests included on Test/Tests.daml (3 happy paths, 3 unhappy paths)

When starting up the project, the board will be automatically set up, and all the the needed contracts will be initialized. Three mocked parties will be onboarded and they will be available to login with via navigator.

For starting to play, using the bank party, exercise the choice StartNewTurn on Board providing a acceptable dice roll and lists for all active BuyProposal, FundsPayments and Receipts (These lists are intended to be used whith a proper frontend, at the start of the game they will all be empty). The player whose turn is currently will move to a new square. Case the target case be an unowned Property, a BuyProposal will be issued. Otherwise, a FundsPayment will be created to ensure that the player pays rent to another player.

After moving a player to an unowned Property Square, this property can be bought by him provided that his balance his higher than the property value. Exercising AcceptProperty on a BuyProposal with the player party will create a Receipt. The bank can then use the Receipt to make effective ownership change of the square (choice ChangeOwnership).

If the player moves to a previously owned property, then is mandatory that the player pays rent: a FundsPayment will be issued and Exercising the Pay choice will create a Receipt. The bank can then use the Receipt to transfer the funds between players (choice TransferFunds).

There can be no active BuyProposal, FundsPayments or Receipts at the start of each turn.

Board's choice EndTurn will ensure that there are no pending FundsPayments or Receipts before performing. If there aren't any of those contracts still active, then all BuyProposal contracts will be archived (the player may choose not to buy a property even though it's available or he can afford it).
