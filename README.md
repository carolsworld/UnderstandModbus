# Modbus Protocol Overview

## Background

In industrial automation, Original Equipment Manufacturers (OEMs) use various proprietary communication protocols to manage device communications on the plant floor. Examples of these proprietary protocols include:

- **Profinet**
- **CIP**
- **DeviceNet**
- **ControlNet**

These protocols enable OEM devices to communicate efficiently but create challenges for interoperability with devices from other manufacturers.

## Rationale for Modbus

Given the diversity of manufacturers producing controllers, instruments, and other peripheral devices, a standardized approach for device-to-device communication is necessary. Modbus, along with other open-source protocols, addresses this need by providing a universal method for data exchange across third-party devices.

### Key Points:

- **Interoperability Across Devices**:
  - Many plants operate with equipment from multiple OEMs, each using its proprietary protocol.
  - Modbus enables seamless data exchange among devices from different manufacturers, promoting interoperability on the plant floor.

- **Bridging Proprietary Protocol Gaps**:
  - Proprietary protocols are often limited to devices from the same OEM, which limits flexibility.
  - Modbus and similar open protocols bridge these gaps, facilitating communication with third-party devices.

- **Standardization Benefits**:
  - Open protocols, such as Modbus, provide a consistent, widely accepted standard.
  - Standardized communication reduces complexity, increases efficiency, and makes integrating new devices into existing systems easier.

## Other Standard Open Protocols

In addition to Modbus, other open-source communication protocols commonly used in industrial automation include:

- **OPC DA (OLE for Process Control Data Access)**
- **OPC UA (Unified Architecture)**
- **MQTT (Message Queuing Telemetry Transport)**

---

## What is Modbus

Modbus is a communication protocol developed in 1979 by **Modicon** (now part of Schneider Electric) for use with **programmable logic controllers (PLCs)**. It has since become a widely accepted standard in industrial automation, allowing different devices to communicate efficiently. 

### Key Features:

- **Data Transmission**:
  - Modbus supports data transmission over both **serial lines** and **Ethernet**.
  - Its flexibility has made it compatible with many devices, leading to widespread adoption in industrial environments.

- **Commonly Used in SCADA**:
  - Modbus is widely used in **Supervisory Control and Data Acquisition (SCADA)** systems.
  - SCADA systems rely on Modbus to receive data from **remote terminal units (RTUs)** and **PLCs** for monitoring and control.

### Types of Modbus

Modbus can be categorized into three main types, each suited to different communication requirements:

1. **Modbus RTU**: 
   - A binary protocol commonly used for serial communication.
   - Known for its high data throughput and efficiency.

2. **Modbus ASCII**: 
   - Uses ASCII characters for communication, offering better readability.
   - Often slower than RTU but can be more robust for certain applications.

3. **Modbus TCP**: 
   - Designed for Ethernet networks, allowing Modbus to be used over TCP/IP.
   - Ideal for modern, networked industrial environments.

---

### Modbus RTU

**Modbus RTU** is the most commonly used Modbus protocol, known for its simplicity and reliability in industrial environments. It operates as a straightforward **serial protocol** that utilizes standard **UART (Universal Asynchronous Receiver-Transmitter)** technology.

#### Key Characteristics

- **Data Transmission Speed**:
  - Supports baud rates ranging from **1200 to 115200 bits per second**.
  - Most Modbus RTU devices operate at a typical speed of **38400 bits per second**.
  - Data is transmitted as **8-bit bytes**, one bit at a time.

- **Master-Slave Structure**:
  - Modbus RTU follows a **Master to Slave** communication structure.
  - A **Master** device can communicate with up to **254 Slave devices**.
  - Each Slave is assigned a unique **8-bit address** (also known as a unit number).
  - The Master sends packets containing the address of the intended Slave, and only the specified Slave responds. If the addressed Slave does not respond within a specified time, the Master records a "no response" error.

