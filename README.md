# Asynchronous FIFO (Dual Clock Domain)

## Overview  
This project implements a fully synthesizable Asynchronous FIFO in Verilog to enable reliable data transfer between two independent clock domains. The design addresses key clock domain crossing (CDC) challenges such as metastability, synchronization, and correct flag generation.

## Design Approach  
The FIFO is designed using standard industry techniques:

- Gray code pointer synchronization  
- Two flip-flop synchronizer for CDC  
- Dual-port memory architecture  
- Independent read and write clock domains  

## Architecture  

The design is divided into modular RTL blocks:

- FIFO.v – Top-level module  
- FIFO_memory.v – Dual-port memory  
- wptr_full.v – Write pointer and full flag logic  
- rptr_empty.v – Read pointer and empty flag logic  
- two_ff_sync.v – Synchronizer for clock domain crossing  
- FIFO_tb.v – Testbench for verification  

## FIFO Operation  

### Write Operation (wclk domain)  
- Data is written using the write clock  
- Write pointer increments in Gray code  
- Write operation is disabled when FIFO is full  

### Read Operation (rclk domain)  
- Data is read using the read clock  
- Read pointer increments independently  
- Read operation is disabled when FIFO is empty  

## Full and Empty Conditions  

- Empty Condition:  
  The FIFO is empty when the read pointer equals the synchronized write pointer  

- Full Condition:  
  The FIFO is full when the write pointer catches up to the read pointer with MSB inversion  

An extra MSB bit is used to distinguish between full and empty conditions.

## Clock Domain Crossing (CDC)  

To safely transfer signals between clock domains:

- Pointers are converted to Gray code (single-bit transition)  
- Signals are passed through a two flip-flop synchronizer  
- This reduces metastability and ensures reliable operation  

## Simulation Results  

![Waveform](sim/Asyn_FIFO_sim.png)

### Observations  
- Correct data transfer from write side to read side  
- Independent operation of write and read clocks  
- Proper behavior of full and empty flags  
- No invalid reads or writes observed  

## Testbench  

The testbench validates the design under different conditions:

- Normal write and read operations  
- FIFO full condition handling  
- FIFO empty condition handling  
- Asynchronous clock behavior  

## Project Structure  
