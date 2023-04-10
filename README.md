# Computercraft Mekanism Fission Reactor Script
Computercraft script to control Fission Reactors from Mekanism


Credit for the script goes to [@InternetUnexplorer](https://gist.github.com/InternetUnexplorer)


## Setting up
1. Get yourself an Advanced Computer, 18 Monitors (OPTIONAL), at least 3 Wired Modems and some Networking Cable
2. Connect the wired modems to the Fission Reactor Logic Adapter, the Turbine Valve and the Computer (Additionally to the Displays if they are not adjasoned to the Computer)
3. Right click on all Modems. You should get a message in chat that the devices are now on the network
4. open the Computer and type ```wget https://gist.githubusercontent.com/InternetUnexplorer/ea13f1713d325b914126bcfb9b35e6fd/raw/22178936686a875601f31741d6b2de385d3aa86f/reactor.lua ``` 
5. If you have Monitors connected, start the Program like this: ``` monitor monitor_1 reactor.lua ``` (If the Monitors are adjasoned to the Computer, you can use ``` monitor left/right/top/bottom/back reactor.lua``` depending on where the Monitors are positioned, use the correct direction)
6. Place a Lever on top of the Computer and flick it to start the program

## Disabling Turbine Monitoring 
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
Add these lines as well:
```
reactor_fuel = reactor.getFuelFilledPercentage(),
```
Just Below this line (roughly on line 73):
```
reactor_waste = reactor.getWasteFilledPercentage(),
```


### edit:
I've added the modified Programs to this repo, you can also use these directly, be sure to copy the code to the RAW FILE
