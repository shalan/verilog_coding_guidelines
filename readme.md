# Verilog HDL Coding Guidelines

## Introduction

HDL modeling style has a significant impact on how a digital system design is implemented and ultimately how it performs. However, synthesis tools have significantly improved, it is still the designer’s responsibility to generate HDL code that guides the synthesis tools and achieves the best result for a given architecture. 
This document provides Verilog HDL modeling guidelines for both novice and experienced designers. The document takes open source EDA tools into account. Adopting these guidelines will reduce the amount of time required to get high quality IP cores and will reduce possibilities for functional problems. Following these guidelines will, also, improve reusability and readability of the code. 

The document outlines set of 
+ <span style="color:red">Rules (R)</span>: A rule is a required practice that enables good designs; so, it <em>must</em> be implemented
+ <span style="color:yellow">Guidelines (G)</span>: A guideline is a recommended practice that enhances the design. So, it is Up to the designer to implement them. 
 
## General 
+ <span style="color:red">R</span>: Use Indentation to Improve the Readability 
+ <span style="color:red">R</span>: One Verilog statement per line.
+ <span style="color:red">R</span>: Keep the line length 80 characters or less.
+ <span style="color:red">R</span>: One module per file. Name the file after the module it contains. For example, if the module name is ```smul```, name the file ```smul.v```.

## Commenting 
+ <span style="color:red">R</span>: Comment your code. Be reasonable. Too many comments and too few comments have their drawbacks.
+ <span style="color:yellow">G</span>: Use ```/* ... */``` for multi-line comments.
+ <span style="color:red">R</span>: Add the company header to the top of all of your source files

## Naming 
+ <span style="color:red">R</span>: Create a Naming Convention for the design. Document it and use it consistently throughout the design. 
+ <span style="color:yellow">G</span>: Use capitals to identify constants: e.g.  ```STATE_S0```, ```GO```, ```ZERO```, e.tc.,
+ <span style="color:yellow">G</span>: Use lowercase letters for all signal names, variable names, and port names. 
+ <span style="color:yellow">G</span>: All parameter names must be in capitals
+ <span style="color:red">R</span>: Do not Use Hard Coded Numeric Values. Instead, use parameters to define constants.
+ <span style="color:yellow">G</span>: For active low signals, end the signal name with an underscore followed by a lowercase character (for example, ```_n``` or ```_b```). 
+ <span style="color:red">R</span>: When describing multibit buses, use a consistent ordering of bits 

## Module declaration/instantiation
+ <span style="color:red">R</span>: At most one module per file.
+ <span style="color:red">R</span>: Declare inputs and outputs one per line.
+ <span style="color:yellow">G</span>: Declare the ports in the following order: 
  + Clocks 
  + Resets 
  + Enables 
  + Other control signals 
  + Data and address lines 
+ <span style="color:yellow">G</span>: Group ports logically by their function.
+ <span style="color:red">R</span>: Ports with ```inout``` cannot be used to create tri-state buses. Yosys synthesis tool does not fully support tri-state logic.
+ <span style="color:red">R</span>: Do not instantiate modules using positional arguments.  Always use the dot notation. In other words, connect ports by name in module instantiations.
+ <span style="color:red">R</span>: Port connection widths must match. 
+ <span style="color:red">R</span>: Expressions are not allowed in port connections.
+ <span style="color:red">R</span>: Drive all unused module inputs
+ <span style="color:red">R</span>: Connect unused module outputs
+ <span style="color:yellow">G</span>: Parameterize modules if appropriate and if readability is not degraded.

## Clocking
+ <span style="color:red">R</span>: Core clock signal for a module is ```clk``` or ```clock```. 
+ <span style="color:yellow">G</span>: All clocks, other than the core clock, should have a description of the clock and the frequency (e.g.  ```clk_ssp_625```).
+ <span style="color:yellow">G</span>: Use one clock per module if possible. 
+ <span style="color:yellow">G</span>: Do not use mixed clock edges if possible 
+ <span style="color:red">R</span>: Do not gate the clocks as it generates glitches; instead, use clock gating cells provided by the target technology.

## Resetting
+ <span style="color:red">R</span>: Always reset sequential modules. Reset makes a design more deterministic and easier to verify. 
+ <span style="color:red">R</span>: ```initial``` shall not be used in RTL modeling. 
+ <span style="color:red">R</span>: Use asynchronous active low reset unless the target technology does not support this option.
+ <span style="color:yellow">G</span>: Reset port name must reflect its function; e.g., use ```rst_n``` or ```reset_n``` for a module reset port.

