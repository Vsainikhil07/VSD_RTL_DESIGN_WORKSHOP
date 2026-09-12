# Module 3 — CMOS Technology & Physical Implementation 
 
### CMOS Inverter Characterization | SKY130A Standard Cell | Layout Extraction | 16-Mask Fabrication 
 
--- 
 
## 📌 Module Overview 
 
This module follows the implementation of a CMOS inverter from transistor-level simulation to physical layout and fabrication. 
 
The work is divided into five major stages: 
 
**Circuit Characterization → Standard-Cell Layout → Physical Verification → Post-Layout Simulation → CMOS Fabrication** 
 
The CMOS inverter is first analysed using SPICE to study its switching behaviour, timing, voltage transfer characteristics and transistor sizing. 
 
The design is then implemented as a SKY130A standard cell. Its physical structure, cell boundary and power connections are examined before extracting the layout into an electrical representation. 
 
The extracted circuit is simulated using NGSPICE to verify that the physical implementation maintains the expected inverter behaviour. 
 
The module finally studies the major stages of the 16-mask CMOS fabrication process, connecting the transistor structure to its physical manufacturing steps. 
 
--- 
 
## 🎯 Learning Objectives 
 
- Understand CMOS inverter operation at transistor level. 
- Perform SPICE-based inverter simulation. 
- Analyse transient and static characteristics. 
- Measure rise time, fall time and propagation delay. 
- Study VTC and switching threshold. 
- Understand PMOS/NMOS sizing effects. 
- Implement a SKY130A CMOS inverter standard cell. 
- Understand cell boundary and power/ground structure. 
- Extract the physical layout into a SPICE representation. 
- Perform post-layout simulation using NGSPICE. 
- Understand the major stages of CMOS fabrication. 
- Relate circuit behaviour, layout and fabrication. 
 
--- 
 
## 🛠️ Tools Used 
 
| Tool / Technology | Application | 
|---|---| 
| **SPICE / NGSPICE** | Circuit and post-layout simulation | 
| **Magic VLSI** | Layout creation and extraction | 
| **OpenLane** | Physical-design environment | 
| **SKY130A PDK** | Technology and device information | 
| **Linux Terminal** | Design and simulation commands | 
| **Git** | Repository management | 
| **GitHub** | Documentation and version control | 
 
--- 
 
# 1. CMOS Inverter: From Circuit to Characterization 
 
## 1.1 Circuit Setup 
 
The CMOS inverter is formed using complementary PMOS and NMOS transistors. 
 
The PMOS provides the pull-up path to the supply, while the NMOS provides the pull-down path to ground. SPICE device models, transistor dimensions, supply voltage and input stimulus are defined before simulation. 
 
The selected transistor dimensions determine the relative strength of the pull-up and pull-down networks. 
 
--- 
 
## 1.2 Simulation Configuration 
 
The simulation environment is configured with the required device parameters and operating conditions. 
 
### Device Configuration 
 


 
**Figure:** Device parameters and simulation configuration. 
 <img width="957" height="362" alt="image" src="https://github.com/user-attachments/assets/fccf9148-ec5c-4ddd-955b-0eee4afbc286" />

These parameters are used by SPICE to calculate the electrical response of the inverter. 
 
--- 
 
## 1.3 Functional Simulation 
 
After configuring the circuit, the SPICE simulation is executed. 
 
The common gate input controls both transistors. A LOW input turns the PMOS ON and the NMOS OFF, while a HIGH input turns the NMOS ON and the PMOS OFF. 
 
### Simulation Execution 
 

 
**Figure:** CMOS inverter SPICE simulation execution. 
 <img width="1053" height="470" alt="image" src="https://github.com/user-attachments/assets/c27ff031-c62a-4fcf-a3dc-553d11da0533" />

This confirms the basic logical operation before detailed characterization. 
 
--- 
 
## 1.4 Switching Waveforms 
 
The transient simulation produces complementary input and output signals. 
 
### Input vs Output 
 

 
**Figure:** CMOS inverter input and output waveforms. 
 <img width="992" height="405" alt="image" src="https://github.com/user-attachments/assets/957c8ad2-de63-4a38-9014-2bb7980bebb7" />

The waveform demonstrates the expected inversion behaviour. 
 
--- 
 
## 1.5 Timing Response 
 
The output requires finite time to charge and discharge because of transistor resistance and circuit capacitance. 
 
### Transient Behaviour 
 

 
**Figure:** CMOS inverter transient response. 
 <img width="1017" height="412" alt="image" src="https://github.com/user-attachments/assets/1e693329-1b00-4bbc-b372-7b48c0c35112" />

