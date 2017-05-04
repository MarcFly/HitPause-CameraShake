
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
For the sake of simplicity and availability, we will be starting from the Input module Header, as it will be the only one to always be working until we close the game. 
To perform this in a hardcoded way (the best way), we require a pretty low amount of variables. We need to keep track of which modules will be shut down during each type of pause. For that we also need to have a list of modules and a list of types of pauses:  

 	enum Modules {  
 
	j1Window_ = 0,  
	j1Input_,  
	j1Render_,  
	etc...  
	
	last_module_  
	
	};  
  
	enum Pause_Type {  

	General_ = 0,  
	Inventory_,  
	etc...  
  	
	last_pause__  

	};    

Then we create a container to later decide which modules will be updated during pause or not, and because we are hardcoding this, a auxiliary pause that will help us in times of need:  

	bool pause_array[last_module_][last_pause_type_];  
	bool pause2[last_pause_type_];  

Finally we want every module to have a variable that controls if it's pause or not. We simply create a _*bool pause;*_ in the base module class. Now every module has its pause "button", but our system doesn't know which modules to pause or not, we have to initiate the arrays:   

	void j1Input::Init_Pause_Array()  
	{  
  
	for (int i = 0; i < last_module_; i++)  
		for (int j = 0; j < last_pause_type_; j++)  
			pause_array[i][j] = false;  

	for (int j = 0; j < last_pause_type_; j++)  
		pause2[j] = false;  

	// General Pause  
		pause_array[modules_want_to_pause_][pause_type_] = true;  

	// Inventory Pause  
		pause_array[modules_want_to_pause_][pause_type_] = true;  
	}  

Before anything else let's understand how we will effectively pause the game. In short words, we don't want unnecessary "junk" to be wasting our precious computer resources while we perform certain actions, so we will avoid going through what is unnecessary.  
The main functions are 2, required to either start or stop the pause. These 2 functions are pretty similar, they both have to go through each one of the modules and either activate or deactivate the pause (turn true or false).  
For this we loop through a list containing all modules and changing the _*bool pause*_ value to true if that is the case.  
We also turn to true the auxiliary pause as it helps during other processes.  
These 2 functions aren't in need of returning any value.  

Now we start workign on the main game loop, where we call each individual update for each module and add a condition requesting if the pause value of each module allowing pass only if the value is not true (so pause is not active).

For the modules that require specific attention, mainly the ones that are calling a draw function to show things on screen, we do the good old conditional and only let out the draw.

`bool ModuleX::Update(){`

	`if(pause2[pause_type_] == false)`
	`{`
		`//Allow the update if there is no pause active...`
	`}`
	`ToDraw(...)`
	`return ret;`
`}`

___________________________________________________________________________________________________________________

##Camera Shake
A pretty short topic to present. Whenever you are hit in videogames, an earthqueake happens, a boss appears, someon screeches,... we get a visual confirmation of what is happenning, not only through what we are seeing (a volcano erupting or a dragon screech) but with the screen shaking from side to side, up and down,.. This is camera shake.  
!(https://media3.giphy.com/media/TqWxxYqMhocFi/giphy.gif)  
This is fairly time consuming to get right, maybe it will never look quite right, it takes a lot of time adjusting settings to look well.

##Implementation
Asuming you are already working with a camera in your game, that control what is being blit and what not and acts as a pivot to where things will be blit, we are basically creating a shake effect by changing this camera's position repeatedly.  
This functions will require quite a bit of controller metrics, as we need to sync it properly to the framerate adn make is as less clanky as possible. We need basic metric to control the effect:

	float quantity;		//Control the amount of shakiness
	float duration;		//Time the effect will take effect
	int counter;		//Keep track of time
	int shake_interval;	//Every how much the effect is updated
	int shake_ret;		//What will be changed and will modify the camera
	bool trigger_shake;	//Allow or deny the effect
	
Entering more in detail, the counter we will use, is in order to take care of how much time we will need. Given that we call with the duration in seconds, we need a float for short spurts of shake and the shake interval is basically a way to limit the times it is being changed from side to side.  
But why do we need to limit how many times it is being updated? Basically we are syncing this to the program's framerate in order to get a consistent effect, if we sync it to a timer, it might sometimes return same values in terms of pair and odd numbers, so we make sure by having our own frame counter. The basic function will be updating as long as the shake is activated, but we don't want to continuously jump from side to side as it will create some visual errors. That's why we need a shake interval that along with the counter changes from side to side depending on if the frame we are currently is pair or odd, and that is a divisor of the interval, so we change from side to side but not every frame.  
In order to achieve this we need the shake_interval to be an odd number, because when added consecutively they give odd-pair-odd-pair... series.

Whenever we want to start a camera shake, we need to activate it giving him the strength and duration desired. As we already planned to have the shake updated every frame while activated, we only need to feed him the values that change the effect. Then this will put the values in the previously created protected variables.
	
