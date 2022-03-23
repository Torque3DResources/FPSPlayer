# FPS Player
A example module that implements various a basic FPS-style player character. In addition to implementing general movement and animations, also provides a set of generic weapon/equipment animations that other modules can hook against for quickly getting third person animations up and running with custom weapons.
 
# Usage and Instructions
### Installation
Copy entire FPSPlayer folder into the Torque3D project's data/ folder, restart the project if it's running and it'll be integrated.

### Player/Data
The PlayerData datablock is the DB used for players. It not only defines the 3d soldier model, but also defines a lot of values and settings to set up
movement, physics, collisions, health, etc. All properties that go into a standard FPS-style player character.
In addition to defining the effects, models and parameters for the player, it also implements function callbacks for mounting to other objects like vehicles
and handling collision and damage events, state changes such as death, and entering/leaving volumes like triggers, water or mission areas.

## callGamemodeFunction callbacks
Uses the core callGamemodeFunction function to signal to any active game modes active that we have a particular event happening. It behaves akin to a regular signal-listen system.
Here, we indicate to all active gamevmodes that the event "onDeath" is called when the Player's state changes to being dead, allowing game modes to respond and react to this, such as incrementing scores, reducing lives counts or respawning the player.

```
function PlayerData::damage(%this, %obj, %sourceObject, %position, %damage, %damageType)
{...
   if (%obj.getState() $= "Dead")
   {
       callGamemodeFunction("onDeath", %client, %sourceObject, %sourceClient, %damageType, %location);
   }
}
```

The actie gamemodes then can respond to this via the matched callback function and do it's own behavior.
In our example, it will push the MainChatHUD gui element to the Canvas stack so it will display, as well as prep the message vector.
```
function DeathMatchGame::onDeath(%this, %client, %sourceObject, %sourceClient, %damageType, %damLoc)
{
   // Clear out the name on the corpse
   ...

   // Switch the client over to the death cam and unhook the player object.
   ...

   // Display damage appropriate kill message
   ...

   // Dole out points and check for win
   ...
}
```