The waveform is used to determine rise time, fall time and propagation delay. 
 
### Detailed Transition 
 

 
**Figure:** Detailed transient waveform observation. 
 <img width="1022" height="476" alt="image" src="https://github.com/user-attachments/assets/b7fd18d4-a286-4a18-ab7e-21c7995db1e4" />

The transition points are examined to obtain accurate timing measurements. 
 
--- 
 
## 1.6 Sizing and Drive Strength 
 
The relative PMOS/NMOS dimensions influence the switching point, drive strength and timing behaviour. 
 
Increasing transistor width can increase drive current, but it also increases capacitance. Therefore, suitable sizing is required for balanced inverter performance. 
 
### Sizing Effect 
 

 
**Figure:** Effect of PMOS/NMOS sizing on inverter behaviour. 
 <img width="1085" height="477" alt="image" src="https://github.com/user-attachments/assets/a1429277-c000-4ee3-8107-442db6451599" />


--- 
 
## 1.7 Static Characterization 
 
The Voltage Transfer Characteristic describes the relationship between input and output voltage. 
 
It identifies the LOW-output region, transition region and HIGH-output region. 
 
### VTC 
 

 
**Figure:** Static voltage-transfer characteristic. 
 <img width="1050" height="477" alt="image" src="https://github.com/user-attachments/assets/5760723a-abdc-4d29-9517-11da15ec4699" />

A steep transition indicates strong switching behaviour and helps evaluate the noise-margin characteristics of the inverter. 
 
--- 
 
## 1.8 Switching Point and Robustness 
 
Changing the PMOS/NMOS strength ratio shifts the inverter switching point. 
 
### Robustness Study 
 

 
**Figure:** CMOS inverter robustness evaluation. 
 <img width="1067" height="440" alt="image" src="https://github.com/user-attachments/assets/eeca1e01-124d-4b08-bb83-12bee52fc4e3" />

### Switching Threshold 
 

 
**Figure:** Switching threshold analysis. 
 <img width="1071" height="432" alt="image" src="https://github.com/user-attachments/assets/bb470183-fc81-4105-a0c1-4f1ed1299c7e" />

The switching threshold is affected by transistor characteristics, sizing, body voltage and process parameters. 
 
The body effect becomes important when the source and body terminals are at different potentials. 
 
--- 
 
## 1.9 VTC Comparison 
 
The complete VTC is used to understand the static operation of the inverter. 
 
At low input voltage, the PMOS dominates and the output approaches VDD. At high input voltage, the NMOS dominates and the output approaches GND. 
 
 
 
**Figure:** CMOS inverter Voltage Transfer Characteristic. 
 <img width="1055" height="433" alt="image" src="https://github.com/user-attachments/assets/a530b71b-da66-4eb2-9673-251a97e4869c" />
 <img width="1046" height="418" alt="image" src="https://github.com/user-attachments/assets/912d8f81-d150-455f-8390-81b4ef0c39f7" />
--- 
 
## 1.10 Characterization Verification 
 
The measured simulation values are compared with calculated values to verify the extracted parameters. 
 

**Figure:** Verification of calculated simulation parameter. 
 <img width="1081" height="433" alt="image" src="https://github.com/user-attachments/assets/41707fc7-72fc-45c9-9e7d-6ad57bb45eae" />


### Final Characterization 
 

**Figure:** Final CMOS inverter characterization. 
<img width="1047" height="431" alt="image" src="https://github.com/user-attachments/assets/73ec63fd-55fc-40bf-bc4d-c259b375a14f" />

The final results provide the electrical and timing information required before physical implementation. 
 
--- 
 
# 2. SKY130A Standard-Cell Implementation 
 
## 2.1 Preparing the Design Environment 
 
The standard-cell environment is prepared using the required repository and SKY130A technology files. 
 
### Repository Setup 
 
The required standard-cell repository is cloned into the working environment using Git. 
 

**Figure:** Standard-cell design repository cloning. 
 <img width="507" height="502" alt="image" src="https://github.com/user-attachments/assets/8d382c30-e1f2-4fb3-af76-d24d1e19a7be" />

### Technology Setup 
 
The SKY130A technology file is copied from the Magic technology directory into the standard-cell working directory using the `cp` command. 
 

 
**Figure:** SKY130A technology file setup. 
 <img width="777" height="751" alt="image" src="https://github.com/user-attachments/assets/234c70dc-9143-471f-85e3-81e4661a45cf" />

