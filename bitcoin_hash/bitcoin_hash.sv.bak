`include "simplified_sha256.sv"

module bitcoin_hash (input logic        clk, reset_n, start,
                     input logic [15:0] message_addr, output_addr,
                    output logic        done, mem_clk, mem_we,
                    output logic [15:0] mem_addr,
                    output logic [31:0] mem_write_data,
                     input logic [31:0] mem_read_data);
					 
parameter num_nonces = 16;

enum logic [ 3:0] {IDLE, READ_BUFFER, READ, BLOCK1, BLOCK2, BLOCK3, WRITE} state;
logic [31:0] message[32];
logic [31:0] w[num_nonces][16];
logic [31:0] h[num_nonces+1][8];
logic d[num_nonces+1];
logic [ 4:0] offset; 
logic        cur_we;
logic [15:0] cur_addr;
logic [31:0] cur_write_data;
logic computeI, compute, load;

assign mem_clk = clk;
assign mem_addr = cur_addr + offset;
assign mem_we = cur_we;
assign mem_write_data = cur_write_data;


	simplified_sha256 s0(
	.clk(clk),
	.reset_n(reset_n),
	.start( computeI),
	.message(w[0]),
	.data_in(h[0]),
	.load(1'b0),
	.done(d[0]),
	.data_out(h[0])
	);

	simplified_sha256 s1(
	.clk(clk),
	.reset_n(reset_n),
	.start( compute),
	.message(w[0]),
	.data_in(h[0]),
	.load(load),
	.done(d[1]),
	.data_out(h[1])
	);
	
	simplified_sha256 s2(
	.clk(clk),
	.reset_n(reset_n),
	.start( compute),
	.message(w[1]),
	.data_in(h[0]),
	.load(load),
	.done(d[2]),
	.data_out(h[2])
	);
	
	simplified_sha256 s3(
	.clk(clk),
	.reset_n(reset_n),
	.start( compute),
	.message(w[2]),
	.data_in(h[0]),
	.load(load),
	.done(d[3]),
	.data_out(h[3])
	);
	
	simplified_sha256 s4(
	.clk(clk),
	.reset_n(reset_n),
	.start( compute),
	.message(w[3]),
	.data_in(h[0]),
	.load(load),
	.done(d[4]),
	.data_out(h[4])
	);
	
	simplified_sha256 s5(
	.clk(clk),
	.reset_n(reset_n),
	.start( compute),
	.message(w[4]),
	.data_in(h[0]),
	.load(load),
	.done(d[5]),
	.data_out(h[5])
	);
	
	simplified_sha256 s6(
	.clk(clk),
	.reset_n(reset_n),
	.start( compute),
	.message(w[5]),
	.data_in(h[0]),
	.load(load),
	.done(d[6]),
	.data_out(h[6])
	);
	
	simplified_sha256 s7(
	.clk(clk),
	.reset_n(reset_n),
	.start( compute),
	.message(w[6]),
	.data_in(h[0]),
	.load(load),
	.done(d[7]),
	.data_out(h[7])
	);
	
	simplified_sha256 s8(
	.clk(clk),
	.reset_n(reset_n),
	.start( compute),
	.message(w[7]),
	.data_in(h[0]),
	.load(load),
	.done(d[8]),
	.data_out(h[8])
	);
	
	simplified_sha256 s9(
	.clk(clk),
	.reset_n(reset_n),
	.start( compute),
	.message(w[8]),
	.data_in(h[0]),
	.load(load),
	.done(d[9]),
	.data_out(h[9])
	);
	
	simplified_sha256 s10(
	.clk(clk),
	.reset_n(reset_n),
	.start( compute),
	.message(w[9]),
	.data_in(h[0]),
	.load(load),
	.done(d[10]),
	.data_out(h[10])
	);
	
	simplified_sha256 s11(
	.clk(clk),
	.reset_n(reset_n),
	.start( compute),
	.message(w[10]),
	.data_in(h[0]),
	.load(load),
	.done(d[11]),
	.data_out(h[11])
	);
	
	simplified_sha256 s12(
	.clk(clk),
	.reset_n(reset_n),
	.start( compute),
	.message(w[11]),
	.data_in(h[0]),
	.load(load),
	.done(d[12]),
	.data_out(h[12])
	);
	
	simplified_sha256 s13(
	.clk(clk),
	.reset_n(reset_n),
	.start( compute),
	.message(w[12]),
	.data_in(h[0]),
	.load(load),
	.done(d[13]),
	.data_out(h[13])
	);
	
	simplified_sha256 s14(
	.clk(clk),
	.reset_n(reset_n),
	.start( compute),
	.message(w[13]),
	.data_in(h[0]),
	.load(load),
	.done(d[14]),
	.data_out(h[14])
	);
	
	simplified_sha256 s15(
	.clk(clk),
	.reset_n(reset_n),
	.start( compute),
	.message(w[14]),
	.data_in(h[0]),
	.load(load),
	.done(d[15]),
	.data_out(h[15])
	);
	
	simplified_sha256 s16(
	.clk(clk),
	.reset_n(reset_n),
	.start( compute),
	.message(w[15]),
	.data_in(h[0]),
	.load(load),
	.done(d[16]),
	.data_out(h[16])
	);
	
always_ff @(posedge clk, negedge reset_n)
begin
  if (!reset_n) begin
    state <= IDLE;
  end 
  else case (state)
   IDLE: begin 
       if(start) begin
			cur_we <= 0;
			offset <= 0;
			computeI <= 0;	
			compute <= 0;	
			cur_addr <= message_addr;
			
			state <= READ_BUFFER;
       end
    end

	 READ_BUFFER: state <= READ;
	 
	  READ: begin
			if(offset < 19) 
				begin
					message[offset] <= mem_read_data;
					offset++;
					state <= READ_BUFFER;
				end
			else begin
				for(int i = 0; i < 16; i++) w[0][i] <= message[i];
				for(int i = 21; i < 31; i++) message[i] <= 0;
				message[20] <= 32'h80000000;
				message[31] <= 32'd640;
				
				offset <= 0;
				computeI <= 1;
				load <= 0;

				state <= BLOCK1;	
			end
		end			
	
	 BLOCK1: begin
		if(d[0]) begin
			for(int i = 0; i < 16; i++) begin
				for(int j = 0; j < 3; j++) w[i][j] <= message[j+16];
			end

			for(int i = 0; i < 16; i++) w[i][3] <= i;
			
			for(int i = 0; i < 16; i++) begin
				for(int j = 4; j < 16; j++) w[i][j] <= message[j+16];
			end
			
			compute <= 1;
			load <= 1;
			state <= BLOCK2;
		end
		else state <= BLOCK1;
	end

	 BLOCK2: begin
		if(d[1]) begin
			for(int i  = 0; i < 16; i++) begin
				for(int j = 0; j < 8; j++) w[i][j] <= h[i+1][j];
			end
			
			for(int j = 0; j < 16; j++) w[j][8] <= 32'h80000000;
			
			for( int i = 0; i < 16; i++) begin
				for(int j = 9; j < 15; j++) w[i][j] <= 0;
			end
			
			for( int i = 0; i < 16; i++) w[i][15] <= 32'd256;
			
			compute <= 1;
			load <= 0;
			state <= BLOCK3;
		end
		else begin
			compute <= 0;
			state <= BLOCK2;
		end
	end

	 BLOCK3: begin
		if(d[1]) begin
			cur_addr <= output_addr;
			cur_we <= 1;
			cur_write_data <= h[1][0]; 
			state <= WRITE;
		end
		
		else begin
			 compute <= 0;
			state <= BLOCK3;
		end
	end
		
	 WRITE: begin
		if(offset < num_nonces) begin
			if(offset == 15) cur_write_data <= h[16][0];
			else cur_write_data <= h[offset+2][0];
		  
			offset++;
			state <= WRITE;
		end
		else state <= IDLE;
    end
   endcase
  end
	
assign done = (state == IDLE);	
endmodule