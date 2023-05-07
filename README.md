Download Link: https://assignmentchef.com/product/solved-mips-hw4-single-cycle-cpu-ii
<br>
Based on Lab 3 (simple single-cycle CPU), add a memory unit to implement a complete single-cycle CPU which can run R-type, I-type and jump instructions.




<h1>2.          Demands</h1>

<ol>

 <li>Please use iverilog as your HDL simulator.</li>

 <li>“v”, and “TestBench.v” are supplied. Please use these modules and modules in Lab 3 to accomplish the design of your CPU.</li>

</ol>

Specify in your report if you have any other files in your design.

<ol start="3">

 <li>Submit all *.v source files and report(pdf) on new e3. Other form of file will get -10%.</li>

 <li>Refer to Lab 3 for top module’s name and IO ports.</li>

</ol>

Initialize the stack pointer (i.e., Reg_File[29]) to 128, and other registers to 0

Decoder may add control signals:

<ul>

 <li>Branch_o</li>

 <li>Jump_o</li>

 <li>MemRead_o</li>

 <li>MemWrite_o</li>

 <li>MemtoReg_o</li>

</ul>

<h1>3.          Requirement description</h1>

<ol>

 <li><strong> Basic instruction: </strong></li>

</ol>

Lab 3 instruction + lw、sw、beq、bne、j




Format:

R-type

<table width="554">

 <tbody>

  <tr>

   <td width="92">Op[31:26]</td>

   <td width="92">Rs[25:21]</td>

   <td width="92">Rt[20:16]</td>

   <td width="92">Rd[15:11]</td>

   <td width="94">Shamt[10:6]</td>

   <td width="92">Func[5:0]</td>

  </tr>

  <tr>

   <td width="92">I-type</td>

   <td width="92"> </td>

   <td width="92"> </td>

   <td colspan="3" width="278"> </td>

  </tr>

  <tr>

   <td width="92">Op[31:26]</td>

   <td width="92">Rs[25:21]</td>

   <td width="92">Rt[20:16]</td>

   <td colspan="3" width="278">Immediate[15:0]</td>

  </tr>

  <tr>

   <td width="92">Jump</td>

   <td width="92"> </td>

   <td width="92"> </td>

   <td colspan="3" width="278"> </td>

  </tr>

  <tr>

   <td width="92">Op[31:26]</td>

   <td width="92"> </td>

   <td width="92"> </td>

   <td colspan="3" width="278">Address[25:0]</td>

  </tr>

 </tbody>

</table>







Definition: <strong>lw</strong> instruction:

memwrite is 0, memread is 1, regwrite is 1 Reg[rt] ← Mem[rs+imm]




<strong>sw</strong> instruction:

memwrite is 1, memread is 0 Mem[rs+imm] ← Reg[rt]




<strong>branch </strong>instruction:

branch is 1, and decide branch or not by do AND with the zero signal from ALU beq:

if (rs==rt) then PC=PC+4+ (sign_Imm&lt;&lt;2)

bne:

if (rs!=rt) then PC=PC+4+ (sign_Imm&lt;&lt;2)

<strong>Jump </strong>instruction:

jump is 1

PC={PC[31:28], address&lt;&lt;2}

Op field:

<table width="208">

 <tbody>

  <tr>

   <td width="91"><strong>instruction </strong></td>

   <td width="117"><strong>Op[31:26] </strong></td>

  </tr>

  <tr>

   <td width="91">lw</td>

   <td width="117">6’b101100</td>

  </tr>

  <tr>

   <td width="91">sw</td>

   <td width="117">6’b101101</td>

  </tr>

  <tr>

   <td width="91">beq</td>

   <td width="117">6’b001010</td>

  </tr>

  <tr>

   <td width="91">bne</td>

   <td width="117">6’b001011</td>

  </tr>

  <tr>

   <td width="91">jump</td>

   <td width="117">6’b000010</td>

  </tr>

 </tbody>

</table>




Extend ALUOp from 2-bit to 3-bit: (You can modify this if necessary)

