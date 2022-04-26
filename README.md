# RTL-Design-and-Synthesis-Workshop-using-Skywater-130-PDKs

## Day 1: Introduction to Verilog RTL Design and Synthesis
### Introduction to Opensource simulator iVerilog
RTL design is actually the implementation of a given specifications using a Hardware description language such as Verilog. The RTL design is checked for the adherence to the specification using a simulator by simulating the design. Tool used for simulating the design is iVerilog. The RTL design could be a single module or it could be separated to multiple modules/files. For testing the functionality of the Design a stimulas is also written using verilog/systemverilog. The simulator looks for the changes on the input of the design module and based on these changes it changes the output of the design. If no changes happen on the input no change occur on the outputs. An RTL design has a set primary inputs and a set of primary outputs. A test bench is used for testing the functionality of the RTL design which provides the a set of stimulus on the inputs of design and wathces the outputs stimulus. A simple flow is shown in below figure. 

![pic1](https://user-images.githubusercontent.com/43933912/165361584-e9b549e9-6bd1-4d65-a6e9-7540a9a0ee6f.PNG)

The outputs of the design can be viewed using waveform. For functional verification of the RTL design both the RTL design and test bench are provided to the simulator which is in our case is iVerilog. This simulator dumps a VCD file ( value change dump ) which is later on visualized using an other tool GTKwave. The tool gives the waveform from which one can analyze the output signals with respect to the input signals. With the help of this waveform one can verify the functionality of the RTL design. A general flow for functional verification of RTL design using iVerilog+GTKwave is shown in below figure.

![pic2](https://user-images.githubusercontent.com/43933912/165363252-f4676839-03d8-41d6-9737-4a012a51b5f3.PNG)


If any bug occurs then by applying changes to the RTL design it is taken again from the above flow and verified using wavforms.


### Labs using iVerilog and Gtkwave
### Introduction to Yosys and Logic Synthesis
### Labs Using Yosys and Sky130 PDKs
