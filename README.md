# JOHNSON-COUNTER

Design code:

module johnson_counter (
    input wire clk,    
    input wire reset,  
    output reg [3:0] q 
);

always @(posedge clk or posedge reset) begin
    if (reset) begin
        q <= 4'b0000;  
    end else begin
        q <= {~q[0], q[3:1]};  
    end
end

endmodule

Test bench:

module johnson_counter_tb;
    reg clk;
    reg reset;
    wire [3:0] q;
    johnson_counter uut (
        .clk(clk),
        .reset(reset),
        .q(q)
    );
    initial begin
        clk = 0;
        forever #5 clk = ~clk;  
    end
    initial begin
        reset = 1;
        #10 reset = 0;  
        #100 $finish;   
    end
    initial begin
        $monitor("At time %t: q = %b", $time, q);
    end
endmodule

