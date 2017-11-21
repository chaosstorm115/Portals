# Portals

Adds portals that can teleport players (and only players!). Build the portals using the portal gun: Left click for the orange portal, SHIFT+ Left click (ghost build) for the blue portal. Every player can have one set of portals which can be distinguished using the number next to the portal. Adds combustible lemons that work similiar to cluster grenades, just with fire. This mod can also be found on [the forums.](https://forums.factorio.com/viewtopic.php?f=93&t=44305)

Known issues: The teleportation sound does not play for the player using the portal if the portals are on different surfaces. It does however play for any other player near the portals. This is an issue with the game, not the mod.

## Custom events

on_player_teleported_event:
- When a player teleports using a portal.
- Contains:
  - player = LuaPlayer, the player that is teleporting
  - old_position = Position, the old position of the player
  - target_portal = LuaEntity, the portal the player is teleporting to

on_player_placed_portal_event:
- When a player places a portal using the portal gun.
- Contains:
  - portal = LuaEntity, the portal that was placed
  - player = LuaPlayer, the player that placed the portal, which is the player the portal belongs to
	
### Example usage

	script.on_load(function()
		script.on_event(remote.call("portals", "on_player_teleported"), function(event)
			game.print("Woosh!")
			game.print(event.player.name .. " teleported from " .. serpent.line(event.old_position) .. " to " .. serpent.line(event.target_portal.position))
		end)
	
		script.on_event(remote.call("portals", "on_player_placed_portal"), function(event)
			game.print(event.player.name .. " created " .. event.portal.name .. " at " .. serpent.line(event.portal.position))
		end)
	end)
	
	script.on_init(function()
		script.on_event(remote.call("portals", "on_player_teleported"), function(event)
			game.print("Woosh!")
			game.print(event.player.name .. " teleported from " .. serpent.line(event.old_position) .. " to " .. serpent.line(event.target_portal.position))
		end)
		
		script.on_event(remote.call("portals", "on_player_placed_portal"), function(event)
			game.print(event.player.name .. " created " .. event.portal.name .. " at " .. serpent.line(event.portal.position))
		end)
	end)


## Remote functions

build_portal_a: function(player, surface, position)
- Build an orange portal.
- Parameters:
  - position: Position of the new portal
  - surface: LuaSurface, the surface of the new portal
  - player: LuaPlayer that the portal belongs to. This player can't have more than one pair, build_portal will delete any excess portals

build_portal_b: function(player, surface, position)
- Build a blue portal.
- Parameters:
  - position: Position of the new portal
  - surface: LuaSurface, the surface of the new portal
  - player: LuaPlayer that the portal belongs to. This player can't have more than one pair, build_portal will delete any excess portals

destroy_portal: function(entity)
- Destroy the given entity if it is a portal (also destroys label + animation).
- Parameter:
  - entity: LuaEntity, the portal to destroy

disable_long_distance_placing: function(bool)
- Change whether the blue portal can be placed from a long distance.
- Parameter:
  - bool: Boolean, if this is true the player can never place from a long distance, if it is false, the player can place from a long distance depending on the mod option
	
### Example usage in the console

	/c remote.call("portals", "build_portal_b", game.player, game.player.surface, game.player.position) --place at current player position
	/c remote.call("portals", "destroy_portal", game.player.selected) --destroy currently selected portal
	/c remote.call("portals", "disable_long_distance_placing", true) --disable long distance placing


## Changelog

0.2.8

- Added remote functions
- Added custom events
- Nicer entity code

0.2.7

- Added setting to disable the ability to place blue portals from long distances.
- Cleaned up entity code

0.2.6
- Fixed possible desync (Thank you Nexela!)

0.2.5
- Added sounds to the portal gun, the portals and teleportation
- Added combustible lemons
- Extended teleportation delay to 47 ticks (nearly a second)
- Fixed that it was easy to miss the portal when running fast
- Improved the code
- Removed the ability to migrate from version 0.1.5
- Fixed that portals would break when renamed
- Fixed that portals would show the researched lab speed in their tooltip
- Fixed that the technology GUI would show if the portal was placed when no labs were placed for this force

0.2.3
- Fixed error that occured when using the mod in version 0.15.12

0.2.2
- Fixed localisation
- Adjusted graphics
- Portal gun recipe no longer uses processing units

0.2.1
 - same as 0.1.10 but for 0.15
 
0.1.11
- Adjusted graphics

0.1.10
- Portal orb is now a portal gun
- New textures by u/Its-M4RC05 on reddit

0.1.9
- Fixed that the animation of the old portal wouldn't get deleted when a player placed a new portal on another surface.

0.1.8
- Extended teleportation delay to 40 ticks (0.666 seconds)
- Fixed that sometimes portals were on the wrong surface when upgrading from version 0.1.5

0.1.7
- Every player now has their own set of portals. This means that there can be 2*(number of players) portals in the game
- Portals are now numbered (Orange portal 1 teleports to Blue portal 1 and so on)
- Portals are now animated, textures by u/Its-M4RC05 on reddit
- Added a 0.5 second delay between teleporting (per player)
- Portals are no longer destroyed when path tiles (concrete etc) are placed over them

0.1.5
- Initial release
