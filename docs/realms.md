# Realms

Realms are the method used by Respawn to control Entity visibility to other Entities sharing one server.

An analogy for a realm would be a "pocket dimension" that certain Entities share. Those Entities have visibility of each other only if and while they share the same realm, allowing many different events to occur concommitently on the same server. A realm is essentially a discrete entity container.

This is the reason why, in Retail Apex Legends, players sometimes see effects, screen shake, etc. appear out of nowhere - it's a result of Entities from other realms (weapons, abilities, ordnance, etc.), cast by players on the same server, but which are not visible to each other. Improper realm handling makes particle effects, screen shake, etc. visible to players that do not share the same realm and should not see them.



# Entity class realm methods

entity.AddToRealm(realm)

entity.AddToOtherEntitysRealms( entity )

entity.RemoveFromAllRealms()

entity.DoesShareRealms( entity )

entity.IsInRealm( realm )

player.GetRealms()

entity.AddToAllRealms()


# Other realm related functions

ClearActiveProjectilesForRealm( int realm)

Survival_GetPlayerRealm( entity player )

CopyRealmsFromTo( entity source, entity target ) 

SetRealms( entity, [ newRealm ] ) <- the second argument is an array containing the realm ids

AddToUltimateRealm( entity, entity )

Examples:

AddToUltimateRealm( weapon.GetWeaponOwner(), missile )
AddToUltimateRealm( weapon.GetWeaponOwner(), shake )
AddToUltimateRealm( weaponOwner, targetEffect )