## Internal Tri-state logic
+ <span style="color:yellow">G</span>: Internal tristates for data bus access within a circuit must be used with care, and should be avoided if possible.
+ <span style="color:red">R</span>: A node cannot be left floating at any point of time; if a node is connected to tri-state cells then one of them should be enabled to provide a defined state (```0``` or ```1```) at any point of time. If this is not the case, an extra tri-state cell must be introduced to provide a ```0``` or ```1``` when other tri-state cells are disabled.

## Modeling
+ <span style="color:red">R</span>: Blocking assignments should only be used for modeling combinational logic (used in ```always_comb``` blocks) 
+ <span style="color:red">R</span>: Non-blocking assignments should only be used for modeling sequential logic (used in ```always_ff``` blocks) 
+ <span style="color:red">R</span>: Don't mix blocking and non-blocking assignments within a code block. Separate combinational from Sequential.
+ <span style="color:red">R</span>: Do not assign to the same variable from different always blocks to avoid race conditions
+ <span style="color:red">R</span>: Use one clock per always sensitivity list.
+ <span style="color:red">R</span>: Use ```@*``` for the sensitivity list for combinational logic always blocks.
+ <span style="color:red">R</span>: ```$signed()``` can only be used around the operands to ```>>>```, ```>```, ```>=```, ```<```, ```<=``` to ensure that these operators perform the signed equivalents. 
+ <span style="color:red">R</span>: Wires must be declared before using them. This can be enforced by adding the following to the top of any source file: ``` `default_nettype         none```
+ <span style="color:yellow">G</span>: If you would like to generate hardware using loops, then you should use generate blocks. 
+ <span style="color:yellow">G</span>: Multipliers can be synthesized and small multipliers can even be synthesized efficiently, but we recommend not to use the multiplication operator. Design your multiplier to be optimized for the design.
+ <span style="color:yellow">G</span>: Follow one of the recommended templates outlined by<sup>1</sup> to code an FSM.
+ <span style="color:red">R</span>: Avoid LATCHES! Check the results of synthesis and ensure you do not have any inferred latches—it usually means you missed something in your coding.
  + Every ```if``` statement has ```else```.
  + Every ```case``` statement has a ```default```.
  + All signals are initialized within a combinational block
+ <span style="color:red">R</span>: Do not use ```casex``` in RTL modeling. 
+ <span style="color:red">R</span>: ```casez``` can only be used in very specific situations to compactly implement priority encoder style hardware structures.
+ <span style="color:red">R</span>: No asynchronous logic/combinational feedback loops/self-timed logic.
+ <span style="color:red">R</span>: Synchronize any asynchronous input signal. Use brute-force synchronizer (2-3 flip flops in series) for single signals. Consult [2] for synchronizing buses.
+ <span style="color:red">R</span>: Synchronize any signal that crosses clock domains. Use brute force synchronizer to synchronize a signal coming from slower domain to a faster domain. Consult<sup>2</sup> for other scenarios.
+ <span style="color:yellow">G</span>: Eliminate Glue Logic at the Top Level 
+ <span style="color:red">R</span>: Do not assign ```x``` value to signals
+ <span style="color:red">R</span>: Operand sizes must match; if you don't know the size of the right hand side omit teh constant size; e.g., ```'hAB3```
+ <span style="color:red">R</span>: Verilog primitives cannot be used.
+ <span style="color:red">R</span>: Use parentheses in complex equations
+ <span style="color:red">R</span>: Avoid unbounded loops.
+ <span style="color:yellow">G</span>: Avoid hand instantiating technology specific cells. If they are needed then isolate them into separate module(s) to make the RTL portable.
+ <span style="color:red">R</span>: If you instantiate technology specific cells, make sure that the relative fan out does not not exceed 8.

## Memory
+ <span style="color:red">R</span>: Do not use large arrays. If your design needs SRAM memory block(s), consider using OpenRAM<sup>7</sup> or DFFRAM<sup>8</sup> projects.

## Miscellaneous
+ <span style="color:red">R</span>: ```initial``` shall not be used in RTL modeling. It can be used only to develop test benches. Use a reset signal to initialize registers if needed  (even though some synthesis tools support them for FPGA) 
+ <span style="color:red">R</span>: Delays shall not be used in RTL modeling. They can be used in test benching only.

## References
1. Clifford E. Cummings, The Fundamentals of Efficient Synthesizable Finite State Machine Design. 
2. Clifford E. Cummings, Clock Domain Crossing (CDC) Design & Verification Techniques Using SystemVerilog.
3. OpenCores, HDL modeling guidelines. 
4. Lattice Semiconductor, HDL Coding Guidelines.
5. Microsemi, Actel HDL Coding, Style Guide. 
6. Atmel, ASIC Design Guidelines.
7. OpenRAM, https://vlsida.github.io/OpenRAM/
8. DFFRAM, https://github.com/Cloud-V/DFFRAM
