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
