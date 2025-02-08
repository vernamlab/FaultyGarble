# FaultyGarble - Fault Attack on Secure Multiparty Neural Network Inference

## Overview
This project provides a **testbench for the Lite_MIPS module**, supporting **fault injection** and execution of garbled circuits. The testbench is designed to analyze the **first fault attack against secure inference implementations relying on garbled circuits**, a key example of **secure multiparty computation (MPC) schemes**.

The success of deep learning across various applications, including inference on edge devices, raises significant concerns about the privacy of users' data and proprietary deep learning models. Secure multiparty computation (MPC) enables privacy-preserving computations, and many secure inference protocols assume that the client follows the protocol honestly. However, adversarial clients may actively manipulate the execution to extract private model parameters.

This project explores the vulnerabilities of **garbled circuit-based MPC** to fault injection attacks, particularly **laser fault injection coupled with model-extraction techniques**. Our approach demonstrates that existing solutions, assumed to be secure against active attacks, remain vulnerable when subjected to fault-based adversarial interventions. The number of queries required for the attack is comparable to the best known model-extraction attacks under semi-honest settings, effectively reducing the security of MPC-based neural network inference to that of an unprotected model.

This testbench enables researchers and practitioners to:
- Simulate **secure MIPS execution** in an MPC-based environment.
- Introduce **faults dynamically** at different execution stages.
- Observe **how fault injection can compromise secure inference**.

## Components
1. **Lite_MIPS.v** - The Verilog implementation of the MIPS processor.
2. **Lite_MIPS_Fault_TB.v** - The testbench file that includes:
   - Clock and reset logic.
   - **Fault injection mechanism** to modify execution behavior.
   - **Input handling from files** (`g_init.txt` and `e_init.txt`).
   - **Simulation control and execution logs**.
3. **g_init.txt** - The **garbled instructions** input file.
4. **e_init.txt** - The **encrypted input data** file.
5. **Simulation tool** - Recommended: Xilinx Vivado or ModelSim.

## Running the Simulation
### Step 1: Prepare Input Files
Before running the simulation, **initialize `g_init.txt` and `e_init.txt`** with your test data. These files should contain **hexadecimal values** representing:
- `g_init.txt`: **Garbled instructions**.
- `e_init.txt`: **Encrypted data inputs**.

Each file should contain **256 lines** of hex values corresponding to the **2048-bit registers** in the design.

### Step 2: Compile and Simulate
1. **Open your Verilog simulation tool (Vivado, ModelSim, etc.).**
2. **Compile the design:**
   ```
   vlog Lite_MIPS.v Lite_MIPS_Fault_TB.v
   ```
3. **Run the simulation:**
   ```
   vsim -c -do "run -all"
   ```
4. **Observe the waveforms and logs.** The testbench will display messages regarding execution and fault injection.

## Fault Injection Mechanism
The testbench allows injecting faults into different parts of the execution at specified cycles:
- **ALU Fault (2'b01)**: Flips the output of the ALU.
- **Instruction Decode Fault (2'b10)**: Corrupts the instruction decode stage.
- **Input Data Fault (2'b11)**: Modifies input values.

### Injecting Faults
The `inject_fault` task is used to inject faults at runtime. The following example injects faults at **specific clock cycles**:
```verilog
#50 inject_fault(2'b01); // Inject ALU fault at cycle 50
#30 inject_fault(2'b10); // Inject instruction decode fault at cycle 80
#20 inject_fault(2'b11); // Inject input fault at cycle 100
```

Each fault type affects execution in the following ways:
1. **ALU Fault**: The output is flipped using a bitwise XOR operation.
2. **Instruction Decode Fault**: The instruction is modified by flipping bits.
3. **Input Fault**: The encrypted input is changed, affecting the computation.

## Expected Output
- **Standard Execution Logs**:
  - Instruction execution flow.
  - Values of registers and outputs.
- **Fault Injection Logs**:
  - Messages when faults are injected.
  - Changes in output due to injected faults.
- **Waveform Visualization (if applicable)**:
  - Expected vs. faulty execution paths.

## Use Cases
- **Security Analysis**: Evaluate the effect of **fault injection attacks** on secure MIPS execution.
- **Secure Computation Research**: Study how **garbled circuits-based MPC** handles faults.
- **Processor Resilience Testing**: Assess how a **secure processor** behaves under adversarial conditions.

## Further Customization
- Modify **fault injection points** to target other execution stages.
- Add **new fault types** for **instruction memory or control flow corruption**.
- Enhance **logging and debugging** for deeper fault analysis.

## References
If you use this code, please cite the following paper using BibTeX:

```bibtex
@article{hashemi2024faultygarble,
  title={FaultyGarble: Fault Attack on Secure Multiparty Neural Network Inference},
  author={Hashemi, Mohammad and Mehta, Dev and Mitard, Kyle and Tajik, Shahin and Ganji, Fatemeh},
  journal={Cryptology ePrint Archive},
  year={2024}
}
```