- **Slave Devices**:
  - A Slave in Modbus RTU is any peripheral device capable of processing data and returning a response to the Master.
  - Typical Slave devices include **I/O transducers, valves, network drives, and other measuring devices**.

- **Communication Media**:
  - Modbus RTU uses serial communication, typically through **RS232, RS422, or RS485** connections.

#### Summary

Modbus RTU is ideal for scenarios where straightforward, serial-based communication is needed between a central Master and multiple peripheral devices. Its reliability and simplicity make it suitable for many industrial applications.

---

### Modbus ASCII

**Modbus ASCII** is an older version of the Modbus protocol. It shares many structural elements with Modbus RTU but is distinguished by using **ASCII characters** instead of binary for communication.

#### Key Characteristics

- **Readable Format**:
  - Modbus ASCII transmits data in **ASCII characters**, making the protocol more readable and easier to interpret manually compared to the binary-based Modbus RTU.

- **Limited Support**:
  - Modbus ASCII is **not widely supported** and lacks the compatibility found with Modbus RTU and Modbus TCP.
  - It is **not included in the official Modbus protocol specification**, making it less common in modern industrial applications.

#### Summary

While Modbus ASCII was once a popular choice, it has largely fallen out of use in favor of more efficient protocols like Modbus RTU and Modbus TCP. Its readable ASCII format can still be useful in specific applications, but it is generally not recommended for new implementations due to limited support and compatibility.

---

### Modbus TCP

**Modbus TCP** is a variant of the Modbus protocol designed to operate over **Ethernet networks** using **TCP/IP**. It wraps traditional Modbus RTU data packets within a TCP packet, allowing for communication over standard Ethernet infrastructure.

#### Key Characteristics

- **Integration with TCP/IP**:
  - Modbus TCP embeds Modbus RTU packets within **TCP packets**, enabling them to be sent over conventional Ethernet networks.
  - Uses **IP addresses** (e.g., 192.168.0.20) for addressing instead of relying solely on slave addresses, as with Modbus RTU.

- **Port and Network Model**:
  - Modbus TCP typically communicates over **port 502**, though this port can be reconfigured if needed.
  - It aligns with the **OSI network model**, making it compatible with modern networking standards and protocols.

- **Client-Server Architecture**:
  - Modbus TCP shifts from the traditional **Master-Slave** model to a **Client-Server** model:
    - The **Master** (initiator of communication) is now called the **Client**. For instance, OpenPLC Runtime and Siemens TIA portal running the PLC control logic and parameters are the master device or client, which send out instructions and requests.
    - The **Slave** (responding device) is now referred to as the **Server**. For instance, Factory i/o software or Siemens NX are the slave device, which receive instruction.  
  - Supports **multiple clients and multiple servers**, leveraging **peer-to-peer communication** capabilities of Ethernet IP.

- **Physical Connection**:
  - Devices that are Modbus TCP compliant can be connected using a standard **RJ45 (LAN) cable**.

#### Summary

Modbus TCP is ideal for applications where Ethernet networks are already established, offering the flexibility and scalability of TCP/IP communication. The shift to a client-server model and the use of IP addressing make it a powerful choice for integrating Modbus with modern network infrastructures.

---

### Modbus Message Structure

Understanding the structure of Modbus messages is essential for utilizing the protocol effectively. Modbus relies on **registers** to store and transmit different types of data. Registers function as ‘buckets’ that hold various data points, each type serving a specific purpose.

### Modbus Protocol | Message Structure

| Register Type       | Register Number    | Register Size | Permission |
|---------------------|--------------------|---------------|------------|
| **Coil**            | 1 - 9999          | 1 bit        | R/W        |
| **Discrete Inputs** | 10001 - 19999      | 1 bit        | R          |
| **Input Register**  | 30001 - 39999      | 16 bit       | R          |
| **Holding Register**| 400001 - 49999     | 16 bit       | R/W        |

*Modbus Protocol | Message Structure*

#### Types of Modbus Registers