--- 
 
## 2.2 Building the Inverter Layout 
 
The CMOS inverter is physically implemented using the SKY130A technology. 
 
The layout contains the required: 
 
- PMOS and NMOS regions 
- Diffusion 
- Polysilicon 
- Contacts 
- Metal layers 
- Well regions 
 

 
**Figure:** SKY130A CMOS inverter layout. 
 <img width="517" height="503" alt="image" src="https://github.com/user-attachments/assets/1e18bdb7-4019-4898-ac9e-13614e602cb8" />


--- 
 
# 3. Standard-Cell Physical Organization 
 
## 3.1 Layout and Abstract Representation 
 
The completed cell is viewed in both layout and abstract form. 
 
The abstract representation provides the simplified physical information required for standard-cell use. 
 

 
**Figure:** Layout and abstract representation of the standard cell. 
 <img width="1087" height="482" alt="image" src="https://github.com/user-attachments/assets/bc033754-0779-484d-8995-31838356165f" />

--- 
 
## 3.2 Cell Boundary Definition 
 
A fixed boundary is defined around the standard cell. 
 
The boundary provides consistent cell dimensions and allows the cell to be placed correctly with neighbouring cells. 
 

 
**Figure:** Defined standard-cell boundary. 
 <img width="1090" height="561" alt="image" src="https://github.com/user-attachments/assets/73c8c982-1e06-48bb-9901-bb99f048356f" />

--- 
 
## 3.3 Power and Ground Structure 
 
The standard cell requires proper VDD and GND connections. 
 
VDD supplies the PMOS network, while GND provides the return path for the NMOS network. 
 

**Figure:** Power and ground connections in the layout. 
 <img width="1088" height="572" alt="image" src="https://github.com/user-attachments/assets/6d574839-d36d-4158-a29d-1054a0b092b5" />

--- 
 
# 4. From Physical Layout to Electrical Circuit 
 
## 4.1 Layout Extraction 
 
Once the layout is completed, the physical geometry is extracted into an electrical representation. 
 
The extraction identifies devices, nodes, connections, device dimensions and parasitic information. 
 

 
**Figure:** Layout extraction. 
 <img width="517" height="497" alt="image" src="https://github.com/user-attachments/assets/b5667c91-d6de-4f3f-94dd-4f513d7094c7" />

--- 
 
## 4.2 Extracted Netlist 
 
The extraction process produces the required files and netlist containing the electrical representation of the physical cell. 
 

 
**Figure:** Generated extracted files and netlist. 
 <img width="773" height="751" alt="image" src="https://github.com/user-attachments/assets/b6108509-2870-4206-ad16-2de0c0fc62de" />

--- 
 
## 4.3 SPICE Representation 
 
The extracted information is converted into a SPICE-compatible file. 
 
The simulation file contains the cell definition, transistor information, supply connections, input/output nodes and simulation conditions. 
 

 
**Figure:** SPICE file generated from the extracted layout. 
 <img width="775" height="753" alt="image" src="https://github.com/user-attachments/assets/ce4f0c06-e300-4ff5-810e-bd5ae3ec60a7" />

--- 
 
# 5. Post-Layout Verification 
 
## 5.1 NGSPICE Simulation 
 
The extracted circuit is simulated using NGSPICE. 
 
Transient analysis is performed by applying a time-varying input while monitoring the output, VDD and GND nodes. 
 

 
**Figure:** NGSPICE transient simulation. 
 <img width="778" height="752" alt="image" src="https://github.com/user-attachments/assets/78f88b53-cadc-4cad-af74-1d1ff333b5c3" />

--- 
 
## 5.2 Extracted Input/Output Response 
 
The simulated output is compared with the applied input. 
 
For a correct CMOS inverter: 
 
**Input LOW → Output HIGH** 
 
**Input HIGH → Output LOW** 
 
 
 
**Figure:** Simulated input and output transient waveforms. 
 <img width="762" height="753" alt="image" src="https://github.com/user-attachments/assets/86806fc3-1db7-4cd7-bca7-fa27ca183777" />

The waveform verifies that the extracted physical cell maintains the intended inverter functionality. 
 
--- 
 
## 5.3 Physical and Electrical Verification 
 
The extracted circuit provides a closer representation of the actual physical implementation than an ideal schematic. 
 
The extracted simulation can therefore reveal the influence of: 
 
- Parasitic capacitance 
- Interconnect resistance 
- Device dimensions 
- Rise/fall behaviour 
- Propagation delay 
 
