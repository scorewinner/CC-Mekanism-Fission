# Computercraft Mekanism Fission Reactor Script
Computercraft script to control Fission Reactors from Mekanism


Credit for the original script goes to [@InternetUnexplorer](https://github.com/InternetUnexplorer) 

I've added the modified Programs to this repo, you can also use these directly instead of editing it yourself, be sure to copy the link to the RAW FILE!


## Setting up
1. Get yourself an Advanced Computer, 18 Monitors (OPTIONAL), at least 3 Wired Modems and some Networking Cable
2. Connect the wired modems to the Fission Reactor Logic Adapter, the Turbine Valve and the Computer (Additionally to the Displays if they are not adjasoned to the Computer)
3. Right click on all Modems. You should get a message in chat that the devices are now on the network
4. open the Computer and type ```wget https://gist.githubusercontent.com/InternetUnexplorer/ea13f1713d325b914126bcfb9b35e6fd/raw/22178936686a875601f31741d6b2de385d3aa86f/reactor.lua ``` 
5. If you have Monitors connected, start the Program like this: ``` monitor monitor_1 reactor.lua ``` (If the Monitors are adjasoned to the Computer, you can use ``` monitor left/right/top/bottom/back reactor.lua``` depending on where the Monitors are positioned, use the correct direction)
6. Place a Lever on top of the Computer and flick it to start monitoring your reactor and turbine

## Disabling Turbine Monitoring 
You may want to remove the turbine monitoring from the script if you don't have a turbine or maybe you have the turbine set to "dump excess steam" or something.
If you want to remove the Turbine from the Monitoring, you will need to remove a couple lines of code

1. Open the Program with ```edit reactor.lua```
2. find and remove these lines of code:
```
add_rule("TURBINE ENERGY LEVEL  <=  95%", function()
	local value = string.format("%3d%%", math.ceil(data.turbine_energy * 100))
	return data.turbine_energy <= 0.95, value
end)
```
```
turbine_energy = turbine.getEnergyFilledPercentage(),
```
```
turbine = peripheral.find("turbineValve")
```
```
elseif data.turbine_energy == nil then
	-- Turbine is not connected
state = STATES.UNKNOWN
```

## Add Fuel Level Monitoring to the Program
In case you want to add the fuel level as a shutdown requirement as well. Note that by default the reactor will stop anyway if it has no fuel. I've just added the option to stop it, if the fuel level drops below 10%.

Here too, you will need to edit the Program a bit
1. Open the Program with ```edit reactor.lua```
2. Add these lines of code:
```
add_rule("REACTOR FUEL LEVEL  >=  10%", function()
	local value = string.format("%3d%%", math.ceil(data.reactor_fuel * 100))
	return data.reactor_fuel >= 0.10, value
end)
```
these should be added below this code block (roughly on line 49):
```
add_rule("TURBINE ENERGY LEVEL  <=  95%", function()
	local value = string.format("%3d%%", math.ceil(data.turbine_energy * 100))
	return data.turbine_energy <= 0.95, value
end)
```
Add this line as well:
```
reactor_fuel = reactor.getFuelFilledPercentage(),
```
Just Below this line (roughly on line 73):
```
reactor_waste = reactor.getWasteFilledPercentage(),
```

## Add Redstone Output if the Reactor has stopped
In some cases, you may want to have a redstone signal if the reactor has stopped, this is fairly easy to achieve:
1. Open the Program with ```edit reactor.lua```
2. Add these lines of code:
```
redstone.setOutput("left", true)
redstone.setOutput("left", false)
```
inside this code block like this (roughly on line 124):
```
if state == STATES.READY then
	colored("READY, flip lever to start", colors.blue)
	redstone.setOutput("left", false)
elseif state == STATES.RUNNING then
	colored("RUNNING, flip lever to stop", colors.green)
	redstone.setOutput("left", false)
elseif state == STATES.ESTOP and not all_rules_met() then
	colored("EMERGENCY STOP, safety rules violated", colors.red)
	redstone.setOutput("left", true)
elseif state == STATES.ESTOP then
	colored("EMERGENCY STOP, toggle lever to reset", colors.red)
	redstone.setOutput("left", true)
end -- STATES.UNKNOWN cases handled above
```
now, the computer outputs a redstone signal out of its left side as soon as the reactor gets shut down automatically by the program


## Images

![Screenshot 2023-04-10 215928](https://user-images.githubusercontent.com/19328039/231000435-70f41249-62fe-4f84-a10a-4778e942248a.png)