1. **Discrete Inputs (Contacts)**:
   - **Description**: Discrete Inputs are bit-based contact registers, meaning they represent binary (on/off) states.
   - **Function**: **Read-only**
   - **Use Case**: These registers are often used to represent contacts in PLC programming.

2. **Discrete Outputs (Coils)**:
   - **Description**: Coils are one-bit registers used as outputs in the system.
   - **Function**: **Read and Write**
   - **Use Case**: Ideal for output control, such as activating or deactivating devices.

3. **Input Registers**:
   - **Description**: Input registers are **16-bit registers** designed for input data.
   - **Function**: **Read-only**
   - **Use Case**: Commonly used for reading sensor values or other input data that does not require modification.

4. **Holding Registers**:
   - **Description**: Holding registers are **16-bit registers** that support both input and output functions.
   - **Function**: **Read and Write**
   - **Use Case**: As the most versatile register type, holding registers are used to store any kind of data, making them suitable for inputs, outputs, and general data storage.

#### Summary

Modbus message structure leverages these registers to transmit data efficiently. Each register type has a distinct role, allowing for flexible data handling in various industrial applications.

---

### Modbus Function Codes

The Modbus protocol defines a set of **function codes** used to access Modbus registers. Modbus separates data into four distinct data blocks, each with overlapping addresses or register numbers. To accurately locate and interpret data, both the **address (or register number)** and the **function code** are necessary to identify the specific data type and its location.

Below is a list of commonly used Modbus function codes. This list covers the essential codes, though Modbus supports additional codes for various advanced functions.

| Function Code | Description                          |
|---------------|--------------------------------------|
| 01            | Read Coils                           |
| 02            | Read Discrete Inputs                 |
| 03            | Read Holding Registers               |
| 04            | Read Input Registers                 |
| 05            | Write Single Coil                    |
| 06            | Write Single Holding Register        |
| 15            | Write Multiple Coils                 |
| 16            | Write Multiple Holding Registers     |

*Note*: This table includes the most widely used Modbus function codes. Additional function codes exist for specialized applications.

#### Summary

Understanding Modbus function codes is essential for accessing and manipulating data within the protocol's structure. Each function code corresponds to a specific action and register type, enabling comprehensive data communication across Modbus-compliant devices.

---

### Modbus Error (Exception) Code Explanation

In Modbus communication, when a **Modbus slave** detects a packet but identifies an error in the request, it does not send a data response. Instead, it responds with an **exception code** to indicate the issue.

#### Structure of an Exception Reply

An exception reply from a Modbus slave includes the following components:

- **Slave Address (Unit Number)**: Identifies the specific slave device that detected the error.
- **Modified Function Code**: The function code from the original request, with the **high bit set** (indicating an error).
- **Exception Code**: A specific code that describes the nature of the error.

#### Purpose

Exception codes provide a clear indication of errors in Modbus requests, helping troubleshoot communication issues by identifying common problems such as invalid function codes, data addresses, or values.

### Modbus Protocol | Error Code Explanations

| Error Code | Error Type           | Description                                                                     |
|------------|-----------------------|---------------------------------------------------------------------------------|
| 1          | Illegal function      | The function code received is not recognized by the slave.                      |
| 2          | Illegal data address  | The register number is not recognized by the slave. It doesn’t exist.           |
| 3          | Illegal data value    | The value contained in the data is not accepted by the slave.                   |
| 4          | Slave device failure  | An error that occurs while the slave is attempting to perform an action.        |
| 6          | Slave device busy     | The slave is engaged in processing a long-duration command.                     |

*Modbus Protocol | Error Code Explanations*


---

### Importance of Modbus TCP Protocol

The **Modbus TCP protocol** is an essential skill for every automation and controls engineer or technician. Its simplicity, reliability, and wide adoption in industrial settings make it a fundamental protocol to understand and implement in factory automation.

### Step-by-Step Guide: Configuring Siemens S7-1500 PLC as a Modbus Server in TIA Portal

This tutorial includes:

