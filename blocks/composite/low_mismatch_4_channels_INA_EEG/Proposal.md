# Low Mismatch 4 Channels Instrumentation Amplifier for Electroencephalography (EEG) Measurement using LLM

## **GenYZ Team Proposal**  
### *Track: Analog Automation AI/LLM using Glayout*

---
## Datasheet
### **1. Functionality and Target Specifications**

This chip is a high-precision, low mismatch instrumentation amplifier array specifically designed for multichannel Electroencephalography (EEG) signal measurement. This instrumentation amplifier employs a group-chopping technique, in which multiple chopper switches (MOSFETs) are cascaded to sequentially exchange differential input signals across channels. This cyclic routing allows each input to be amplified by every amplifier in the array, effectively averaging out gain mismatches across channels. Additionally, input-referred DC offset is mitigated through chopper modulation and demodulation, which shifts low-frequency noise and offset to higher frequencies where they can be filtered out. As a result, the amplification across all channels becomes uniform and free from bias. 

<h4 align="center" style="font-size:16px;">Table 1. Target Specification</h4>

| **Parameter**                  | **Target Specs**           | **Notes**                                                                 |
|-------------------------------|----------------------------|---------------------------------------------------------------------------|
| Number of Channels            | 4                          | Scalable up to 8, 16, or more (depends on functionality of chip)         |
| Supply Voltage                | 0.5 V (Analog), 1.2 V (Digital) [1] | Analog supply for chopper switches, digital for clocks                    |
| Input-Referred Noise          | 39 nV/√Hz [1]              | -                                                                         |
| CMRR                          | 100–135 dB [2]             | For 4 channels, typically at 50–60 Hz                                     |
| Area per Channel              | 0.017 mm² [1]              | -                                                                         |
| Power Consumption per Channel | 2.1 µW [1]                 | At 0.5 V supply                                                           |
| NEF (Noise Efficiency Factor) | 2.1 [1]                    | Very low noise relative to power                                          |
| Gain Mismatch Between Channels| 400 ppm [1]                | Approximate value                                                        |

### **2. Block Diagram**
![BlockDiagram](../images/BlockDiagram.jpg)
<h4 align="center" style="font-size:16px;">Figure 1. Block Diagram of the System Design</h4>


<p align="center">
  <img src="https://raw.githubusercontent.com/aurxdeqo/gLayout-genyz-team/main/blocks/composite/images/figure2.jpg" alt="Blokdiagram" width="600"/>
</p>
<h4 align="center" style="font-size:16px;">Figure 2. Open-Loop Amplifier [1]</h4>


<p align="center">
  <img src="../images/chopperswitchcell.jpg" alt="chopperswithcell" width="400"/>
</p>
<h4 align="center" style="font-size:16px;">Figure 3. Chopper Switch Cell [4]</h4>

<p align="center">
  <img src="../images/adoptionchopperswitch.jpg" alt="adoptionchopperswitch" width="400"/>
</p>
<h4 align="center" style="font-size:16px;">Figure 4. Adoption of the Chopper Switch Cell [4]</h4>

<p align="center">
  <img src="https://raw.githubusercontent.com/aurxdeqo/gLayout-genyz-team/main/blocks/composite/images/figure3_2.jpg" alt="Blokdiagram" width="600"/>
</p>
<h4 align="center" style="font-size:16px;">Figure 5. Clocking Scheme [1]</h4>


<p align="center">
  <img src="https://raw.githubusercontent.com/aurxdeqo/gLayout-genyz-team/main/blocks/composite/images/figure5.jpg" alt="Blokdiagram" width="600"/>
</p>
<h4 align="center" style="font-size:16px;">Figure 6. Switched-Cap Low-Pass Filter [1]</h4>

<h4 align="center" style="font-size:16px;">Table 2. Complexity and Functionality of Each Block Diagram</h4>

