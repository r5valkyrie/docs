# Color Correction Materials


Before a color correction overlay material can be used, it must first be registered with


ColorCorrection_Register( string materialPath )

After a color correction material is registered, it can be used with


ColorCorrection_SetWeight( material, weightFloat)

Example:

ColorCorrection_SetWeight( ColorCorrection_Register("materials/correction/fx_phase_shift.raw_hdr"), 5.0 )

