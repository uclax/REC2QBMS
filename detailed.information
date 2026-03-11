# Detailed information for parameter setting REC 2Q BMS

[REC 2Q BMS](http://192.168.1.184/settings)
## Voltage settings

### Balancing END Voltage (BVOL)
**Balancing END Voltage (BVOL)** threshold that determines when a cell is considered "full enough" to be balanced regardless of other conditions, it serves as the upper limit for the balancing process.

#### Key Functions of BVOL

- **Forced Balancing:** If any individual cell voltage rises above the BVOL setting, the BMS will balance that cell regardless of the battery pack current.

- **Upper Window Limit:** It defines the top end of the "balancing window." While the Balancing START Voltage (BMIN) initiates the algorithm during charging, BVOL is the point where the cell is considered at its target balanced state.

- **100% Balancing:** According to REC documentation, it is the voltage above which an individual cell is balanced at 100% capacity of the balancing circuit. 

#### Typical Settings
The exact value depends on the battery chemistry, which you can set via the REC PC User Interface: 

>Typical BVOL Setting LiFePO4 (LFP):	3.55V – 3.65V

#### Why It Matters

**Efficiency:** Setting BVOL too low can cause the BMS to waste energy by balancing before the cells have reached the "steep" part of the voltage curve where imbalances are most visible.

**Safety:** BVOL is typically set slightly below the Cell Over-Voltage Switch-off (CMAX) to ensure cells are balanced before they trigger a high-voltage alarm or disconnect. 

### Balancing START voltage (BMIN)

The **Balancing START voltage (BMIN)** is the specific voltage threshold that a battery cell must reach before the BMS begins its "top balancing" process. 

#### How it Works

- **The Threshold:** The BMS monitors each individual cell. Once the voltage of **any single cell** in the pack exceeds this set value, the balancing algorithm is activated.
- **Passive Balancing:** The REC 2Q uses **passive balancing** via 4 Ω resistors. When balancing is active, the BMS identifies cells with higher voltages and diverts a small amount of current through these resistors, dissipating it as heat.
- **Goal:** This slows down the charging of the "stronger" cells, allowing the "weaker" (lower voltage) cells to "catch up" so that all cells reach a full state of charge (SOC) simultaneously. 

#### Why it is Used

- **Efficiency:** For chemistries like LiFePO4, the voltage curve is very flat between 20% and 90% SOC. Balancing during this period is inaccurate. The "Start Voltage" ensures balancing only happens at the "top" (usually >90% SOC) where voltage differences clearly indicate an actual charge imbalance.
- **Heat Management:** Because passive balancing generates heat, you don't want the resistors active throughout the entire charge cycle. Setting a start voltage limits balancing to the final phase of charging. 

#### Common Default Values

These values are typically pre-set based on the battery chemistry you select:

- **LiFePO4:** Often set to **3.38 V to 3.45 V** per cell.

>**Proactive Recommendation:** If you notice your cells are consistently reaching high-voltage disconnect before they can balance, you might consider **lowering the Start voltage** slightly (e.g., to 3.38 V for LiFePO4) to give the BMS more time to work during the charge cycle.
### Maximum allowed cell voltage difference (RAZL)

In REC 2Q BMS, the RAZL parameter defines the maximum allowed voltage difference (delta) between the highest and lowest cells in your pack.

#### Recommended Settings
For a LiFePO4 (LFP) battery, typical settings are 0.10V to 0.20V. This is a standard "safe" range for general operation.

>**Lower (< 0.10V):** Makes the BMS more sensitive. If your cells are brand new and top-balanced, you can tighten this, but it may cause nuisance alarms during high current spikes.
 
>**Higher (> 0.30V):** Too loose. This might allow a single cell to drift dangerously high or low without the BMS tripping a protection warning.

#### Why RAZL Matters

- **Safety Trigger:** If the difference between your strongest and weakest cell exceeds this value, the BMS will trigger an Error 4 (Cell Differential) and may disconnect the pack or stop charging to prevent damage.
- **Balancing Indicator:** High delta usually indicates your cells are out of balance or that one cell has higher internal resistance.
- **SOC Accuracy:** Since you are having SOC issues, a high RAZL during the top of the charge cycle will prevent the BMS from accurately "syncing" to 100%, because it may stop the charge early to protect a high cell. 

### END of Charging Voltage (CHAR)(CC/CV)

**END of Charging [V]** is the specific voltage threshold per cell at which the BMS determines the battery is fully charged and signals the charger to stop. 

#### Key Functions of this Setting

- **Charging Termination:** When the first cell in the pack reaches this voltage, the BMS sends a command (via CAN-bus) to the charger to stop or reset the charging current to 0 A.
- **SOC Synchronization:** Reaching this voltage typically triggers the BMS to reset its State of Charge (SoC) counter to 100%.
- **Balancing Integration:** This value is often used as the final target for the balancing algorithm. The BMS will prioritize balancing cells until they all reach or approach this "End" level. 

#### Common Values & Related Settings

For a standard **LiFePO4** system, the default is typically **3.55 V to 3.65 V** per cell. 

- **Max allowed cell voltage (CMAX):** This should be set **0.12V – 0.2V higher** than the End of Charging (CHAR) voltage to act as a safety "trip" if the charger fails to stop.
- **End of charging hysteresis (CHIS):** This is the voltage drop required _after_ a full charge before the BMS will allow the charger to start again (e.g., 0.15 V to 0.25 V). 

>**Recommended Configuration Tip**

>Ensure your **End of Charging** voltage in the BMS is slightly lower than the charger's own "Bulk" or "Absorption" setting to ensure the BMS maintains control over the final charging stage.

### END of charging voltage hysteresis per cell (CHIS)

**END of charging voltage hysteresis per cell [V] (CHIS)** is the voltage drop required before the BMS will allow a new charging cycle to begin after reaching a full charge. 

#### Core Function

- **Restart Threshold:** Once the battery is "Full" (reached CHAR), the BMS tells the charger to stop. It will only signal the charger to start again when the highest cell voltage drops below End of Charging Voltage Hysteresis (CHIS).
- **Prevents "Short-Cycling":** This prevents the charger from rapidly switching on and off (oscillating) when the battery is near 100%, which protects the battery and the charging hardware. 

>Relationship to Float Voltage

>This parameter is critical if you use a "Float" stage. The REC BMS calculates its **Float Voltage** using this hysteresis value: 

> **Float Voltage** = End of Charging (CHAR) – (Charge Float Voltage Coefficient (CFVC) × END of charging voltage hysteresis per cell (CHIS)

#### Typical Values

- **LiFePO4 (LFP):** The default is usually **0.25 V**.

	_Example:_ If your End of Charge is 3.55V and Hysteresis is 0.25V, the battery must drop to 3.30V before bulk charging restarts.

>**Note:** When the battery voltage is within this hysteresis range, the BMS typically resets its **State of Charge (SOC) to 100%** and may turn on a "Power LED" or status indicator until the voltage drops below the threshold.

### Max allowed cell voltage (CMAX)
The **Max allowed cell voltage [V] (CMAX) is a safety threshold that triggers a protective shutdown to prevent battery damage.

#### Functional Definition

>Protection Trigger: If any single cell hits this voltage, the BMS signals the system to stop charging immediately.

>System Response: The BMS will typically open its internal or external relay (HVD - High Voltage Disconnect) and send a "zero charge" command via the CAN-bus to the Victron GX device.

>Distinction from Charging Voltage: This is a limit, not the target. The "End of Charge Voltage" (CHAR) is your actual target for daily use (e.g., 3.55V), while the Max Allowed Cell Voltage (VMAX) is the emergency ceiling (e.g., 3.75V). 

#### Dynamic Control Logic

The REC 2Q BMS uses these individual cell limits to dynamically calculate the Charge Voltage Limit (CVL) sent to the Victron system: 

Ramping Down: When the highest cell approaches the balance start voltage, the BMS commands the MultiPlus-II to reduce the charging current.

Final Calculation: Once the target is reached, the BMS sets the CVL using a specific formula: Number of cells x (End of Charge Voltage per cell – 0.2 x end of charge hysteresis per cell). 

#### Configurable Values

Typical values for LiFePO4 cells are:

    Max allowed cell voltage (CMAX): 3.65V .. 3.85V per cell
    END of Charging Voltage (CHAR): 3.50V .. 3.60V per cell 

You can view or adjust these thresholds through the REC Wi-Fi module or BMS Master Control PC software by querying the CMAX? and CHAR? parameters. 

>> .

>> .

>>**ucla - continue editing**

>> .

>> .


### Max allowed cell voltage hysteresis
In the REC 2Q BMS, the **Max allowed cell voltage hysteresis** (often labeled as Over-voltage switch-off hysteresis per cell) is the voltage drop required for the BMS to clear an over-voltage error and allow charging to resume.

Functional Role

When a single cell hits the VMAX threshold (Max allowed cell voltage), the BMS triggers a protection event, signaling the Victron MultiPlus-II (via the GX device) to stop charging immediately by setting the Charge Current Limit (CCL) to 0A. 

The Reset Point: The system will stay in this "locked" or protected state until the highest cell's voltage drops below VMAX minus the Hysteresis value.

Preventing "Oscillation": Without this hysteresis, the charger would rapidly toggle on and off as soon as the cell voltage dropped by even 1mV, which could damage contactors or the inverter's charging circuit. 

Common Values and Commands

For a standard LiFePO4 system, the following settings are typical:

    VMAX (Max Allowed): 3.75V – 3.85V.
    Hysteresis: Typically 0.10V to 0.25V.
    Example: If VMAX is 3.75V and Hysteresis is 0.10V, the BMS will not allow the MultiPlus-II to start charging again until that cell drops below 3.65V. 

How to View or Change It

You can manage this parameter through the REC Wi-Fi module or PC software:

Query current value: Type VMAXH? (or look for "Over-voltage switch-off hysteresis" in the settings list).

Set new value: Type VMAXH 0.10 (after unlocking the BMS with your serial number). 

Note: If you are using LiFePO4 cells, ensure your Hysteresis isn't set too small (below 0.05V), as LFP cells often "surface charge" and will immediately bounce back to a high voltage if the charger kicks back in too soon.

### Min allowed cell voltage
In a system with a REC 2Q BMS and a Victron MultiPlus-II, the **Min allowed cell voltage (parameter VMIN)** is the emergency "floor" for your battery cells.

Functional Definition

Protection Trigger: If any single cell drops below this voltage, the BMS triggers a Low Voltage Disconnect (LVD).

System Response: The BMS sends a command via CAN-bus to the Victron GX device to set the Discharge Current Limit (DCL) to 0A, effectively turning off the inverter to prevent permanent battery damage. If a physical contactor is installed, the BMS will also open it.

Safety Margin: This is a hard limit, not a regular discharge target. In a healthy Victron system, the "Dynamic Cut-off" or "SOC Limit" in the ESS settings should stop the discharge before the REC BMS reaches this VMIN threshold. 

Typical VMIN Values

Battery Chemistry 	Recommended VMIN	Terminal Command
LiFePO4 (LFP)	2.50V – 2.80V	VMIN 2.80

Related Parameter: VMINH (Hysteresis) 
Once the BMS cuts off due to low voltage, it will not allow the MultiPlus-II to discharge again until the cell voltage rises above VMIN + VMINH.

Example: If VMIN is 2.80V and VMINH is 0.20V, the battery must be charged back up until the lowest cell reaches 3.00V before you can use the inverter again.

How to Check and Set

Query: Type VMIN? in the REC Wi-Fi/PC terminal to see the current limit. (A result of 280 means 2.80V).

Unlock: Send your serial number (e.g., 2Q-XXXX) to the BMS.
Set: Type VMIN 2.80 (use the decimal format if your firmware version requires it).

Warning: If your previous VMAX was set to 4.12V, check if your VMIN is also set for a different chemistry. For LiFePO4, ensure VMIN is not set below 2.50V, as discharging below this point causes rapid degradation.

### Min allowed cell voltage hysteresis
In the REC 2Q BMS, the **Min allowed cell voltage hysteresis** (often queried as VMINH) is the "recovery buffer" that determines when the BMS will clear a Low Voltage alarm and allow the battery to be used again.

How it works with the MultiPlus-II

The Cut-off: If any cell drops below your VMIN (e.g., 2.80V), the BMS tells the Victron system to stop discharging (DCL = 0A) and may open a physical contactor.

The Lock: The system stays locked in a "Low Voltage" state even if the voltage bounces back slightly after the load is removed.

The Recovery: To resume operation, the voltage must rise above VMIN + VMINH.

Example: If VMIN is 2.80V and VMINH is 0.20V, the cell must reach 3.00V before the BMS allows the MultiPlus to draw power again.

Why is this necessary?

Preventing "Chattering": LiFePO4 cells have a "voltage sag" under load and a "bounce back" when the load is removed. Without hysteresis, the system would turn off, the voltage would bounce back, the system would turn on, sag again, and repeat (chatter), which can destroy electronic components.

Ensuring Charge Priority: It forces the system to prioritize charging the battery to a safe level before allowing a load to be placed on it again.

How to Check and Set

Query current value: Type VMINH? in your terminal.
Set new value: Type VMINH 0.20 (standard for LiFePO4) or VMINH 0.30 (if you want a larger safety margin).

Requirement: As with other settings, you must unlock the BMS with your serial number before sending the change command.

Typical Settings for LiFePO4:

    VMIN: 2.80V
    VMINH: 0.20V
    Result: Shutdown at 2.80V, Restart allowed at 3.00V

### Min Vcell discharge
In the REC 2Q BMS, Min Vcell discharge (often labeled as V_low_dis) is the voltage threshold where the BMS sends a signal to the Victron system to stop all discharging (sets DCL to 0A) but does not necessarily trip the emergency hardware relay yet.
Key Difference from VMIN

Min Vcell discharge (Operational): This is your "Soft Limit." When the lowest cell hits this (e.g., 2.90V), the BMS tells the MultiPlus-II via CAN-bus: "Stop drawing power now."

VMIN (Emergency): This is the "Hard Limit." If the voltage continues to drop to this level (e.g., 2.80V), the BMS trips the internal/external relay (disconnects the battery) and triggers a Low Voltage Alarm.

Typical LiFePO4 Configuration

| Parameter | Typical Value | Purpose                                                    |
| --------- | ------------- | ---------------------------------------------------------- |
| V_low_dis | 2.90V         | Stops the Inverter/Loads via software                      |
| VMIN      | 2.80V         | Emergency hardware disconnect (protects cells)             |
| V_low_off | 3.05V         | Hysteresis: Voltage must rise to this to restart discharge |


How to Check/Set

This is found in the "Voltage Settings" section of the REC Wi-Fi or PC software.

Command: VMIN usually sets the hard limit.
    
Operational Limit: In newer firmware, this is often a dedicated field in the software UI labeled "Cell Under Voltage Discharge."

Important: For a healthy system, your Victron ESS (Energy Storage System) "Minimum SOC" or "Dynamic Cut-off" should be set to trigger before the REC BMS hits these limits. This ensures the MultiPlus stops gracefully rather than the BMS "slamming the door" on the system.

In the REC 2Q BMS, there is a distinction between the absolute safety cutoff (VMIN) and the operational discharge limit (Under Voltage Discharge Protection).

Cell Under Voltage Discharge Protection
This is the threshold where the BMS first signals the Victron MultiPlus-II to stop discharging.
    
Function: When the lowest cell reaches this voltage, the BMS sends a command via CAN-bus to the GX device to set the Discharge Current Limit (DCL) to 0A.
    
Typical Value: Usually set slightly higher than the hard cutoff, around 2.90V – 2.95V for LiFePO4.
    
Hysteresis: Controlled by the SOC discharge hysteresis (default ~5%), meaning the battery must be charged significantly before discharge is permitted again. 

Cell Under Voltage Protection Switch-off (VMIN) 
This is the emergency "hard" floor for the cells. 

    
Function: If any cell drops below this point, the BMS triggers an Error 2 and may physically open the battery contactor to prevent permanent damage.
    
SOC Reset: Reaching this limit automatically resets the BMS State of Charge (SOC) to 1% (or 0% depending on firmware) to force a recalibration.
    Typical Value: 2.80V per cell. 

How to Check and Set
To adjust these limits, use the REC Wi-Fi module or PC software:
    
Unlock: Send your serial number (e.g., 2Q-XXXX) as the password to enable changes.
    
View Current: Type VMIN? for the hard switch-off or look for the "Under Voltage Discharge" field in the settings menu.
    
Set New Value: Type VMIN 2.80 (or your preferred value). Use the decimal if your firmware version requires it.

### Charge Float Voltage Coefficient (CFVC)


The **Charge Float Voltage Coefficient (CFVC)** is a parameter used to calculate the final target voltage the BMS sends to your charger or inverter (via CAN bus) after the battery is fully charged. 

Definition and Function

The coefficient determines the **voltage drop** from the maximum "End of Charge" voltage to a lower "Float" level. It allows the battery to stay connected to a power source (like solar) without being held at its absolute maximum stress voltage indefinitely. 

- **Range:** [0.1 – 1.0] (Note: Older firmware may allow 0, which effectively disables the float reduction).
- **The Calculation:** The BMS calculates the Maximum Charging Voltage using this formula:  
    `Target Voltage = Number of Cells × (End of Charge Voltage - (CFVC × End of Charge Hysteresis))`. 

What the Values Mean

- **Low Coefficient (e.g., 0.1):** The float voltage remains very close to the full charge voltage. The battery stays "fuller," but under slightly more chemical stress.
- **High Coefficient (e.g., 1.0):** The float voltage drops significantly (by the full value of your hysteresis setting). This is safer for long-term battery health as it moves the cells away from their upper voltage limit.
- **Mid-range (e.g., 0.5):** This is often a default or recommended middle ground. At 0.5, the BMS typically allows roughly 50% of the maximum charging current to supply DC loads directly from your chargers without discharging the battery below its restart threshold. 

Why Use It?

For Lithium (LiFePO4) batteries, holding a 100% State of Charge (SOC) at high voltage for long periods accelerates degradation. By setting a coefficient like **0.8**, you tell the BMS to "relax" the voltage slightly after reaching 100%, which preserves battery life while still keeping the pack ready for use.

### State of Charge Hysteresis (SOCH)

On the REC 2Q BMS **SOCH** stands for State of Charge Hysteresis.
It is a configurable parameter that prevents the charger from "hunting" or constantly toggling on and off once the battery is nearly full.

How SOCH Works

The SOCH parameter defines a required drop in the State of Charge (SoC) before the BMS will allow a new bulk charging cycle to begin. 

>Logic: If your battery reaches 100% and charging stops, the BMS will not signal the inverter to start a full recharge until the SoC drops below the (100% - SOCH) threshold.

>Example: If SOCH is set to 0.050 (5%), the battery must discharge to 95% SoC before the BMS allows the charger to switch from "Float" back to "Bulk/Absorption" mode.

>Purpose: This reduces wear on the battery and contactors by ensuring the system only performs significant charging cycles rather than micro-charging for every minor percentage drop.

.....

## Current settings
### Max.discharging current per inverter device [A]

typically labeled **MAXD** in the REC 2Q BMS firmware, defines the maximum continuous discharge current that a **single** inverter (or inverter/charger) in your system is allowed to draw from the battery. 

How the Calculation Works

The BMS uses this value to calculate the total **Discharge Current Limit (DCL)** that it sends to your Victron GX device via the CAN bus. 

- **The Formula**: Total DCL = `Max discharging current per device` × `Number of inverter devices`.
- **The "Lowest Wins" Rule**: The BMS also calculates a current limit based on your battery capacity (e.g., `Discharge Coefficient` × `Battery Capacity`). It then compares this to the "per device" calculation and selects the **lower** of the two values to send to the GX device. 

Why This Setting Matters

- **Hardware Protection**: It ensures you don't exceed the safe DC current rating of your specific inverter or the cabling/fuses connected to it.
- **System Scaling**: If you add more inverters in parallel, you simply update the `Number of inverter devices` setting, and the BMS automatically scales the total allowed discharge current.
- **Load Management**: When the GX device receives this DCL value, it instructs the inverters to limit their total output so they don't draw more from the battery than the BMS allows. 

Example Scenario

If you have **two** Victron MultiPlus-II 5000 units and set:

- `Max discharging current per inverter device` = **100A**
- `Number of inverter devices` = **2**

The BMS will tell the system the maximum total discharge is **200A** (assuming your battery bank is large enough to handle it). 

### Charging coefficient [C rating]

The **Charging coefficient [C rating]** in the REC 2Q BMS is a multiplier used to calculate the **Charge Current Limit (CCL)** based on your battery's total capacity. It defines how fast the battery is allowed to charge relative to its size. 

How the BMS Uses This Setting

The BMS calculates the maximum charging current using this simple formula: 

- **Calculated Current** = `Charging coefficient` × `Battery Capacity (Ah)`. 

What the Values Mean

The "C" stands for Capacity. A **1C** rate means a current equal to the battery's full capacity, which would theoretically charge it from 0% to 100% in one hour. 

- **0.5C**: Charges the battery in **2 hours**. (e.g., 50A for a 100Ah battery).
- **1.0C**: Charges the battery in **1 hour**. (e.g., 100A for a 100Ah battery).
- **0.1C**: Charges the battery in **10 hours**. (e.g., 10A for a 100Ah battery). 

Impact on Your System

1. **Safety & Longevity**: Lower coefficients (like 0.3C or 0.5C) generate less heat and extend the overall lifespan of LiFePO4 cells.
2. **Dynamic Limits**: The BMS sends this calculated limit to your Victron GX device via the CAN bus. The GX then tells your MPPTs and MultiPlus chargers exactly how much current they are allowed to provide.
3. **The "Lowest Wins" Logic**: The final limit the BMS sends is the **lower** of:
    - The `Charging coefficient` × `Capacity`.
    - The `Max charging current per device` × `Number of devices`.