- Setting up, programming and configuring a Siemens **S7-1500 PLC** in the **TIA Portal** as a Modbus **server**.
- Using **Siemens PLCSIM Advanced v4.0** for advanced simulation and testing of Modbus configuration before deployment.
- **Simenes NX**: A Digital Twin of conveyor belt acts as a Modbus server.
- **Data Communication Over Modbus**: Reading and writing data over the Modbus network.

1. **In the PLCSIM Advanced**:
  - Enable the PLCSIM Virtual Eth. Adapter. TCP/IP communication with Ethernet. Set the instance name ModbusTCPTest, with IP Address set as the one set at the S7-1500 PLC in TIA portal, i.e. 192.168.0.10, 255.255.255.0. 

2. **In the TIA Portal**:
   - Select one of the **S7-1500 PLC** as your device.
   - Right click project name under the project tree and select properties, then under protect tick the support simulation during block compilation.
   - Navigate to **Device Configuration**. Locate the **Ethernet address** of the PLC and set it to `192.168.0.10`.
   - Navigate to **Program blocks** and open **OB1 (Main)**. This is the main program block where the Modbus server configuration will be set up.
   - Add the Modbus Server Instruction: On the right-hand side of TIA Portal, go to **Instructions**.
   - Navigate to the **Communication** tab, then **Others**, and find **Modbus TCP**.
   - Select **MB_SERVER** to add the Modbus server instruction to the program.
   - Choose **Automatic** when prompted, then click **OK** to confirm the configuration. A single DB instance will be created to saves the dta.
   - Create a new **Data Block** under the Program Blocks to store Modbus configuration parameters for smooth client-server communication.
   - The Main OB1 will have the function block created and below is the descriptions of the related parameters
      - **DISCONNECT**: Controls the connection to the Modbus server, allowing it to be established or terminated.
      - **MB_HOLD_REG**: Points to the Modbus holding register accessed by the Modbus client.
      - **NDR**: Indicates if new data has been written by the Modbus client (1 = new data, 0 = no new data).
      - **DR**: Shows if data has been read by the Modbus client (1 = data read, 0 = no data read).
      - **ERROR**: Sets to 1 if an error occurs during the MB_SERVER instruction call.
      - **STATUS**: Provides detailed status information about the MB_SERVER instruction.
      - **CONNECT**: Points to the connection structure (TCON_IPV4), defining the IP address for the server connection. 
  - We need to create the modbus parameters by creating the Modbus database instance as a new Data Block.
      - For the connect, setpoint has to be ticked.
      - For interface ID, it could be found in Device configuration > System constants. Here, 64 is the HW-identifier of IE-interface submodule.
      - The Local Port should be set to 502.
      - For ID should be started at 16#1
      - After putting the configuration, compile the data block. 
  - We also need to create another new Data Block that will contain the Modbus data. Simply create array to store DATA_1 and DATA_2. On the project tree,for Modbus Data DB, select properties and uncheck the optimize block access under the attributes pane. This will enable you to use absolute addressing for this data block. After putting the configuration, compile the data block. After compilation, the Modbus_parameters data block will have offset pane.
  - Finally, the Main OB1 block will have to be connected back to the Modbus parameter database. For the MB_Holding_Reg, it will be set to the absolute address structure P#DB3.DBX0.0 BYTE 62, which is Siemens's absolute addressing method. DB3 is the data block number of MODBUS DATA. DBX0.0 means that the starting data offset in DB3 0.0. BYTE 62 dictates the endpoint of our data which has the offset number 62.0

3. After setting up step 2, download the PLC program. For PG/PC interface, select Simeens PLCSIM Virtual Ethernet Adapter, click search. The address 192.168.0.10 should be shown and click Load to proceed. It may ask you to create additional IP address. Go ahead.

3. **In the Radzio Modbus Master Simulator**:

- Configure the IP address to 192.168.0.10, port 502. Read 


### Reference

https://www.solisplc.com/tutorials/modbus

https://www.solisplc.com/tutorials/modbus-tcp-communications-in-siemens-tia-portal
