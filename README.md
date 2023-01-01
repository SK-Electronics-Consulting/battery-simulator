# Battery Simulator

When developing new hardware, engineers often need to see how the device will perform while running on battery power. 
Because it is not always convenient to use real batteries for testing, battery simulators have been developed. 
However, commercial battery simulators are expensive, sometimes costing well into the thousands of dollars. 
As an alternative, I have developed an open-source battery simulator that can be built in a home workshop for under $100. 

A battery simulator is a piece of text equipment which does exactly that, simulates a battery. 
The main difference between a battery simulator and a "general purpose" power supply is that the power supply will only source current at a positive voltage, while the battery simulator will both source and sink current also at a positive voltage. 
If you map this capability out to a [Cartesian Plane](https://en.wikipedia.org/wiki/Quadrant_(plane_geometry)), you can use the quadrants as a short-hand way of describing the capability of the device. 
A general purpose power supply (e.g. [Siglent SPD1000X](https://siglentna.com/power-supplies/spd1000x-series-programmable-dc-power-supply/)) is a 1-quadrant power supply because it can only source current and can only do positive voltage. 
A battery simulator like the [Keithley 2302](https://www.tek.com/en/datasheet-/model-2302-battery-simulator-models-2306-and-2306-pj-battery-chargers-simulato) is a 2-quadrant supply because it operates only at a positive voltage, but can source (positive) or sink (negative) current. 
An example of a 4-quadrant power supply would be an SMU (Source Measure Unit) like the [Keithley 2400](https://www.tek.com/en/products/keithley/source-measure-units/2400-graphical-series-sourcemeter). 

> NOTE: Terms like battery simulator, SMU, and power supply may imply a certain number of quadrants, but unfortunately it is not guaranteed.  Always check the datasheet to make sure it's not a marketing term.  

The goal of this project is to make an open-source, lowish-cost battery simulator. This is both to give people access to using devices like this, but also as a design exercise in analog design.  

# Requirements
1. Device shall simulate a single cell Lithium Ion battery, so a voltage of 2.0 V - 4.4 V
1. Device shall have a current output of 1A, both sourcing and sinking.
1. Device shall be powered by an external power supply. 

# Phases
## Phase 1
Create a circuit that will operate as a 2-quadrant power supply.  This might be as simple as using a high-power output op-amp and a potentiometer. 

* TODO: Check if the op-amp will sink current in addition to sourcing. 

### Design Output

The [OPA569](https://www.ti.com/lit/ds/symlink/opa569.pdf) was selected as an available (yaaay chip shortage!) high power op-amp. 
It has some nifty features that are on the [wishlist](#future-phase-wishlist) while satisfying the basic needs of this phase. 
It has a 2A output, along with a 300mV drop-out at 2A. This means that assuming a 5V source (USB or the like), it can maintain it's output at 4.7V, which is well above the 4.2-4.4V max needed simulating 1S Lithium cells. 
This chip does leave some room for improvement.  It has a surprisingly high quiescent current, especially one that varies with current limit.  

### Limitations of Phase 1

* The OPA569 has a max voltage of 5.5V, so only 1S Lithium batteries will be doable.
* Current Monitor pin can only indicate sourcing current.  This could be fixed a couple ways, but is going to stay out of scope for this phase. 

## Future Phase Wishlist

These are features that will be added in future versions.  The list is roughly in order of priority.  

1. USB-C Power Supply
1. Current Monitor Output (Analog to a scope.  Need to define bandwidth required)
1. A good UI (MCU + screen + knob/buttons?)
1. Adding ESR to the current path.  
1. Programmability for adding smarts.  
    1. Voltage limits based on chemistry.  (e.g. 0.5-1.5V for Alkaline, 2.0-4.4V for Lithium-Ion, 1.0-1.2V for NiMH)
    1. Ability to simulate actual charging and discharging based on pre-made curves and a user-defined capacity
1. Considering moving away from an op-amp design, and moving towards a more bespoke and power efficient design.  
1. It might be cool to add "between cells" taps to allow multi-cell chargers who do cell balancing to operate correctly. 

# Out of scope

1. At the moment, this device will not have a way to be adjusted for the sake of calibration.  Not to say this is impossible, but for an open-source, low-cost project, this is unnecessary.
1. This device will never have a need for [4 quadrant operation](https://edadocs.software.keysight.com/kkbopen/what-is-a-bipolar-four-quadrant-power-supply-583425137.html)