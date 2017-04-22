
To avoid false operand dependencies, which would decrease the frequency when instructions could be issued out of order, a technique called register renaming is used. In this scheme, there are more physical registers than defined by the architecture. The physical registers are tagged so that multiple versions of the same architectural register can exist at the same time.