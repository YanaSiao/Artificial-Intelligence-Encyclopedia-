**Volatile Memory (RAM)**

- **Function**: Working memory used to store the system state and variables during program operation.
    
- **The RAM Bottleneck**: RAM is considered the primary bottleneck in Embedded and Edge AI for several reasons:
    - **Energy Consumption**: It is very energy-intensive to maintain.
    - **Volatility**: Data is lost as soon as the power is shut down.    
    - **Resource Constraints**: It is expensive and takes up significant physical space on the device, leading to very limited availability (e.g., 2KB to 1MB for high-end MCUs).

**Non-Volatile Memory (Flash)**

- **Definition**: In these systems, non-volatile memory is typically **Flash memory**.
- **Function**: Used to store data that must be preserved when the system is powered off, such as the software program (firmware) and device configuration.
- **Performance**: It is characterized by being slow to read and extremely slow to write compared to RAM.