| **Block Components**        | **Function**                                                                 | **Complexity Scale** | **Note**                                                                                                                                       |
|----------------------------|-------------------------------------------------------------------------------|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------|
| Chopper Switch             | Reduction of flicker (1/f) Noise, Modulation, and Demodulation of Frequency  | High                 | Multi-phase clock for multi-level chopper switches                                                                                             |
| INA (Instrumentation Amplifier Array) | Amplify differential bipotential signal from each channel with high CMRR | High                 | - Uses time-division multiplexing across channels  <br> - Fast time settlement required for quick switching  <br> - Correct VBias for FET ops <br> - 9 FETs per INA; optimize power |
| Low Pass Filter (LPF)      | Suppress high-frequency noise from the bipotential signal                     | Low                  | Simple analog RC circuit using several capacitors                                                                                              |
| Frequency Divider          | Divide clock signal based on required frequencies                            | Moderate (Medium)    | - Converts high-freq clock into clkA, clkB, clkC using sequential logic <br> - Uses flip-flops and handles clock edges <br> - Easily scalable  |
| Clock Generator            | Generate clock signal with specific frequency                                | Low                  | Digital block generating logic level 1 and 0 in a specific frequency range                                                                    |

### **3. Pin Out**
![Pin](../images/Pin.jpg)
<h4 align="center" style="font-size:16px;">Figure 7. Chip Architecture</h4>

<h4 align="center" style="font-size:16px;">Table 3.  External Pin Function</h4>

<div align="center">

<table>
  <thead>
    <tr>
      <th><b>Pin</b></th>
      <th><b>Name</b></th>
      <th><b>Type</b></th>
      <th><b>Domain</b></th>
      <th><b>Description</b></th>
    </tr>
  </thead>
  <tbody>
    <tr><td align="center">1</td><td>AVCC</td><td>Power</td><td>Analog</td><td>Analog Supply Voltage</td></tr>
    <tr><td align="center">2</td><td>AGND</td><td>Ground</td><td>Analog</td><td>Analog Ground</td></tr>
    <tr><td align="center">3</td><td>VCC</td><td>Power</td><td>Digital</td><td>Digital Supply Voltage</td></tr>
    <tr><td align="center">4</td><td>GND</td><td>Ground</td><td>Digital</td><td>Digital Ground</td></tr>
    <tr><td align="center">5</td><td>IN1P</td><td>Input</td><td>Analog</td><td>Channel 1 Analog Positive Input</td></tr>
    <tr><td align="center">6</td><td>IN1N</td><td>Input</td><td>Analog</td><td>Channel 1 Analog Negative Input</td></tr>
    <tr><td align="center">7</td><td>IN2P</td><td>Input</td><td>Analog</td><td>Channel 2 Analog Positive Input</td></tr>
    <tr><td align="center">8</td><td>IN2N</td><td>Input</td><td>Analog</td><td>Channel 2 Analog Negative Input</td></tr>
    <tr><td align="center">9</td><td>IN3P</td><td>Input</td><td>Analog</td><td>Channel 3 Analog Positive Input</td></tr>
    <tr><td align="center">10</td><td>IN3N</td><td>Input</td><td>Analog</td><td>Channel 3 Analog Negative Input</td></tr>
    <tr><td align="center">11</td><td>IN4P</td><td>Input</td><td>Analog</td><td>Channel 4 Analog Positive Input</td></tr>
    <tr><td align="center">12</td><td>IN4N</td><td>Input</td><td>Analog</td><td>Channel 4 Analog Negative Input</td></tr>
    <tr><td align="center">13</td><td>OUT1P</td><td>Output</td><td>Analog</td><td>Channel 5 Analog Positive Input</td></tr>
    <tr><td align="center">14</td><td>OUT1N</td><td>Output</td><td>Analog</td><td>Channel 5 Analog Negative Input</td></tr>
    <tr><td align="center">15</td><td>OUT2P</td><td>Output</td><td>Analog</td><td>Channel 6 Analog Positive Input</td></tr>
    <tr><td align="center">16</td><td>OUT2N</td><td>Output</td><td>Analog</td><td>Channel 6 Analog Negative Input</td></tr>
    <tr><td align="center">17</td><td>OUT3P</td><td>Output</td><td>Analog</td><td>Channel 7 Analog Positive Input</td></tr>
    <tr><td align="center">18</td><td>OUT3N</td><td>Output</td><td>Analog</td><td>Channel 7 Analog Negative Input</td></tr>
    <tr><td align="center">19</td><td>OUT4P</td><td>Output</td><td>Analog</td><td>Channel 8 Analog Positive Input</td></tr>
    <tr><td align="center">20</td><td>OUT4N</td><td>Output</td><td>Analog</td><td>Channel 8 Analog Negative Input</td></tr>
    <tr><td align="center">21</td><td>VBIAS</td><td>Input</td><td>Analog/Digital</td><td>Biasing Voltage for Analog and Digital Components</td></tr>
    <tr><td align="center">22</td><td>CLK</td><td>Input</td><td>Digital</td><td>Clock as Digital Controller for Chopper Switches, etc.</td></tr>
  </tbody>