This establishes the connection between the physical layout and its electrical performance. 
 
--- 
 
# 6. CMOS Inverter Performance Analysis 
 
## 6.1 Logic Operation 
 
The CMOS inverter operates through complementary switching. 
 
### LOW Input 
 
- PMOS ON 
- NMOS OFF 
- Output pulled towards VDD 
- Output HIGH 
 
### HIGH Input 
 
- PMOS OFF 
- NMOS ON 
- Output pulled towards GND 
- Output LOW 
 
--- 
 
## 6.2 Rise and Fall Behaviour 
 
The output transition is limited by the charging and discharging of circuit capacitances. 
 
The PMOS mainly controls the output charging path, while the NMOS mainly controls the discharge path. 
 
The physical layout and extracted parasitics can further influence these transitions. 
 
--- 
 
## 6.3 Timing Characteristics 
 
The main timing parameters considered are: 
 
- Rise time 
- Fall time 
- Propagation delay 
- Input transition time 
- Output transition time 
 
These parameters are important when the cell is used in larger digital circuits. 
 
--- 
 
## 6.4 Voltage Levels 
 
The output HIGH level should approach the supply voltage, while the output LOW level should approach ground. 
 
The simulated waveform and VTC are used to verify these logic levels. 
 
--- 
 
## 6.5 Transistor Sizing 
 
Transistor dimensions influence: 
 
- Drive strength 
- Switching threshold 
- Rise time 
- Fall time 
- Propagation delay 
- Power behaviour 
 
Therefore, PMOS and NMOS sizing is an important part of standard-cell optimization. 
 
--- 
 
# 7. 16-Mask CMOS Fabrication 
 
The fabrication section explains how the CMOS transistor structure is formed through the 16-mask process studied in this module. 
 
The sequence progresses from the silicon substrate through well formation, gate formation, LDD implantation, source/drain formation, contacts and metal interconnections. 
 
--- 
 
## 7.1 Starting Substrate 
 
The process begins with a P-type silicon substrate. 
 
The substrate provides the foundation for CMOS device fabrication. 
 

 
**Figure:** Selection of P-type silicon substrate. 
<img width="1053" height="432" alt="image" src="https://github.com/user-attachments/assets/bd867ad2-ca70-4577-bde2-0b4747b26f0e" />

--- 
 
## 7.2 Active Region Definition — Mask 1 
 
The active regions are defined using the first mask. 
 
Field oxide provides isolation between neighbouring active device regions. The LOCOS process is used to create the required isolation structure. 
 
 
 
**Figure:** Active-region formation using Mask 1. 
 <img width="1077" height="432" alt="image" src="https://github.com/user-attachments/assets/12050530-ea0b-4944-b786-457c7059e456" />

--- 
 
## 7.3 Well Formation 
 
### P-Well 
 
Boron implantation is used to form the P-well region required for NMOS fabrication. 
 

**Figure:** P-well formation using boron implantation. 
<img width="1066" height="437" alt="image" src="https://github.com/user-attachments/assets/a571e7fb-d9e1-46ec-9d9e-40c0f51f0151" />

### N-Well 
 
Phosphorus implantation is used to form the N-well region required for PMOS fabrication. 
 

 
**Figure:** N-well formation using phosphorus implantation. 
<img width="1091" height="427" alt="image" src="https://github.com/user-attachments/assets/742021dc-26a9-4bfd-9b29-e54148b32565" />

Together, the P-well and N-well regions enable complementary PMOS and NMOS devices to be fabricated. 
 
--- 
 
## 7.4 Gate Formation 
 
The gate structure controls the channel between source and drain. 
 

**Figure:** Initial gate structure. 
 <img width="1081" height="442" alt="image" src="https://github.com/user-attachments/assets/98ac6d32-5300-40a5-a002-e927479043cb" />


The gate dimensions have a direct influence on transistor behaviour. 
 
--- 
 
## 7.5 Threshold Voltage and Body Effect 
 
Threshold voltage depends on process parameters, substrate doping, oxide characteristics and source-to-body voltage. 
 
The body effect represents the change in threshold voltage caused by a non-zero source-to-body voltage. 
 

 
**Figure:** Threshold voltage and body-effect analysis. 
<img width="1062" height="440" alt="image" src="https://github.com/user-attachments/assets/ef81d218-34f9-43c9-9197-de4bf0a1c504" />

 
## 7.6 Gate Patterning 
 
The gate structure is defined through successive masking and patterning operations. 
 
### Mask 4 
 

