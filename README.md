# RTL-Design-and-Synthesis-Workshop-using-Skywater-130-PDKs

## Day 1: Introduction to Verilog RTL Design and Synthesis
### Introduction to Opensource simulator iVerilog
RTL design is actually the implementation of a given specifications using a Hardware description language such as Verilog. The RTL design is checked for the adherence to the specification using a simulator by simulating the design. Tool used for simulating the design is iVerilog. The RTL design could be a single module or it could be separated to multiple modules/files. For testing the functionality of the Design a stimulas is also written using verilog/systemverilog. The simulator looks for the changes on the input of the design module and based on these changes it changes the output of the design. If no changes happen on the input no change occur on the outputs. An RTL design has a set primary inputs and a set of primary outputs. A test bench is used for testing the functionality of the RTL design which provides the a set of stimulus on the inputs of design and wathces the outputs stimulus. A simple flow is shown in below figure. 

![pic1](https://user-images.githubusercontent.com/43933912/165361584-e9b549e9-6bd1-4d65-a6e9-7540a9a0ee6f.PNG)

The outputs of the design can be viewed using waveform. For functional verification of the RTL design both the RTL design and test bench are provided to the simulator which is in our case is iVerilog. This simulator dumps a VCD file ( value change dump ) which is later on visualized using an other tool GTKwave. The tool gives the waveform from which one can analyze the output signals with respect to the input signals. With the help of this waveform one can verify the functionality of the RTL design. A general flow for functional verification of RTL design using iVerilog+GTKwave is shown in below figure.

![pic2](https://user-images.githubusercontent.com/43933912/165363252-f4676839-03d8-41d6-9737-4a012a51b5f3.PNG)


If any bug occurs then by applying changes to the RTL design it is taken again from the above flow and verified using wavforms.


### Labs using iVerilog and Gtkwave

A 2x1 Mux is designed using Verilog as shown in below figure.

![pic3_mux](https://user-images.githubusercontent.com/43933912/165370236-c6b70711-d4e3-4865-9af8-6fe9b5a796a4.PNG)

A testbench is also written using verilog in which this 2x1 Mux having module name as good_mux is instantiated as uut. The uut ports are mapped with the design.

![pic4mux](https://user-images.githubusercontent.com/43933912/165370699-5235f8e4-7a8b-46a4-8046-4605df582490.PNG)

Following commands are used for running the good_mux.v and its test bench tb_good_mux.v 
. iverilog good_mux.v tb_good_mux.v 
. ./a.out
.gtkwave tb_good_mux.vcd

![pic6_terminal](https://user-images.githubusercontent.com/43933912/165371235-05fa96b4-3b9e-47bf-9048-0e551a4443db.PNG)

gtkwave provide the output wavform as shown below.

![pic5_muxwaveform](https://user-images.githubusercontent.com/43933912/165371357-4ef82838-9da9-4213-8a0c-b2dec2802091.PNG)


### Introduction to Yosys and Logic Synthesis

![pic9_synthesis](https://user-images.githubusercontent.com/43933912/165377708-8da2aafe-dd42-4ba1-8028-cb841697d869.PNG)

![synthesis](https://user-images.githubusercontent.com/43933912/165377765-e924d694-5437-4b18-a508-41e737da81f3.PNG)


Yosys is RTL synthesizer. A synthesizer is a tool which converts RTL into gate level netlist.

![pic7_yosys](https://user-images.githubusercontent.com/43933912/165377815-78ffcbe3-fc2e-495f-ae21-a1c29a4027c2.PNG)

![pic8_verify_netflow](https://user-images.githubusercontent.com/43933912/165377851-3dd62759-6010-4940-b8e0-6a70735baa9f.PNG)

### Labs Using Yosys and Sky130 PDKs
First of all run the pre installed Yosys tool by giving the command yosys  in the terminal.

![yosys_1](https://user-images.githubusercontent.com/43933912/165382689-06f22358-9ed7-48b9-8cbb-b2b7fd026b81.PNG)

Read the sky130 .lib technolgy library file.

![read_lib](https://user-images.githubusercontent.com/43933912/165382818-da2abc0a-95ec-47ad-aede-b868c329e9cc.PNG)

Read the RTL design file "good_mux.v".

![read_verilog](https://user-images.githubusercontent.com/43933912/165382935-b7f544db-20fe-4417-a8ab-855675f25aef.PNG)

elaborate the design for synthesis by specifying the top module name in the comand "synth -top good_mux".

![synth_top](https://user-images.githubusercontent.com/43933912/165383259-f135e5da-4039-47bd-a177-c92ce2200191.PNG)

A general statstics is shown here based on library cells used.

![statistics](https://user-images.githubusercontent.com/43933912/165383727-ebd9b4ec-9dac-43cb-bf6b-b8e00fad2736.PNG)

synthesized the design using "abc -liberity sky130.lib" command which mapped the design on library cells and perform optimizations.

![abc_](https://user-images.githubusercontent.com/43933912/165383847-7a9dcf3b-e4ce-48ca-bcd6-188e4b671d95.PNG)

This "abc" command also gives the number of input/output signals and cells present in the netlist.

![cell_signals](https://user-images.githubusercontent.com/43933912/165384069-ccfce644-bbe7-4622-a8ea-c85a3b537f2f.PNG)

"show" command is used for graphical visulization of the netlist.

![show_viz](https://user-images.githubusercontent.com/43933912/165384306-a62262e4-8f2c-4923-99a8-3c77e2a87b0a.PNG)

In the graphical schematic a sky130 library cell is used for mux implementation along with some buffers.

![schematic](https://user-images.githubusercontent.com/43933912/165384582-fdaff51f-3fba-4e86-8e50-c5615c06dc51.PNG)

Now at the end the netlist is generated using the below command in Yosys

![write_netlist](https://user-images.githubusercontent.com/43933912/165384874-9cba3a41-102a-44c1-9dae-5ff8aa5ed7d7.PNG)

The netlist for the 2x1 mux which is generated using Yosys and sky130 PDK is shown in the below figure.

![netlist](https://user-images.githubusercontent.com/43933912/165385073-8cbbf7cf-b628-441d-834c-eedd335eafe4.PNG)

## Day 2: Timing Libs, Hierachical vs Flat Synthesis and Efficient Flop coding styles
### Introduction to Timing Libs
In this workshop we are using Skywater130 PDK. This sky130 PDK has different timing libraries based on 130 nm process node. A timing library is actually the collection of standard cells like AND, OR, NOT and flip flop etc. A timing Library provides information about:
* Cells power for each input pin of the cells
* Timing delays for each cell in the form of lookup tables
* Area foot print for the cells <br>

These Timing Libraries are categerized based on PVT corner that is process, voltage and temperature corner e.g the library which we are using is **sky130_fd_sc_hd__tt_25c_1v80.lib**. Here, "tt" represents the process, "25c" represents the temperature of 25 centigrade and "1v80" represents the voltage of 1.80 Volts. PVT directly effects the performance of the cells.
#### Process:
Process represents the variations during the fabrication of chip. It could be due to temperature, pressure, dopant concentation or due to instruments used in the manufacturing of the chip. Due to these process variations transitors may have different chanel lengths throughout the chip, some transistors could have larger length so they behave faster whereas others may have shorter lengths so they work slow. So based on the process timing library could be of: <br>

** Typical, Typical (tt):  typical normal process variation
** Fast ,Fast (ff): fast process variation 
** Slow,Slow (ss): slow process variation
#### Voltage:
Voltage effects the performance of the cells. In a chip the cells which have high volatge (placed closer to the supply) have less delay where as the cells which have low volatge due high IR drop posses more delay.
#### Temperature:
As the chip is to be used in different parts of the world under different different temperatures. So, where the temperature is high the cell delay increases so they behave slow whereas where the temperature is low they behave fast. So timing library are categorized based on operating temperatures. <br>

When we open **sky130_fd_sc_hd__tt_25c_1v80.lib** , on the very first line the name of the library appears. It also contains technolgy which is CMOS. A typical snap shot of top lines is shown below:

![newlib](https://user-images.githubusercontent.com/43933912/165589250-316a9905-20b4-4a66-9ec4-119324f13e64.PNG)


There are many cells in this library out of which and2 cell is shown in below figure. Here you can see that it is of 2 pin AND gate. And for these 2 inputs there are four combinations possible and for each combination leakage power is given here. Along with this area  and total leakage power of the cell are also given as highlighted in yellow in belwo figure.


![andup](https://user-images.githubusercontent.com/43933912/165586879-8f8c0bd3-6a84-4615-9378-66504320f4e5.PNG)

This .lib also conatains multiple versions of the same cell based on the strength e.g above 2 input AND gate has two more versions as shown in below figure. Here "and2_0" , "and2_2" and "and2_4" comparison is shown. It can be seen that as we move from left "and2_0" to right "and2_4" the cell area and leakage power both are increasing that means the speed of the cell is increasing. It means "and2_0" is slowest and "and2_4" is fastest with minimum delay but at the cost of large area and power.

![andcell_compp1](https://user-images.githubusercontent.com/43933912/165588553-7aa5d8bd-6b2a-48c4-ae7d-eecb7be29188.PNG)

### Hierarchical vs Flat Synthesis

#### Hierarchical Synthesis
A hierarchical synthesis is one in which the netlist generated preserves the same hierarchy which is present in the original RTL code. As we know that when we have a large design to code, we code it verilog by firstdesigning the its submodules and then we combine these submodules into a top module. Its a hirachical way of designing adigital design. For simple example let's RTL design named as **multiple_modules** which has three inputs A,B and C and a single ouput Y. This top module has two submodules, one submodule is an AND gate and the second submodule is an OR gate. The hierarchical schematic is shown below:<br>

![1651086037282](https://user-images.githubusercontent.com/43933912/165778660-55c68722-53e1-4908-83ab-68eed318ea5a.jpg)

The RTL code for design is written in verilog as follows:<br>

![hierarchnet](https://user-images.githubusercontent.com/43933912/165779339-e4915e68-40d0-4b45-940f-3c4983b383c0.PNG)

A hierachical synthesis is one which generates such a netlsit which preserves the this hierarchy that one top module contains these submodules inside it. This type of netlist is known as hierarchical netlist. For generating this netlist this designis synthesized using Yosys by following below steps:<br>
* Read the .lib of sky130 in Yosys

![read_lib](https://user-images.githubusercontent.com/43933912/165780708-c533033a-88af-407c-a33a-345d8e3f2328.PNG)

* Read the RTL design **multiple_modules**

![mul1](https://user-images.githubusercontent.com/43933912/165780945-9257b6e3-511a-468d-bbe9-b172cc4f0e2e.PNG)

* synthesize the design by giving the top module name as "multiple_modules"

![hierarchy](https://user-images.githubusercontent.com/43933912/165783892-9559ef73-c2ed-4af7-9ab4-93b0f2540259.PNG)

* Performed technology mapping using "abc -liberty sky130_fd_sc_hd__tt_25c_1v80.lib"
* Now the schematic for the hierachical netlist is obtained using "show" command in yosys which shown in below figure. This conatains submodule1 and submodule2 and muitple_module as top module, that means the hierarchy is same as in the RTL.

![schematic](https://user-images.githubusercontent.com/43933912/165781948-4c9ee1ea-7c24-46ca-9980-7021c35686a0.PNG)

A hierarchical synthesis beneficial in two cases:<br>
1. When we have multiple instances of asame module in a design, so synthesize this single design and replicate it multiple times in the top module this saves time and synthesizer effort.
2. We want to employ divide and conquer approach in case of massive designs.
3. In hierarchcial netlsi the the pins on submodules are accessible so it helps in functional verification as well as in case of static timing analysis of the design.<br>
#### Flat Synthesis
If a synthesizer do not preserves the hierachy of the RTL design rather it generates a flat signle module netlist. Then this type of netlist is known as flatten netlist and such type of synthesis is known as flat synthesis. 
 * A flatten netlist is generated by giving a commmand "flatten" to the Yosys before writing the netlsit command. The flatten netlsit for the above RTL design "multiple_module" is shown below. It can seen that there is no submodules it rather it is gnerated in the for of basic AND and OR gates.<br>
 
 ![flatten_net](https://user-images.githubusercontent.com/43933912/165783265-44303864-5b9c-465f-99df-bf21a1fda5eb.PNG)

* The schematic for flatten netlist is shown below.

![flatten_schem](https://user-images.githubusercontent.com/43933912/165783734-8a3ae0bf-00b8-4bef-bf03-535d060bc0a6.PNG)

### Various Flip Flop Coding Styles and Optimization
#### WHY Flip Flops
In digital circuits a flip flop is used to restrict the glitches that are produced in the combinational circuits due to the propagation delays present in them. When we give input to the combinational circuit its ouput changes after the propagtion delay present in them due to which the ouput glitches. To understand this let's suppose we have a simple combinational circuit consists of an And gate and OR gate. There are three inputs A,B and C and one single ouput Y. The boolean equation for the circuit is **Y= (A & B) | C**. <br>
![1651167737775](https://user-images.githubusercontent.com/43933912/165815436-57ffefad-ab31-4f6b-b5ae-e5c71806fe21.jpg)



