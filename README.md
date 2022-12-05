# VSD Hardware Design Program

## Index
+ [Tools required during the project](#DTools-required-during-the-project)
+ [Chapter 1 - RTL DESIGN USING VERILOG WITH SKY130 TECHNOLOGY](#Chapter-1---RTL-DESIGN-USING-VERILOG-WITH-SKY130-TECHNOLOGY)
    - [1.1 Simulation Using iverilog and gtkwave](#11-Simulation-Using-iverilog-and-gtkwave)
    - [1.2 Logic synthesis using Yosys](#12-Logic-synthesis-using-Yosys)
    - [1.3 Understanding the .libs files](#13-Understanding-the-libs-files)
    - [1.4 Flop Coding](#14-Flop-Coding)
    - [1.5 Flop Simulation](#15-Flop-Simulation)
    - [1.6 Flop Synthesis](#16-Flop-Synthesis)
    - [1.7 Optimizations in Synthesis](#17-Optimizations-in-Synthesis)


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

### 1.2 Logic synthesis using Yosys

Logic synthesis defines converting the behavioral verilog code to an RTL code described using the vendor specific standard cells.<br />
These standard cells are described in the .lib file. The design file and the lib file are the inputs to the synthesizer tool.

![](Resources/1-3.png)

Yosys is a synthesizer which produces the required rtl netlist, file which can again be verified using iverilog and gtkwave. <br/>
Since the netlist is true description of the design the same testbench file can be used for the netlist. 

![](Resources/1-4.png)

A .lib contains multiple variants of a logic gate as fast and slow cells are required to meet the SETUP and HOLD times respectively.

Faster cells drive more current using transistors to charge and discharge the capacitive load in a circuit.
Fast cells have a low delay but consume more area and power due to wider transistors.

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
5. Generate the verilog file using 
```
write_verilog -noattr good_mux_netlist.v
```

The output summary is shown as follows

![](Resources/1-5.png)


A flow chart containing the used cells and the wires can be seen using the `show` command

![](Resources/1-6.png)

The `-noattr` switch generates a clean netlist file

![](Resources/1-7.png)


### 1.3 Understanding the .libs files

Library name `sky130_fd_sc_hd__tt_025C_1v80`:
   -`sky130_fd_sc_hd` : process namePVT variations
   - `tt` - typical process from (slow, fast , typical) 
   - `025C` - temperature1.5 Flop Coding 
Hierarchical Synthesis considers and instantiates each module described in the design individually. The module may be optimized by the synthesizer using different gates while maintaining the same logic.Usually when a stacked PMOS can be replaced buy circuit with stacked NMOS the circuit will be more optimized.

![](Resources/1-8.png)

The `flatten` command synthesis the design as described and eliminates the sub modules.

![](Resources/1-9.png)

The sub modules can also be synthesized individually this has the following advantages
- If the design contains repeated sub modules, they do not need to be synthesizer multiple times
- Big designs can be broken down to small parts to reduce load on synthesizer 


### 1.4 Flop Coding 

**Why are flops required ?**

Due to the propagation delay of combinational circuits the output has glitches. Flops between stages of combinational circuits make sure that there is a stable output and the glitches do not propagate.

**Set and reset signals**

To initialize the flop and have them in known state, set and reset signals are used to make the output 1 or 0 respectively 

**Asynchronous and synchronous signals**

asynchronous signals are independent of the clock and the output sigal is changed immediately whereas in synchronous signal it changes on the next clock signal.

![](Resources/1-10.png)

### 1.5 Flop Simulation

**Asynchronous reset**
![](Resources/1-11.png)
**Asynchronous set**
![](Resources/1-12.png)
**Synchronous Reset**
![](Resources/1-13.png)

### 1.6 Flop Synthesis
during flop synthesis `dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib` is used to map the library containing the flop standard cells
**Asynchronous reset**
![](Resources/1-14.png)
**Asynchronous set**
![](Resources/1-15.png)
**Synchronous Reset**
![](Resources/1-16.png)

### 1.7 Optimizations in Synthesis 

**optimization in multipliers**<br />
![](Resources/1-17.png)

**3 Bit number multiplied by 9**
![](Resources/1-18.png)

