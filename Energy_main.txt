module Energy_Main(
    input clk,
    input reset,
    input [3:0] voltage,
    input [3:0] current,
    output reg [31:0] final_energy
);

    // State encoding
    parameter CALC_POWER = 2'b00,
              CALC_E     = 2'b01,
              ACCUMULATE = 2'b10;

    reg [1:0] state_reg;

    // Internal registers
    reg [7:0] power;      
    reg [11:0] energy;    

    always @(posedge clk) begin
        if (reset) begin
            state_reg    <= CALC_POWER;
            power        <= 0;
            energy       <= 0;
            final_energy <= 0;
        end
        else begin
            case (state_reg)

                CALC_POWER: begin
                    power <= voltage * current;
                    state_reg <= CALC_E;
                end

                CALC_E: begin
                    energy <= power * 10;
                    state_reg <= ACCUMULATE;
                end

                ACCUMULATE: begin
                    final_energy <= final_energy + energy;
                    state_reg <= CALC_POWER;
                end

                default: begin
                    state_reg <= CALC_POWER;
                end

            endcase
        end
    end

endmodule