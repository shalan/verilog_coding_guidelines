|#	|Check|		Yes/No|
+---+-----+---------+
|1	|Do you use blocking assignments to model sequential logic?| |

2	Do you use non-blocking assignments to model combinational logic?		
3	Do you have "`default_nettype none" at the top of each Verilog file?		
4	Do you have one module per file?		
5	Is the file named after the module name it has?		
6	Do you reset all the design flip-flops (if it has any)?		
7	Do you initialize any register as you declare it inside the RTL module?		
8	Does every case statemnet have a default?		
9	In asynchronous always block, do you provide default values for all variables being assigned?		
10	Do you provide the size for every numberical constant?		
11	Do you decalre the type (wire/reg) of every module port?		
12	Do you have a file header that provides your information as well as the license?		
13	Do you mix blocking and non-blocking assignments within a procedural block?		
14	Do you assign to the same variable from different always blocks?		
15	Do you use both clock edges?		
16	Do you instantiate modules using positional arguments?		
17	Do you have any undriven port in a module instance?		
18	Does the clock appear anywhere in the code other than next to  a posedge/negedge keyword?		
19	Do you use inout ports?		
20	Do you use upper case identifiers for parameters?		
