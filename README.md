
## Pause
As simple as it sounds, pausing a game consist in stopping most of the core of the game to the bare minimum interactions decided by the games' developer itself. Whenever you go to setting, you check on your inventory,etc... most of the games focus solely on that task in order to save resources, and top give a thinking time to the player to perform the required actions out of the "in-game" environment.

![Skyrim Pause Settings Menu](http://i.imgur.com/qOyXw.jpg)

## The first instances of Pause in videogames  
Given that the first videogames console (widely available for the general public) was available around late 1972, it took long for this feature to reach the market. In 1976, the **_Fairchild Channel F_** console was launched, which came with the first own microprocessor, consoles previously sold were made with discrete electronic components (such as transistors, resistors,...) and discrete logic (mainly logic gates).  This allowed for the system to perform tasks simultaneously to the player, allowing for the first AI subroutines, and the pause feature, thanks to a communication between the cartridge and the main microprocessor.

![Fairchilde Channel F](https://cdn.arstechnica.net/wp-content/uploads/2016/02/Fairchild-Channel-F-640x421.jpg)

As you may have guessed, since then, there is flose to no game that comes without a pause function, when not involving online player to player interaction.

## How to Pause a game?  
This is fairly simple step. But before anything else, we need to keep in mind the following things:
1 What modules do we want to keep at every frame?
2 How are we going to catch up later?
In short, we need to make sure the game keeps running and the information required is shown on the screen, be it a menus screen, an inventory, a grey layer,...
And we need to make sure that any function that is depending on a specific timer is updated properly after finishing.

### Basics
To perform this in a hardcoded way (the best way), we require a pretty low amount of variables. We need to keep track of which modules will be shut down during each type of pause. For that we also need to have a list of modules and a list of types of pauses:

  enum Modules {
	  j1Window_ = 0,
	  j1Input_,
	  j1Render_,
	  j1Textures_,
  	j1Audio_,
  	j1FileSystem_,
  	SceneManager_,
	  j1Map_,
  	j1PathFinding_,
  	j1Fonts_,
  	j1Gui_,
	  j1Collision_,
	  HUD_,
	  j1Player_,
	  EntityManager_,
  	ModuleParticles_,
  	DialogManager_,

  	last_module_
  };

  enum Pause_Type {

	  General_ = 0,
	  Inventory_,

	  last_pause__

  };