<table width="208">

 <tbody>

  <tr>

   <td width="91"><strong>instruction </strong></td>

   <td width="117"><strong>ALUOp </strong></td>

  </tr>

  <tr>

   <td width="91">R-type</td>

   <td width="117">010</td>

  </tr>

  <tr>

   <td width="91">addi</td>

   <td width="117">100</td>

  </tr>

  <tr>

   <td width="91">lui</td>

   <td width="117">101</td>

  </tr>

  <tr>

   <td width="91">lw、sw</td>

   <td width="117">000</td>

  </tr>

  <tr>

   <td width="91">beq</td>

   <td width="117">001</td>

  </tr>

  <tr>

   <td width="91">bne</td>

   <td width="117">110</td>

  </tr>

  <tr>

   <td width="91">jump</td>

   <td width="117">x</td>

  </tr>

 </tbody>

</table>




<ol>

 <li><strong>Advance set 1: </strong></li>

</ol>

<strong>Jal: jump and link </strong>

In MIPS, 31th register is used to save return address for function call Reg[31] save PC+4 and perform jump




Reg[31]=PC+4

PC={PC[31:28], address[25:0]&lt;&lt;2}







<table width="554">

 <tbody>

  <tr>

   <td width="92">Op[31:26]</td>

   <td width="462">Address[25:0]</td>

  </tr>

  <tr>

   <td width="92">6’b000011</td>

   <td width="462">Address[25:0]</td>

  </tr>

 </tbody>

</table>










<strong>Jr: jump to the address in the register rs </strong>PC=reg[rs]

e.g. In MIPS, return could be used by jr r31 to jump to return address from JAL.




<table width="554">

 <tbody>

  <tr>

   <td width="92">Op[31:26]</td>

   <td width="92">Rs[25:21]</td>

   <td width="92">Rt[20:16]</td>

   <td width="92">Rd[15:11]</td>

   <td width="94">Shamt[10:6]</td>

   <td width="92">Func[5:0]</td>

  </tr>

  <tr>

   <td width="92">6’b000000</td>

   <td width="92">rs</td>

   <td width="92">0</td>

   <td width="92">0</td>

   <td width="94">0</td>

   <td width="92">6’b001000</td>

  </tr>

 </tbody>

</table>




<ol>

 <li><strong>Advance set 2: </strong>blt (branch on less than): if( rs&lt;rt ) then branch</li>

</ol>

<table width="554">

 <tbody>

  <tr>

   <td width="92">Op[31:26]</td>

   <td width="92">Rs[25:21]</td>

   <td width="92">Rt[20:16]</td>

   <td width="278">Immediate[15:0]</td>

  </tr>

  <tr>

   <td width="92">6’b001110</td>

   <td width="92">rs</td>

   <td width="92">rt</td>

   <td width="278">offset</td>

  </tr>

  <tr>

   <td colspan="3" width="276">bnez (branch non equal zero): if( rs!=0) th</td>

   <td width="278">en branch (it is same as bne)</td>

  </tr>

  <tr>

   <td width="92">Op[31:26]</td>

   <td width="92">Rs[25:21]</td>

   <td width="92">Rt[20:16]</td>

   <td width="278">Immediate[15:0]</td>

  </tr>

  <tr>

   <td width="92">6’b001100</td>

   <td width="92">rs</td>

   <td width="92">000000</td>

   <td width="278">offset</td>

  </tr>

 </tbody>

</table>




bgez (branch greater equal zero): if(rs&gt;=0) then branch

<table width="553">

 <tbody>

  <tr>

   <td width="92">Op[31:26]</td>

   <td width="92">Rs[25:21]</td>

   <td width="92">Rt[20:16]</td>

   <td width="278">Immediate[15:0]</td>

  </tr>

  <tr>

   <td width="92">6’b001101</td>

   <td width="92">rs</td>

   <td width="92">000001</td>

   <td width="278">offset</td>

  </tr>

 </tbody>

</table>




Example: when CPU executes function call:










If you want to execute recursive function, you must use the stack point (REGISTER_BANK [29]). First, store the register to memory and load back after function call has been finished.

<h1>4.          Architecture Diagram</h1>

<strong> </strong>

<h1>5.          Test</h1>

Modify line 139 to 141 of TestBench.v to read different data.




CO_P4_test_data1.txt tests the basic instructions.

CO_P4_test_data2.txt tests the advanced set 1.

CO_P4_test_data2_2.txt test the advanced set 2.




After the simulation of TestBench, you will get the file CO_P4_result.txt. You can  verify the result with dataX_result.txt.

If your design passes the test data, the following words would show  in the terminal.

You can add more “`include” instructions if necessary.