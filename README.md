# BuoyancySystem
A system for buoyancy and boat physics in Unreal Engine 4.

There are a few main parts to this plugin:

* **BuoyantComponent**: This is an ActorComponent which can be added to anything which can float. This uses spheres to test for buoyancy -- faster than calculating how much of the object is underwater, but also less accurate and requires the user to manually specify where the spheres should be placed and how large they should be. There are options for setting the Displacement Ratio (how much force is applied upwards to provide buoyancy) and the Mass Multiplier (how much the force of gravity is multiplied by when the BuoyantComponent is falling downwards).

* **BuoyancyManager**: This is in charge of actually managing and creating the waves in the water. It uses overlapping clusters of Gerstner Waves to generate a realistic ocean. There is a MaterialParameterCollection which goes along with the BuoyancyManager; if this collection is set, then any defaults set up in the BuoyancyManager will be propagated to the MaterialParameterCollection. Keep in mind that this class is intended to be a child of the GameState class.

* **BoatVehicle**: This is a class inheriting from Unreal's `WheeledVehicle` class. The wheels on this boat are intended to be invisible, and the suspension has a very large drop value allowing the "wheels" to fall to the ocean floor to do their work. There is a very narrow base between the wheels, which causes a larger amount of tilt when the boat takes corners. There are also classes inheriting from the various `Wheel` classes and the like to support the `BoatVehicle` class. The `BoatVehicle` also has a `BuoyancyComponent` to allow it to move up and down with the waves.

* **ShipSystem**: This is intended for any extra parts which fit on the boat. These can either be visual parts, parts which add extra functionality to the ship, etc. The only class currently implemented that inherits from `ShipSystemComponent` is the `EngineSystem`, which provides appropriate engine noises when the ship is throttled up or down.

#Installation

First, make a `Plugins` folder at your project root (where the .uproject file is), if you haven't already. Then, clone this project into a subfolder in your Plugins directory. After that, open up your project's .uproject file in Notepad (or a similar text editor), and change the `"AdditionalDependencies"` and `"Plugins"` sections to look like this:

```
	"Modules": [
		{
			"Name": "YourProjectName",
			"Type": "Runtime",
			"LoadingPhase": "Default",
			"AdditionalDependencies": [
				// Any other dependencies go here
				"BuoyancySystem"
			]
		}
	],
	"Plugins": [
		// Other plugins go here  
		{
			"Name": "BuoyancySystem",
			"Enabled": true
		}
	]
```

You can now open up your project in Unreal. You might be told that your project is out of date, and the editor will ask if you want to rebuild them. You do. After that, open up the Plugins menu, scroll down to the bottom, and ensure that the "BuoyancySystem" plugin is enabled.

After that, you're done! You can subclass `ABoatVehicle` in C++ or Blueprints and tweak it to your heart's desire, until you have your very own boat!
