
## Overview:
On Day 2, I focused on understanding timing libraries (`.libs`), the differences between hierarchical and flat synthesis, and efficient flip-flop coding styles. These concepts are crucial for improving the design and synthesis processes in RTL design. Below is the breakdown of the tasks and labs completed today.

<details>
  <summary>Introduction to Timing Libraries (.libs)</summary>

  ### Introduction to .lib Files (Parts 1-3)
  - Studied the structure and significance of `.lib` files, which are essential for timing analysis and cell characterization.
  - Analyzed timing arcs, cell delays, and power information contained in `.lib` files.
  - Explanation of .lib library:
     - File path: `!gvim ../mylib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib`
     - Key breakdown:
       - `tt`: typical (Process)
       - `025C`: 77 °F (Temperature)
       - `1v80`: Voltage
     - ![lib filename explanation](https://github.com/user-attachments/assets/8e2fac9d-6ef7-4dce-8e20-e829e6c7bbef)
       
<img width="594" alt="Screenshot 2024-10-23 at 10 48 57 AM" src="https://github.com/user-attachments/assets/b45fc544-f0ea-4148-ae2c-7318dc72fc6f">

  - The logic gate `sky130_fd_sc_hd__a2111o_1`:
     - Has 5 inputs (A1, A2, B1, C1, D1), resulting in 2^5 = 32 possible input combinations.
     - Gate function explained:
       - `a2111o`:
         - `a`: represents the AND gate.
         - `o`: represents the OR gate.
         - `2111`: indicates the structure:
           - Two inputs (A1, A2) go into an AND gate.
           - The result of the AND gate feeds into the first input of a 4-input OR gate (A1, A2, B1, C1, D1).
             
     - ![Gate explanation](https://github.com/user-attachments/assets/65f2c6bb-37e8-4b5a-9481-8261973ef198)
       
     - As the number of gates increases, the area also increases, i.e., a larger cell is employing wider transistors.
     - Wider cells are faster but require larger areas and consume more power.
     - Smaller cells have more delay but require less area and consume less power.
       
<img width="1392" alt="Screenshot 2024-10-23 at 11 32 28 AM" src="https://github.com/user-attachments/assets/487a0c75-c2db-4eae-9e8a-a0f992110ac2">

</details>

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<details>
  <summary>Hierarchical vs Flat Synthesis</summary>

  ### Hierarchical vs Flat Synthesis (Parts 1-2)
  - Understood how hierarchical synthesis allows for modular design, while flat synthesis provides a single-level design view.
  - Conducted synthesis experiments to observe the impact on area, power, and performance.
  
    Example:
    ```verilog
    module sub_module2 (input a, input b, output y);
	    assign y = a | b;
    endmodule
  
    module sub_module1 (input a, input b, output y);
	    assign y = a & b;
    endmodule

    module multiple_modules (input a, input b, input c, output y);
	    wire net1;
	    sub_module1 u1(.a(a),.b(b),.y(net1));  // net1 = a & b
	    sub_module2 u2(.a(net1),.b(c),.y(y));  // y = net1 | c, i.e., y = a & b + c;
    endmodule
    ```
    
    #### After synthesis
    
    <img width="628" alt="Screenshot 2024-10-23 at 12 29 24 PM" src="https://github.com/user-attachments/assets/6927187b-04af-44e8-ba60-48db3aefbd9e">
		
    **multiple_modules**
    
    <img width="263" alt="Screenshot 2024-10-23 at 12 34 11 PM" src="https://github.com/user-attachments/assets/3431a5ca-2378-4106-ae19-3233d59fbff2">
    
    **sub_module1**
    
    <br><img width="262" alt="Screenshot 2024-10-23 at 12 34 35 PM" src="https://github.com/user-attachments/assets/1a840c8c-9809-4337-bd3d-c0eb55fcb740">
    
    **sub_module2**
    
    <br><img width="260" alt="Screenshot 2024-10-23 at 12 35 04 PM" src="https://github.com/user-attachments/assets/daf0dcde-46e2-43f7-9d1a-7cd56d40ed23">
    
    **design hierarchy**
    
    <br><img width="254" alt="Screenshot 2024-10-23 at 12 35 32 PM" src="https://github.com/user-attachments/assets/60bc031d-7865-411f-836d-017b0657f710">
    
    **Linking to liberty file**
    
    <img width="557" alt="Screenshot 2024-10-23 at 7 44 43 PM" src="https://github.com/user-attachments/assets/fda168af-7074-43f5-885a-dc52c7ab1b17">
    
    **Output**
    
    <img width="928" alt="Screenshot 2024-10-23 at 7 46 49 PM" src="https://github.com/user-attachments/assets/c10a87d2-1504-4e6d-b966-3d8853217659">
		
    **Writing netlists**
    
    ```bash
    write_verilog -noattr multiple_modules_netlist.v
    ```
    
    <img width="282" alt="Screenshot 2024-10-23 at 7 48 34 PM" src="https://github.com/user-attachments/assets/eca53284-8f78-467b-b9cf-5be39335b359">

    <img width="569" alt="Screenshot 2024-10-23 at 8 27 42 PM" src="https://github.com/user-attachments/assets/81b2a745-0722-4865-b841-2045f82e65db">

    <img width="664" alt="Screenshot 2024-10-23 at 8 04 51 PM" src="https://github.com/user-attachments/assets/75400a5a-b918-4703-9a51-98cf8225b0c9"><br>

    **Flat netlist**
    ```bash
    flatten
    ```

    <img width="352" alt="Screenshot 2024-10-23 at 8 31 00 PM" src="https://github.com/user-attachments/assets/6c291d00-46f5-4ec5-93ef-0af30ecf619d">
		
    <img width="564" alt="Screenshot 2024-10-23 at 8 33 46 PM" src="https://github.com/user-attachments/assets/1e0d0a1c-0a3e-451d-b777-b1eb9b90e672">
    
    **Output**

    <img width="1435" alt="Screenshot 2024-10-23 at 8 34 46 PM" src="https://github.com/user-attachments/assets/72e43ef5-83be-46be-9132-8c6fab7b5882">
    
    **sub_module.v**

    <img width="292" alt="Screenshot 2024-10-23 at 8 37 08 PM" src="https://github.com/user-attachments/assets/cc57b91e-6dbd-4caa-b86e-22f61ed8aeaf">
    
    **Linking to netlist**

    <img width="442" alt="Screenshot 2024-10-23 at 8 37 44 PM" src="https://github.com/user-attachments/assets/22c926f2-f3ee-4d74-b465-8c9efe546d97">
    
    **Output**
	
    <img width="742" alt="Screenshot 2024-10-23 at 8 38 20 PM" src="https://github.com/user-attachments/assets/4bbee867-044b-4a88-995a-a620d1b45dec">
    
    - When we have multiple instances of the same module.
    - To get rid of this redundancy, we `flatten` the netlist.
		
</details>

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<details>
  <summary>Various Flop Coding Styles and Optimization</summary>

  ### Why Flops and Flop Coding Styles
  - Learned about the importance of flip-flops in digital design and explored different flop coding styles.
  - Flops are used to minimize the glitches.
    
    <img width="720" alt="Screenshot 2024-10-23 at 7 27 47 PM" src="https://github.com/user-attachments/assets/c0488023-1a2c-46ae-955d-955815aab11c">
    
  #### Flop Coding Styles

  **dff_asyncres.v**
  
  ```verilog
    module dff_asyncres ( input clk ,  input async_reset , input d , output reg q );
    always @ (posedge clk , posedge async_reset)
    begin
	if(async_reset)
	    q <= 1'b0;
	else	
	    q <= d;
    end
    endmodule
  ```

   **Waveforms**
   
	![WhatsApp Image 2024-10-23 at 19 01 24](https://github.com/user-attachments/assets/82f4bc94-4b1e-418b-bb9f-6f1e52d45df2)
 
**dff_syncres.**

```verilog
module dff_syncres ( input clk , input async_reset , input sync_reset , input d , output reg q );
always @ (posedge clk )
begin
	if (sync_reset)
		q <= 1'b0;
	else	
		q <= d;
end
endmodule
```

**Waveforms**

![WhatsApp Image 2024-10-23 at 19 37 15](https://github.com/user-attachments/assets/8d5e39c9-977b-4801-bfd5-9397bdf07d3d)

  ### Lab of Asyncronous Flop Synthesis Simulations 
  
  **dff_asyncres.v**
  
  ```verilog
module dff_asyncres ( input clk ,  input async_reset , input d , output reg q );
always @ (posedge clk , posedge async_reset)
begin
	if(async_reset)
		q <= 1'b0;
	else	
		q <= d;
end
endmodule
```

<img width="1286" alt="Screenshot 2024-10-24 at 9 25 00 AM" src="https://github.com/user-attachments/assets/880462f3-c69c-4e6b-9ac1-86a9f8e72650">

**dff_async_set.v**

```verilog
module dff_async_set ( input clk ,  input async_set , input d , output reg q );
always @ (posedge clk , posedge async_set)
begin
	if(async_set)
		q <= 1'b1;
	else	
		q <= d;
end
endmodule
```

<img width="1285" alt="Screenshot 2024-10-24 at 9 27 24 AM" src="https://github.com/user-attachments/assets/0dfc239e-1007-412b-b14b-72165391f41c">

### Lab of Syncronous Flop Synthesis Simulations 

**dff_syncres.v**

<img width="314" alt="Screenshot 2024-10-24 at 9 41 40 AM" src="https://github.com/user-attachments/assets/cce3482e-ca65-4e01-aadb-246202de04a3">

**Output**

<img width="855" alt="Screenshot 2024-10-24 at 9 43 51 AM" src="https://github.com/user-attachments/assets/02117e39-8828-45c8-ac16-3bea622eadcf">

**dff_async_set.v**

<img width="361" alt="Screenshot 2024-10-24 at 9 56 23 AM" src="https://github.com/user-attachments/assets/dc3de8fb-3e91-41bc-802a-74827495563d">

**Output**

<img width="923" alt="Screenshot 2024-10-24 at 10 00 45 AM" src="https://github.com/user-attachments/assets/177f96c6-fc1d-4b64-8441-e2a515d90d3d">

**dff_syncres.v**

<img width="306" alt="Screenshot 2024-10-24 at 10 02 33 AM" src="https://github.com/user-attachments/assets/976bd57e-2ae1-4790-b07f-e1f569b52bfe">

**Output**

<img width="889" alt="Screenshot 2024-10-24 at 10 03 58 AM" src="https://github.com/user-attachments/assets/7d6c40b0-9b2b-4123-b818-a83985370f4c">

**Expected Output vs Original Output**

<img width="1320" alt="Screenshot 2024-10-24 at 10 07 50 AM" src="https://github.com/user-attachments/assets/4f62104f-8c44-476b-a51d-7c1855e1ef15">

  ### Interesting Optimizations
  
  - Applied optimizations to the flop designs to reduce area and improve performance.
    
    **Mult_2.v**
    
    ```verilog
    module mul2 (input [2:0] a, output [3:0] y);
		assign y = a * 2;
    endmodule
    ```
    ![WhatsApp Image 2024-10-24 at 13 38 58](https://github.com/user-attachments/assets/787894a5-ab87-402e-afe9-67982b078b13)
    
    **Synthesis**<br>
    
    <img width="325" alt="Screenshot 2024-10-24 at 1 44 02 PM" src="https://github.com/user-attachments/assets/cefce0e2-fd98-4243-ac3a-691f71cf5397">

    **Output**<br>
    
    <img width="741" alt="Screenshot 2024-10-24 at 1 42 52 PM" src="https://github.com/user-attachments/assets/1b83903d-204a-4aa3-a738-6f0f88201afa">
    
    **Netlist**<br>
    
    ```verilog
    module mul2(a, y);
  		input [2:0] a;
  		wire [2:0] a;
  		output [3:0] y;
  		wire [3:0] y;
  		assign y = { a, 1'h0 };
    endmodule
    ```
    
    **Mult_8.v**<br>
    
    ```verilog
    module mul8 (input [2:0] a, output [5:0] y);
	assign y = a * 9;
    endmodule
    ```
    
    ![WhatsApp Image 2024-10-24 at 13 58 25](https://github.com/user-attachments/assets/74e57bd6-70c4-4e60-9c03-a1764dfc7a0c)

    **Synthesis**<br>
    
    <img width="393" alt="Screenshot 2024-10-24 at 1 59 42 PM" src="https://github.com/user-attachments/assets/45b4bdb0-593e-4f76-a0be-1d4f53dab8ca">

    **Output**<br>
    
    <img width="927" alt="Screenshot 2024-10-24 at 2 00 09 PM" src="https://github.com/user-attachments/assets/e72bdaa7-184f-463d-bbd7-5e4aae01ee1a">
    
    **Netlist**<br>
    ```verilog
    module mult8(a, y);
  		input [2:0] a;
 		wire [2:0] a;
  		output [5:0] y;
  		wire [5:0] y;
  		assign y = { a, a };
    endmodule
    ```

    
</details>

