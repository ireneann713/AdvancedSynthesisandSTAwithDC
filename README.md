# AdvancedSynthesisandSTCwithDC






## Table of Contents
- [Introduction](#-introduction)
- [1. Day 1 - Introduction to Logic Synthesis](#1-day-1---introduction-to-logic-synthesis)
  - [1.1. Introduction to DC](#11-introduction-to-dc)
  - [1.2. Invoking dc Basic setup](#invoking-dc-basic-setup)
  - [1.3. Introduction to ddc gui with Design vision](#13-introduction-to-ddc-gui-with-design-vision)
  - [1.4. Labs using DC Synopsys DC Setup](#14-labs-using-dc-synopsys-dc-setup)
  - [1.5 TCL Scripting](#15-tcl-scripting)
 - [2. Day 2 -  Basics to STA](#2-day-2---Introduction-to-sta)
   - [2.1. Introduction to STA](#21-introduction-to-sta)
   - [2.2. Timing dot Libs](#22-timing-dot-libs)
   - [2.3. Exploring dotLib](#23-various-flop-coding-styles-and-optimization)
- [3. Day 3 - Advanced Constraints](#3-day-3---advanced-constraints)
  - [3.1. Clock Tree Modelling - Uncertainty](#31-clock-tree-modelling---uncertainty)
  - [3.2. Combinational logic Optimizations](#32-combinational-logic-optimizations)
  - [3.3. Sequential logic Optimizations](#33-sequential-logic-optimizations)
- [4. Day 4 - GLS, blocking vs non-blocking and Synthesis-Simulation mismatch](#4-day-4---GLS,-blocking-vs-non-blocking-and-Synthesis-Simulation-mismatch)
  - [4.1. GLS, Synthesis-Simulation mismatch and Blocking/Non-blocking statements](#41-GLS,-Synthesis-Simulation-mismatch-and-Blocking/Non-blocking-statements)
  - [4.2. Labs on GLS and Synthesis-Simulation Mismatch](#42-Labs-on-GLS-and-Synthesis-Simulation-Mismatch)
  - [4.3. Labs on synth-sim mismatch for blocking statement](#43-Labs-on-synth-sim-mismatch-for-blocking-statement)
- [5. Day 5 - Optimization in synthesis](#1-day-1---optimization-in-synthesis)
  - [5.1. If Case constructs](#51-if-Case-constructs)
  - [5.2. Labs on "Incomplete If Case"](#52-labs-on-"incomplete-if-case")
  - [5.3. Labs on "Incomplete overlapping Case"](#53-labs-on-"incomplete-overlapping-case")
  - [5.4. for loop and for generate"](#54-for-loop-and-for-generate")
  - [5.5. Labs on "for loop" and "for generate"](#55-labs-on-"for-loop"-and-"for-generate")
  - 
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


![image](https://user-images.githubusercontent.com/55539862/175650960-523b2ba9-51da-4d0d-b9a3-0c143f677a9d.png)

![image](https://user-images.githubusercontent.com/55539862/175654780-5051236c-ccab-442b-a79d-bdbd13b0f096.png)


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
![image](https://user-images.githubusercontent.com/55539862/175760281-4092c6f6-814e-4d7f-a0a5-323c926384bf.png)
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

### Loading design get_cells, get_ports, get_nets

![image](https://user-images.githubusercontent.com/55539862/175768690-3564622e-68d2-46e4-8621-f2ec4d502339.png)


-------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 4. Day 4 - Gate Level Simulation(GLS), Blocking vs Non-blocking and Synthesis-Simulation Mismatch

### 4.1.GLS, Synthesis-Simulation mismatch and Blocking/Non-blocking statements
**What is Gate Level Simulation (GLS) ?**

Running the testbench against the synthesized netlist ouput as a DUT is known as Gate Level Simulation (GLS). The Output netlist should logically be same as the RTL code so that the testbench will align itself when we simulate both the files to obtain the waveforms.

**_Advantages of GLS:_**

 *To logically verify the correctness of the design after Synthesis.
 *During the RTL Simulation, timing was not accounted. But for practical applications, there is a need to ensure the timing of the design to be met.
 
**Why GLS?**

GLS is required to verify the logical correctness of the design post synthesis with the help of the netlist file. It ensures whether the timing of the design is met and for thi, the GLS used to run with delay annotations.

<img width="400" alt="GLS" src="https://user-images.githubusercontent.com/93824690/166241284-6638d96e-4e21-4c93-a238-ed43834ecf37.png">

```
//consider a netlist
and uand (.a(a),.b(b))
or uor (.a(a),.b(b))
//There is a need to define the meaning of and and or
//Thus we need netlist, testbench and verilog models of the standard cells
```

>_Netlist consists of all standard cells instantiated and it's meaning is conveyed to the iVerilog using Gate Level Verilog Models. Gate Level Verilog Models can be functional or timing aware. If the gate level models are delay annotated, then GLS can be performed for timing validation also in addition to functional validation._
### SYNTHESIS SIMULATION MISMATCH

If netlist is a true reciprocation of RTL, what is the need to validate the functionality of netlist? There may be synthesis and simulation mismatch due to the following reasons:

**(I)_Missing Sensitivity List_**
**(II)_Blocking Vs Non Blocking Assignments_**
**(III)_Non Standard Verilog Coding_**

**(I)Missing Sensitivity List**

```
module mux(
input i0,input i1
input sel,
output reg y
);
always @ (sel)
begin
   if (sel)
            y = i1;
   else 
            y = i0;          
end
endmodule
```
The output of Simulator changes only when the input changes. The output is not evaluated when there is no activity. In the above 2x1 mux code, when select is changing (when select is 1), the output is 1 when input is 1 else the output is 0. The always block evaluates only when there is a transition change in select pin, and is not sensitive (output does not reflect) to changes in the inputs 0 and 1.

**_Corrected code for missing sensitivity list:_**
```
module mux(
input i0,input i1
input sel,
output reg y
);
always @ (*)
begin
   if (sel)
            y = i1;
   else 
            y = i0;        
end
endmodule
```
>_mismatch is corrected by having always @ (*) where the always block is evaluated when any signal changes. So, any changes in inputs will also be seen in the output._
#### (II)Blocking Vs Non Blocking Assignments in Verilog

Blocking and Non-blocking statements are procedural assignment statements that can be implemented only inside an always block.

*Blocking Assignments --> =
   *Executes the statements in the order in which they are coded.
   
*Non-blocking Assignments --> <=
   *Executes the RHS of all such assignments when the always block is entered and assigned to LHS in a parallel evaluation.
   
_Synthesis-Simulation mismatches due to incorrect ordering of the blocking assignments done inside an always block._

#### CAVEAT 1:-
```
module code (input clk,input reset,
input d,
output reg q);
always @ (posedge clk,posedge reset)
begin
if(reset)
begin
        q0 = 1'b0;
        q = 1'b0;
end
else
        q = q0;
        q0 = d;    
end
endmodule
```
<img width="400" alt="cav1" src="https://user-images.githubusercontent.com/93824690/166245183-8f2608db-88cb-4966-beba-5038ff45390b.png">

>_The assignments inside the code represent the blocking statements. q0 and q are assigned to 1 bit 0s - so asynchronous reset connection happens. However, in the later parts, q0 is assigned to q and then d gets assigned to q0. If suppose, there is a change in the code._
```
module code (input clk,input reset,
input d,
output reg q);
always @ (posedge clk,posedge reset)
begin
if(reset)
begin
        q0 = 1'b0;
        q = 1'b0;
end
else
        q0 = d;
        q = q0;    
end
endmodule
```
>_In this case, d is assigned to q0 and then q0 is assigned to q. So, by the time the second statment gets executed, q0 has the value of d. This will lead to implementation of only one flop. Previously, q has the value of q0 and q0 has the value of d - which lead to implementation of 2 storage elements._
#### _Always Use Non- Blocking Statements when writng the Sequential circuits code_

#### CAVEAT 2:- Causing Synthesis Simulation Mismatch
```
module code (input a,b,c
output reg y);
reg q0;
always @ (*)
begin
        y = q0 & c;
        q0 = a|b ;    
end 
endmodule
```
>_The code is aimed to create a function of y = (A+B).C. In the above code, when the code enters always block, due to the presence of blocking statements, they get evaulated in order. So y gets evaluated first (q0.C), where the q0 results corresponds to the previous iteration's result. The q0 value gets updated only in the second statement._
When the order of the statements is changed: In this case, a OR b is evaluated first and the latest value is used for calculating y.
```
module code (input a,b,c
output reg y);
reg q0;
always @ (*)
begin
        q0 = a|b ;
        y = q0 & c;
end 
endmodule
```

**> Therefore there is a paramount importance to run the GLS on the netlist and match the specifications, to ensure there is no simulation synthesis mismatch.**

#### (i) ternary_operator.v
>_Note: Mux function is written using a ternary operator. Ternary operator takes 3 operands with the format.
```
<Condition>?<True>:<False>
```
**_Verilog File_**
<img width="641" alt="Screenshot (288)" src="https://user-images.githubusercontent.com/93824690/166249218-f94c6544-cef7-4861-853b-9e99f0f250f5.png">

**_GTK Wave_**

<img width="641" alt="Screenshot (237)" src="https://user-images.githubusercontent.com/93824690/166249367-e4b5567f-f683-4a5e-89fb-b875ad6eeaf4.png">

**_Statistics_**

<img width="400" alt="ter st" src="https://user-images.githubusercontent.com/93824690/166249983-99797adb-801c-43d4-983d-4c8154f34cbd.png">

**_Realization of Logic_**

<img width="641" alt="Screenshot (238)" src="https://user-images.githubusercontent.com/93824690/166249396-10ea2950-f074-4f53-b8e1-296fea18491d.png">

>_NAND gate with i1 and sel, inverted io and Or to And invert gate, to which the inputs are sel and inverted i0. The output y is given by the expression = sel'.i0 + sel.i1_

**_GLS OUTPUT_**

<img width="700" alt="gls gtk" src="https://user-images.githubusercontent.com/93824690/166250831-c7b446ef-d96e-46a0-893c-edf6e9780d30.png">

#### MISSING SENSITIVITY LIST

#### (ii): bad_mux.v showing mismatch due to missing sensitivity list

**_Verilog File_**

<img width="400" alt="bad mo" src="https://user-images.githubusercontent.com/93824690/166252999-87ca21b7-ef7e-4f61-98fa-3167fa3be26f.png">

>_during Simulation, the logic acts as a latch and during synthesis, it acts as a mux._
**_GTK Wave_**

<img width="700" alt="bad gtk" src="https://user-images.githubusercontent.com/93824690/166253027-c182567f-82ff-483a-af77-aabdf6e1d2cd.png">

**_Synthesis Statistics_**

<img width="400" alt="bad st" src="https://user-images.githubusercontent.com/93824690/166253012-6caad5c4-e707-454b-b2f8-d5f1f58885cd.png">

**_GLS Output_**


<img width="700" alt="bad gls" src="https://user-images.githubusercontent.com/93824690/166253052-a961d8b0-dc45-4b4a-a90e-ff5409fa0448.png">


**_Realization of Logic_**

<img width="641" alt="Screenshot (241)" src="https://user-images.githubusercontent.com/93824690/166253100-20d54e2c-58b6-479c-bb7a-296833f7d4ee.png">

>_Confirms the functionality of 2x1 mux after synthesis where when the select is low, activity of input 0 is reflected on y. Similarly, when the select is hight, activity of input 1 is reflected on y. Hence there is a synthesis simulation mismatch due to missing sensitivity list._
#### CAVEATS IN BLOCKING ASSIGNMENTS

#### (iii) blocking_caveat.v showing mismatch due to blocking assignments

**_Verilog File_**

<img width="400" alt="cav mo" src="https://user-images.githubusercontent.com/93824690/166254558-27b5559d-cf33-4089-9863-116efb61f9f3.png">

>_when the code enters always block, due to the presence of blocking statements, they get evaulated in order. So d gets evaluated first (x.c), where the x results corresponds to the previous iteration's result (a|b). The d value gets updated only in the second statement. The output expression is given as d = (a+b).c_
**_Synthesis Statistics_**

<img width="400" alt="cav st" src="https://user-images.githubusercontent.com/93824690/166254570-e078ddec-8f95-4aba-ac01-4d552eb742a3.png">

**_GTK Wave_**

<img width="700" alt="cav gtk" src="https://user-images.githubusercontent.com/93824690/166254577-420c3b5f-2e2e-43ba-a670-f41a828e0e75.png">

>_d = (a+b).c, if the inputs a,b = 0; then a+b = 0. The output d = 0. But, we observe the output d = 1 because it looks at the past value where a+b was 1._
**_GLS Output_**

<img width="700" alt="cav gls" src="https://user-images.githubusercontent.com/93824690/166254673-5b2ff6aa-4aea-4ca1-bb06-28079dc50910.png">

**_Realization of Logic_**

<img width="750" alt="cav re" src="https://user-images.githubusercontent.com/93824690/166254689-2095c677-6fe6-4264-b840-880b8017675b.png">

<_value of output d is 0 after simulation and 1 after synthesis for the same set of input values. Hence there is a synthesis simulation mismatch due to blocking assignments._

-----------------------------------------------------------------------------------------------------------------------------------------------------------------

## 5. Day 5 - Optimization in synthesis

### 5.1 If Case constructs
The if statement is a conditional statement which uses boolean conditions to determine which blocks of verilog code to execute. If always translates into Multiplexer. It is used for priority Logic and are always used inside always block.The variable should be assigned as a register.

**_Syntax for IF Statement_**
```
if<cond>
begin
.....
.....
end
else
begin
.....
.....
end
```
**_Syntax for IF- ELSE-IF Statement_**
```
if<cond1>
begin
.....
 executes cb1
.....
end
else if<cond2>
begin
.....
  executes cb2
.....
end
else if<cond3>
begin
.....
  executes cb3
.....
end
else
begin
.....
  executes cb4
.....
end
```
**_Hardware Implementation_**

<img width="400" alt="hd imp" src="https://user-images.githubusercontent.com/93824690/166259138-8166cfbc-1ae2-4260-87ad-4aca82c56c94.png">

**_Cautions with using IF Statements_**
Inferred latches can serve as a 'warning sign' that the logic design might not be implemented as intended. They represent a bad coding style, which happens because of incomplete if statements/crucial statements missing in the design. For ex: if a else statement is missing in the logic code, the hardware has not been informed on the decision, and hence it will latch and will tried retain the value. This type of design should be completely avoided unless intended to meet the design functionality (ex: Counter Design).

<img width="400" alt="inf if" src="https://user-images.githubusercontent.com/93824690/166259507-9a71c456-5108-42dc-ad1c-1ebb6e9b0d6f.png">

#### CASE Statements
The hardware implementation is a Multiplexer. Similar to IF Statements, Case statements are also used inside always block and the variable should be a register variable.

**Case Statements**
```
_reg y
always @ (*)
begin
	case(sel)
		2'b00:begin
		      ....
		      end
		2'b01:begin
		      ....
		      end
		      .
		      .
		      .
	endcase	
end
```
**Caveats in CASE Statements**

*Case statements are dangerous when there is an incomplete Case Statement Structure may lead to inferred latches. To avoid inferred latches, code Case with default conditions. 
```
reg y
always @ (*)
begin
	case(sel)
		2'b00:begin
		      ....
		      end
		2'b01:begin
		      ....
		      end
		      .
		      .
	default:begin
         ....
		       end
	endcase	
end
```
*Partial Assignments in Case statements - not specifying the values. This will also create inferred latches. To avoid inferred latches, assign all the inputs in all the segments of the case statement.

### 5.2 INCOMPLETE IF STATEMENTS

#### * (Case 1) incomplete if statements

**_Verilog File_**

<img width="400" alt="100" src="https://user-images.githubusercontent.com/93824690/166263361-bd07fa49-2177-47a7-96d2-5a23e67e9a30.png">

**_GTK Wave_**

<img width="" alt="Screenshot (250)" src="https://user-images.githubusercontent.com/93824690/166262139-4b40b5d0-55e9-448b-968f-1bc5dbd59c4a.png">

>_Else case is missing so there will be a D latch._
**_Synthesis Statistics_**																		  
<img width="400" alt="Screenshot (251)" src="https://user-images.githubusercontent.com/93824690/166262175-01dc3362-8128-43a2-bda4-14be20b26d7d.png">

**_Realization of Logic_**
<img width="700" alt="Screenshot (252)" src="https://user-images.githubusercontent.com/93824690/166262206-b46f9ea8-6fec-4c6f-a1ce-bae4dafc566a.png">

>_synthesized design has a D Latch inferred due to incomplete if structure (missing else statement)._
#### * (Case2) incomplete if statements

**_Verilog File_**

<img width="641" alt="200" src="https://user-images.githubusercontent.com/93824690/166263847-c791f930-eeb1-45ff-8052-f1d247b8bebd.png">

**_GTK Wave_**

<img width="700" alt="Screenshot (254)" src="https://user-images.githubusercontent.com/93824690/166262258-db09ddf3-47d0-44d3-888c-6b45b56d1cdf.png">

>_When i0 is high, the output follows i1. When i0 is low, the output latches to a constant value (when both i0 and i2 are 0). Presence of inferred latches due to incomplete if structure._
**_Synthesis Statistics_**

<img width="400" alt="Screenshot (256)" src="https://user-images.githubusercontent.com/93824690/166262277-2398c16a-7007-4b1e-937d-398989c891f6.png">

**_Realization of Logic_**

<img width="700" alt="Screenshot (257)" src="https://user-images.githubusercontent.com/93824690/166262309-0c66e904-20fb-49ba-859a-97667ca61996.png">


### 5.2 INCOMPLETE CASE STATEMENTS

#### CASE 1: incomplete case statements
**_Verilog File_**

<img width="641" alt="33" src="https://user-images.githubusercontent.com/93824690/166265632-97eb0312-e58f-40e6-9f26-fa6b54abb8ca.png">

**_GTK Wave_**
<img width="700" alt="Screenshot (258)" src="https://user-images.githubusercontent.com/93824690/166264918-dab97bee-102e-43b2-9887-d8d07419b281.png">

>_When select signal is 00, the output follows i0 and is i1 when the select value is 01. Since the output is undefined for 10 and 11 values, the ouput latches to the previously available value._
**_Synthesis Statistics_**

<img width="400" alt="i st" src="https://user-images.githubusercontent.com/93824690/166266006-0a03e9e8-0fc9-45bf-be6a-b82edbaa6f5c.png">

**_Realization of Logic_**

<img width="750" alt="Screenshot (259)" src="https://user-images.githubusercontent.com/93824690/166264934-397bc9a5-7e18-419c-898b-14d910aeafe5.png">

>_The synthesized design has a D Latch inferred due to incomplete case structure (missing output definition for 2 of the select statements)._
#### CASE 2: incomplete case statements with default

**_Verilog File_**

<img width="641" alt="comp case mo" src="https://user-images.githubusercontent.com/93824690/166266795-cf2d4b1c-76f3-434f-a86a-873ead2f66e8.png">

**_Synthesis Statistics_**

<img width="400" alt="comp st" src="https://user-images.githubusercontent.com/93824690/166266812-2ae22097-2241-49a3-8a10-96bab1d0ff09.png">

**_GTK Wave_**

<img width="700" alt="Screenshot (260)" src="https://user-images.githubusercontent.com/93824690/166264948-65dc25ba-0bdd-4ff2-b6de-8b3616d665f5.png">

>_When select signal is 00, the output follows i0 and is i1 when the select value is 01. Since the output is undefined for 10 and 11 values, the presence of default sets the output to i2 when the select line is 10 or 11. The ouput will not latch and be a proper combinational circuit._
**_Realization of Logic_**
<img width="750" alt="Screenshot (261)" src="https://user-images.githubusercontent.com/93824690/166264984-ce6f0d0d-473f-409c-9149-f37b5b82234b.png">

#### CASE 3: partial case statement
**_Verilog File_**

<img width="641" alt="p mo" src="https://user-images.githubusercontent.com/93824690/166267883-dfbcec82-f803-458d-bec6-81cd09ad7d4e.png">

**_Synthesis Statistics_**

<img width="400" alt="p st" src="https://user-images.githubusercontent.com/93824690/166267889-5976618e-4223-470e-b9ad-2a33636b8ba6.png">

**_Realization of Logic_**

<img width="781" alt="p re" src="https://user-images.githubusercontent.com/93824690/166267900-ca0787fd-c00b-4a33-96b4-7ea15e5f1c41.png">


### 5.3 STATEMENTS USING FOR

_Understanding the Usage of For and Generate Statements:_

|FOR STATEMENTS|GENERATE STATEMENTS|
|:---:|:---:|
|These statements are used inside the always block|These statements are used outsde the always block|
|Used for evaluating expressions|Used for instantiating/replicating Hardwares|

#### CASE 1: using generate if statement

**_Verilog File_**

<img width="641" alt="1 mo" src="https://user-images.githubusercontent.com/93824690/166277488-bee0a9c4-dc04-4d27-a4fb-28a255550110.png">

**_GTK Wave_**

<img width="750" alt="1 gtk" src="https://user-images.githubusercontent.com/93824690/166269945-ad59baba-f1d2-46a5-94e5-f1bbe2c33c57.png">

#### CASE 2: demux using case statement.v

**_Verilog File_**

<img width="641" alt="2 mo" src="https://user-images.githubusercontent.com/93824690/166270108-568450d2-3a12-4a2c-8a6b-fd9985507a16.png">

**_GTK Wave_**

<img width="750" alt="2 gtk" src="https://user-images.githubusercontent.com/93824690/166270133-35d829cf-49c9-4dd3-bb01-f4c6e3ad7c40.png">

**_Synthesis Statistics_**

<img width="400" alt="2 stk" src="https://user-images.githubusercontent.com/93824690/166270162-b0ac5bc1-e795-427c-8ba2-ef292afaa420.png">

**_Realization of Logic_**

<img width="780" alt="2 re" src="https://user-images.githubusercontent.com/93824690/166270182-340652d4-b730-4d3f-aa08-e41d6a3a8657.png">

**_GLS Output_**

<img width="750" alt="2 gls" src="https://user-images.githubusercontent.com/93824690/166270199-ac2a299b-d0c1-43af-90f8-47070c8dcaaf.png">

#### CASE 3: demux using generate if statement.v 

**_Verilog File_**

<img width="641" alt="3 mo" src="https://user-images.githubusercontent.com/93824690/166270221-3f80357f-b973-46b2-b406-a629f891de48.png">

**_GTK Wave_**

<img width="750" alt="3 gtk" src="https://user-images.githubusercontent.com/93824690/166270250-42c21b20-c041-4a00-b594-5152620a00ac.png">

**_Synthesis Statistics_**

<img width="400" alt="3 st" src="https://user-images.githubusercontent.com/93824690/166270278-9deb11bb-847f-4d8f-9917-3ddddba214e4.png">

**_Realization of Logic_**

<img width="750" alt="3 re" src="https://user-images.githubusercontent.com/93824690/166270294-71548a2e-6f23-49bb-9aa5-a39e7d1bf1d0.png">

**_GLS Output_**

<img width="750" alt="3 gls" src="https://user-images.githubusercontent.com/93824690/166270307-c3b44ff7-47eb-43c3-a2dd-8fd28150234e.png">


### 5.3 STATEMENTS USING GENERATE

**_Experiment on Ripple Carry Adder_**
>_Instantiating the full adder in a loop to replicate the hardware_
**_Verilog File_**
<img width="641" alt="1 mo" src="https://user-images.githubusercontent.com/93824690/166288993-0b0dbf2a-6e77-4510-b38c-9aa11013dedf.png">

**_GTK Wave_**

<img width="750" alt="1 gtk" src="https://user-images.githubusercontent.com/93824690/166289014-d5cc1b41-f990-48d8-99b0-acf9813e3f45.png">

**_Synthesis Statistics_**

<img width="400" alt="1 st" src="https://user-images.githubusercontent.com/93824690/166289026-0906729e-2892-436e-b297-b176b97c0fc2.png">

**_Realization of Logic - rca_**

<img width="850" alt="1 re" src="https://user-images.githubusercontent.com/93824690/166289044-113508a0-3f96-4aca-9edf-3f492b0f265d.png">

**_Realization of Logic- fa_**

<img width="850" alt="1 re2" src="https://user-images.githubusercontent.com/93824690/166289066-d06cbd35-015e-4dfb-baf9-138399256918.png">

**_GLS Output_**

<img width="750" alt="1 gls" src="https://user-images.githubusercontent.com/93824690/166289126-d0980b69-c9d8-4e66-8011-631610d98480.png">

>_The observed waveform in simulation and synthesis matches and conforms code functionality._
## ACKNOWLEDGEMENTS

  * Kunal Ghosh, Co-Founder (VLSI SYSTEM DESIGN - VSD)
  
## References
   * https://www.vlsisystemdesign.com/rtl-design-using-verilog-with-sky130-technology/?q=%2Frtl-design-using-verilog-with-sky130-technology%2F&v=a98eef2a3105
   * https://github.com/kunalg123/vsdflow
   * https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop
   * https://github.com/google/skywater-pdk
