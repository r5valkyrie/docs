# Proxy Entities

## What Are Proxy Entities?

Proxies can be conceptualized / conceived of as indirect expressions of an entity, or client-sided entities (only the local player can see them) and ways to access those local / first person entities.  

It is useful to refer to the expression "by-proxy":  


"By proxy" means doing something indirectly, where an authorized person or agent acts on your behalf rather than you doing it yourself. It indicates substitution, allowing actions—like voting, signing documents, or experiencing events—to be completed via a representative.  


It is also useful to refer to the meanings of the term "proxy":  

- The authority to represent someone else, especially in voting.  

- A figure that can be used to represent the value of something in a calculation.  


## What Are Proxy Entities Used For?

Respawn used and uses proxies in three main ways in Titanfall and Apex Legends:

1) To effect changes on a client's first-person view (getting the player's "Cockpit" entity (basically the first-person camera) and rendering screen effects like Stim, Gravity Suction, Armor Damage Effects, EVO Shield Effects)  
2) To preview ability / gadget placements and inform the player of the validity of the placement locations (i.e.: Care Packages, Fence Nodes, Mobile Respawn Beacons, Evac Towers, Rampart Walls, The Mounted Sheila HMG, Revenant's Death Totem)  
3) To show a flawed APPROXIMATION of what other clients see doing when spectating another player  

As with any other online multiplayer game, Titanfall and Apex Legends use SERVER / CLIENT code gating. Additionally Respawn use a third VM just for UI.  

The division into different VMs means that the world and different entities are rendered differently depending on the Client viewing them  

As such, only the local Client can see their headless, first-person view body and weapon viewmodel, except for when other Clients are spectating that Client player.  

The player's first person body, weapon viewmodel and FX are rendered on top of the world (observe how gun models never clip into walls in first-person).  

For other player clients looking at that player, a different, third-person character model is used. A high-contrast example would be Revenant. His third-person character model is significantly taller and wider than his first-person body model. This is evident when previewing the two-models using RSX (mdl/Humans/class/heavy/pilot_heavy_revenant.rmdl and mdl/Weapons/arms/pov_pilot_heavy_revenant.rmdl).  


![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/revenant3p.png)
![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/revenant1p.png)


Tall character models are shrunk down in first-person so that they can properly use the already existing, common / shared weapon rigs. For an illustration of what happens when a character model is not adjusted to account for a rig, it is possible to use Revenant's Silence tactical ability with Horizon's first person body model. Horizon's left hand becomes highly twisted and deformed. 

![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/horizonwithsilence.png)


## Hologram Proxies

Hologram proxies use a special shader assigned with DeployableModelHighlight( entity proxy) which draws them with a special transparent shader with blue flashes and interlaced lines. There is also a version for displaying invalid placement positions: DeployableModelInvalidHighlight( entity proxy ); this is the same as DeployableModelHighlight() except the highlight is red.  

These are CLientside Dynamic Props (they are not rendered for other players).

![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/carepackageproxyvalid.png)
![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/carepackageproxyinvalid.png)
![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/wattsonfenceproxy.png)
![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/pathfinderziplineproxy.png)
![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/mobilerespawnbeaconproxy.png)
![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/rampartwallproxy.png)



## Rendering KeyValues

Important model rendering KeyValues:  

- proxy.kv.renderamt controls the alpha channel of the model's materials (controls transparency / opacity)  

- proxy.kv.rendercolor controls the color of the model's emissive map  

- proxy.kv.rendermode controls the rendering mode of the model  

Depending on the rendermode (alpha, no alpha, etc.) rendercolor can have 3 or 4 values (the 4th value representing the alpha channel)  

## Cockpits

In Respawn terminology, the "Cockpit" stands for the fullscreen player view / camera entity // the term "cockpit" is a remnant from Titanfall, which had literal cockpits for Titans  

GetLocalViewPlayer() // gets the player entity that is currently on display on the local client  

GetLocalViewPlayer().GetCockpit().GetModelName() // used for both Titans and Pilots, as Titans in first-person are another type of player class that the player's "Soul" is transferred to. Many refenreces to "Souls" and "Titan Souls" can be found in the codebases of Titanfall 1, Titanfall 2 and Apex Legends.  

```
int fxHandle = StartParticleEffectOnEntity( cockpit, GetParticleSystemIndex( PHASE_TUNNEL_1P_FX ), FX_PATTACH_ABSORIGIN_FOLLOW, -1 )  
```

![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/stimeffect.png)
![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/bloodhoundeffect.png)
![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/gravlifteffect.png)
![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/blackholeeffect.png)

Note: some screen effects are not cockpit particle effects but screen VGUI overlays, such as the Arc Star "EMP" effect.

![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/arcstareffect.png)
![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/devscanline.png)
![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/empflow.png)
![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/mosaicnoise.png)
![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/titanempvmt.png)


## Visibility Flags

```
entity.kv.VisibilityFlags = ENTITY_VISIBLE_TO_ENEMY

// for multiple flags, use bitwise operators: (ENTITY_VISIBLE_TO_FRIENDLY | ENTITY_VISIBLE_TO_ENEMY)

ENTITY_VISIBLE_ONLY_PARENT_PLAYER  
ENTITY_VISIBLE_TO_OWNER  
ENTITY_VISIBLE_EXCLUDE_PARENT_PLAYER  
ENTITY_VISIBLE_TO_FRIENDLY  
ENTITY_VISIBLE_TO_ENEMY  
ENTITY_VISIBLE_TO_EVERYONE  
ENTITY_VISIBLE_TO_NOBODY  
```



## Garbage Collection
```
entity.Destroy() // instant destruction
entity.Dissolve() // gradual dissolution effect until it disappears
```






selective rendering  

i.e. rampart walls look different to enemies and friendlies -> render flags  

different models / particle effects / highlights / outlines  

one way walls?  


entities rendered only on the local client for the player placing stuff  