**Figure:** Gate patterning using Mask 4. 
<img width="1075" height="435" alt="image" src="https://github.com/user-attachments/assets/6c20f5b9-bfb3-4551-af7f-6959cffb065e" />

### Mask 5 
 
 
 
**Figure:** Gate processing using Mask 5. 
<img width="1072" height="446" alt="image" src="https://github.com/user-attachments/assets/4e65cb51-b14b-466f-96a5-99ae8cbb984c" />

### Mask 6 
 

**Figure:** Gate formation using Mask 6. 
 <img width="1073" height="437" alt="image" src="https://github.com/user-attachments/assets/988de0cd-50af-4105-8f7c-6ca3f435ab46" />


The patterned gate provides the reference for subsequent source/drain processing. 
 
--- 
 
# 8. LDD and Source/Drain Formation 
 
## 8.1 Lightly Doped Drain — Mask 7 
 
LDD regions are introduced near the channel to reduce the electric field near the drain and improve device reliability. 
 

 
**Figure:** LDD formation using Mask 7. 
<img width="1071" height="433" alt="image" src="https://github.com/user-attachments/assets/a3e02614-e8fa-4ac6-be3c-b73fc42f938b" />


--- 
 
## 8.2 Complementary LDD — Mask 8 
 
The corresponding LDD implantation is performed for the opposite transistor type. 
 


 
**Figure:** Complementary LDD implantation using Mask 8. 
<img width="1068" height="433" alt="image" src="https://github.com/user-attachments/assets/1a2608c6-b509-4039-8a55-74b49ae53358" />

--- 
 
## 8.3 Source and Drain — Mask 9 
 
Heavily doped source and drain regions are formed around the channel. 
 

 
**Figure:** Source and drain formation using Mask 9. 
 <img width="1075" height="425" alt="image" src="https://github.com/user-attachments/assets/8baf2d7f-d1db-44e7-8ed1-d10aa21d91f5" />


--- 
 
## 8.4 Source and Drain — Mask 10 
 
The complementary source/drain implantation is performed for the opposite transistor type. 
 

 
**Figure:** Complementary source and drain implantation. 
 
<img width="1053" height="427" alt="image" src="https://github.com/user-attachments/assets/c13f31ac-2ef2-4206-9664-85a8f6bcf740" />


At this stage, the essential PMOS and NMOS structures are established. 
 
--- 
 
# 9. Interconnect Formation 
 
## 9.1 Local Contacts 
 
Contacts connect the transistor terminals to the metal interconnect system. 
 
They provide electrical paths from the gate, source and drain regions to the required routing layers. 
 

**Figure:** Local contacts and interconnect formation. 
 <img width="1072" height="423" alt="image" src="https://github.com/user-attachments/assets/0245e178-d269-4347-9866-20fc47ec3254" />

--- 
 
## 9.2 Higher-Level Metal 
 
Higher metal layers provide routing between different parts of the integrated circuit. 
 
These layers are used for signal, power and ground connections. 
 

**Figure:** Higher-level metal interconnections. 
 <img width="872" height="433" alt="image" src="https://github.com/user-attachments/assets/22b4633f-e399-46ba-9de7-786b0054cc77" />

The final metallization completes the electrical connections of the CMOS structure. 
 
--- 
 
# 10. Design-to-Fabrication Relationship 
 
The complete practical flow studied in this module can be represented as: 
 
```text 
CMOS Circuit Design 
        ↓ 
SPICE Characterization 
        ↓ 
Timing & VTC Analysis 
        ↓ 
Transistor Sizing 
        ↓ 
SKY130A Technology Setup 
        ↓ 
Standard-Cell Layout 
        ↓ 
Cell Boundary & Power/Ground 
        ↓ 
Layout Extraction 
        ↓ 
SPICE Netlist 
        ↓ 
NGSPICE Post-Layout Simulation 
        ↓ 
Waveform Verification 
        ↓ 
CMOS Fabrication Process 
        ↓ 
Completed Physical Device
```
## 🧠 Key Learnings

* Understood CMOS inverter design and SPICE characterization.
* Analysed VTC, switching threshold, rise/fall time, and propagation delay.
* Implemented a CMOS inverter using the SKY130A standard-cell flow.
* Performed layout extraction and post-layout simulation using NGSPICE.
* Understood the major stages of the 16-mask CMOS fabrication process.

---

## 🏁 Conclusion

This module provided practical understanding of the complete CMOS design flow:

**Circuit Design → Simulation → Layout → Extraction → Verification → Fabrication**

It connected transistor-level design with physical implementation and CMOS fabrication.