</table>

</div>

### **4. Application Diagram**
![Application](../images/Application.jpg)

<h4 align="center" style="font-size:16px;">Figure 8. Example Circuits Application</h4>

The application of this system is to amplify biopotential signals such as EEG, which are received from a 4-channel signal source. Each channel takes in two analog differential inputs from a pair of electrodes. The circuit outputs an analog signal (output signal), which can be observed on an oscilloscope.

The signal first passes through a chopper switch block, starting with CSa, which is implemented in each individual channel. This stage performs chopper-stabilization to suppress flicker noise (1/f noise) that typically dominates in low-frequency biomedical signals. The technique shifts the baseband signal to a higher frequency, where the noise floor is lower, and then demodulates it back to recover the clean signal.

In addition to CSa, this system includes two additional dynamic switches, CSb and CSc, which are activated in specific clock-controlled phases to perform dynamic offset cancellation and signal path swapping between adjacent channels. These switches improve the matching across channels by ensuring that each amplifier stage processes signals from different input sources over time.

The time-sequenced behavior of the switches is as follows:

![Phase1](../images/Phase1.jpg)
<h4 align="center" style="font-size:16px;">Figure 9. Three Level Chopper Switches Phase 1</h4>

Phase 1: Only CSa is active. The signal flows straight through its assigned path:
- Ch1 → INA1
- Ch2 → INA2
- Ch3 → INA3
- Ch4 → INA4

![Phase2](../images/Phase2.jpg)
<h4 align="center" style="font-size:16px;">Figure 10. Three Level Chopper Switches Phase 2</h4>

Phase 2: CSb is ON, performing the first set of signal path swaps between adjacent channel pairs:
- Ch1 → INA2
- Ch2 → INA1
- Ch3 → INA4
- Ch4 → INA3

![Phase3](../images/Phase3.jpg)
<h4 align="center" style="font-size:16px;">Figure 11. Three Level Chopper Switches Phase 3</h4>

Phase 3: CSb turns OFF and CSc turns ON, causing the following routing:
- Ch1 → INA3
- Ch2 → INA4
- Ch3 → INA1
- Ch4 → INA2

![Phase4](../images/Phase4.jpg)
<h4 align="center" style="font-size:16px;">Figure 12. Three Level Chopper Switches Phase 4</h4>

Phase 4: CSb and CSc turns ON, causing:
- Ch1 → INA4
- Ch2 → INA3
- Ch3 → INA2
- Ch4 → INA1
  
The Instrumentation Amplifiers (INA1–INA4) then amplify the incoming signals with high gain and excellent Common-Mode Rejection Ratio (CMRR), critical for extracting small EEG signals while rejecting noise and interferences.

Following amplification, a second chopper switch stage (also CSa) demodulates the signal back to its original low-frequency range. The signal then passes through a Low-Pass Filter (LPF) to eliminate residual high-frequency components introduced by the chopping process. The final output is a clean, amplified analog signal representing the original EEG input.

In summary, this example implementation of a 4-channel chopper amplifier system combines conventional chopper-stabilization (CSa) with dynamic signal path-switching (CSb and CSc) across three phases to achieve low-noise, high-accuracy biopotential signal amplification suitable for EEG applications.

---
## Work Distribution 

<h4 align="center" style="font-size:16px;">Table 4. Work Distribution Across Team</h4>

<div align="center">

