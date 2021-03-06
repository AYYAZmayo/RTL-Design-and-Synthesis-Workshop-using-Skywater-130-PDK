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
Logic synthesis is a process in which RTL design based on an HDL such as verilog, system verilog or VHDL is mapped into standard logic cell based on particular technology library. Here belwo figure is a depiction of the synthesis process.

![pic9_synthesis](https://user-images.githubusercontent.com/43933912/165377708-8da2aafe-dd42-4ba1-8028-cb841697d869.PNG)

A RTL design written in verilog is converted to respective gate level logic based on different constructs. For example, the top moduel's inputs and output are resulted into the ports of the design. The assign statement which is in below figure is implemented using a ternary operator is converted to a MUX. Similarly, the always block having clock in it's sentivity list is converted a register as shown in below figure.

![synthesis](https://user-images.githubusercontent.com/43933912/165377765-e924d694-5437-4b18-a508-41e737da81f3.PNG)


Yosys is an RTL synthesizer. A synthesizer is a tool which converts RTL into gate level netlist. So, Yosys takes the verilog based RTL design, standard cell library as input and generate a netlist.

![pic7_yosys](https://user-images.githubusercontent.com/43933912/165377815-78ffcbe3-fc2e-495f-ae21-a1c29a4027c2.PNG)

On the other hand iVerilog is a simulator which takes the RTL code for a design or a netlist, and a testbench and dumps a VCD file which is reading by GTKwave. GTKwave generates the resulting waveform of the design.

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

![1651086037282](https://user-images.githubusercontent.com/43933912/165815722-ab06d4a1-391c-4910-b9d5-312f6cbbd018.jpg)


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

Inputs A,B and C are applied to the circuit according to the waveform shown in below figure. At t=0ns ,initially A=0, B=0 and C=1. So, output is Y=1 at start. <br>

![combinational circuit](https://user-images.githubusercontent.com/43933912/165820794-47cca898-5f69-4980-874b-e37074d11011.PNG)

At t=1ns, both A and B are set to 1 and C is set to 0. As the propagation delay for AND gate is 2ns so, it's output repesented by wire "i" will get high after 2ns that is at t=3 as shown in below figure. Before this "i" is zero so the net result at the OR gate which is "(i | C)=Y" would make Y=0 at t=2ns as the propagation delay of OR gate is 1ns. At t=3ns the updated value of "i" appears and as result Y=1 again at t=4ns. According to the applied Inputs, Y should remain stable at 1 as can be seen in the below table:

| A | B  | C  | Y |
| :---:  | :-:| :-: | :-: |
| 0 | 0  | 1  | 1 |
| 1 | 1  | 0  | 1 |

But due to the propagation delays we can see in the waveform does not remain 1 thorughout rather it Y glitches. So, in combinationa circuits output glitch is an isssue. <br>
If we have series of combination circuits in which combinational circuit's output is fed to next combinational circuit as input and so on so forth, as shown in below figure. Then the not only the output of each combinational block would be glitching but also the whole circuit's output would contineously be glitch prone.<br> 
![1651170751622](https://user-images.githubusercontent.com/43933912/165824708-2bfa3d96-7111-4b3f-b259-ed8431b4b097.jpg)

To restrict the glitches flip flop(FF) is used. As the flop output only change at the clock edge otherwise it remains stable. SO even if the D input of flop is glitching , the output Q of the flop will be stable, which it feeds to the next combination block. So, the next combinational block see it as a stable input as result it outputs will also setle down rather than to be glitching. <br>
![1651171292504](https://user-images.githubusercontent.com/43933912/165825977-a8c7378b-fcf5-41e7-bc1f-177698ca87e7.jpg)

#### Flip Flop Coding Styles
Flip flop as we know are edge triggered sequential element that is it works at the positive edge of the clock. One of the most comonly used flop is the D flip flop whose output Q flow the input D but only on the edge of the clock. A simple schematic of D-flip flop is shown below.<br>
![image](https://user-images.githubusercontent.com/43933912/165985778-b5f1278e-d6e8-4e76-9ad5-d34df6275acf.png)

The D-FF can be coded in different styles in Verilog. Here, we are mentioning below 4 type of D-flip flops.
#### Asynchronous Reset D-Flip Flop
An asynchronous reset flip flop is actually a flip whose output Q can be reset to zero irrespective of the clock edge. It means that whenever a high reset signal appears on the input of D-FF, its output Q goes low otherwise it follows the input D on every clock edge. The verilog code for asynchronous reset flipflop is shown below:<br>
![async_rst](https://user-images.githubusercontent.com/43933912/165992693-ae6b6ec0-6cff-4a2e-bc98-9bab46dd2807.PNG)
![async_reset_dia](https://user-images.githubusercontent.com/43933912/165992727-2eb53b73-ea1c-489c-be23-35d06fa37659.PNG)

This flop is simulated on iverilog using a testbench, the output waveform is shown below.<br>
![async_flop_wave_1](https://user-images.githubusercontent.com/43933912/165995356-4ccec8af-d8c9-4f47-a523-54f330390a0d.PNG)

#### Asynchronous set D-Flip Flop
An asynchronous set flip flop is actually a flip whose output Q can be set to 1 irrespective of the clock edge. It means that whenever a high 'set' signal appears on the input of D-FF, its output Q goes high otherwise it follows the input D on every clock edge. The verilog code for asynchronous set flipflop is shown below:<br>
![verilog_async_set](https://user-images.githubusercontent.com/43933912/165996066-d580a4f8-d188-4bf2-83c7-b77c5cd3c613.PNG)
![async_set_dia](https://user-images.githubusercontent.com/43933912/165996098-4b04174c-6488-468c-9bdf-e97013300c7f.PNG)

This flop is simulated on iverilog using a testbench, the output waveform is shown below.<br>
![async_set_wave](https://user-images.githubusercontent.com/43933912/165998045-ca9b92f3-2957-4e0d-b3fe-09c82f1916ad.PNG)
#### Synchronous reset D-Flip Flop
Synchronous reset flip flop is actually a flip whose output Q can be set to 0  only with respective of the clock edge. It means that whenever a high 'reset' signal appears on the input of D-FF, its output Q goes low but only at the positive edge oc clock. Otherwise it follows the input D on every clock edge. The verilog code for synchronous reset flipflop is shown below:<br>
![sync_reset_verilog](https://user-images.githubusercontent.com/43933912/165998764-5f81c28b-5215-494f-b4cb-fcbcae35d8a8.PNG)
![sync_reset dia](https://user-images.githubusercontent.com/43933912/165998932-6be030c2-5803-4335-b51c-320669dd3380.PNG)

This flop is simulated on iverilog using a testbench, the output waveform is shown below.<br>
![sync_reset_dia](https://user-images.githubusercontent.com/43933912/166000921-37da03fc-89e9-44e7-9cff-9687ddb42c89.PNG)

#### Asynchronous-Synchronous reset D-Flip Flop
Such flip flop has both the capability to behave as asynchronous reset as well as synchronous reset. But asynchronous reset has higher periority over synchronous reset. The verilog code for this flipflop is shown below:<br>
![async_sync_code](https://user-images.githubusercontent.com/43933912/166007851-967ffb36-4fa8-465f-89a8-f5f39cb8429b.PNG)
![async_sync_reset_dia](https://user-images.githubusercontent.com/43933912/166007911-e8e1a792-ea1d-404c-8298-10a2ce717a6a.PNG)

This flop is simulated on iverilog using a testbench, the output waveform is shown below.<br>
![async](https://user-images.githubusercontent.com/43933912/166008004-91e64ceb-8f15-46d9-a1a5-8f98fb2b3817.PNG)

## Day 3: Combinational and Sequential Optimizations
### Introduction to Logic Optimization
Logic optimization is actuallly squeezing the logic in order to get the most optimized design which is efficient in terms of area, power and performance. In otherwords it has optimum PPA. There are different techniques which are used for logic optimization both for combinational logic and sequential logic. For combinational logic there are two techniques that is:
1. Constant propagation 
2. boolean logic optimization

In case of constant propagation technique the logic is optimized based on the signal that is contantly propagating either 0 or 1. For example, let's suppose we have a circuit based on Y= ((AB)+ C)`. The circuit diagraam for this expression is shown in belwo figure. Here you can see that if the signal A is constantly propagated as 0 then this circuit optimzed to an inverter just.<br>
![1651319116220](https://user-images.githubusercontent.com/43933912/166110870-59f95752-29ce-44c6-a171-93ee15fa85a7.jpg)

This can also observed in terms of CMOS logic. This original expression is modeled using 6 CMOS transisters, whereas incase of constant propagation of signal 'A' reduces the CMOS logic to only 2 CMOS transister that is reduces the area.<br>

![1651319133924](https://user-images.githubusercontent.com/43933912/166111089-094fa96c-2c93-46d9-a8da-13009ba59e9a.jpg)

Incase of second technique that is boolean logic optimization, the circuit is optimized based on the K-map techinque. Here the boolean expression is reduces to minimal number of literals. To understand this let's suppose we have an expression as follows:<br>

                                                   assign  Y = a?(b?c:(c?a:0):(!c) 

This expression is actully implemented interms of MUXs as shown in below figure. Based on the boolean logic this expression is optmized and reduces to an XOr gate when we write it using the boolean equqtions of a MUX.

![1651319165095](https://user-images.githubusercontent.com/43933912/166111497-45d06b1d-dee2-469f-8a07-41009df2ed3d.jpg)

Incase of sequational optimization there are two techniques one is basic and others are advanced.<br>
1. Basic
  * sequential constant propagation
2. Advanced
  * State optimization
  * Retiminng
  * Sequential logic cloning/Floor plan aware synthesis   

## Day 4: GLS, Blocking vs Non Blocking and Synthesis Mismatch
Gate level simulation (GLS) is actually runing the testbench with the netlist as design under test (DUT). Netlist is logically same as RTL code so same testbench will allign with netlist as well. The question is why GLS is used for? So the answer to this question is that GLS is used for:<br>
1. Verify the logical correctness of design after synthesis
2. Ensuring the timing of the design is met. For this GLS need to be run with delay annotation.
### GLS using iVerilog
iVerilog just like used for simulating the RTL design can also be used for netlist siumulation. The workflow is quite similar. Here one more this have to included that is verilog models for the standard cells as netlist contains instances of different standard cells. Following figure shows the work flow of the GLS using iVerilog.<br>

![gls_iverilog](https://user-images.githubusercontent.com/43933912/166114503-af093854-a397-4045-90a5-ca996d26e130.PNG)
### Synthesis Simulation Mismatch
The synthesis simulation mismatch could be occure due following reasons;<br>
* Missing senstivity list
* Blocking Vs Non blocking assignments
* Non standard verilog coding
#### Missing senstivity list
In real cases the simulator works on the activity which means that it evaluates the output whenever there is a change in the input or the inputs included in the senstivity list. On the otherhand synthesizer does not look into the senstivity list it only looks into the logic. T o understand this let's suppose we have implemented the a MUX using two different methods as shown in below figure as bad_mux.v and good_mux.v.

![sentivity](https://user-images.githubusercontent.com/43933912/166115684-4bff13c5-2089-41d4-98e5-5eeedd702a04.PNG)

In case of bad_mux we can see that in the sentivity list of the always block there is only "sel", that means the output willl only be evaluated based on it it is not sentive to the inputs "i0" and "i1" which is not a good thing as the synthesizer mapped this a latch. Whereas incase of good_mux.v the senstivity list contains (*), which any input changes the out will be evaluated(*). This results in a MUX in a synthesizer. <br>
#### Blocking Vs Non blocking assignments
The blocking and non blocking assignments are used inside an always block in verilog code. The blocking assignment (=) executes the statements in the order it is written . So, the first statement is evaluated before the second statement just like a C code.

Whereas the non blocking assignment (<=) executes all the RHS first whenever the always block is entered and assigns to LHS. It means it excutes in parallel. 
#### Example on GLS
Let's we a verilog code for a 2x1 MUX using ternary operator as shown in below figure.<br>
![e1](https://user-images.githubusercontent.com/43933912/166117197-2fa2a13c-8930-4e82-80ca-af16df6861a8.PNG)

 And simulated it using iVerilog with testbench. It clearly behaves as 2x1 MUX.
 
 ![e1 1](https://user-images.githubusercontent.com/43933912/166117429-8877e8f9-2f8d-4e63-b493-d57ec4c0d0ee.PNG)

Now generated the netlist using Yosys for 2x1 MUX. The Yosys statistics are shown below.

![e1 2](https://user-images.githubusercontent.com/43933912/166117592-11eec407-8706-466b-bfad-72ccc14d27fc.PNG)

Now generated the schematic for the synthesized design using Yosys, which clearly a 2x1 MUX cell.  

![e1 4](https://user-images.githubusercontent.com/43933912/166117813-06b3bfad-0e1c-4718-b4a7-55986fc832bd.PNG)

Write the verilog netlist using Yosys

![e1 3](https://user-images.githubusercontent.com/43933912/166117746-fed9b3d9-f8d8-40e0-bdeb-62d0cea03233.PNG)
Now we perform GLS using iVerilog for this we need primitives.v and verilog models for standard cells along with netlist and testbench.<br>

![e1 5](https://user-images.githubusercontent.com/43933912/166118142-3c7c0e13-2f76-4b33-9898-98ceed832434.PNG)

On observing the waveform on gtkwave it can clearly concluded that it is behaving like a 2x1 MUX. So, GLS is functionaly verified.

![e1 6](https://user-images.githubusercontent.com/43933912/166118247-45380755-a6aa-4442-8da2-4ec8561df869.PNG)

#### Example on Synthesis Simulation Mismatch

Let's have another code for 2x1 MUX but it is coded diffrently this time. It ic coded using always block having 'SEL' in it's senstivity list. <br>

![e2 1](https://user-images.githubusercontent.com/43933912/166118568-111f82b1-7ae5-443e-bb3f-43d91b01b13d.PNG)

Now simulated it using test bench in iVerilog, the resulting waveform is generated using GTKwave is also shown below. It can be seen in the waveform that it is not like a 2x1 MUX output rather it is behaving like a register. 

![e2 2](https://user-images.githubusercontent.com/43933912/166118908-0c7d9051-6bce-4b5f-bc24-e7579e0714fe.PNG)
 
 Now we generate the netlist using Yosys. It can be seen in the Yosys statistics that Yosys infering a MUX from the bad_mux.v RTL. 

![e2 3](https://user-images.githubusercontent.com/43933912/166119080-3d8b7be2-0319-4089-b51b-af1773e310c3.PNG)

Wrote the verilog netlist using Yosys 
![e2 4](https://user-images.githubusercontent.com/43933912/166119176-cf0f40b3-a7b7-4df5-aa3a-9160be84832a.PNG)

![e2 5](https://user-images.githubusercontent.com/43933912/166119203-1bb2767c-273c-43d1-b655-2404e5e21904.PNG)

Now performed the GLS on this bad_mux.v netlist using iVerilog. Here it can clearly be seen that it is behaving like 2x1 MUX totally opposite to the RTL simulation. So, here we have a clear observation of synthesis and simulation mismatch.<br>
![e2 6](https://user-images.githubusercontent.com/43933912/166119349-5d27916c-32f4-4156-8478-c0127c6182e8.PNG)

#### Synthesis Simulation Mismatch for Blocking statements

Let's have simple logic using blocking statements coded in verilog as shown in below figure.<br>

![3 1](https://user-images.githubusercontent.com/43933912/166120086-81510d72-9ef9-438e-9732-1738fd26ab8a.PNG)

According to code it is seemed to be that it performing OR on 'a' and 'b' and assigning it to 'x' which is AND with 'c' and assigned to 'd'.

![e3 2](https://user-images.githubusercontent.com/43933912/166120292-75c6599e-5457-42e2-aa89-90801a19a746.PNG)

Now we first simulate it using iVerilog. The simulation waveform is shown below. This behaving as latch is formed which is storing the previous value and the d is evalusted based on these values.

 ![e3 3](https://user-images.githubusercontent.com/43933912/166120629-481cf158-3117-4aa4-928e-b360891f355b.PNG)

Now perform GLS by simulating the GLS. For this first we generate the netlist using Yosys. It can be seen in the netlist viewer of Yosys that it is synthesized into or_and gate.

![e3 4](https://user-images.githubusercontent.com/43933912/166120789-556d68a5-7b76-437f-896d-52df3032c50a.PNG)

The GLS simulation is performed using iVerilog as follows. Here it can be seen that there is no latch like behaviour rathe the value of 'd' is correctly evaluated at the current values of 'a' and 'b'. So, from this synthesis simulation mismatch one should be very carefull while using the blocking statements.

![e3 5](https://user-images.githubusercontent.com/43933912/166120936-5d0bce22-b9e8-45f8-896a-1381be94d9d1.PNG)

## Day 5: IF, Case, For Loop and For Generate
### IF constructs
**If** construct is used in verilog for periorty logic implementation. A sample code for IF construct is shown below. Here if the condition 1 <cond1> is true than 'c1' part of code will be excuted, else if the condition 2 <cond2> gets true than 'c2' part of code will run, else if condition 3 <cond3> gets true than 'c3' part of code will run if none of the above conditions are true than the 'else' part of code which 'c4' will be executed.
 
![if_code1](https://user-images.githubusercontent.com/43933912/166132806-a3cd1bbf-2e46-42f6-b0ac-38fb9c01817b.PNG)
 
 In hardware this **If** construct is evaluated as MUXs as shon in the belwo figure. If <cond1> is true than 'c1' will be at the output, else if <cond2> gets true than 'c2' will be available at the output, else if <cond3> gets true than 'c3' will be available at the output else the 'c4' will reach to the output.
 
 ![if_hardware2](https://user-images.githubusercontent.com/43933912/166132922-04a7805e-03b8-4157-a3a0-abdad9f604f7.PNG)
 #### IF constructs  danger/caution
 It infered latches incase of bad coding practices or missing else construct in the code. e.g we have bad coding practise of missing the final else in teh code as shown in belwo figure.<br>
 
 ![c3](https://user-images.githubusercontent.com/43933912/166133268-140cc2e8-3320-4ffe-81ec-84dc5201be4b.PNG)

This in hardaware will infered a latch that have OR gate at the enable having inputs <cond1> and <cond2>. When ever both of this condition are false than this latch enables and store the value of Y and provides it next time. So, latch is not intentional rather it is created because of bad coding of IF construct.
 
 ![c4](https://user-images.githubusercontent.com/43933912/166133363-db1bd170-5340-4118-baa0-c25bd1ad2225.PNG)

 There are some cases where latch inferening is intentional and needed for the proper operation of circuit. For example let's suppose we write a 3 bit counter as follows, in the always block we have reset at which the counter reset to 0 and an enable 'en' at which it starts incrementing otherwise it stays at the same number.

 ![c5](https://user-images.githubusercontent.com/43933912/166133972-6f9a6dec-9961-4901-ab9c-1b719aa1b1db.PNG)

As can be seen in belwo figure for the missing else for 'en' the counter latch to previous value if 'en' is zero. So, here latch is intetional.<br>

 ![c6](https://user-images.githubusercontent.com/43933912/166134344-c57f22e7-177b-4179-a69f-4065dd869967.PNG)

**In combinational circuits latchtes are not allowed.** <br>
 
 ### Case Statement
Just like IF construct 'Case' is also used inside always block and whatever you want to assign must be a register variable. Here, we have a simple 'case' statement code. It is evaluated for all the possible combinations of 'sel'. As can be seen that for every possible combination 'Y' assigned to a value. In terms of hardware this code implements a MUX. <br>
![c7](https://user-images.githubusercontent.com/43933912/166135131-8a792e45-519b-4d0e-9a0b-7b42db508251.PNG)
 ### Caveats with Case
 #### 1. Incompelete case statement infers latches
 If we have not assigned the output Y for every possible commbination of case then it will infer a latch for missing case statements.
 
![c8](https://user-images.githubusercontent.com/43933912/166136545-1b65f484-7b4d-44b2-a1ed-4223d0872907.PNG)
 
 To avoid latch infering fro missing case statements we case use default case statment.e.g
![c9](https://user-images.githubusercontent.com/43933912/166135754-19159414-3f04-4b1b-8d01-1da8b6548ad8.PNG)

#### 2. Partial assignments in case
If we have missing or partial assignments in case statements it will also infer latch for those missing asignments. e.g. <br>

![c10](https://user-images.githubusercontent.com/43933912/166136261-e231c7d2-9210-4f88-a0fe-bc2723b04807.PNG)

 #### 3. Overlapping Case statments
 
In case statment all the posiible combinations of the variable are evaluated which is present in the case statement. If somehow we have used overlaping combination in the case statement then it results in unpredictable output e.g.

![c11](https://user-images.githubusercontent.com/43933912/166136552-27a21530-5066-4e34-acec-4c32be67b70d.PNG)
### Synthesis and Simulation of IF construct
As we have mentioned previously that an incomplete IF result in a Latch. Now we code an incomplete IF and perform its simulation and synthesis to check whether it infers a latch or not. To do this we have following verilog code:

 ![c12](https://user-images.githubusercontent.com/43933912/166137116-267c64e4-93d2-467a-bcdd-798c9a4698d8.PNG)

Accoring to the code its hardware would be a D latch as shown in below figure.
 
 ![c13](https://user-images.githubusercontent.com/43933912/166137346-f1c0ddb5-1e18-44a1-86fd-d8f47318067c.PNG)

Now we simulate it on iVerilog using a testbench. According to the waveform it can be seen it is infering a latch.
 
 ![c14](https://user-images.githubusercontent.com/43933912/166137574-37deb707-8fea-4790-bb9b-dd120c238545.PNG)

Now we perform its synthesis to check whether in synthesis it also refering a Latch. The Yosys statistics are shown below.
 
 ![c15](https://user-images.githubusercontent.com/43933912/166137686-56e876d7-2483-4376-847a-fc57946a662c.PNG)

The synthesis schematic generated from yosys it can also be seen that it is inferring a Latch.
 
![c16](https://user-images.githubusercontent.com/43933912/166137749-75b58240-aa84-44dd-a9a1-113dd4a60e2d.PNG)
 
 #### Another example of incomplete IF construct
Now we code an other incomplete IF and perform its simulation and synthesis to check whether it infers a latch or not. To do this we have following verilog code:
 
 ![c17](https://user-images.githubusercontent.com/43933912/166139255-064f2450-1b88-4cf1-8bbe-1156c8f818cc.PNG)
Accoring to the code its hardware would infer a latch as shown in below figure.
 
 ![c18](https://user-images.githubusercontent.com/43933912/166139441-5e34c450-130c-40d2-8ff6-b042449741f7.PNG)
 
Now we simulate it on iVerilog using a testbench. According to the waveform it can be seen it is infering a latch.
 
 ![c19](https://user-images.githubusercontent.com/43933912/166139968-8571e83a-1919-43af-a79c-5e6f8341914c.PNG)


Now we perform its synthesis to check whether in synthesis it also refering a Latch. The Yosys statistics are shown below.
 
![c20](https://user-images.githubusercontent.com/43933912/166140054-544fa4be-673e-4ed4-b19c-9f42027f70de.PNG)

The synthesis schematic generated from yosys it can also be seen that it is inferring a Latch.
 
 ![c21](https://user-images.githubusercontent.com/43933912/166140096-10e88b0e-7c02-447f-8018-258bad23fa51.PNG)

### Synthesis and Simulation of Incomplete Case statement 
As we have mentioned previously that an incomplete case statement result in a Latch. Now we code an incomplete case and perform its simulation and synthesis to check whether it infers a latch or not. To do this we have following verilog code:

![c21](https://user-images.githubusercontent.com/43933912/166140365-6b5b658b-07b2-434a-954a-2acfaf718817.PNG)

Accoring to the code its hardware would be a D latch as shown in below figure.
 
![c23](https://user-images.githubusercontent.com/43933912/166140753-df08e792-9c66-47ee-b3a1-f726f6115d9d.PNG)

Now we simulate it on iVerilog using a testbench. According to the waveform it can be seen it is infering a latch.

 ![c24](https://user-images.githubusercontent.com/43933912/166140813-9054314a-f610-47be-b56a-79af3c181acb.PNG)

Now we perform its synthesis to check whether in synthesis it also refering a Latch. The Yosys statistics are shown below.
 
 ![c25](https://user-images.githubusercontent.com/43933912/166140880-b47d42ee-5ed5-4826-824e-ff2e40ae6def.PNG)

The synthesis schematic is generated from yosys. It can also be seen that it is inferring a Latch.

 ![c26](https://user-images.githubusercontent.com/43933912/166140945-cb69e29e-38d3-4722-b6d1-ee40dfaf6e2b.PNG)
#### Overlapping Case statments sytnhesis and simulation
As we have mentioned previously that an overlaping case statement result in unpredictable output. Now we code a overlaping case statement and perform its simulation and synthesis to check whether it infers a latch or not. To do this we have following verilog code:
 
 ![image](https://user-images.githubusercontent.com/43933912/166141512-d4ed7d0a-0915-4105-bfac-3a6999e7cd6b.png)
 
Now we simulate it on iVerilog using a testbench. According to the waveform it can be seen the output is unpredictable in the highlighted box.

 ![c27](https://user-images.githubusercontent.com/43933912/166141807-2d5e388b-dfb7-48a0-8158-a44b31cb678c.PNG)
 
Now we perform its synthesis in Yosys

 ![c28](https://user-images.githubusercontent.com/43933912/166141886-65f98a7d-88df-447d-bc31-47e3c7579ce0.PNG)

The synthesis schematic is generated from yosys.
 
 ![c29](https://user-images.githubusercontent.com/43933912/166141920-83031842-6756-4f27-bb53-f2d80ef7e1a8.PNG)

 Now we generate the netlsit for performing the GLS
 
 ![c30](https://user-images.githubusercontent.com/43933912/166142107-04f65257-fae1-496e-9bf2-4d6170635ed4.PNG)

 The generated verilog netlist is shown below.
 
![c31](https://user-images.githubusercontent.com/43933912/166142448-ef2baec5-8498-4507-a4c6-5e4ef76daa06.PNG)

 Upon performing the GLS in iVerilog, it can be seen in the waveform that there is no unpredictability in the ouput. rather it follows 'i0' when 'sel=00' , 'i1' when 'sel=01', 'i2' when 'sel=10' and 'i3' when 'sel=11'.
 
![c32](https://user-images.githubusercontent.com/43933912/166142384-f44364c7-312f-4260-bf36-baa961787c18.PNG)

### For Loop and For Generate
In verilog there are two type of looping constructs are used.
1. For Loop 
It is used inside the always block and it used for evaluating the expressions.
2. For generate
It is outside the always block. It mainly used for instantiating the hardware for multipe times.
#### For loop
 To understand the use of foor loop let's take the example of MUX. As we know that it is quite easy to a 2x1 MUX using IF constructs. Similary, a 4x1 MUX can be written using CASE statements. But when we going to write a 32x1 MUX using similar case statments it quite laborious and lenth task as can be seen in the below figure, we have to write all the possible 32 combinations for 32x1. 
 ![c33](https://user-images.githubusercontent.com/43933912/166143892-69a0992a-ec75-4a36-bff3-9507e0b1eb2e.PNG)
 
 To make the task easy we can use a For loop inside the always block for genetating 32x1 MUX or higer MUXs in few lines of code.
 
 ![c34](https://user-images.githubusercontent.com/43933912/166144239-b3889b72-7449-4286-a62c-8e1707f7fe20.PNG)
 
 So, for loop is helpful in creating a very wide MUX or DMUX.
 #### For generate Loop
 This type of loop is used for instantiating the hardware for multiple times. Let's for some reason we have to instantiate an AND in module for 8 times. We can instantiate it using for-generate loop as follows.

 ![c35](https://user-images.githubusercontent.com/43933912/166144670-2bdf56a2-970f-4d0d-9187-66f39dcdc8ee.PNG)
 #### Example of MUX using FOR Loop 
 Let's suppose we generate a MUX using a for loop. The verilog code is shown below. Here we have used 'i_int', which is actually bus combining the {i0,i1,i2,i3} in a single variable.

  ![c36](https://user-images.githubusercontent.com/43933912/166146956-a794f313-86b0-4348-aabb-2f9ffa1bd5ba.PNG)
 
Now we simulate this code using iVerilog and a tesetbench. Clearly it can be sen that it is acurately simulating a 4x1 mux. For sel=00 'y' following the 'i0', for sel=01 'y' is following the 'i1', for sel=10 'y' following 'i2' and for sel=11 'y' is following 'i3'.
 
![c37](https://user-images.githubusercontent.com/43933912/166147161-c2aae9aa-9fa0-4872-b27a-e9c7f0d00d8b.PNG)
 
Now we perform its synthesis in Yosys

![c38](https://user-images.githubusercontent.com/43933912/166147726-69659ded-73f1-4686-af41-885e54c2ef57.PNG)

The synthesis schematic is generated from yosys.
 
![c39](https://user-images.githubusercontent.com/43933912/166147736-1c89a1df-6151-448e-b2d7-bb3898b1c5f9.PNG)

 Now we generate the netlsit for performing the GLS
 
![c40](https://user-images.githubusercontent.com/43933912/166147741-761ec06b-a638-47d7-9102-2d264353a310.PNG)

 The generated verilog netlist is shown below.
 
 ![c41](https://user-images.githubusercontent.com/43933912/166147878-150c30b8-1d7b-4c25-8a8c-afc46c5e3d05.PNG)
 ![c42](https://user-images.githubusercontent.com/43933912/166147887-45d72d63-3f48-4176-8beb-a3a400976159.PNG)

 Upon performing the GLS in iVerilog, it can be seen in the waveform that the ouput follows 'i0' when 'sel=00' , 'i1' when 'sel=01', 'i2' when 'sel=10' and 'i3' when 'sel=11'. So, there is no simulation synthesis mismatch here.
 
 ![c43](https://user-images.githubusercontent.com/43933912/166148041-4007d303-4c55-4744-b54b-fbbb2a5ff518.PNG)
 #### Example of DMUX using FOR Loop 
 Let's suppose we generate a DMUX using a for loop. The verilog code is shown below. 
 
 ![c44](https://user-images.githubusercontent.com/43933912/166148279-c072d4a9-6f6e-4c9d-a6df-46a13730554c.PNG)
 
Now we simulated this code using iVerilog and a tesetbench. Clearly it can be sen that it is acurately simulating a 1x8 Dmux. For sel=000 'o0' following the 'i', for sel=001 'o1' is following the 'i', for sel=010 'o2' following 'i' and for sel=011 'o3' is following 'i' so on and so forth.
 
![c45](https://user-images.githubusercontent.com/43933912/166148444-96e77c2e-6451-49f2-82f0-183893f6e167.PNG)
 
Now we perform its synthesis in Yosys

 ![c46](https://user-images.githubusercontent.com/43933912/166150116-043c41d1-5d48-4d84-a4f4-3273958251df.PNG)

The synthesis schematic is generated from yosys.
 ![c47](https://user-images.githubusercontent.com/43933912/166150126-4a1e7973-25e6-48ce-af21-11e947104a23.PNG)

Now we generate the netlsit for performing the GLS
 
![c48](https://user-images.githubusercontent.com/43933912/166150140-9153446f-812d-4297-9834-50de2b5f8435.PNG)
 
 Upon performing the GLS in iVerilog, it can be seen in the waveform that the ouput follows 'i'.For sel=000 'o0' following the 'i', for sel=001 'o1' is following the 'i', for sel=010 'o2' following 'i' and for sel=011 'o3' is following 'i' so on.
 
![c49](https://user-images.githubusercontent.com/43933912/166150819-0dba0389-671e-488d-9715-d88b07ea7600.PNG)

  #### Example of For generate using Ripple Carry Adder
A simple adder for adding binary number is a ripple carry adder. It's buliding block is full adder. For example a 4 bit ripple carry adder is formed by connecting back to back four full adders. So, for writing the a 4 bit ripplr carry adder we need to instantiate 4 full aders in our code.

 ![c50](https://user-images.githubusercontent.com/43933912/166152234-8ac6d0bf-043a-4fae-86f2-f1cca6a59e15.PNG)

 If we have to make 8 bit ripple carry adder than we have to instantiate 8 full adders in our code, Or in case 32 bit ripple carry adder we need 32 instantiation of same basic building blokc of full adder. This can be done efficiently using for-generate loop. Let;s take the example of a 8 bit ripple carry adder which coded using for-generate loop as shown below figure.
 
 ![c51](https://user-images.githubusercontent.com/43933912/166152594-07c3f98e-e2df-4682-a983-f9c4bfe30916.PNG)
 
 Now we perform its simulatio using iverilog and a tesbench. The resulting waveform is shown in belwo figure.

 ![c52](https://user-images.githubusercontent.com/43933912/166152751-6a421eba-1793-4936-a8e4-ae6beac37b34.PNG)

Now we perform its synthesis in Yosys

![c53](https://user-images.githubusercontent.com/43933912/166152947-4fe0527a-3239-40ea-b494-4c38e817f128.PNG)

The synthesis schematic is generated from yosys.
 
 ![c54](https://user-images.githubusercontent.com/43933912/166153101-ccb367c0-f251-4643-80f4-b3cabb6ba7a9.PNG)

Now performed the GLS of the rippple carry adder.

![c55](https://user-images.githubusercontent.com/43933912/166153342-4526583d-f690-440c-93c8-41aaeccea0ed.PNG)

Here the 5 day work concluded.





 

 
 
 
 
 
 
 
 
 


