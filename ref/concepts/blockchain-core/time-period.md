# CESS Time Periods
The time periods of CESS are **Slot**, **Epoch** and **Era**.

## Slot 
Slot is a discrete time unit of 6 seconds length. Each slot can contain a block, but it may not. 
Every 6 seconds is a slot, that is, 1 block is generated every 6 seconds in average.

## Epoch
An epoch is a time duration  that is broken into smaller time slots. Each slot has at least one slot leader who has the right to propose a block.

Every 600 slots becomes an epoch, that is, 1 epoch per hour, and each epoch will generate about 600 blocks.

## Era
A (whole) number of Epochs, which is the period that the validator set (and each validator's active nominator set) is recalculated and where rewards are paid out.

Each era has 6 epochs, that is, 6 hours. At the beginning of the 5th epoch, the validators of the next era are determined; at the end of the 6th epoch, the next era is entered, and new validators start working.
Each era will generate about 3600 blocks.
