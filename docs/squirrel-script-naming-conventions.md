# Squirrel_re Script Naming Conventions

Naming conventions are useful in programming, especially across a very large codebase, such as R5V's, so that multiple programmers can work together in harmony and so that code can be more easily read and understood. These sets of standards are meant to achieve a level of uniformity in code so as to increase efficiency and encourage open collaboration.

Squirrel_re is the name used for Respawn's fork of the Squirrel scripting language. Squirrel was used by Respawn because it was the main language for VScripts (Valve Scripting) and Respawn used (and continue to use) Valve's Source Engine, with heavy modifications, forked from the Portal 2 branch.

## File Names

Server only scripts: underscore prefix

```
_entitystructs.gnut
```

Client only scripts: cl_ prefix

```
cl_entitystructs.gnut
```

Shared scripts: sh_ prefix

```
sh_init.gnut
```

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