<table>
  <thead>
    <tr>
      <th><b>Member</b></th>
      <th><b>Main Responsibilities</b></th>
      <th><b>Collaboration Notes</b></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><b>Keisya Andretta Basoeki</b></td>
      <td>
        - Design 3-Level Chopper Switches <br>
        - Support integration with clock and layout
      </td>
      <td>
        Aleta & Angelina help with layout <br>
        Angelina helps debugging
      </td>
    </tr>
    <tr>
      <td><b>Aleta Edna Jessalyn</b></td>
      <td>
        - Design Open-Loop Amplifier (including layout) <br>
        - Lead routing and DRC for all blocks
      </td>
      <td>
        Infall & Keisya assist with simulation <br>
        All assist with routing check
      </td>
    </tr>
    <tr>
      <td><b>Angelina Wahyuni</b></td>
      <td>
        - Design LPF, Clock Generator, and Frequency Divider <br>
        - Debug signal integrity
      </td>
      <td>
        Keisya & Abdillah assist with debugging
      </td>
    </tr>
    <tr>
      <td><b>Abdillah Aziz</b></td>
      <td>
        - Verify final system behavior <br>
        - Cross-check integration results from all modules
      </td>
      <td>
        Infall & Angelina help verify
      </td>
    </tr>
    <tr>
      <td><b>Infall Syafalni</b></td>
      <td>
        - Finalize and revise project proposal <br>
        - Oversee task distribution and review
      </td>
      <td>
        Supported by all team members for proposal review
      </td>
    </tr>
  </tbody>
</table>

</div>

## Schedule

<h4 align="center" style="font-size:16px;">Table 5.  Work Timeline</h4>

<div align="center">

<table border="1" cellspacing="0" cellpadding="6">
  <thead>
    <tr>
      <th><b>Chipathon Schedule</b></th>
      <th><b>Week</b></th>
      <th><b>Job Description</b></th>
    </tr>
  </thead>
  <tbody>
    <!-- Team Formation -->
    <tr>
      <td rowspan="3">Team Formation and Project Planning</td>
      <td>28</td>
      <td>Revise and finalize the project proposal to ensure clear objectives, methodology, and deliverables.</td>
    </tr>
    <tr>
      <td>29</td>
  <td rowspan="2">Design the open-loop amplifier module using <i>Glayout</i> with attention to matching and biasing.<br></td>
</tr>
<tr>
  <td>30</td>
    </tr>
    <tr>
<!-- Design and Simulation -->
    <tr>
      <td rowspan="3">Design and Simulation</td>
      <td>31</td>
      <td>Develop the chopper switch module with synchronized multi-phase clock control.</td>
    </tr>
    <tr>
      <td>32</td>
      <td>Implement the LPF, clock generator, and frequency divider modules to support analog signal processing.</td>
    </tr>
    <tr>
      <td>33</td>
      <td>Replicate (with LLM) and simulate the full system across 4 channels to verify functionality and performance.</td>
    </tr>
    <!-- Layout and Verification -->
    <tr>
      <td rowspan="4">Layout and Verification</td>
      <td>34</td>
      <td rowspan="2">Route and perform DRC checks on each layout block to ensure compliance with design rules.</td>
    </tr>
    <tr>
      <td>35</td>
      </tr>
    <tr>
      <td>36</td>
      <td rowspan="2">Conduct review and final verification through post-layout simulation and internal evaluation.</td>
    </tr>
    <tr>
      <td>37</td>
</tr>
  </tbody>
</table>

</div>

---
### **Reference**

[1] Tao Tang, Jeong Hoan Park, Lian Zhang, Kian Ann Ng, and Jerald Yoo. “Group Chopping: An 8-Channel, 0.04% Gain Mismatch, 2.1 uW 0.017 mm2 Instrumentation Amplifier for Bio-potential Recording". IEEE Journal of Solid-State Circuits, vol. 16, no. 3, pp. 1061–1071, Jun. 2022, doi: 10.1109/TBCAS.2022.3166513


[2] WhaleTeq Medical Device Manufacturer. “CMRR Test Principle and Method.” Accessed from https://www.whaleteq.com/en/app/view18-cmrr-test-principle-and-method at July 9th, 22.19 PM UTC+7.00.


[3] S. Yazicioglu, T. Torfs, P. Merken, H. Kim, J. Penders, R. F. Yazicioglu, and C. Van Hoof, “A 0.5 V 2.1 µW EEG acquisition IC with differential and common-mode active DC offset rejection,” IEEE Journal of Solid-State Circuits, vol. 57, no. 4, pp. 1061–1071, Apr. 2022, doi: 10.1109/JSSC.2022.3161704.

[4] Z. Yao, X. Zhou, D. Huang, W. Wu, and M. Je, "A 0.8 μW/Channel Readout IC for Sub-µV/√Hz Noise Scalable EEG Acquisition System With an Analog Accumulator and a Fully Digital Chopper," Sensors, vol. 25, no. 4, p. 1157, Feb. 2025. doi: 10.3390/s25041157
