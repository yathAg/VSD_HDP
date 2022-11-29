# VSD Hardware Design Program

## Index
* [Day 0 - Getting the tools](#Day-0---Getting-the-tools)

## Tools required during the project
### yosys
![spice_test](Resources/yosys.png)

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
![spice_test](Resources/opensta.png)
### icarus verilog

### gtkwave



## Chapter 1 - RTL DESIGN USING VERILOG WITH SKY130 TECHNOLOGY

The files used in this section can be cloned from: [https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git](https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git)

### 1.1 Using iverilog and gtkwave

Icarus Verilog or any other simulation program takes a verilog file and its corresponding testbench to produce a .vcd file.<br />
vcd stands for value change dump and as the name suggests vcd files only track the output when there is a change in the input.

To generate the .vcd file
```
iverilog <specfile.v> <testbench.v>
```
The vcd file simulation can be seen in gtkwave using
```
gtkwave <file.vcd>
```
### 1.2 Understanding yosys and Logic sysnthesis

Logic synthesis defines converting the behavorial verilog code to an RTL code described using the vendor specific standard cells.<br />
These standard cells are described in the .lib file. the design file and the lib file are the inputs to the synthesizer tool.

Yosys is a synthesiszer which produces the required rtl netlist file which can again be verified using iverilog and gtkwave. <br/>
Since the netlist is true dscription of the design the same testbench file can be used for the netlist. 