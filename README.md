
## Hit-Pause
The Hit-Pause is a graphical effect that is done in order to emphasize a specific action, normally accompained by different visual On-Hit effects:  
![Hit Pause Example](https://i.makeagif.com/media/5-25-2015/15o23B.gif)  
![Hit Pause Example 2](http://rivalsofaether.com/blog/wp-content/uploads/2015/05/big_hit_gif.gif)
A part from a graphical effect, it can be used in order to give the user a small window of time to decide and input its next action, also called buffer time. Most fighting games feature this in order to perform buffered action to make easier the connection of different moves:  
![Karen Combo](https://media.giphy.com/media/3o85gbdVDqEOMcmReU/giphy.gif)
![Guilty Gear Combo](https://justicesoultuna.files.wordpress.com/2013/05/ibfumz81jvoma6.gif)  

## Implementantion
During a base hit pause, you are stopping the game logic for a couple frames, only projecting a specific frame. In other words, you will be blitting a specific frame over and over during a certain amount of time, then continuing the action.  
***
## Camera Shake
A pretty short topic to present. Whenever you are hit in videogames, an earthqueake happens, a boss appears, someon screeches,... we get a visual confirmation of what is happenning, not only through what we are seeing (a volcano erupting or a dragon screech) but with the screen shaking from side to side, up and down,.. This is camera shake.  
![Camera Shake GIF](https://media3.giphy.com/media/TqWxxYqMhocFi/giphy.gif)  
This is fairly time consuming to get right, maybe it will never look quite right, it takes a lot of time adjusting settings to look well.

## Implementation
Asuming you are already working with a camera in your game, that control what is being blit and what not and acts as a pivot to where things will be blit, we are basically creating a shake effect by changing this camera's position repeatedly.  
This functions will require quite a bit of controller metrics, as we need to sync it properly to the framerate adn make is as less clanky as possible. We need basic metric to control the effect:

	float quantity;		//Control the amount of shakiness
	float duration;		//Time the effect will take effect
	int counter;		//Keep track of time
	int shake_interval;	//Every how much the effect is updated
	int shake_ret;		//What will be changed and will modify the camera
	bool trigger_shake;	//Allow or deny the effect
	
Entering more in detail, the counter we will use, is in order to take care of how much time we will need. Given that we call with the duration in seconds, we need a float for short spurts of shake and the shake interval is basically a way to limit the times it is being changed from side to side.  
But why do we need to limit how many times it is being updated? Basically we are syncing this to the program's framerate in order to get a consistent effect, if we sync it to a timer, it might sometimes return same values in terms of pair and odd numbers, so we make sure by having our own frame counter.  

The basic function will be updating as long as the shake is activated, but we don't want to continuously jump from side to side as it will create some visual errors. That's the reason we need a shake interval that with the counter changes from side to side depending on if the frame we are currently on is pair or odd, so we change from side to side but not every frame.  
In order to achieve this we need the shake_interval to be an odd number, because when added consecutively they give odd-pair-odd-pair... series.  

Whenever we want to start a camera shake, we need to activate it giving him the strength and duration desired. As we already planned to have the shake updated every frame while activated, we only need to feed him the values that change the effect. Then this will put the values in the previously created protected variables.
	
	void Activate_Shake(float quantity_, float duration_){
		//assign corresponding values
		//initiate counter to 0 and put the shake_interval to an odd number (3 is the best result so far)
		//turn trigger_shake to true to "activate" the shake
		
		//Some values will be either too big or small, I suggest you put a limit to them
		//For example I limit to 100 and use a scale from 1 to 100 for the quantity and limit the duration to 10sec
		//The shake interval has to be calculated related to the quantity and duration
		//The more duration you have, the more updates you need at a time
		//The more quantity you have, the less updates you need
	}
	
Finally we have to create the update function for the shake:

	void Camera_Shake(){
		//update the counter each time it enters
		
		//While making the loop, take into account that we only want to switch sides every x frames
		//We need to measure if x frames have passed easily
			//We check if the current frame is pair or odd and then decide to which side we are shaking
		
		//If X frames have not passed, we still have to update the shake, but in the direction we were shaking previously
			//Check if shake_ret was negative or positive, as easy as that
			//Then update shake_ret accordingly
			
		//Now we update the quantity, as we want to smooth out the
		//Basically we want to keep reducing the quantity of shakiness as time goes by
		
		//Check that the counter is over the duration, keeping in mind that you input duration in seonds
			//If so reset all variables, including shake_ret
			
	}
	
Good Job! You now have a Camera Shake, now you only have to add the shake_ret to the camera position value you want, but only when the shake has been triggered.

## Exercise:
### Todo 1: Define the variables used to calculate the shake
The variables mentioned beforehand, that will be used to keep track and calculate the shakiness.
### Todo 2: Declare the 2 main functions, Activate and do the Shake
Keep in mind that the Activate function only has to receive the values you want to change calculations with, and SHake does not need anything as we have defined the variablesin the same header and will be shared.
### Todo 2.2: Define the Activate function
Put the limits to the quantity and durations, you don't want infinite and absurd amounts of shakiness.
### Todo 3: Define the Shake Function
Hints/Explanations are in the handouts.

![Download the Exercise](https://github.com/MarcFly/Pause-CameraShake/releases/tag/0.1)
