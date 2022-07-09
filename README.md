# AdvancedSynthesisandSTAwithDC






## Table of Contents
- [Introduction](#-introduction)
- [1. Day 1 - Introduction to Logic Synthesis](#1-day-1---introduction-to-logic-synthesis)
  - [1.1. Introduction to DC](#11-introduction-to-dc)
  - [1.2. Invoking dc Basic setup](#invoking-dc-basic-setup)
  - [1.3. Introduction to ddc gui with Design vision](#13-introduction-to-ddc-gui-with-design-vision)
  - [1.4. Labs using DC Synopsys DC Setup](#14-labs-using-dc-synopsys-dc-setup)
  - [1.5 TCL Scripting](#15-tcl-scripting)
 - [2. Day 2 -  Basics to STA](#2-day-2---basics-of-sta)
   - [2.1. Introduction to STA](#21-introduction-to-sta)
   - [2.2. Timing dot Libs](#22-timing-dot-libs)
   - [2.3. Exploring dotLib](#23-various-flop-coding-styles-and-optimization)
- [3. Day 3 - Advanced Constraints](#3-day-3---advanced-constraints)
  - [3.1. Clock Tree Modelling - Uncertainty](#31-clock-tree-modelling---uncertainty)
  - [3.2. Loading Design get_cells, get_ports, get_nets](#32-loading-design-get_cells-get_ports-get_nets)
  - [3.3. Loading Design get_pins, get_clocks, querying_clocks](#33--loading-design-get_pins-get_clocks-querying_clocks)
  - [3.4 Creating Clock Waveforms](#34-creating-clock-waveforms)
  - [3.5 Clock Network Modelling - Uncertainty, report_timing](#35-clock-network-modelling---uncertainty-report_timing)
  -  [3.6 IO Delays](#36-io-delays)
  -  [3.7 SDC  generated_clk](#37-sdc--generated_clk)
  -  [3.8 SDC  vclk, max_latency, rise_fall IODelays](#38-sdc--vclk-max_latency-rise_fall-iodelays)
- [4.Day 4 - Optimizations](#day-4---optimizations)
  - [4.1 Combinational  Optimizations](#41-combinational--optimizations)
  - [4.2 Sequential  Optimizations]()
  - [4.3 Boundary Optimization]()
  - [4.4 Register Retiming]()

- [5. Day 5 - Quality Checks]()
  - [ 5.1 Report timing]()
  - [5.2 Lab Check_timing, Check_design, Set_max_capacitance, HFN]()
 


# Introduction
Design Compiler is an Advanced Synthesis Tool used by leading semiconductor companies across world.

Synthesis of logic circuits plays a crucial role in optimizing the logic and achieving the targeted performance, area and power goals of an IC.

Understanding the fundamentals of design are very important to give the correct inputs to the tool to achieve the best-in-class netlist quality.

This workshop explores the following aspects,

- Design fundamentals
- Setting up DC for synthesis
- Understanding STA
- Understanding and writing the Synopsys Design Constraints [SDC].
- Analyzing the quality of netlist synthesized.
---------------------------------------------------------------------------------------------------------------------------------------------------------------------
## 1. Day 1 - Introduction to Logic Synthesis
### 1.1. Introduction to DC
Design Compiler RTL synthesis solution enables users to meet today's design challenges with concurrent optimization of timing, area, power and test. Design Compiler includes innovative topographical technology that enables a predictable flow resulting in faster time to results.  

**Benefits**:

- Concurrent optimization of timing, area, power and test
- Results correlate within 10% of physical implementation
- Removes timing bottlenecks by creating fast critical paths
- Gate-to-gate optimization for smaller area on new or legacy designs while maintaining timing Quality of Results (QoR)
- Cross-probing between RTL, schematic, and timing reports for fast debug
- Offers more flexibility for users to control optimization on specific areas of designs
- Enables higher efficiency with integrated static timing analysis, test synthesis and power  synthesis
- Support for multi voltage and multi supply
- 2X faster runtime on quad-core compute servers

**SDC**
The Synopsys Design Constraints (SDC) format is used to specify the design intent, including timing, power and area constraints for a design. This format is used by different EDA tools to synthesize and analyse a design.SDC file syntax is based on TCL format and all commands of sdc file follow the TCL syntax. 
**DC Setup**
![image](https://user-images.githubusercontent.com/55539862/175645409-80bbf9f1-0048-4591-b8ca-0ca366e2aa69.png)


### 1.2. Labs using Design Compiler
 
#### ENVIRONMENT SETUP

```
//create a directory
$ mkdir DC_WORKSHOP 
//Git Clone sky130RTLDesignAndSynthesisWorkshop. 
$ git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
```
![image](https://user-images.githubusercontent.com/55539862/175645982-dfc2be32-01f3-4836-8c75-f9d4ae32a2f5.png)
 
**sky130RTLDesignAndSynthesisWorkshop** Directory has: My_Lib - which contains all the necessary library files; where lib has the standard cell libraries to be used in synthesis and verilog_model with all standard cell verilog models for the standard cells present in the lib. Ther verilog_files folder contains all the experiments for lab sessions including both verilog code and test bench codes.


## Invoking dc Basic setup


![image](https://user-images.githubusercontent.com/55539862/175649128-efb97bae-bb6e-4269-a2f7-20fab63d511c.png)



![image](https://user-images.githubusercontent.com/55539862/177386077-ebdd87e2-e360-40b3-94dd-0c677e07e198.png)


#### Access Module Files
```
$ gedit /home/irene/sky130RTLDesignAndSynthesisWorkshop/DC_WORKSHOP/verilog_files/lab1_flop_with_en.v
```

![image](https://user-images.githubusercontent.com/55539862/175655761-819d376a-e843-4c96-bd37-bbe664f6a7e2.png)
![image](https://user-images.githubusercontent.com/55539862/175656311-fad775f2-0c2f-40e7-9a45-6b64b583dd5b.png)


### 1.3. Introduction to ddc gui with Design vision



![image](https://user-images.githubusercontent.com/55539862/175759734-64338b9b-097d-4c2a-9848-ce91dee15840.png)




>_.lib file is a collection of logical modules which includes all basic logic gates. It may also contain different flavors of the same gate (2 input AND, 3 input AND – slow, medium and fast version)._
#### Faster cells and Slower Cells

A cell delay in the digital logic circuit depends on the load of the circuit which here is Capacitance.

Faster the charging / discharging of the capacitance --> Lesser is the Cell Delay

Inorder to charge/discharge the capacitance faster, we use wider transistors that can source more current. This will help us reduce the cell delay but at the same time, wider transistors consumer more power and area. Similarly, using narrower transistors help in reduced area and power but the circuit will have a higher cell delay. Hence, we have to compromise on area and power if we are to design a circuit with low cell delay.

#### Constraints

A Constraint is a guidance file given to a synthesizer inorder to enable an optimum implementation of the logic circuit by selecting the appropriate flavour of cells (fast or slow).

### 1.4. Labs using DC Synopsys DC Setup
![image](https://user-images.githubusercontent.com/55539862/177386518-d49a44b7-9d77-4ad0-9aff-d4c0fb6217c9.png)
![image](https://user-images.githubusercontent.com/55539862/175760776-17ddb649-2aa8-4944-882b-75ee9e61b796.png)


### 1.5 TCL Scripting

![image](https://user-images.githubusercontent.com/55539862/175760634-2fb0b3b5-143b-4408-a809-c788e58fc742.png)
![image](https://user-images.githubusercontent.com/55539862/175761470-88a4e96b-9e3a-4a65-aebe-477a072d0a64.png)

![image](https://user-images.githubusercontent.com/55539862/175761429-e891e889-7514-4b83-88e6-aadb3bf7025a.png)

-------------------------------------------------------------------------------------------------------------------------------------------------------------------- 

## 2. Day 2 - Basics of STA
### 2.1. Introduction to STA
Static timing analysis (STA) is a method of validating the timing performance of a design by checking all possible paths for timing violations. STA breaks a design down into timing paths, calculates the signal propagation delay along each path, and checks for violations of timing constraints inside the design and at the input/output interface.
![image](https://user-images.githubusercontent.com/55539862/175761772-7a7ab402-6495-4f4d-959e-f009dbdca220.png)






### 2.2. Timing dot Libs
![image](https://user-images.githubusercontent.com/55539862/175762586-57be47d9-c7de-4ed0-8a22-03281a84769d.png)

### 2.3. Exploring Exploring dotLib 
![image](https://user-images.githubusercontent.com/55539862/175762887-a75aad9b-46b1-49c1-ba12-44948eefc10a.png)

![image](https://user-images.githubusercontent.com/55539862/175765808-18b21585-ccd8-441c-9a9f-8a8202e6d816.png)

![image](https://user-images.githubusercontent.com/55539862/175765782-c2b70c26-4c6e-413b-96fd-3be0e8575d3f.png)




----------------------------------------------------------------------------------------------------------------------------------------------------------

## 3. Day 3 - Advanced Constraints
**Clock**
A clock signal oscillates between a high and a low state and is used like a metronome to coordinate actions of digital circuits. A clock signal is produced by a clock generator.

![image](https://user-images.githubusercontent.com/55539862/175766162-54bb5bf7-d9d6-417d-818d-f07249ba2de2.png)

### 3.1. Clock Tree Modelling - Uncertainty
Clock uncertainty is the difference between the arrivals of clocks at registers in one clock domain or between domains. it can be classified as static and dynamic clock uncertainties.

Timing Uncertainty of clock period is set by the command set_clock_uncertainty at the synthesis stage to reserve some part of the clock period for uncertain factors (like skew, jitter, OCV, CROSS TALK, MARGIN or any other pessimism) which will occur in PNR stage. The uncertainty can be used to model various factors that can reduce the clock period.
It can define for both setup and hold.

![image](https://user-images.githubusercontent.com/55539862/175766016-26ebf238-999b-4ae1-a55a-2ddbe158e7b5.png)

### 3.2 Loading design get_cells, get_ports, get_nets

![image](https://user-images.githubusercontent.com/55539862/175768690-3564622e-68d2-46e4-8621-f2ec4d502339.png)
 
 ### 3.3  Loading Design get_pins, get_clocks, querying_clocks
 
 ### #get_pins
 
 ![image](https://user-images.githubusercontent.com/55539862/175775642-ec759034-54f0-4de9-b68c-1b01fb1b8935.png)
 
 #### querying_clocks
![image](https://user-images.githubusercontent.com/55539862/177390652-a82580e3-e330-469e-a8b3-f2a1458485b6.png)

### 3.4 Creating Clock Waveforms

![image](https://user-images.githubusercontent.com/55539862/177392240-b064a88a-e3f6-4908-ac21-45ba27cd0b63.png)

#### 25% Duty cycle Clock
![image](https://user-images.githubusercontent.com/55539862/177393409-4959d5a5-92fd-4d84-b792-0af91e870a83.png)

### 3.5 Clock Network Modelling - Uncertainty, report_timing

![image](https://user-images.githubusercontent.com/55539862/177475503-1c1c715f-4871-4fad-9d7d-c7ea7cbed09d.png)


![image](https://user-images.githubusercontent.com/55539862/177475340-cf038439-bd0a-4d6d-a1a1-82cfb2ffc83c.png)
```
$  report_timing -to REGC_reg/D -delay min

```
![image](https://user-images.githubusercontent.com/55539862/177477065-1d8261ac-5fc6-4775-b5bf-ccb846aab54f.png)

```
$ report_timing -to REGC_reg/D -delay max

```
![image](https://user-images.githubusercontent.com/55539862/177476692-33a64f3f-e84f-4b86-83f9-4365d1c4b98f.png)


### 3.6 IO Delays

```
$ set_input_delay -max 5 -clock [get_clocks myclk] [get_ports IN_A]
$set_input_delay -max 5 -clock [get_clocks myclk] [get_ports IN_B]
$report_port -verbose

```
![image](https://user-images.githubusercontent.com/55539862/177478803-51ac0cd8-1d82-485f-abf6-871e5900d21f.png)

```
$ set_input_delay -min 1 -clock [get_clocks myclk] [get_ports IN_A]
$set_input_delay -min 1 -clock [get_clocks myclk] [get_ports IN_B]
$report_timing -from IN_A -trans -nosplit

```
![image](https://user-images.githubusercontent.com/55539862/177480559-20f6aa9d-b9d5-45bd-a02a-b375d8095818.png)

```
$ set_input_delay -max 5 -clock [get_clocks myclk] [get_ports OUT_Y]
$set_input_delay -min 1 -clock [get_clocks myclk] [get_ports OUT_Y]
$report_timing -from OUT_Y -trans -nosplit

```
![image](https://user-images.githubusercontent.com/55539862/177481519-af9be744-1038-46c2-83c8-d81ca9380035.png)
```
$ set_load -max 0.4 [get_ports OUT_Y]

$report_timing -to OUT_Y -cap -trans -nosplit

```


![image](https://user-images.githubusercontent.com/55539862/177482184-fd893cb8-e8e5-4c89-8241-4f8509f1a44f.png)

```
$ set_load -min 0.1 [get_ports OUT_Y]

$report_timing -to OUT_Y -cap -trans -nosplit -delay min

```

![image](https://user-images.githubusercontent.com/55539862/177482935-58140255-d013-45ee-90df-8d7096bfa933.png)

### 3.7 SDC  generated_clk
```
$ create_generated_clock -source reference_pin [-divide_by divide_factor] [-multiply_by multiply_factor] [-invert] source

```
![image](https://user-images.githubusercontent.com/55539862/177484874-a14a1b77-ae5d-4d1b-bb5e-04108a4dcc79.png)

Creates a generated clock in the current design at a declared source by defining its frequency with respect to the frequency at the reference pin. The static timing analysis tool uses this information to compute and propagate its waveform across the clock network to the clock pins of all sequential elements driven by this source. 

The generated clock information is also used to compute the slacks in the specified clock domain that drive optimization tools such as place-and-route.

```
$ create_generated_clock -name MYGEN_CLK -master myclk -source [get_ports clk] -div 1 [get_ports out_clk]
$report_clocks *

```
![image](https://user-images.githubusercontent.com/55539862/177486649-850d9256-73c9-45e7-ac87-7832a1098ca0.png)

![image](https://user-images.githubusercontent.com/55539862/177492430-650443f3-546e-4c04-a9fd-08a132ec21eb.png)

![image](https://user-images.githubusercontent.com/55539862/177492559-28a3b370-e311-4d9a-b596-f1047431d5ce.png)


### 3.8 SDC  vclk, max_latency, rise_fall IODelays

#### - Virtual clock - purpose and timing
A virtual clock is used as a reference to constrain the interface pins by relating the arrivals at input/output ports with respect to it with the help of input and output delays.
![image](https://user-images.githubusercontent.com/55539862/177493447-df6fbcb2-7fe7-48e3-9f6b-a55004eeb7ca.png)


```
$ create_clock –name VCLK –period 10

```

#### Set driving cells
It specifies the drive characteristics of input or inout ports that are driven by the cells in the technology library. These commands associate a library pin with input ports so that delay calculation can be accurately modelled.


![image](https://user-images.githubusercontent.com/55539862/177722339-3dae7b36-b199-4d69-b9d9-99202ec02d1f.png)

![image](https://user-images.githubusercontent.com/55539862/177723514-8993da75-91e6-4848-8398-0d87c164ee32.png)

- VCLK
```
$ create_clock -name MYCLK -per 10
```
![image](https://user-images.githubusercontent.com/55539862/177740212-dd76b3a6-3952-49c9-bcbc-20b421cb6742.png)

![image](https://user-images.githubusercontent.com/55539862/177740101-bc6ae068-a8f9-433a-8fd0-2b2253f29f88.png)

-------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Day 4 - Optimizations

  ### 4.1 Combinational  Optimizations
The combinational optimization phase transforms the logic-level description of the
combinational logic to a gate-level netlist.
Combinational optimization includes
- Technology-Independent Optimization
This optimization operates at the logic level. Design Compiler represents the gates as a
set of Boolean logic equations.
- Mapping
During this process, Design Compiler selects components from the logic library to implement the logic structure.
- Technology-Specific Optimization
This optimization operates at the gate level.
  
  ![image](https://user-images.githubusercontent.com/55539862/177751853-221a5598-934b-416e-b49b-6d7051275603.png)

  ![image](https://user-images.githubusercontent.com/55539862/177748680-7cdec838-3469-44ce-b650-6fa1480bebfb.png)

       
  ### 4.2 Sequential  Optimizations
  
   Sequential optimization includes the initial optimization phase, which maps sequential cells
to cells in the library, and the final optimization phase, where Design Compiler optimizes
timing-critical sequential cells (cells on the critical path):
- Initial Sequential Optimization
- Final Sequential Optimization
- 
  ![image](https://user-images.githubusercontent.com/55539862/177751439-8270e1cd-1133-4b24-84ed-86ae7667bdb0.png)
![image](https://user-images.githubusercontent.com/55539862/177834410-ffcb2792-92b7-4fe6-8051-6cbb51a1f1d3.png)

 
  
  ### 4.3 Boundary Optimization
  ```
  $ set_boundary_optimization u_im false
  
  ```
  ![image](https://user-images.githubusercontent.com/55539862/177837375-e5670dbe-c0dd-4093-91e4-cdff5acb3ee6.png)
  
### 4.4 Register Retiming
![image](https://user-images.githubusercontent.com/55539862/177840659-32fbc01d-0f0e-40e9-a976-de41ab282d53.png)

 
 ### 4.5 Isolating output ports
 
 ![image](https://user-images.githubusercontent.com/55539862/177844399-c37c628c-e86e-4b79-b3d1-3b52e10238fb.png)
![image](https://user-images.githubusercontent.com/55539862/177849079-957b1222-4c60-4660-8132-ef7d8f459409.png)

 
 ### 4.6 Multicycle Paths
 
 ```
 $set_multicycle_path -setup 2 -to prod_reg[*]/D -from [all_inputs]

 $set_multicycle_path -hold 1 -to prod_reg[*]/D -from [all_inputs]
 
 ```
 ![image](https://user-images.githubusercontent.com/55539862/177851690-0f65dcc4-ac1a-4f6e-b1b9-22302becd13a.png)

 -------------------------------------------------------------------------------------------------------------------------------------------------------------------
 
## Day 5 - Quality Checks

### Report timing
![image](https://user-images.githubusercontent.com/55539862/178103719-94d643b9-1942-40b9-bd47-cd402cbd3245.png)
![image](https://user-images.githubusercontent.com/55539862/178105529-5e986d43-9747-4e70-b949-f203a9b97133.png)


### Check_timing, Check_design, Set_max_capacitance, HFN

![image](https://user-images.githubusercontent.com/55539862/178107936-d236bf53-d476-4e5a-b36d-22fb53279ed5.png)

![image](https://user-images.githubusercontent.com/55539862/178107377-49df4a56-e400-48c2-8364-3fa978df0ea2.png)







## ACKNOWLEDGEMENTS

  * Kunal Ghosh, Co-Founder (VLSI SYSTEM DESIGN - VSD)
  
## References
   * https://www.vlsisystemdesign.com/rtl-design-using-verilog-with-sky130-technology/?q=%2Frtl-design-using-verilog-with-sky130-technology%2F&v=a98eef2a3105
   * https://github.com/kunalg123/vsdflow
   * https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop
   * https://github.com/google/skywater-pdk
