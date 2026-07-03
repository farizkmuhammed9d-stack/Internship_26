module Energy_Main_tb;

    reg clk;
    reg reset;
    reg [3:0] voltage;
    reg [3:0] current;

    wire [31:0] final_energy;

    // Instantiate DUT (Design Under Test)
    Energy_Main uut (
        .clk(clk),
        .reset(reset),
        .voltage(voltage),
        .current(current),
        .final_energy(final_energy)
    );

    // Clock generation (10ns period)
    always #5 clk = ~clk;

    // Stimulus
    initial begin
        // Initial values
        clk = 0;
        reset = 1;
        voltage = 0;
        current = 0;

        // Apply reset
        #12;
        reset = 0;

        // Test case 1
        #10;
        voltage = 4'd2;
        current = 4'd3;

        // Let it run for few cycles
        #40;

        // Test case 2
        voltage = 4'd4;
        current = 4'd5;

        #40;

        // Test case 3
        voltage = 4'd7;
        current = 4'd2;

        #40;

        // Finish simulation
        $stop;
    end

    // Monitor outputs
    initial begin
        $monitor("Time=%0t | V=%d | I=%d | Energy=%d",
                  $time, voltage, current, final_energy);
    end

endmodule