# Color Correction Materials


Before a color correction overlay material can be used, it must first be registered with  
```
ColorCorrection_Register( string materialPath )
```

After a color correction material is registered, it can be used with  
```
ColorCorrection_SetWeight( material, weightFloat)

Example for Phase Shift / Phase Walk:

ColorCorrection_SetWeight( ColorCorrection_Register("materials/correction/fx_phase_shift.raw_hdr"), 5.0 )

// paths usually start with materials/correction and end with the _hdr suffix, from High Dynamic Range
```

![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/phaseshiftcolorcorrection.png)
![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/bloodhoundultcolorcorrection.png)

Function list:    
```

ColorCorrection_Register( string pathToAsset ) //  the material MUST be registered prior to use!

ColorCorrection_SetWeight( string pathToAsset, float weightFrom0To1) // at 0.0 it is disabled, at 1.0 it has maximum intensity

ColorCorrection_LerpWeight( string pathToAsset, int startingWeight, int endingWeight, float durationFromMinToMaxIntensity ) 

// lerp = linear interpolation

ColorCorrection_SetExclusive( string pathToAsset, bool ) // no compounding with other color correction curves, overrides everything

ColorCorrection_LoadAsync( string pathToAsset ) // for layering & compounding color corrections 

ColorCorrection_Release( string pathToAsset ) // for removing layers

ColorCorrection_PollAsync( string pathToAsset )
```