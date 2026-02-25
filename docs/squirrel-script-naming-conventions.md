# Squirrel_re Script Naming Conventions

## Variables

In general, variables should use lowerCamelCase

```
var someVariable

int someInteger = 1

float someFloatingPointNumber

string someString = "somestring"
```

## Functions

Function names should use CamelCase

```
global function SomeFunction
```

## Constants

Constants should be designated in ALL CAPS. They can use underscores
```

global const CONST_TABLE = {}

const asset NEMESIS_FX_IDLE_MAGNET_FP = $"P_wpn_nem_idle_magnet_FP"
```

## Enums

Enums should start with lowercase e and use lowerCamelCase

```
global enum eActiveInventorySlots
{
    
}
```

## For-loop iterators

"For-loop" iterators should be named "i", from "index"

```
for(int i = 0; i< max; i++)
```

