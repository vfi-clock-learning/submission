# About
This repository contains the datasets used in the work "Machine Learning Fault Injection Detection in Clock Signals: An Analysis of Frequency Impact".

The dataset consists of recorded clock and Vcap traces from the glitches injected on an [AES software execution](https://github.com/kokke/tiny-AES-c). Each dataset includes the metadata used to generate the glitches and covers a different clock frequency evaluation, including 16 MHz, 32 MHz, and 50 MHz.

## Fault Injection Setup

- **Target Board**: [STM32F410RB NUCLEO](https://www.st.com/en/evaluation-tools/nucleo-f410rb.html) (with RST, 3V3, and VCAP capacitors removed).
- **Glitch Injection**: Conducted using [Riscure's Inspector](https://www.keysight.com/us/en/products/network-test/device-vulnerability-analysis.html) fault injection workbench.
- **Power Supply**:  
  - **E5V Pin**: Provides standard 5V power supply.
  - **VCAP Pin**: Used as the glitch injection point, bridged to maintain nominal Vcap voltage when no glitch is applied.

When the device under test (DuT) raises its trigger, the glitch injector waits **delay** ns before injecting a rectangular pulse with a voltage of **glitch voltage** and a duration of **duration** ns. The oscilloscope is configured to start its measurement 200 ns before the glitch injection is completed. The fault model pursued consists on a single fault on the state register bewteen the two last MixColumns() in rounds 8 and 9 as published by [Dusart et al](https://link.springer.com/chapter/10.1007/978-3-540-45203-4_23).  

Relevant parameters of the evaluation are as follows:

| **Parameter**             | **16 MHz**    | **32 MHz**    | **50 MHz**    |
|---------------------------|---------------|---------------|---------------|
| **Total measurements**    | 567,000       | 21,252        | 21,252        |
| **AES Attack Window**     | 169 µs        | 93 µs         | 59 µs         |
| **Sample Rate**           | 500 MS/s      | 1 GS/s        | 1 GS/s        |
| **Trace Samples**         | 1,000         | 1,000         | 1,000         |
| **Oscilloscope Offset**   | -200 ns       | -200 ns       | -200 ns       |
| **Nominal Vcap**          | 1.1 V         | 1.2 V         | 1.2 V         |
| **E5V Current Limit**     | 100 mA        | 150 mA        | 150 mA        |


## Dataset Structure

The datasets are in [H5](http://www.h5py.org/) format and contain the following structure:  

- **id**: The index of the measurement in the evaluation.  
- **glitch**: A 1000-sample trace of the Vcap signal during the glitch injection window.  
- **clk**: A 1000-sample trace of the clock signal during the glitch injection window.  
- **glitch voltage**  (V): The voltage of the glitch pulse for its duration.  
- **duration** (ns): The width of the injected voltage glitch.  
- **delay** (ns): The offset between the trigger event and the glitch injection.  
- **power**: The power of the injected glitch.  
- **data**: The received feedback message consisting of a ciphertext block.  
- **verdict**: Labels assigned to the received data based on system behavior, such as:
  - 3 for expected
  - 4 for faulty
  - 5 for mute
  - 10 for incomplete communication (incorrect)

## Dataset Download

The datasets can be downloaded from the following links:

- [Dataset - 16 MHz](https://drive.google.com/file/d/1ce_hDuRB16098wDqluXlPL1NJVxVKMyi/view)  
- [Dataset - 30 MHz](https://drive.google.com/file/d/16ZO0kQptEp1NcAXJQv14FvJzX3pqTs2d/view)  
- [Dataset - 50 MHz](https://drive.google.com/file/d/11b9R263tQOrXSGuv45SQjo9HT4240-z0/view)  
