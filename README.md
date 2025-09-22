# Project Description and Proposal

This work introduces an open-source system-on-chip design for optically detected magnetic resonance (ODMR) control and readout, aimed at advancing accessible quantum sensing platforms. Quantum sensors are a near-term application of quantum technologies that promise advantages in sensitivity, size, weight, power, and cost compared to classical sensors [1]. These advantages create new opportunities in biomedical, aerospace, automotive, and energy applications where compact and precise measurement tools are desired [2-5].

In 2025, the nonprofit Quantum Village (QV) released an open design for a low-cost quantum sensor based on nitrogen vacancy (NV) centers [6,7]. NV centers are room-temperature qubits whose states can be controlled and read out using ODMR methods. This approach enables precise measurements of magnetic field, temperature, and pressure with robustness and sensitivity suitable for real-world conditions. Although ODMR systems have been studied extensively in laboratories, they typically depend on bulky and expensive analytical equipment, limiting their practicality outside controlled research environments [8]. Alternative implementations using microcontrollers [9] or FPGA-based controllers [10] have been tested, but they often lack transparency, reproducibility, and design continuity.

To address these gaps, we propose an *ODMR-SoC* that is purpose-built for driving, controlling, and processing ODMR-based quantum sensor signals. By integrating this design with the QV reference design [7], we establish a fully open and high-performance chip platform for continuous-wave ODMR sensors. This platform seeks to lower barriers for chip design education for quantum electronics, improve the reproducibility of quantum electronics research, and support the development of low-cost, portable quantum technology demonstrations for students, teachers, and hobbyists.

### References
1. Degen, C. L., Reinhard, F., & Cappellaro, P. (2017). "Quantum Sensing." *Reviews of Modern Physics*, 89(3), 035002.
2. Aslam, N., Zhou, H., et al. (2023). "Quantum Sensors for Biomedical Applications." *Nature Reviews Physics*, 5(3), 157-169.
3. Bennett, J. S., Vyhnalek, B. E., et al. (2021). "Precision magnetometers for aerospace applications: A review." *Sensors*, 21(16), 5568.
4. Hatano, Y., Tanigawa, J., et al. (2024). "A wide dynamic range diamond quantum sensor as an electric vehicle battery monitor." *Philosophical Transactions of the Royal Society A*, 382(2265), 20220312.
5. Crawford, S. E., Shugayev, R. A., et al. (2021). "Quantum sensing for energy applications: Review and perspective." *Advanced Quantum Technologies*, 4(8), 2100049.
6. Quantum Village. Uncut Gem. Uncut Gem by Quantum Village. https://quantumvillage.org/ 
7. Quantum Village. "UNCUT GEM: A prototype full-stack, fully open-source NV center diamond magnetometer." GitHub Repository. https://github.com/QuantumVillage/UncutGem
8. Esmaeili, S., Schmalenberg, P., et al. (2024). "Evolution of quantum spin sensing: From bench-scale ODMR to compact integrations." *APL Materials*, 12(4).
9. Mariani, G., Umemoto, A., & Nomura, S. (2022). "A home-made portable device based on Arduino Uno for pulsed magnetic resonance of NV centers in diamond." *AIP Advances*, 12(6).
10. Tong, Y., Zhang, W., et al. (2023). "A customized control and readout device for vector magnetometers based on nitrogen-vacancy centers." *Review of Scientific Instruments*, 94(1).

# OpenFrame Overview

The OpenFrame Project provides an empty harness chip that differs significantly from the Caravel and Caravan designs. Unlike Caravel and Caravan, which include integrated SoCs and additional features, OpenFrame offers only the essential padframe, providing users with a clean slate for their custom designs.

<img width="256" alt="Screenshot 2024-06-24 at 12 53 39 PM" src="https://github.com/efabless/openframe_timer_example/assets/67271180/ff58b58b-b9c8-4d5e-b9bc-bf344355fa80">

## Key Characteristics of OpenFrame

1. **Minimalist Design:** 
   - No integrated SoC or additional circuitry.
   - Only includes the padframe, a power-on-reset circuit, and a digital ROM containing the 32-bit project ID.

2. **Padframe Compatibility:**
   - The padframe design and pin placements match those of the Caravel and Caravan chips, ensuring compatibility and ease of transition between designs.
   - Pin types are identical, with power and ground pins positioned similarly and the same power domains available.

3. **Flexibility:**
   - Provides full access to all GPIO controls.
   - Maximizes the user project area, allowing for greater customization and integration of alternative SoCs or user-specific projects at the same hierarchy level.

4. **Simplified I/O:**
   - Pins that previously connected to CPU functions (e.g., flash controller interface, SPI interface, UART) are now repurposed as general-purpose I/O, offering flexibility for various applications.

The OpenFrame harness is ideal for those looking to implement custom SoCs or integrate user projects without the constraints of an existing SoC.

## Features

1. 44 configurable GPIOs.
2. User area of approximately 15mm².
3. Supports digital, analog, or mixed-signal designs.

# openframe_timer_example

This example implements a simple timer and connects it to the GPIOs.

## Installation and Setup

First, clone the repository:

```bash
git clone https://github.com/efabless/openframe_timer_example.git
cd openframe_timer_example
```

Then, download all dependencies:

```bash
make setup
```

## Hardening the Design

In this example, we will harden the timer. You will need to harden your own design similarly.

```bash
make user_proj_timer
```

Once you have hardened your design, integrate it into the OpenFrame wrapper:

```bash
make openframe_project_wrapper
```

## Important Notes

1. **Connecting to Power:**
   - Ensure your design is connected to power using the power pins on the wrapper.
   - Use the `vccd1_connection` and `vssd1_connection` macros, which contain the necessary vias and nets for power connections.

2. **Flattening the Design:**
   - If you plan to flatten your design within the `openframe_project_wrapper`, do not buffer the analog pins using standard cells.

3. **Running Custom Steps:**
   - Execute the custom step in OpenLane that copies the power pins from the template DEF. If this step is skipped, the precheck will fail, and your design will not be powered.
