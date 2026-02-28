# Player Entity Methods and Functions


GetPlayerArray() // the local player is ALWAYS the first element of the returned array, meaning the easiest way to get the local player is with GetPlayerArray()[0] or the shorthand gp()[0]
GetPlayerArray_Alive()
GetPlayerArray_AliveConnected() // for iterating / indexing with for or foreach( alivePlayer in ... )
GetPlayerArrayOfTeam( player.GetTeam() )
GetPlayerArrayOfTeam_AliveConnected( player.GetTeam( ) )
GetPlayerTitansOnTeam( player.GetTeam() ) // the team is an int value
GetPlayerTitansReadyOnTeam( player.GetTeam() ) // the team is an int value
GetLocalViewPlayer()
GetLocalClientPlayer()
player.GetActiveWeapon( eActiveInventorySlot.SLOTNAME )
player.GetPlayerName()
player.GiveNormalWeapon()
player.TakeNormalWeapon()
player.TakeWeapons()
holster take weapons?
localViewPlayer.GetCockpit() // GetLocalViewPlayer().GetCockpit(), gets the player's first person "screen" / "hull" / "cockpit" entity
player.GetTeam()
player.SetTeam()
IsPlayer
IsPilot
player.GetPlayerNetInt( "tutorialContext" )
player.GetPlayerNetBool( "skydiveFreelookActive" )
player.GetPlayerNetEnt( "revivePlayerHealer" )
player.GetPlayerSettings()
player.GetPlayerSettingsMods()
player.GetPlayerModsForPos( pose )
SURVIVAL_GetPlayerInventory( entity player )
SURVIVAL_Loot_GetLootDataByIndex( itemSurvivalint ) // lootItem.GetSurvivalInt()
SURVIVAL_CountItemsInInventory( weapon.GetOwner() info.lootData.ref )
SURVIVAL_AddToPlayerInventory( entity player, string(?) lootRef, int amount)
SURVIVAL_RemoveFromPlayerInventory( player, vaultData.vaultKeylootType, 1 )
SURVIVAL_GetPrimaryWeaponsSorted( ownerPlayer )
SURVIVAL_GetLastActiveWeapon (player )
SURVIVAL_PlayerAllowedToPickup( player )
SURVIVAL_IsPlayerCarringLoot( player )
HolsterAndDisableWeapons( player )
DeployAndEnableWeapons( player )
player.HolsterWeapon()

GetPlayerUID()
weapon.GetWeaponOwner()
weapon.GetOwner()
weapon.GetParent()
GetPlayerFromTitanWeapon( entity weapon )



from _internal.nut 
GetPlayerStatArrayInt()




takeweaponsbytype
giveweaponsbytype?


player.p

