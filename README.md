# VSD Hardware Design Program

## Index
* [Day 0 - Getting the tools](#Day-0---Getting-the-tools)

## Tools required during the project
### yosys
![](Resources/yosys.png)

### Ngspice
ngspice is the open-source spice simulator for electric and electronic circuits.<br /><br />
Install steps:<br />

After downloading the tarball from [https://sourceforge.net/projects/ngspice/files/](https://sourceforge.net/projects/ngspice/files/) to a local directory, unpack it using:
```
 $ tar -zxvf ngspice-38.tar.gz
 $ cd ngspice-38
 $ mkdir release
 $ cd release
 $ ../configure  --with-x --with-readline=yes --disable-debug
 $ make
 $ sudo make install
```
More info can be found at [https://ngspice.sourceforge.io/index.html](https://ngspice.sourceforge.io/index.html)

Please note that to view the simulation graphs of ngspice, xterm is required and can be installed using.
```
$ sudo apt-get update
$ sudo apt-get install xterm
```
### opensta
![](Resources/opensta.png)
### icarus verilog

### gtkwave



## Chapter 1 - RTL DESIGN USING VERILOG WITH SKY130 TECHNOLOGY

The files used in this section can be cloned from: [https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git](https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git)

### 1.1 Simulation Using iverilog and gtkwave

Icarus Verilog or any other simulation program takes a verilog file and its corresponding testbench to produce a .vcd file.<br />
vcd stands for value change dump and as the name suggests vcd files only track the output when there is a change in the input.

![](Resources/1-1.png)

iverilog is run with the following commands to output a.out file. this file can be executed to generate the .vcd file 
```
iverilog <specfile.v> <testbench.v>
```

The vcd file simulation can be seen in gtkwave using
```
gtkwave <file.vcd>
```
![](Resources/1-2.png)

### 1.2 Logic sysnthesis using Yosys

Logic synthesis defines converting the behavorial verilog code to an RTL code described using the vendor specific standard cells.<br />
These standard cells are described in the .lib file. The design file and the lib file are the inputs to the synthesizer tool.

![](Resources/1-3.png)

Yosys is a synthesiszer which produces the required rtl netlist, file which can again be verified using iverilog and gtkwave. <br/>
Since the netlist is true dscription of the design the same testbench file can be used for the netlist. 

![](Resources/1-4.png)

A .lib contains multiple varients of a logic gate as fast and slow cells are required to meet the SETUP and HOLD times respectively.

Faster cells drive more current using transistors to charge and discharge the capacitive load in a circuit.
Fast cells have a low delap but consume more area and power due to wider transistors.

Steps for running Yosys are as follows

1. Load the library 
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
2. Load the RTL design 
```
read_verilog good_mux.v
```
3. Declare the top module 
```
synth -top good_mux
```
4. Run the synthesis
```
abc -liberty  ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
The output summary is shown as follows
![](Resources/1-5.png)


A flow chart containing the used cells and the wires can be seen using the `show` command

![](Resources/1-6.png)

