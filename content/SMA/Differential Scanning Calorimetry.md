## 1. Definition and Operating Principle

Differential scanning calorimetry (DSC) is a thermoanalytical technique that measures the difference in the heat required to increase the temperature of a sample and a reference as a function of temperature. The sample and reference are maintained at nearly the same temperature throughout the experiment.

In a typical DSC analysis, the sample-holder temperature increases approximately linearly with time. The reference should have a well-defined heat capacity over the temperature range of interest. It should also be stable, highly pure, and relatively unchanged during the scan. Common reference standards include indium, tin, bismuth, and lead; polyethylene and fatty acids have also been proposed for polymers and organic compounds, respectively.

## 2. DSC Configurations

### 2.1 Heat-flux DSC

In heat-flux DSC, changes in heat flow are calculated from the temperature difference between the sample and reference, $\Delta T_{\mathrm{ref}}$. The sample and reference crucibles are placed on a sample holder with integrated temperature sensors inside a temperature-controlled oven. A typical arrangement uses flat temperature sensors around a flat heater, allowing a relatively small and low-heat-capacity structure while retaining the function of a conventional DSC oven.

![[Differential Scanning Calorimetry/image 2.png]]

_Schematic arrangement of a heat-flux DSC cell._

For a linear temperature program:

$$
T_f(t) = T_0 + \beta t
$$

The measured heat-flow difference can be represented as:

$$
\dot{Q}_{\mathrm{heat\ flux}} = \dot{Q}_{\mathrm{sample}} - \dot{Q}_{\mathrm{reference}}
$$

### 2.2 Power-compensation DSC

In power-compensation DSC, the sample and reference crucibles are placed in thermally insulated furnaces rather than next to each other in a shared furnace. The temperatures of the two chambers are controlled so that the sample and reference remain at the same temperature. The electrical power required to establish and maintain this condition is recorded instead of the temperature difference between the crucibles.

## 3. Detection of Phase Transitions

When a sample undergoes a physical transformation, such as a phase transition, the heat flow required to maintain the sample and reference at the same temperature changes. The direction of this change depends on whether the process is endothermic or exothermic.

For example, when a solid melts, the sample requires additional heat at the same heating rate because it absorbs heat during the endothermic solid-to-liquid transition. During an exothermic process such as crystallization, less heat is required to raise the sample temperature.

![[Differential Scanning Calorimetry/image.png]]

_Example DSC trace showing heating and cooling signals with annotated transformation temperatures._

By measuring the heat-flow difference between the sample and reference, a DSC can quantify heat absorbed or released during these transitions. DSC can also detect subtler changes, such as glass transitions. It is widely used for industrial quality control, evaluating sample purity, and studying polymer curing.

## 4. Related Notes

- [[Shape Memory Alloy]]
- [[Hysteresis]]
