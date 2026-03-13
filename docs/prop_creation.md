# Prop Creation

Props are Entities with associated Models, hence they are Model Entities.  

Lots of useful Prop-related functions are located in _utility.gnut  

# Prop Methods and Functions
```
Props that have simulated physics, using VPhysics (which incorporates the Havok physics engine)
Function definition starts at line 1822 of _utility.gnut

entity function CreatePropPhysics( asset model, vector origin, vector angles, int spawnFlags = 0 ) //    
```



```
TODO: Figure out what the differences between this and prop_dynamic are
Function definition starts at line 1762 of _utility.gnut

entity function CreatePropScript( asset model, vector ornull origin = null, vector ornull angles = null, int solidType = 0, float fadeDist = -1, bool dispatchSpawn = true )  
entity function CreatePropScript_NoDispatchSpawn( asset model, vector ornull origin = null, vector ornull angles = null, int solidType = 0, float fadeDist = -1 )  

Note: No DispatchSpawn() means that the Prop isn't actually spawned in the world. Props can be spawned in 3D only with DispatchSpawn().
```

```
TODO: Figure out what the differences between this and prop_script are
Function definition starts at line 1591 of _utility.gnut

entity function CreatePropDynamic( asset model, vector ornull origin = null, vector ornull angles = null, int solidType = 0, float fadeDist = -1, bool dispatchSpawn = true )  
entity function CreatePropDynamic_NoDispatchSpawn( asset model, vector ornull origin = null, vector ornull angles = null, int solidType = 0, float fadeDist = -1 )  
```

```
Lightweight Dynamic Props take fewer resources than regular Dynamic Props. To be used to save processing power.
Function definition starts at line 1689 of _utility.gnut

entity function CreatePropDynamicLightweight( asset model, vector ornull origin = null, vector ornull angles = null, int solidType = 0, float fadeDist = -1, bool dispatchSpawn = true  )
entity function CreatePropDynamicLightweight_NoDispatchSpawn( asset model, vector ornull origin = null, vector ornull angles = null, int solidType = 0, float fadeDist = -1 )
```

```

Function definition starts at line 1645 of _utility.gnut

entity function CreatePropShield( asset model, int impacteffectcolorID, vector ornull origin = null, vector ornull angles = null, int solidType = 0, float fadeDist = -1, bool dispatchSpawn = true )
```

```

Function definition starts at line 1723 of _utility.gnut

entity function CreatePropSurvival( asset model, vector ornull origin = null, vector ornull angles = null, int solidType = 0, float fadeDist = -1 )
```

```

Function definition starts at line 5950 of _utility.gnut

entity function CreatePropDoor( asset model, vector pos, vector ang )
```


```

Function definition starts at line 5926 of _utility.gnut

entity function CreatePropDeathBox_NoDispatchSpawn( asset model, vector ornull origin = null, vector ornull angles = null, var solidType = 0, float fadeDist = -1 )

```