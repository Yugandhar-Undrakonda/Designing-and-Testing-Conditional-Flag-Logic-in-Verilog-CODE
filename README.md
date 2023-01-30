# Designing-Arithmetic-Unit-With-Register-Addressing-Mode.CODE-
`timescale 1ns / 1ps
 
 
module top();
 
reg [15:0] GPR [32:0]; ////32 Register are user accessible , R[32] -- mul
reg [31:0] IR; 
reg [31:0] temp;
 
`define opcode IR[31:27]
`define rdst IR[26:22]
`define src1 IR[21:17]
`define imm_sel IR[16]
`define src2 IR[15:11]
 
//// opcode(5) reg_dst(5) src1_reg (5) sel_mode src2_reg(5)
/// rdst = rsrc1 + rsrc2;
/// rdst = rsrc1 + immediate_number
`define mov 5'b00000
`define add 5'b00001
`define sub 5'b00010
`define mul 5'b00011
 
always@(*)
begin
 
case(`opcode)
/////////Updating Register data
`mov : begin
if(`imm_sel == 1'b1)
GPR[`rdst] = IR[15:0];
else
GPR[`rdst] = GPR[`src1];
end
 
`add: begin
GPR[`rdst] = GPR[`src1] + GPR[`src2];
end
 
`sub: begin
GPR[`rdst] = GPR[`src1] - GPR[`src2];
end
 
`mul : begin
temp = GPR[`src1] * GPR[`src2];
GPR[`rdst] =temp[15:0];
GPR[32] = temp[31:16];
end
 
endcase
end
 
endmodule
