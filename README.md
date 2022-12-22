# Battery Simulator

A battery simulator is a piece of text equipment which does exactly that, simulates a battery.  The main difference between a battery simulator and a power supply is that the power supply will only source current, while the battery simulator will both source and sink current.  Another way this is said is it is a 1 quadrant vs 2 quadrant power supply.  

# Requirements
1. Device shall simulate a single cell Lithium Ion battery, so a voltage of 2.0 V - 4.4 V
1. Device shall have a current output of 1A, both sourcing and sinking.
1. Device shall be powered by an external power supply. 

# Phases
## Phase 1
Create a circuit that will operate as a 2-quadrant power supply.  This might be as simple as using a high-power output op-amp and a potentiometer. 

TODO: Check if the op-amp will sink current in addition to sourcing. 


## Future Phase Wishlist

These are features that will be added in future versions.  The list is roughly in order of priority.  

1. USB-C Power Supply
1. Current Monitor Output (Analog to a scope.  Need to define bandwidth required)
1. A good UI (MCU + screen + knob/buttons?)
1. Adding ESR to the current path.  
1. Programmability for adding smarts.  
    1. Voltage limits based on chemistry
    1. Ability to simulate actual charging and discharging based on pre-made curves and a user-defined capacity
1. Considering moving away from an op-amp design, and moving towards a more bespoke and power efficient design.  
1. It might be cool to add "between cells" taps to allow multi-cell chargers who do cell balancing to operate correctly. 

# Out of scope

1. At the moment, this device will not have a way to be adjusted for the sake of calibration.  Not to say this is impossible, but for an open-source, low-cost project, this is unnecessary.
1. This device will never have a need for 4 quadrant operation.