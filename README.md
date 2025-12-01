# Unified Diagnostic Services (UDS) Protocol –

# Comprehensive Training Guide

# Table of Contents

### Executive Summary

### 1. Introduction to Automotive Diagnostics

### 2. Basics of Automotive Communication

### 3. UDS Protocol Fundamentals

### 4. Diagnostic Session Management

### 5. Security and Access Control

### 6. Reading and Writing Diagnostic Data

### 7. Diagnostic Trouble Codes (DTC)

### 8. Routine Control and ECU Operations

### 9. UDS Communication Protocols and Transport Layers

### 10. Advanced UDS Topics

### 11. Practical Hands-On Exercises

### Pseudocode for reading VIN via CAN/UDS

### 1. Open CAN bus connection

### 2. Create CAN IDs (OEM-specific, example values)

### 3. Read VIN using UDS 0x

### 4. Wait for response (with timeout)

### 5. Parse response

### 1. Read DTCs before clearing

### 2. Verify repair was completed (confirm with technician)

### 3. Perform ECU clear operation

### 4. Verify DTCs are cleared

### 1. Switch to Extended Diagnostic Session

### 2. Perform Security Access (Seed/Key)

### 3. Erase Memory

### Poll for completion

### 4. Load and prepare firmware

### 5. Request Download (initiate programming)

### 6. Transfer firmware data


### This training guide provides a comprehensive exploration of Unified Diagnostic Services (UDS) ,

### standardized under ISO 14229 , from beginner to expert levels. UDS is the de-facto vehicle

### diagnostics protocol used by virtually all modern automotive original equipment manufacturers

### (OEMs) and suppliers. It enables standardized communication between diagnostic testers (clients)

### and Electronic Control Units (ECUs) across the complete vehicle lifecycle—from development and

### manufacturing through service and field updates. This guide progresses systematically from

### foundational concepts to advanced implementation strategies, including practical examples, message

### formats, security considerations, and best practices for engineers, testers, and developers working in

### automotive diagnostics.

### Unified Diagnostic Services (UDS) is a standardized communication protocol that enables

### diagnostic interaction between external test equipment and vehicle Electronic Control Units (ECUs).

### UDS operates at the application layer (Layers 5–7 of the OSI model) and provides a comprehensive

### set of diagnostic services including:

### Unlike proprietary diagnostic protocols developed by individual manufacturers, UDS provides a

### unified, standardized approach that ensures interoperability across different vehicle models,

### manufacturers, and diagnostic tools. Modern vehicles contain anywhere from 50 to 150 ECUs

### managing systems such as engine control, transmission, brakes (ABS), stability control (ESP),

### infotainment, and safety systems (airbags, seat belts). Each of these ECUs requires reliable

### diagnostic capabilities, which UDS provides in a consistent, manufacturer-agnostic manner.

### 7. Request Transfer Exit

### 8. Verify with checksum routine

### 9. Reset ECU

### 10. Verify programming

### 12. Summary and Best Practices

### Appendices

### References and Further Learning

## Executive Summary

## 1. Introduction to Automotive Diagnostics

## 1.1 What is UDS?

### Reading and clearing fault codes (Diagnostic Trouble Codes - DTCs)

### Reading and writing vehicle data by identifiers

### Performing ECU reset and routine control operations

### Managing diagnostic sessions and security access

### Uploading and downloading ECU firmware for reprogramming


### Before the introduction of UDS, the automotive industry relied on multiple diagnostic protocols,

### creating significant fragmentation and compatibility issues:

### KWP 2000 (ISO 14230) : An earlier protocol based on K-Line communication, designed for OBD (On-

### Board Diagnostics) systems. While functional, KWP 2000 was limited in scope and had regional

### variations that complicated global vehicle support.

### OBD-II : The U.S. standard for on-board diagnostics, primarily focused on emissions systems. OBD-II

### used a limited set of standardized PIDs (Parameter Identifiers) but lacked the comprehensive

### diagnostic capabilities needed for modern vehicles.

### Proprietary Protocols : Individual OEMs developed custom diagnostic protocols, requiring separate

### tools and training for each manufacturer.

### The automotive industry recognized the need for a unified, comprehensive standard. In 2006, ISO

### released ISO 14229 (Unified Diagnostic Services), which:

### Today, UDS is the dominant global standard adopted by virtually all OEMs and Tier 1 suppliers,

### making it an essential skill for automotive engineers, technicians, and developers.

### Understanding UDS is critical for multiple roles in the automotive industry:

### For Embedded Systems Engineers : Implementing UDS in ECU firmware requires deep knowledge

### of the protocol, security mechanisms, and timing requirements. Engineers must design robust

### diagnostic communication stacks that handle multiple service requests reliably.

### For Diagnostic Tool Developers : Creating diagnostic testers, scan tools, and programming

### equipment requires comprehensive understanding of UDS message structures, flow control, and

### session management.

### For Quality and Test Engineers : Validating ECU behavior involves extensive use of UDS to read

### data, trigger routines, and verify fault detection. Testing teams use UDS to automate end-of-line (EOL)

### testing in manufacturing.

### For Field Service Technicians : Workshop technicians use UDS-based scan tools to diagnose

### vehicle problems, perform software updates, and reset warning lights. Understanding the underlying

### protocol improves troubleshooting effectiveness.

## 1.2 Historical Context: From KWP2000 to UDS

### Consolidated diagnostic services from KWP 2000, ISO 15765, and other protocols

### Provided a standardized service structure with Service Identifiers (SIDs)

### Supported multiple transport layers (CAN, LIN, FlexRay, Ethernet)

### Introduced advanced security mechanisms (Seed/Key authentication)

### Enabled flexible OEM-specific extensions while maintaining core compatibility

## 1.3 Why Learn UDS?


### For Entrepreneurial Opportunities : Building diagnostic middleware, ODX tools, and ECU

### reprogramming solutions for Tier 1 suppliers and OEMs represents significant business potential in the

### automotive diagnostics space.

### An Electronic Control Unit (ECU) is a specialized embedded computer that monitors and controls

### specific vehicle functions. Modern vehicles are essentially distributed computing systems , with

### multiple ECUs communicating over shared networks:

### Engine Control Module (ECM) : Controls fuel injection, ignition timing, emission systems, and engine

### performance optimization.

### Transmission Control Module (TCM) : Manages automatic transmission shifting, torque converter

### lockup, and shift quality.

### Anti-lock Braking System (ABS) and Electronic Stability Control (ESC) : Control brake pressure

### distribution and vehicle stability during emergency maneuvers.

### Body Control Module (BCM) : Manages lighting, power windows, locks, wipers, and comfort

### functions.

### Instrument Cluster : Displays vehicle information and warning indicators.

### Infotainment System : Manages audio, navigation, and connectivity features.

### A typical vehicle may contain 50–150 ECUs depending on complexity. These ECUs must exchange

### data reliably in real-time, requiring robust communication protocols.

### Multiple communication protocols coexist in modern vehicles, each optimized for specific use cases:

### CAN (Controller Area Network) : The most widely used vehicle network, operating at 250 kbps to 1

### Mbps. CAN is reliable, deterministic, and well-suited for real-time control and diagnostics. Most UDS

### implementations use CAN as the transport layer.

### LIN (Local Interconnect Network) : A lower-cost, lower-speed protocol (9.6 kbps to 20 kbps) used for

### less-critical systems like window controls and seat adjustments.

### FlexRay : A high-speed, deterministic protocol (10 Mbps) used in advanced vehicle platforms for

### critical systems like brake-by-wire and steer-by-wire.

### Automotive Ethernet : Increasingly adopted for high-bandwidth applications (100 Mbps to 1 Gbps)

### including ADAS, infotainment, and diagnostics in next-generation vehicles. UDS can run over Ethernet

### via DoIP (Diagnostics over Internet Protocol).

## 2. Basics of Automotive Communication

## 2.1 Electronic Control Units (ECUs) and Vehicle Networks

## 2.2 Communication Networks in Vehicles


### The OSI (Open Systems Interconnection) model provides a framework for understanding protocol

### layers:

```
OSI
Layer Name Automotive Example UDS Role
```
```
1 Physical CAN wiring, termination, voltage levels Physical signal transmission
```
```
2 Data Link CAN frame structure, arbitration, error
detection
```
```
Frame formatting
```
```
3 Network ISO-TP segmentation, flow control Message segmentation for long
payloads
```
```
4 Transport ISO-TP reassembly, sequence ordering Reliable multi-frame delivery
```
```
5 Session Session establishment, timeout management Diagnostic session control
```
```
6 Presentation Data encoding, compression Data format translation
```
```
7 Application UDS services (0x10, 0x22, 0x2E, etc.) Diagnostic services
```
### UDS operates primarily at Layers 5–7 (Session, Presentation, Application) , providing high-level

### diagnostic services independent of the underlying transport mechanism. This allows the same UDS

### services to work over CAN, LIN, Ethernet, or other transport layers.

### A typical diagnostic communication stack layers UDS on top of transport protocols:

#### ┌─────────────────────────────────────┐

```
│ UDS Services (0x10–0x3E) │ ← Application Layer
│ (Session Control, DTC, Data I/O) │
└──────────────────┬──────────────────┘
│
┌──────────────────┴──────────────────┐
│ Presentation / Encoding Layer │ ← Data formatting
│ (Encoding schemes, DID mapping) │
└──────────────────┬──────────────────┘
│
┌──────────────────┴──────────────────┐
│ ISO-TP Transport Layer │ ← Segmentation &amp; Flow Control
│ (Single frames, multi-frames, FC) │ (ISO 15765-2)
└──────────────────┬──────────────────┘
│
┌──────────────────┴──────────────────┐
│ CAN Data Link Layer │ ← CAN Frames (8/64 bytes)
│ (Frame structure, arbitration) │ (ISO 11898)
└──────────────────┬──────────────────┘
│
┌──────────────────┴──────────────────┐
│ CAN Physical Layer │ ← Voltage levels, connectors
```
## 2.3 The OSI Model in Automotive Context

## 2.4 How UDS Fits in the Protocol Stack


```
│ (Wiring, termination, signaling) │
└──────────────────────────────────────┘
```
### For UDS over Ethernet (DoIP) , the stack differs:

#### ┌─────────────────────────────────────┐

```
│ UDS Services (0x10–0x3E) │
└──────────────────┬──────────────────┘
│
┌──────────────────┴──────────────────┐
│ DoIP Gateway Layer (ISO 13400) │ ← Vehicle identification, routing
│ (VIN broadcast, TCP encapsulation) │
└──────────────────┬──────────────────┘
│
┌──────────────────┴──────────────────┐
│ TCP/UDP Transport │ ← TCP: Reliable delivery
│ (Reliable stream communication) │
└──────────────────┬──────────────────┘
│
┌──────────────────┴──────────────────┐
│ Ethernet Physical Layer │ ← 100Base-T, 1GbE
└──────────────────────────────────────┘
```
### UDS defines 27 standard services , each identified by a unique Service Identifier (SID) byte. These

### services organize diagnostic functionality into manageable categories:

### Session and Mode Management :

### Error/Fault Management :

### Data Access :

## 3. UDS Protocol Fundamentals

## 3.1 Overview of UDS Services

### 0x10 - Diagnostic Session Control : Switches ECU between Default, Programming, and

### Extended Diagnostic sessions

### 0x3E - Tester Present : Maintains an active diagnostic session by preventing timeout

### 0x19 - Read DTC Information : Retrieves stored fault codes (DTCs) from ECU memory

### 0x14 - Clear Diagnostic Information : Erases DTCs after repairs

### 0x85 - Control DTC Setting : Enables/disables DTC recording

### 0x22 - Read Data By Identifier : Reads vehicle data (e.g., VIN, sensor values, calibration data)

### 0x2E - Write Data By Identifier : Writes configuration data to ECU memory

### 0x2F - Input Output Control By Identifier : Controls outputs (e.g., relay control, actuator

### activation)


### Security :

### System Control :

### Routine Execution :

### Memory Operations :

### Advanced Services :

### Additional services exist for special functions like link control, access timing management, and OEM-

### specific extensions.

### UDS communication uses a Request-Response model where:

### All UDS request messages follow this basic structure:

#### ┌──────────────────────────────────────────────────────────────┐

```
│ Byte 1: Service ID (SID) │
│ Examples: 0x10, 0x22, 0x2E, 0x31, etc. │
└──────────────────────────────────────────────────────────────┘
│
┌──────────────────────────────────────────────────────────────┐
│ Byte 2 (Optional): Sub-Function Byte │
│ Used by services like 0x10, 0x11, 0x27, 0x31, etc. │
│ Specifies variant of the service (e.g., session type, etc.) │
```
### 0x24 - Read Scaling Information : Retrieves data encoding and scaling parameters

### 0x27 - Security Access : Implements Seed/Key authentication to restrict access to sensitive

### operations

### 0x11 - ECU Reset : Resets the ECU (Hard Reset, Key OFF-ON, Soft Reset)

### 0x28 - Communication Control : Enables/disables message transmission on the network

### 0x31 - Routine Control : Executes ECU routines (self-tests, calibration, flash memory erase)

### 0x34 - Request Download : Initiates firmware download for ECU reprogramming

### 0x35 - Request Upload : Prepares to read large data blocks from ECU

### 0x36 - Transfer Data : Transfers data blocks during download/upload operations

### 0x37 - Request Transfer Exit : Terminates a data transfer session

### 0x38 - Request File Transfer : Manages file-based operations (optional)

### 0x3D - Write Memory By Address : Writes data to specific memory addresses

### 0x23 - Read Memory By Address : Reads data from specific memory addresses

## 3.2 UDS Message Structure and Format

### 1. Tester (Client) sends a Request Message to the ECU

### 2. ECU (Server) processes the request and sends back a Response Message

## Request Message Format


#### └──────────────────────────────────────────────────────────────┘

#### │

#### ┌──────────────────────────────────────────────────────────────┐

```
│ Bytes 3-N (Optional): Service Parameters │
│ Data Identifiers (DIDs), addresses, routine IDs, etc. │
│ Format depends on specific service definition │
└──────────────────────────────────────────────────────────────┘
```
### Example: Read Data By Identifier (0x22) Request

### Request to read VIN (Data Identifier 0xF190):

```
Byte 1: 0x22 (Service ID - Read Data By Identifier)
Byte 2: 0xF1 (DID High Byte)
Byte 3: 0x90 (DID Low Byte)
```
```
Complete Request: [0x22 0xF1 0x90]
```
### Responses fall into two categories:

### Positive Response : Indicates the request was successfully processed.

#### ┌──────────────────────────────────────────────────────────────┐

```
│ Byte 1: Positive Response SID (Original SID + 0x40) │
│ Example: If request was 0x22, response is 0x22 + 0x40 = 0x62│
└──────────────────────────────────────────────────────────────┘
│
┌──────────────────────────────────────────────────────────────┐
│ Bytes 2-N (Optional): Response Data │
│ Format depends on service (data values, status info, etc.) │
└──────────────────────────────────────────────────────────────┘
```
### Example: Positive Response to 0x22 (Read DID 0xF190)

```
Byte 1: 0x62 (Positive Response: 0x22 + 0x40 = 0x62)
Byte 2: 0x17 (VIN Length: 17 characters)
Bytes 3-19: [ASCII characters of VIN]
```
```
Complete Response: [0x62 0x57 0x56 0x57 ... ]
(W V W ...) ← VIN characters in ASCII
```
### Negative Response : Indicates the request could not be processed.

#### ┌──────────────────────────────────────────────────────────────┐

```
│ Byte 1: Negative Response SID (Always 0x7F) │
└──────────────────────────────────────────────────────────────┘
│
┌──────────────────────────────────────────────────────────────┐
```
## Response Messages


```
│ Byte 2: Original Service ID (Echo of requested service) │
│ Example: 0x22 (the service that was requested) │
└──────────────────────────────────────────────────────────────┘
│
┌──────────────────────────────────────────────────────────────┐
│ Byte 3: Negative Response Code (NRC) │
│ Indicates why the request failed (see NRC table) │
└──────────────────────────────────────────────────────────────┘
```
### Example: Negative Response

```
Byte 1: 0x7F (Negative Response Indicator)
Byte 2: 0x22 (Original Service: 0x22 - Read DID)
Byte 3: 0x31 (NRC: Request Out Of Range)
```
```
Complete Response: [0x7F 0x22 0x31]
Meaning: "The requested DID (0xF190) is not valid or out of range"
```
### The relationship between request SID and positive response SID is deterministic :

### Positive Response SID = Request SID + 0x

```
Service Request SID Positive Response SID Negative Response SID
```
```
Diagnostic Session Control 0x10 0x50 0x7F
```
```
ECU Reset 0x11 0x51 0x7F
```
```
Read Data By Identifier 0x22 0x62 0x7F
```
```
Write Data By Identifier 0x2E 0x6E 0x7F
```
```
Routine Control 0x31 0x71 0x7F
```
### Negative responses always use SID 0x7F regardless of the requested service.

### The ECU organizes diagnostic capabilities into distinct sessions , each providing different levels of

### access and functionality. Sessions act as security gates , ensuring that only appropriate diagnostic

### operations are performed in each state.

### Default Session (0x01)

### The Default Session is the normal operational state of the ECU. Upon power-up, the ECU always

### enters Default Session.

### Characteristics:

## 3.3 Service Identifier (SID) and Response Calculation

## 4. Diagnostic Session Management

## 4.1 Diagnostic Session Types


### Typical use cases:

### Extended Diagnostic Session (0x03)

### Extended Diagnostic Session provides enhanced diagnostic access for comprehensive vehicle

### testing.

### Characteristics:

### Typical use cases:

### Programming Session (0x10)

### Programming Session is used exclusively for ECU reprogramming and firmware updates.

### Characteristics:

### Limited diagnostic access : Only non-intrusive services are available (reading DTCs, reading

### non-sensitive data)

### Normal vehicle operation : The vehicle continues normal operation; engine runs, transmission

### controls work, etc.

### No security access required : DIDs can be read without Seed/Key authentication

### Commercial vehicle prioritized : Designed for service stations where technicians diagnose

### without disabling vehicle operation

### Typical P2 timeout : 50–100 ms (standard response time)

### Initial vehicle connection to diagnose problems

### Reading current fault codes

### Reading sensor values during vehicle operation

### Preliminary diagnosis before deeper testing

### Expanded service availability : Additional diagnostic services become available (extended DTC

### information, specialized routines)

### Vehicle behavior modification : Sensor values may be read more aggressively; normal vehicle

### operation may be partially suspended

### Security level increases : Some DIDs now require Seed/Key authentication

### Extended P2 timing : Up to 500 ms or more (ECU has more time to gather complex diagnostic

### data)

### Manual intervention control : Technicians can activate outputs (lights, relays) for testing

### Performing comprehensive vehicle diagnostics

### Testing vehicle systems (electrical, fuel, ignition)

### Reading extended DTC information and environment data

### Controlling outputs for manual verification (e.g., activating fuel pump relay to test fuel pressure)

### ECU reprogramming available : Transfer Data (0x36) and Routine Control (0x31) services

### enabled for flashing


### Typical use cases:

### Safety System Diagnostic Session (0x04)

### Some OEMs define Safety System Diagnostic Session for testing safety-critical systems.

### Characteristics:

### Diagnostic sessions follow a hierarchical state machine :

#### ┌─────────────────────────────────────────────────────┐

```
│ ECU Power-Up or Reset │
└──────────────────────┬────────────────────────────────┘
│
▼
┌────────────────────┐
│ DEFAULT SESSION │ ◄─── Always starts here
│ (SID = 0x01) │
└────────┬───────────┘
│
┌───────────┼───────────┐
│ │ │
▼ ▼ ▼
┌─────────┐ ┌──────────┐ ┌────────────┐
│Extended │ │Programming│ │Safety System│
│Diagnostic│ │ Session │ │ Diagnostic │
│ Session │ │ (0x10) │ │ (0x04) │
│ (0x03) │ │ │ │ │
└────┬────┘ └────┬─────┘ └──────┬─────┘
```
### Most restrictive session : Normal vehicle operation suspended; many services unavailable

### Highest security : Seed/Key authentication required for sensitive operations

### Extended response times : P2* can be seconds or minutes during complex operations like

### memory erase or flash programming

### No normal communications : Standard CAN messages suspended; ECU only responds to

### diagnostic requests

### Uploading new firmware to the ECU

### Updating calibration data

### Erasing and reprogramming flash memory

### Resetting learned values

### Safety system focus : Tests airbags, braking systems, seatbelt systems

### Highest security protocols : Enhanced Seed/Key requirements

### Controlled operations : Safety operations performed in strictly controlled manner to prevent

### accidents

### Validation and authorization : Typically requires technician certification

## 4.2 Session Switching and Flow


#### │ │ │

#### └────────┬───┴─────┬───────┘

#### │ │

#### ▼ ▼

#### ┌────────────────────────┐

```
│ Timeout (S3 Timer) │
│ typically 5-10 sec │
│ without Tester │
│ Present (0x3E) │
└────────────┬───────────┘
│
▼
┌────────────────────┐
│ DEFAULT SESSION │ ◄─── Auto-return for safety
│ (SID = 0x01) │
└────────────────────┘
```
### Session Switching Request and Response

```
Tester Request (switch to Extended Diagnostic):
[0x10 0x03]
Byte 1: 0x10 = Diagnostic Session Control
Byte 2: 0x03 = Extended Diagnostic Session sub-function
```
```
ECU Positive Response:
[0x50 0x03 P2_high P2_low P2*_high P2*_low]
Byte 1: 0x50 = Positive response
Byte 2: 0x03 = Echo of requested session
Bytes 3-6: Timing parameters (P2, P2* values in milliseconds)
```
### Proper timing management is critical for reliable UDS communication. Timing parameters define how

### long the tester waits for responses and when sessions expire.

### P2 (Standard Response Time)

### Definition : The maximum time the tester waits for a standard positive or negative response from the

### ECU after sending a request.

### Example P2 Timing :

```
Time (ms)
0: Tester sends: [0x22 0xF1 0x90] (Read DID)
│
├─ 50 ms: ECU processing...
```
## 4.3 Timing Parameters: P2, P2*, and Session Timeouts

### Typical value : 50–100 ms in Default Session

### Extended Session : Up to 500 ms (ECU may need time to gather complex diagnostic data)

### When applied : For requests that the ECU can process quickly without lengthy computations

### If exceeded : Tester assumes communication failure and may retry or timeout


#### │

```
50: ◄── P2 timeout threshold
│
│ ECU responds: [0x62 0x17 ... VIN DATA ...] ◄── Received within P
│
100: Tester processes response
```
### P2 (Extended Response Time) *

### Definition : An extended wait time used when the ECU responds with NRC 0x78 (Response

### Pending) , indicating it needs more time to complete the request.

### Example P2 Timing *:

```
Time (ms)
0: Tester sends: [0x19 0x02 ... ] (Read DTC)
│
├─ 50 ms: ECU processing...
│
50: ◄── P2 timeout threshold
│
60: ECU responds: [0x7F 0x19 0x78] (NRC 0x78 - Response Pending)
│ This tells tester: "Restart P2* timer, I need more time"
│
├─ ... DTC collection continues ...
│
3000: ◄── P2* timeout threshold (assume 3000 ms)
│
3050: ECU responds: [0x59 ... DTC DATA ...] (Success!)
```
### S3 (Session Timeout)

### Definition : The session inactivity timeout. If the tester doesn't send any request (including Tester

### Present) for the S3 duration, the ECU automatically returns to Default Session.

### Example S3 Timeout :

### Typical value : 1000–5000 ms (1–5 seconds) or longer

### When used : Complex operations like routine execution, DTC reading, or memory operations

### Mechanism : ECU sends NRC 0x78 to signal tester: "Wait longer, I'm still processing"

### If exceeded : Tester timeouts and considers the operation failed

### Typical value : 5–10 seconds

### Purpose : Safety mechanism to ensure ECU doesn't remain in a privileged session indefinitely if

### tester becomes unresponsive

### Reset mechanism : Sending Tester Present (0x3E) resets the S3 timer, keeping the session

### active

### Prevents unintended operations : If technician forgets diagnostic tool connected, ECU

### eventually returns to safe state


```
Time (sec)
0: Tester switches to Extended Diagnostic Session
│ S3 timer starts (assume 5-sec timeout)
│
5: ◄── S3 timeout threshold
│
│ If NO requests in 5 seconds:
│ ECU auto-returns to Default Session
│
│ To maintain session, tester must send:
│ [0x3E 0x80] (Tester Present with sub-function 0x80)
```
### Security is paramount in automotive diagnostics. Unauthorized ECU access could lead to vehicle

### malfunction, emissions violations, or safety hazards. UDS implements security through the

### Seed/Key mechanism , which prevents unauthorized access to sensitive operations.

### Security Levels

### Each ECU defines multiple Security Levels that control access to specific services:

### Seed/Key Authentication Flow

### The Seed/Key mechanism works as follows:

#### ┌──────────────────────────────────────────────────────────┐

```
│ Step 1: Tester Requests Seed (Security Level 0x03) │
├──────────────────────────────────────────────────────────┤
│ Request: [0x27 0x01] (Service 0x27, Sub-fn 0x01 = Request Seed) │
│ Security Level: 0x01 (must be ODD number for Request Seed) │
└──────────────────────────────────┬───────────────────────┘
│
┌──────────────────────────────────┴───────────────────────┐
│ Step 2: ECU Sends Seed │
├──────────────────────────────────────────────────────────┤
│ Response: [0x67 0x04 &lt;seed_bytes&gt;] │
│ 0x67 = Positive Response (0x27 + 0x40) │
│ 0x04 = Seed length (4 bytes in this example) │
│ &lt;seed&gt; = Random seed value generated by ECU │
└──────────────────────────────────┬───────────────────────┘
│
│ Tester receives seed and uses LOCAL encryption algorithm │
```
## 5. Security and Access Control

## 5.1 Security Access Service (0x27) and Seed/Key Mechanism

### Security Level 0x01 : Read-only DID access (publicly available data like VIN, odometer)

### Security Level 0x03 : Write access to configuration data (requires Seed/Key)

### Security Level 0x05 : Routine control and memory operations (requires Seed/Key)

### Security Level 0x07 : Flash reprogramming (requires Seed/Key with highest priority)


```
│ to generate KEY from seed (algorithm is OEM-specific, │
│ must be known by tester tool provider) │
│
┌──────────────────────────────────┴───────────────────────┐
│ Step 3: Tester Sends Key (Security Level 0x02) │
├──────────────────────────────────────────────────────────┤
│ Request: [0x27 0x02 &lt;key_bytes&gt;] │
│ 0x27 = Service (Security Access) │
│ 0x02 = Sub-fn (Send Key = Request Seed + 1) │
│ &lt;key&gt; = Calculated key from seed │
└──────────────────────────────────┬───────────────────────┘
│
┌──────────────────────────────────┴───────────────────────┐
│ Step 4: ECU Validates Key │
├──────────────────────────────────────────────────────────┤
│ ECU calculates same KEY using same algorithm and seed │
│ If tester key == ECU key: UNLOCK! (can now access │
│ restricted operations) │
│ If keys don't match: Return NRC 0x35 (Invalid Key) │
│ NRC 0x36 (Exceed Attempts) │
└──────────────────────────────────────────────────────────┘
```
### Practical Example: 8-Bit XOR Seed/Key Algorithm

### This is a simplified example. Real OEM algorithms are much more complex:

```
Seed received: 0x12 0x34 0x56 0x
```
```
Tester calculates key using XOR with master key:
Master Key (OEM-specific): 0x55 0x55 0x55 0x
```
```
Key = Seed XOR Master Key
0x12 XOR 0x55 = 0x
0x34 XOR 0x55 = 0x
0x56 XOR 0x55 = 0x
0x78 XOR 0x55 = 0x2D
```
```
Tester sends Key: [0x27 0x02 0x47 0x61 0x03 0x2D]
```
```
ECU verifies by calculating same operation and comparing.
```
### Real OEM algorithms often use:

### Advanced XOR operations with bit shifting and rotation

### AES encryption (symmetric encryption)

### RSA encryption (asymmetric encryption)

### XTEA algorithm (commonly seen in automotive)

### Combined approaches with multiple transformations


### RequestSeed and SendKey Subfunctions

### UDS uses an elegant mechanism where Seed/Key levels follow a pattern:

```
Example: Unlock Security Level 0x
├─ RequestSeed: 0x27 0x01 (0x01 is odd) ◄── First request
├─ ECU responds with seed
└─ SendKey: 0x27 0x02 (0x02 = 0x01 + 1) ◄── Send matching key
```
```
Example: Unlock Security Level 0x
├─ RequestSeed: 0x27 0x05 (0x05 is odd)
├─ ECU responds with seed
└─ SendKey: 0x27 0x06 (0x06 = 0x05 + 1)
```
### Security Attempt Limits

### To prevent brute-force attacks, ECUs implement:

### Data Identifiers (DIDs) are 2-byte values that uniquely identify specific pieces of data within an ECU.

### DIDs provide a standardized way to reference vehicle data without needing direct memory addresses.

### DID Structure

### DIDs are 16-bit identifiers (0x0000 to 0xFFFF) organized into ranges by purpose:

```
DID Range Purpose Examples
```
```
0xF100–0xF18E Identification Data VIN, ECU ID, Software Version
```
```
0xF190–0xF19E Network Configuration Vehicle Speed, Odometer Reading
```
```
0xF1A0–0xF1EF Engine Data Engine Speed, Temperature, Fuel Pressure
```
```
0xF1F0–0xF1FE Diagnostic Data DTC Counter, Test Status
```
```
0xF200–0xF3FF Periodically Transmitted Data Real-time sensor values
```
## 5.2 Security Levels and Access Control

### RequestSeed subfunction = Odd number (0x01, 0x03, 0x05, 0x07, etc.)

### SendKey subfunction = Odd number + 1 (0x02, 0x04, 0x06, 0x08, etc.)

### Maximum Failed Attempts : Typically 3 consecutive failed key attempts

### Lockout Duration : After exceeding max attempts, security is locked for a time period (e.g., 30

### seconds)

### NRC 0x36 : Returned when max attempts exceeded (requestSequenceError)

### NRC 0x35 : Returned when incorrect key sent (invalidKey)

## 6. Reading and Writing Diagnostic Data

## 6.1 Data Identifiers (DIDs)


```
DID Range Purpose Examples
```
```
0xF400–0xF4FF Dynamically Defined Data Manufacturer-specific data
```
```
0xF500–0xFEFF Reserved/Extended Future use, OEM-specific
```
```
0xFF00–0xFFFF Custom/Manufacturer-specific OEM proprietary identifiers
```
### Standard DIDs (Common Across OEMs)

```
0xF186: Active Diagnostic Session (1 byte)
Value: 0x01 = Default, 0x03 = Extended, 0x10 = Programming
```
```
0xF187: Vehicle Manufacturer ECU Hardware Version Number
0xF188: Vehicle Manufacturer ECU Software Version Number
0xF189: Vehicle Manufacturer ECU Software Version Number
0xF18A: System Supplier Specific ECU Hardware Version Number
0xF18B: System Supplier Specific ECU Software Version Number
0xF18C: ECU Calibration Identification Number
0xF18E: ECU Serial Number
```
```
0xF190: Vehicle Identification Number (VIN) - 17 bytes
0xF1A0: Vehicle Manufacturer Kit Assembly Part Number
0xF1B0: System Supplier Specific ECU Hardware Version Number
0xF1F0: Diagnostic Data Identifier
0xF1F1: ODX File Identifier
```
### The Read Data By Identifier service allows the tester to retrieve vehicle data from the ECU.

### Request Format

```
Byte 1: 0x22 (Service ID)
Bytes 2-N: Data Identifier(s) - 2 bytes each
Can request multiple DIDs in one message (if ECU supports)
```
```
Example: Read single DID
Request: [0x22 0xF1 0x90]
0x22 = Read DID service
0xF1 0x90 = DID for VIN
```
```
Example: Read multiple DIDs
Request: [0x22 0xF1 0x90 0xF1 0x87 0xF1 0x88]
0x22 = Read DID service
0xF1 0x90 = DID 1 (VIN)
0xF1 0x87 = DID 2 (Hardware Version)
0xF1 0x88 = DID 3 (Software Version)
```
### Response Format

```
Byte 1: 0x62 (Positive Response: 0x22 + 0x40)
Bytes 2-N: Response Data
```
## 6.2 Read Data By Identifier (0x22)


```
Format depends on specific DIDs
May include DID echo + data
```
```
Example Response (read VIN):
[0x62 0xF1 0x90 ... 17 bytes of VIN data ...]
0x62 = Positive response
0xF1 0x90 = Echoed DID
Remaining bytes = VIN characters in ASCII
```
```
Example Response (read multiple DIDs):
[0x62 0xF1 0x90 &lt;VIN_17_bytes&gt; 0xF1 0x87 &lt;HW_VERSION&gt; 0xF1 0x88 &lt;SW_VERSIO
```
### Negative Response Reasons

```
0x7F 0x22 0x22 = Conditions Not Correct (e.g., security not unlocked)
0x7F 0x22 0x31 = Request Out Of Range (DID not valid)
0x7F 0x22 0x33 = Security Access Denied (need to unlock first)
```
### The Write Data By Identifier service allows writing configuration data and calibration values to the

### ECU.

### Request Format

```
Byte 1: 0x2E (Service ID)
Bytes 2-3: Data Identifier (DID) - 2 bytes
Bytes 4-N: Data To Write
Format and length depend on specific DID
```
```
Example: Write VIN
Request: [0x2E 0xF1 0x90 &lt;17_bytes_of_new_VIN&gt;]
0x2E = Write DID service
0xF1 0x90 = DID (VIN)
Bytes 4-20 = New VIN (17 ASCII characters)
```
### Response Format

```
Byte 1: 0x6E (Positive Response: 0x2E + 0x40)
Bytes 2-3: Echoed DID
Bytes 4-N: Echo of written data (optional)
```
```
Example Response:
[0x6E 0xF1 0x90 &lt;echo_of_17_byte_VIN&gt;]
0x6E = Positive response
0xF1 0x90 = Echoed DID
Remaining = Echo of data written
```
### Important Considerations

## 6.3 Write Data By Identifier (0x2E)


### A Diagnostic Trouble Code (DTC) is a standardized fault code stored in the ECU when a malfunction

### is detected. DTCs enable technicians to quickly identify vehicle problems.

### DTC Structure

### DTCs are standardized under ISO 14229 and follow a 3-byte format:

#### ┌────────────────────────┐

```
│ High Byte (byte 1) │ ← Standard codes (mandated by regulations)
│ 0x01–0x0F = Powertrain│
│ 0x01–0x02 = Chassis │
│ 0x01–0x03 = Body │
│ 0x01–0x04 = Network │
└────────────────────────┘
│
┌────────┴────────────────┐
│ Middle Byte (byte 2) │ ← Subsystem and failure type
│ (Function or subsystem)│
└────────────────────────┘
│
┌────────┴────────────────┐
│ Low Byte (byte 3) │ ← Specific failure condition
│ (Failure condition) │
└────────────────────────┘
```
### Example DTC: 0x0110 (Engine Misfire, Cylinder 1)

### DTC Status Flags

### Each DTC includes status information:

```
Bit 0 (testFailed): Current test result failure
Bit 1 (testFailedThisCycle): Failure occurred during current test cycle
```
### Not all DIDs are writable : Most DIDs are read-only. Attempting to write read-only DIDs returns

### NRC 0x22 (Conditions Not Correct)

### Security may be required : Write operations typically require Seed/Key authentication first

### Permanent storage : Written values usually stored in non-volatile memory (flash), persisting

### across power cycles

### Validation : Some DIDs (like VIN) have format validation; invalid data returns error

## 7. Diagnostic Trouble Codes (DTC)

## 7.1 Understanding DTCs

### High Byte 0x01 : Powertrain system

### Middle Byte 0x01 : Cylinder/ignition system

### Low Byte 0x00 : Condition 00 = General misfire


```
Bit 2 (pendingDTC): Test failed but not confirmed yet
Bit 3 (confirmedDTC): Test failed multiple times (DTC confirmed)
Bit 4 (testNotCompletedThisCycle): Diagnostic test not run this cycle
Bit 5 (testNotCompletedSinceDTCCleared): Test never run since DTC clear
Bit 6 (warningIndicatorRequested): MIL (Malfunction Indicator Lamp) illuminated
Bit 7 (reserved): Reserved for future use
```
### DTC Classes

#### ┌─────────────────────────────────────┐

#### │ PENDING DTC │

#### ├─────────────────────────────────────┤

```
│ • Fault detected but not confirmed │
│ • Status flags: 0x01 (testFailed) │
│ • MIL not illuminated │
│ • May clear if fault is intermittent│
│ • Disappears after 40 warm-up cycles│
│ without re-occurrence │
└─────────────────────────────────────┘
```
```
┌─────────────────────────────────────┐
│ CONFIRMED DTC │
├─────────────────────────────────────┤
│ • Fault confirmed (occurred multiple│
│ times or consistently) │
│ • Status flags include 0x08 │
│ (confirmedDTC) │
│ • MIL illuminated (driver see light)│
│ • Stored permanently until cleared │
│ • Clears only when technician uses │
│ 0x14 (Clear DTC) service │
└─────────────────────────────────────┘
```
```
┌─────────────────────────────────────┐
│ PERMANENT DTC │
├─────────────────────────────────────┤
│ • Most serious: Emissions/safety │
│ • Stored in permanent memory │
│ • Cannot be cleared with 0x14 │
│ • Only clears after successful fix │
│ validated through drive cycle │
│ • Complies with OBD-II regulation │
└─────────────────────────────────────┘
```
### The Read DTC Information service retrieves fault codes from the ECU.

### Request Format

```
Byte 1: 0x19 (Service ID)
Byte 2: Sub-Function - Specifies what DTC data to retrieve
```
## 7.2 Reading DTCs: ReadDTCInformation (0x19)


```
0x01 = Read DTC By Status Mask
Returns DTCs matching specific status criteria
0x02 = Read DTC Snapshot Identification
Returns snapshot data from time of fault
0x03 = Read DTC Extended Data Identification
Returns extended fault information (sensor values at fault time)
0x04 = Read Supported DTC
Lists all DTCs the ECU can generate
0x05 = Read DTC by Fault Counter
Returns DTCs sorted by occurrence count
0x06 = Read DTC By Fault Counter and Status
Combined DTC info with status
0x07 = Read DTC Permanently
Returns only permanent DTCs
```
### Practical Example: Read All Confirmed DTCs

```
Request:
[0x19 0x01 0x08]
0x19 = Read DTC service
0x01 = Read DTC By Status Mask
0x08 = Status mask: bit 3 = confirmed DTC
```
```
Response (example):
[0x59 0x01 0x08]
0x59 = Positive response (0x19 + 0x40)
0x01 = Echo sub-function
0x08 = Echo status mask
[... DTC data ...]
0x11 0x00 0x08 = DTC 0x110008, Status 0x08 (confirmed)
0x21 0x03 0x08 = DTC 0x210308, Status 0x08 (confirmed)
0x43 0x05 0x08 = DTC 0x430508, Status 0x08 (confirmed)
```
### The Clear Diagnostic Information service erases DTCs from ECU memory after repairs.

### Request Format

```
Byte 1: 0x14 (Service ID)
Bytes 2-4: Group of DTC (3 bytes)
Identifies which DTCs to clear
```
```
0xFF 0xFF 0xFF = Clear ALL DTCs (most common)
0xFF 0xFF 0x00 = Clear powertrain DTCs
0xFF 0xFF 0x01 = Clear chassis DTCs
0xFF 0xFF 0x02 = Clear body DTCs
0xFF 0xFF 0x03 = Clear network DTCs
```
### Practical Example: Clear All DTCs

## 7.3 Clearing DTCs: ClearDiagnosticInformation (0x14)


```
Request:
[0x14 0xFF 0xFF 0xFF]
0x14 = Clear Diagnostic Information
0xFF 0xFF 0xFF = Clear all DTCs
```
```
Response:
[0x54] (Positive response only, no data)
0x54 = Positive response (0x14 + 0x40)
(No additional data in response)
```
### Important Notes on Clearing

### The Routine Control service executes ECU-specific diagnostic routines, tests, and operations.

### Request Format

```
Byte 1: 0x31 (Service ID)
Byte 2: Routine Control Type (Sub-Function)
0x01 = Start Routine
0x02 = Stop Routine
0x03 = Request Routine Result
Bytes 3-4: Routine Identifier (RID) - Identifies specific routine
Bytes 5-N: Routine Control Option Record (optional)
Parameters specific to the routine
```
### Common Routine Identifiers

```
RID 0x01 0x01 = Check Memory
Verifies ECU memory integrity before flashing
```
```
RID 0x01 0x02 = Check Memory with Addresses
Verifies specific memory addresses
```
### Post-Repair Verification : Only clear DTCs after verifying the repair. Clearing confirmed DTCs

### hides problems

### Pending DTCs : Generally safe to clear (not confirmed defects)

### Security Requirements : Clearing DTCs often requires Extended or Programming Session +

### Seed/Key authentication

### Permanent DTCs : Not cleared by 0x14; only clear when fault is resolved and drive cycles

### complete

### Technician Responsibility : Clearing without fixing root cause masks problems and may cause

### safety issues

## 8. Routine Control and ECU Operations

## 8.1 Routine Control Service (0x31)


```
RID 0x01 0xFF = Erase Memory
Clears flash memory for new programming
```
```
RID 0x02 0x01 = Read DTC From Memory
Retrieves DTCs for validation
```
```
RID 0x03 0x01 = Self-Test
Executes ECU self-diagnostic test
```
```
RID 0x04 0x01 = Fuel System Self-Test
Tests fuel pump, injectors
```
```
RID 0x10 0x01 = Software Checksum Verification
Validates software integrity
```
### Example: Erase Memory Routine

```
Request - Start Erase Routine:
[0x31 0x01 0x01 0xFF]
0x31 = Routine Control
0x01 = Start Routine
0x01 0xFF = RID 0x01FF (Erase Memory)
```
```
Response - Routine Started:
[0x71 0x01 0x01 0xFF]
0x71 = Positive response
0x01 = Echo start request
0x01 0xFF = Echo RID
```
```
Tester waits (memory erase takes time - send Tester Present 0x3E repeatedly)
```
```
Request - Poll for Completion:
[0x31 0x03 0x01 0xFF]
0x31 = Routine Control
0x03 = Request Routine Result
0x01 0xFF = RID 0x01FF
```
```
Response - Routine Complete:
[0x71 0x03 0x01 0xFF 0x01]
0x71 = Positive response
0x03 = Echo result request
0x01 0xFF = Echo RID
0x01 = Status: 0x01 = Successful completion
```
### The ECU Reset service restarts the ECU with different reset types.

### Reset Types

```
Byte 1: 0x11 (Service ID)
Byte 2: Reset Type
```
## 8.2 ECU Reset Service (0x11)


```
0x01 = Hard Reset
Simulates power loss and restart
Reinitializes ALL hardware and software
Takes longest time
Used when ECU is hanging/non-responsive
```
```
0x02 = Key OFF-ON Reset
Simulates ignition off/on sequence
Similar to hard reset but cleaner
Used after firmware updates
```
```
0x03 = Soft Reset
Minimal restart
Skips hardware initialization
Restarts only the application software
Fastest reset type
Used during diagnostics to recover from error
```
```
0x04 = Enable Rapid Power Shut Down
Powers off ECU (if supported)
Reserved for special applications
```
```
0x05 = Disable Rapid Power Shut Down
Re-enables normal power control
Reserved for special applications
```
### ECU Reset Sequence

```
Request - Soft Reset:
[0x11 0x03]
0x11 = ECU Reset
0x03 = Soft Reset
```
```
Response - Positive Before Reset:
[0x51 0x03]
0x51 = Positive response (0x11 + 0x40)
0x03 = Echo reset type
(Response sent BEFORE reset occurs)
```
```
Then:
┌─────────────────────────────────────┐
│ ECU performs soft reset │
│ Application software reinitializes │
│ Returns to Default Session │
│ Re-establishes communication │
└─────────────────────────────────────┘
```
```
Tester receives positive response, then must re-establish connection
```

### CAN (Controller Area Network) is the most common vehicle network. CAN frames are limited to 8

### bytes of payload. UDS messages often exceed this limit, requiring ISO-TP (ISO 15765-2) for

### segmentation and reassembly.

### ISO-TP Protocol Control Information (PCI)

### ISO-TP adds metadata (PCI) to CAN frames to manage multi-frame communication:

#### ┌─────────────────────────────────────────────────────┐

```
│ Single Frame (SF): │
│ Used when payload fits in one CAN frame │
├─────────────────────────────────────────────────────┤
│ Byte 1: PCI = 0x0N (N = data length, 1-7 bytes) │
│ Bytes 2-8: UDS message data │
├─────────────────────────────────────────────────────┤
│ Example: Read VIN (3 bytes of data) │
│ [0x02 0x22 0xF1 0x90 0x00 0x00 0x00 0x00] │
│ 0x02 = PCI (SF, 2 bytes of data) │
│ [0x22 0xF1 0x90] = UDS Read DID message │
└─────────────────────────────────────────────────────┘
```
```
┌─────────────────────────────────────────────────────┐
│ First Frame (FF): │
│ Starts multi-frame transmission │
├─────────────────────────────────────────────────────┤
│ Bytes 1-2: PCI = 0x1N NN (N = total data length) │
│ Bytes 3-8: First 6 bytes of UDS message │
├─────────────────────────────────────────────────────┤
│ Example: 100-byte message (large firmware block) │
│ [0x10 0x64 0x36 0x00 0x12 0x34 0x56 0x78] │
│ 0x10 0x64 = PCI (FF, 100 bytes total) │
│ [0x36 0x00...] = First 6 bytes of 0x36 message │
└─────────────────────────────────────────────────────┘
```
```
┌─────────────────────────────────────────────────────┐
│ Consecutive Frame (CF): │
│ Continues multi-frame transmission │
├─────────────────────────────────────────────────────┤
│ Byte 1: PCI = 0x2N (N = sequence number 0-15) │
│ Bytes 2-8: Next 7 bytes of UDS message │
├─────────────────────────────────────────────────────┤
│ Multiple CFs sent with incrementing sequence numbers│
│ Sequence wraps: 0, 1, 2, ..., F, 0, 1, ... │
└─────────────────────────────────────────────────────┘
```
```
┌─────────────────────────────────────────────────────┐
│ Flow Control (FC): │
│ Manages transmission rate and buffer │
├─────────────────────────────────────────────────────┤
│ Byte 1: PCI = 0x3F │
```
## 9. UDS Communication Protocols and Transport Layers

## 9.1 UDS Over CAN: ISO-TP (ISO 15765-2)


```
│ Byte 2: Flags: 0x00 = Continue Sending (CS) │
│ 0x01 = Wait/Pause (WT) │
│ 0x02 = Overflow Error (OF) │
│ Byte 3: Block Size (BS): │
│ Number of consecutive frames before │
│ waiting for next FC │
│ 0x00 = No limit │
│ 0x01–0xFF = Send this many CFs, then wait │
│ Byte 4: Separation Time (STmin): │
│ Minimum delay (ms) between consecutive │
│ frames. Allows ECU processing time. │
│ 0x00-0x7F = delay in milliseconds │
│ 0xF1-0xFE = delay in microseconds │
├─────────────────────────────────────────────────────┤
│ Example: [0x30 0x00 0x00] │
│ 0x30 = FC (Continue Sending) │
│ 0x00 = BS (Block size 0 = no limit) │
│ 0x00 = STmin (no delay between frames) │
└─────────────────────────────────────────────────────┘
```
### Multi-Frame Communication Example

```
Tester sends large 50-byte firmware block via Transfer Data (0x36):
```
```
CAN Frame 1 (First Frame - FF):
┌─────────────────────────────────────┐
│ ID: 0x641 (Tester→ECU) │
│ Data: [0x10 0x32 0x36 0x00 0xAA ... │ ← FF with 50 bytes total
│ (0x10 0x32 = FF with 0x32 │
│ bytes = 50 bytes) │
│ [0x36 0x00] + 6 data bytes │
└─────────────────────────────────────┘
```
```
ECU receives FF, processes first 6 data bytes,
then sends Flow Control to request more:
```
```
CAN Frame 2 (ECU sends Flow Control - FC):
┌─────────────────────────────────────┐
│ ID: 0x643 (ECU→Tester) │
│ Data: [0x30 0x00 0x00 ... padding │ ← FC: Continue, no BS/STmin
└─────────────────────────────────────┘
```
```
Tester receives FC with Continue flag, sends next frame:
```
```
CAN Frame 3 (Consecutive Frame - CF #1):
┌─────────────────────────────────────┐
│ ID: 0x641 (Tester→ECU) │
│ Data: [0x21 ... 7 more data bytes │ ← CF with seq#1
└─────────────────────────────────────┘
```
```
CAN Frame 4 (Consecutive Frame - CF #2):
┌─────────────────────────────────────┐
│ ID: 0x641 (Tester→ECU) │
│ Data: [0x22 ... 7 more data bytes │ ← CF with seq#2
└─────────────────────────────────────┘
```

```
CAN Frame 5 (Consecutive Frame - CF #3):
┌─────────────────────────────────────┐
│ ID: 0x641 (Tester→ECU) │
│ Data: [0x23 ... 7 more data bytes │ ← CF with seq#3, final frame
└─────────────────────────────────────┘
```
```
ECU reassembles all frames:
Data = [0x36 0x00] + 6 + 7 + 7 + 6 = 26 bytes of Transfer Data payload
= Full 50-byte firmware block
```
### Modern vehicles increasingly use Automotive Ethernet for high-bandwidth communication. DoIP

### (Diagnostics over Internet Protocol) enables UDS over IP networks, typically Ethernet.

### DoIP Architecture

#### ┌─────────────────────────────────────┐

```
│ Diagnostic Tester (External Tool) │
│ (PC with diagnostic software) │
└────────────────┬────────────────────┘
│ TCP/IP over Ethernet
▼
┌────────────────────┐
│ Workshop Network │
│ (100 Mbps) │
└────────┬───────────┘
│
▼
┌────────────────────────────┐
│ DoIP Gateway ECU (Vehicle)│
│ - Vehicle Identification │
│ - Route UDS messages │
│ - Bridge to internal CAN │
└────────┬───────────────────┘
│
┌─────────┼─────────┐
│ │ │
▼ ▼ ▼
┌───┐ ┌───┐ ┌───┐
│ECU│ │ECU│ │ECU│
│CAN│ │CAN│ │CAN│
└───┘ └───┘ └───┘
```
### DoIP Features

## 9.2 UDS Over DoIP: Diagnostics Over IP (ISO 13400)

### Higher Bandwidth : 100 Mbps–1 Gbps vs. 250 kbps CAN

### Faster Uploads/Downloads : ECU flashing 10–100x faster

### Remote Diagnostics : Cloud-based diagnostic servers

### Vehicle Discovery : UDP broadcast for vehicle identification


### DoIP Communication Flow

1. Tester Discovery Phase:
├─ Tester broadcasts UDP packet to port 13400
└─ Gateway responds with Vehicle ID, IP, port
2. Connection Establishment:
├─ Tester establishes TCP connection to vehicle IP
└─ Gateway confirms connection
3. Diagnostic Communication:
├─ Tester sends UDS messages wrapped in DoIP
├─ Gateway extracts UDS message
├─ Routes to appropriate ECU on CAN bus
└─ Response travels reverse path
4. Session Management:
├─ TCP/IP handles session persistence
├─ Tester Present still required for ECU session
└─ Connection timeout after inactivity

### When an ECU cannot fulfill a request, it responds with a Negative Response Code that indicates the

### specific reason for rejection.

### NRC Categories and Common Codes

#### ┌─────────────────────────────────────────────────────────┐

```
│ GENERAL REJECT CODES (0x10–0x14) │
├─────────────────────────────────────────────────────────┤
│ 0x10 generalReject │
│ Request rejected for unknown reason │
│ │
│ 0x11 serviceNotSupported │
│ Service (SID) not implemented in ECU │
│ Example: 0x7F 0x3D 0x11 (0x3D service not exist) │
│ │
│ 0x12 subFunctionNotSupported │
│ Sub-function valid but not for this variant │
│ Example: Extended Diagnostic not available │
│ │
│ 0x13 incorrectMessageLengthOrInvalidFormat │
│ Message structure incorrect or wrong length │
│ Example: DID requires 2 bytes, sent 1 byte │
│ │
```
### TCP Reliability : Guaranteed message delivery

### Security : TLS/SSL encryption support

## 10. Advanced UDS Topics

## 10.1 Negative Response Codes (NRC)


│ 0x14 responseTooLong │
│ Response data too large for protocol │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│ TIMING/COMMUNICATION CODES (0x21–0x26) │
├─────────────────────────────────────────────────────────┤
│ 0x21 busyRepeatRequest │
│ ECU busy, try again later │
│ Tester should wait and retry │
│ │
│ 0x22 conditionsNotCorrect │
│ Preconditions not met for service │
│ Example: Cannot write VIN while driving │
│ Example: Cannot start routine while engine running │
│ │
│ 0x24 requestSequenceError │
│ Request violates service sequence │
│ Example: SendKey without RequestSeed first │
│ Example: Exceed max failed Seed/Key attempts │
│ │
│ 0x25 noResponseFromSubnetComponent │
│ ECU cannot communicate with sub-component │
│ Example: Gateway cannot reach specific ECU │
│ │
│ 0x26 failurePreventsExecutionOfRequestedAction │
│ System failure prevents operation │
│ Example: Memory error, communication loss │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│ RANGE/VALIDATION CODES (0x31–0x3A) │
├─────────────────────────────────────────────────────────┤
│ 0x31 requestOutOfRange │
│ Parameter/DID out of valid range │
│ Example: DID 0xFFFF doesn't exist │
│ Example: Routine ID not valid │
│ │
│ 0x33 securityAccessDenied │
│ Security level not unlocked │
│ Must perform Seed/Key first │
│ │
│ 0x35 invalidKey │
│ Seed/Key authentication failed │
│ Wrong algorithm or incorrect key │
│ │
│ 0x36 exceedNumberOfAttempts │
│ Too many failed Seed/Key attempts │
│ ECU locked out temporarily │
│ │
│ 0x37 requiredTimeDelayNotExpired │
│ Retry not allowed yet (cooldown) │
│ Wait before retrying │
│ │
│ 0x3A secureDataVerificationFailed │
│ Security signature invalid │


│ Certificate/digital signature check failed │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│ OPERATING CONDITIONS (0x81–0x93) │
├─────────────────────────────────────────────────────────┤
│ 0x81 rpmTooHigh │
│ Engine RPM exceeds limit for operation │
│ Example: Cannot program while engine running │
│ │
│ 0x82 rpmTooLow │
│ Engine RPM too low │
│ │
│ 0x83 engineIsRunning │
│ Cannot perform while engine running │
│ │
│ 0x84 engineIsNotRunning │
│ Operation requires engine running │
│ │
│ 0x88 vehicleSpeedTooHigh │
│ Vehicle moving too fast │
│ Cannot perform while in motion │
│ │
│ 0x8F brakeSwitch(es)NotClosed │
│ Brake not applied (safety check) │
│ Cannot start routine without brake │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│ MEMORY/PROGRAMMING CODES (0x70–0x73) │
├─────────────────────────────────────────────────────────┤
│ 0x70 uploadDownloadNotAccepted │
│ Memory transfer not possible │
│ Example: ECU doesn't support this operation │
│ │
│ 0x72 generalProgrammingFailure │
│ Flash programming error │
│ Example: Write to flash failed │
│ Example: Checksum verification failed │
│ │
│ 0x73 wrongBlockSequenceCounter │
│ Data blocks out of sequence │
│ Transfer Data frames received in wrong order │
│ │
│ 0x78 requestCorrectlyReceived-ResponsePending (*) │
│ Response takes longer, wait for P2* │
│ NOT a true error, tells tester to keep waiting │
└─────────────────────────────────────────────────────────┘


### Implementing Robust Timeout Handling

#### ┌──────────────────────────────────────────────────┐

```
│ Tester Sends Request │
└──────────┬───────────────────────────────────────┘
│ Start P2 Timer (e.g., 100 ms)
│
├─ (50 ms) No response yet...
│
├─ (100 ms) P2 timeout reached
│
│ Check if NRC 0x78 received:
│
├─ YES: Received 0x7F XX 0x78
│ ├─ Reset timer to P2* (e.g., 2000 ms)
│ ├─ Continue waiting
│ │
│ ├─ (1000 ms) Still processing...
│ │
│ └─ (2000 ms) P2* timeout reached
│ Response MUST arrive by now!
│
└─ NO: No response or wrong response
├─ Retry: Resend request
├─ Increment retry counter
└─ If max retries exceeded: FAIL
```
### ECU Bootloader and Flash Programming with UDS

### A typical ECU programming sequence using UDS:

1. Connect to ECU
[0x10 0x03] → Switch to Extended Diagnostic Session
2. Unlock Security
[0x27 0x01] → RequestSeed
[ECU responds with seed]
[0x27 0x02 &lt;calculated_key&gt;] → SendKey
[ECU unlocks security]
3. Prepare for Programming
[0x31 0x01 0x01 0xFF] → Start Erase Memory routine
[Tester polls: 0x31 0x03 0x01 0xFF]
[ECU responds when complete]
4. Switch to Programming Session
[0x10 0x10] → Switch to Programming Session (OEM-specific sub-function)
(Or may stay in Extended Session)
5. Request Download
[0x34 0x00 0x31 0x00 0x00 0x10 0x00 0x00 0x00 0x10 0x00]
→ Initiates firmware download, specifies address range

## 10.2 Timing, Flow Control, and Advanced Topics


6. Transfer Data Blocks
[0x36 0x01] + first block of firmware
[0x36 0x02] + second block of firmware
...
[0x36 0xFF] + last block of firmware
7. Request Transfer Exit
[0x37] → Terminates data transfer
8. Routine Control for Checksum
[0x31 0x01 0x02 0x01] → Start Software Checksum Verification routine
[0x31 0x03 0x02 0x01] → Request Results
[ECU verifies checksum and responds]
9. ECU Reset
[0x11 0x02] → Key OFF-ON reset
[ECU restarts and returns to Default Session]
10. Verification
[0x10 0x03] → Switch to Extended Session
[0x22 0xF1 0x88] → Read Software Version DID
[Verify new version matches programmed version]

### ODX (Open Diagnostic Data Exchange) is a standard format for storing and distributing ECU

### diagnostic data.

### Why ODX?

### Before ODX, each diagnostic tool vendor had to create proprietary diagnostic databases for each ECU

### variant. This led to:

### ODX solves this by standardizing a single diagnostic database that can be used by any compliant tool.

### ODX File Structure

### An ODX database (ISO 22901-1 standard) contains:

```
ODX File Structure (*.odx-c, *.odx-d, etc.):
│
├─ Communication Data (*.odx-c)
│ ├─ Network definitions (CAN, Ethernet, LIN)
│ └─ Physical/functional addressing rules
│
├─ Diagnostic Data (*.odx-d)
```
## 10.3 ODX: Open Diagnostic Data Exchange

### Duplication of effort

### Inconsistencies between tools

### Difficulty updating diagnost data

### High costs for diagnostic tool development


```
│ ├─ Service definitions (0x22, 0x2E, 0x31, etc.)
│ ├─ DIDs and their data types
│ ├─ DTCs and descriptions
│ ├─ Routine definitions
│ └─ Display formats and units
│
├─ Configuration Data (*.odx-e)
│ ├─ Variant coding
│ ├─ Option content
│ └─ Flash data (firmware blocks)
│
├─ Function Dictionary (*.odx-fd)
│ └─ OEM-specific function definitions
│
└─ Packaging (*.pdx - compressed archive)
└─ All above files bundled for distribution
```
### ODX Benefits

### Hardware Requirements:

### Software Requirements:

### Standardization : Single source of truth for diagnostic data

### Tool Interoperability : Any ODX-compliant tool can diagnose any ECU

### Efficiency : Automatic tool configuration without manual setup

### Version Control : Track diagnostic data versions with ECU software

### Reduced Errors : Data consistency across all service channels

### Cost Reduction : Eliminate redundant database creation

## 11. Practical Hands-On Exercises

## 11.1 Lab Environment Setup

### Vehicle or ECU test bench with CAN interface

### OBD-II port connection or dedicated diagnostic connector

### 12V power supply

### CAN transceiver and wiring

### Python : Open-source UDS libraries like python-uds or can module

### Vector CANoe/CANalyzer : Commercial diagnostic tool (industry standard)

### PEAK PCAN-View : CAN message analysis

### RAMN (Rust Automotive Middleware) : Open-source UDS implementation

### Wireshark : CAN message analysis and logging


### Objective : Read VIN from an ECU using UDS 0x22 service

### Steps:

```
# Pseudocode for reading VIN via CAN/UDS<a></a>
```
```
# 1. Open CAN bus connection<a></a>
can_interface = open_can("peak") # or vector, kvaser, etc.
```
```
# 2. Create CAN IDs (OEM-specific, example values)<a></a>
rx_id = 0x7E9 # ECU response ID
tx_id = 0x7E1 # Tester request ID
```
```
# 3. Read VIN using UDS 0x22<a></a>
request = [0x22, 0xF1, 0x90] # Read DID 0xF190 (VIN)
can_message = create_can_frame(tx_id, request)
send_can_message(can_message)
```
```
# 4. Wait for response (with timeout)<a></a>
response = receive_can_frame(rx_id, timeout= 1000 ) # 1 second timeout
```
```
# 5. Parse response<a></a>
if response[^ 0 ] == 0x62: # Positive response
vin = extract_vin_from_response(response)
print(f"VIN: {vin}")
elif response[^ 0 ] == 0x7F: # Negative response
nrc = response[^ 2 ]
print(f"Error reading VIN: NRC 0x{nrc:02X}")
```
### Expected Output:

#### VIN: WVW ZZZ 3C Z9E 123456

### Objective : Clear all DTCs using UDS 0x14 service after verifying repair

### Steps:

```
# 1. Read DTCs before clearing<a></a>
request = [0x19, 0x01, 0x08] # Read confirmed DTCs
send_uds_request(request)
response = receive_uds_response()
print(f"Current DTCs: {parse_dtc_response(response)}")
```
```
# 2. Verify repair was completed (confirm with technician)<a></a>
print("Repair verification: OK? (yes/no)")
if not verified:
print("Clear operation cancelled")
exit()
```
## 11.2 Beginner Exercise: Reading Vehicle Data

## 11.3 Intermediate Exercise: Clearing DTCs


```
# 3. Perform ECU clear operation<a></a>
request = [0x14, 0xFF, 0xFF, 0xFF] # Clear all DTCs
send_uds_request(request)
response = receive_uds_response()
```
```
if response[^ 0 ] == 0x54: # Positive response
print("DTCs cleared successfully")
else:
print(f"Clear operation failed: {response}")
```
```
# 4. Verify DTCs are cleared<a></a>
request = [0x19, 0x01, 0x08] # Read confirmed DTCs again
send_uds_request(request)
response = receive_uds_response()
remaining_dtcs = parse_dtc_response(response)
if not remaining_dtcs:
print("Verification: No DTCs remaining ✓")
else:
print(f"ERROR: DTCs still present: {remaining_dtcs}")
```
### Objective : Complete ECU reprogramming workflow

### Steps:

```
# 1. Switch to Extended Diagnostic Session<a></a>
print("Step 1: Entering Extended Diagnostic Session...")
request = [0x10, 0x03]
send_uds_request(request)
response = receive_uds_response()
if response[^ 0 ] == 0x50:
print("✓ In Extended Diagnostic Session")
else:
print("✗ Failed to switch session")
exit()
```
```
# 2. Perform Security Access (Seed/Key)<a></a>
print("Step 2: Unlocking Security...")
request = [0x27, 0x01] # Request Seed
send_uds_request(request)
seed_response = receive_uds_response()
seed = extract_seed(seed_response)
print(f"Received seed: {seed.hex()}")
```
```
key = calculate_key(seed) # Uses OEM-specific algorithm
request = [0x27, 0x02] + list(key) # Send Key
send_uds_request(request)
response = receive_uds_response()
if response[^ 0 ] == 0x67: # Positive response to 0x27
print("✓ Security unlocked")
else:
print("✗ Security unlock failed")
```
## 11.4 Advanced Exercise: ECU Firmware Update


exit()

# 3. Erase Memory<a></a>
print("Step 3: Erasing ECU memory...")
request = [0x31, 0x01, 0x01, 0xFF] # Start Erase Memory
send_uds_request(request)
response = receive_uds_response()
if response[^ 0 ] == 0x71: # Positive response to 0x31
print("✓ Erase started, waiting...")

# Poll for completion<a></a>
for attempt in range( 100 ): # Poll up to 100 times
request = [0x31, 0x03, 0x01, 0xFF] # Request Results
send_uds_request(request)
response = receive_uds_response()
if response[^ 0 ] == 0x71 and response[^ 1 ] == 0x03:
if response[^ 4 ] == 0x01: # Status: Complete
print("✓ Memory erase complete")
break
else:
print("Erase in progress...")
time.sleep( 1 ) # Wait 1 second before retry

# 4. Load and prepare firmware<a></a>
print("Step 4: Loading firmware...")
firmware = load_firmware_file("engine_ecm_v2.hex")
firmware_blocks = split_into_blocks(firmware, block_size= 256 )
print(f"Firmware size: {len(firmware)} bytes in {len(firmware_blocks)} blocks")

# 5. Request Download (initiate programming)<a></a>
print("Step 5: Initiating download...")
request = [0x34, 0x00, 0x31] # 0x34 = Request Download
request += [0x00, 0x10, 0x00, 0x00] # Start address 0x00100000
request += [0x00, len(firmware) // 256 ] # Size
send_uds_request(request)
response = receive_uds_response()
if response[^ 0 ] == 0x74: # Positive response
print("✓ Download session initiated")
else:
print("✗ Download request failed")
exit()

# 6. Transfer firmware data<a></a>
print("Step 6: Transferring firmware blocks...")
for block_num, block_data in enumerate(firmware_blocks, 1 ):
request = [0x36, block_num % 256 ] + list(block_data)
send_uds_request(request)
response = receive_uds_response()
if block_num % 10 == 0 :
print(f"Blocks transferred: {block_num}/{len(firmware_blocks)}")
print("✓ All blocks transferred")

# 7. Request Transfer Exit<a></a>
print("Step 7: Finalizing transfer...")
request = [0x37]
send_uds_request(request)


```
response = receive_uds_response()
if response[^ 0 ] == 0x77: # Positive response
print("✓ Transfer complete")
else:
print("✗ Transfer exit failed")
exit()
```
```
# 8. Verify with checksum routine<a></a>
print("Step 8: Verifying checksum...")
request = [0x31, 0x01, 0x02, 0x01] # Start Checksum Verification
send_uds_request(request)
time.sleep( 5 ) # Allow time for checksum calculation
```
```
request = [0x31, 0x03, 0x02, 0x01] # Request Results
send_uds_request(request)
response = receive_uds_response()
if response[^ 0 ] == 0x71 and response[- 1 ] == 0x01:
print("✓ Checksum verification passed")
else:
print("✗ Checksum verification failed")
exit()
```
```
# 9. Reset ECU<a></a>
print("Step 9: Resetting ECU...")
request = [0x11, 0x02] # Key OFF-ON Reset
send_uds_request(request)
response = receive_uds_response()
if response[^ 0 ] == 0x51: # Positive response
print("✓ ECU reset initiated")
time.sleep( 5 ) # Wait for ECU to restart
```
```
# 10. Verify programming<a></a>
print("Step 10: Verifying new firmware...")
request = [0x10, 0x03] # Enter Extended Session again
send_uds_request(request)
response = receive_uds_response()
```
```
request = [0x22, 0xF1, 0x88] # Read Software Version
send_uds_request(request)
response = receive_uds_response()
new_version = extract_version(response)
print(f"✓ New firmware version: {new_version}")
print("Programming completed successfully!")
```
### Common Issues and Solutions:

```
Issue: "0x7F XX 0x33 - Security Access Denied"
├─ Cause: Attempting restricted operation without unlocking
└─ Solution:
```
1. Request Seed (0x27 0x01)
2. Calculate and send correct Key (0x27 0x02 ...)
3. Retry original operation

## 11.5 Debugging and Troubleshooting Tips


```
Issue: "0x7F XX 0x22 - Conditions Not Correct"
├─ Cause: Operating conditions prevent execution
└─ Solution:
```
1. Check engine status (running/off required?)
2. Check vehicle speed (must be parked?)
3. Check session type (need Extended/Programming session?)
4. Verify brake, throttle, transmission conditions

```
Issue: "0x7F XX 0x31 - Request Out Of Range"
├─ Cause: Invalid DID or service parameter
└─ Solution:
```
1. Verify DID is correct for this ECU
2. Check ODX specification for valid values
3. Ensure parameter formatting is correct

```
Issue: Timeout waiting for response
├─ Cause: Network congestion or ECU processing delay
└─ Solution:
```
1. Check CAN bus for errors (monitor with CANoe)
2. Increase P2 timeout value (now 100ms→500ms)
3. Check if P2* timer active (NRC 0x78 received?)
4. Verify CAN ID addressing is correct
5. Check cable connections and termination

```
Issue: "0x7F XX 0x36 - Exceed Number of Attempts"
├─ Cause: Too many failed Security Access attempts
└─ Solution:
```
1. Wait for lockout period (typically 30 seconds)
2. Power cycle ECU (if allowed)
3. Use correct Seed/Key algorithm
4. Verify master key matches OEM database

### Beginner Level

### Intermediate Level

## 12. Summary and Best Practices

## 12.1 Key Takeaways from Beginner to Expert

### UDS is a standardized diagnostic protocol (ISO 14229) for vehicle ECU communication

### Request-response message structure: SID, sub-function, parameters

### Positive response SID = Request SID + 0x40; Negative response always 0x7F

### Basic services: 0x10 (Session), 0x22 (Read DID), 0x2E (Write DID), 0x14 (Clear DTC)

### Three diagnostic sessions: Default, Extended, Programming

### P2 and P2* timing parameters control response wait times

### ISO-TP protocol enables multi-frame UDS message transmission over CAN

### Security Access (0x27) uses Seed/Key mechanism to prevent unauthorized ECU access


### Expert Level

### Safety

### Security

### Negative Response Codes (NRC) provide specific error information

### Routine Control (0x31) executes diagnostic routines and tests

### DTCs store fault information with status flags (Pending, Confirmed, Permanent)

### Proper session management and timeout handling is critical for reliability

### DoIP (Diagnostics over IP) enables UDS over Ethernet for high-speed diagnostics

### ODX standard provides unified diagnostic database format for tool compatibility

### Complex firmware update procedures involve multiple UDS services in sequence

### ECU bootloader management and secure flash programming

### OEM-specific UDS extensions and implementation variations

### Architecting diagnostic middleware for production vehicles

### Performance optimization for high-volume diagnostics in manufacturing

## 12.2 Safety and Security Considerations

### Vehicle Operation Restrictions : Some diagnostic operations are disabled while vehicle is

### moving (safety mechanism)

### Fault Confirmation : Always verify repair before clearing DTCs to prevent masking problems

### Brake/Throttle Checks : Safety-critical operations require specific brake and throttle states

### Session Timeouts : Automatic return to Default Session prevents unintended long-term operation

### in privileged sessions

### Seed/Key Mechanism : Protects against unauthorized ECU modification

### Failed Attempt Lockout : Prevents brute-force attacks on security

### Digital Signatures : Modern vehicles use certificate-based authentication

### OEM Proprietary Algorithms : Key calculation algorithms are manufacturer-specific and

### confidential

### Supply Chain Protection : ODX files must be controlled to prevent unauthorized access

## 12.3 Regulatory Compliance

### OBD-II (On-Board Diagnostics) : U.S. EPA standard requires DTCs and emissions diagnostics

### EOBD (European On-Board Diagnostics) : European equivalent with extended requirements

### ISO 26262 (Functional Safety) : Diagnostic systems must meet safety standards for critical

### systems

### GDPR (Data Protection) : Vehicle data handling must comply with privacy regulations


### For ECU Developers

### For Diagnostic Tool Developers

### For Automotive Engineers

### Emerging Trends

## 12.4 Best Practices for Implementation

### 1. Implement all required core services : 0x10, 0x11, 0x14, 0x19, 0x22, 0x27, 0x2E

### 2. Correct timing handling : Implement P2, P2*, and S3 timers properly

### 3. Robust error handling : Provide meaningful NRC codes for all failure modes

### 4. Security by default : Use strong Seed/Key algorithms; lock down sensitive operations

### 5. Memory efficiency : Minimize RAM/ROM overhead of diagnostic stack

### 6. ODX compliance : Ensure diagnostic database aligns with implementation

### 1. Support multiple transport layers : CAN, LIN, Ethernet (DoIP) for compatibility

### 2. Implement ISO-TP correctly : Handle multi-frame messages and flow control

### 3. Timeout management : Implement robust P2/P2* timeout logic

### 4. User-friendly interface : Hide complexity of UDS; provide high-level diagnostic functions

### 5. Logging and replay : Record all diagnostic communication for analysis

### 6. Security : Securely store Seed/Key algorithms and master keys

### 1. Understand protocol hierarchy : Know how UDS relates to CAN, ISO-TP, and vehicle

### architecture

### 2. Master session management : Know when each session is required and how to switch

### 3. Security awareness : Understand Seed/Key limitations and vulnerability to attacks

### 4. Troubleshooting methodology : Systematic approach using NRC codes and message analysis

### 5. Tool selection : Choose diagnostic tools matching vehicle architecture (CAN vs. Ethernet)

### 6. Documentation : Create comprehensive ODX/CDD specifications for all services

## 12.5 Future of Automotive Diagnostics

### DoIP Dominance : Transitioning from CAN-only to Ethernet-based diagnostics

### Cloud Integration : Remote diagnostics and OTA (Over-The-Air) updates

### Cybersecurity : Enhanced authentication (certificates, OAuth) replacing simple Seed/Key

### AI-Assisted Diagnostics : Machine learning for predictive fault detection

### Standardized Formats : Broader adoption of ODX across industry

### Vehicle-as-Device : Connected vehicles transmitting diagnostic data to cloud platforms

### Software-Defined Vehicles (SDV) : Central computing units and zonal ECUs changing diagnostic

### architecture


### Opportunities for Entrepreneurs

```
SID Hex Service Name Purpose Common Uses
```
```
1 0x10 Diagnostic Session Control Switch between diagnosticsessions Enter Extended/Programmingsessions
```
```
2 0x11 ECU Reset Reset the ECU Restart after programming,recovery
```
```
3 0x14 Clear Diagnostic Information Clear stored DTCs Post-repair verification
```
```
4 0x19 Read DTC Information Retrieve fault codes Diagnostics, troubleshooting
```
```
5 0x22 Read Data By Identifier Read ECU data Read VIN, sensor values
```
```
6 0x23 Read Memory By Address Read memory at address Advanced diagnostics
```
```
7 0x24 Read Scaling Information Read DID scaling Data interpretation
```
```
8 0x27 Security Access Seed/Key authentication Unlock restricted operations
```
```
9 0x28 Communication Control Enable/disable messages Network management
```
```
10 0x2A
```
```
Read Data Periodic
Identifier Read periodic data Continuous monitoring
```
```
11 0x2C
```
```
Dynamically Define Data
Identifier Create custom DIDs Advanced testing
```
```
12 0x2E Write Data By Identifier Write ECU data Configure VIN, calibration
```
```
13 0x2F Input Output Control ByIdentifier Control outputs Activate relays, test actuators
```
```
14 0x31 Routine Control Execute routines Self-tests, erase memorychecksum ,
```
```
15 0x34 Request Download Initiate firmware upload ECU reprogramming
```
```
16 0x35 Request Upload Initiate firmware download Read ECU memory content
```
```
17 0x36 Transfer Data Transfer data blocks Upload/download firmware
```
```
18 0x37 Request Transfer Exit End data transfer Complete programming
```
### Diagnostic Middleware Solutions : Building bridges between legacy CAN and modern Ethernet

### ODX Tools : Creating software for ODX file generation and management

### Cloud Diagnostic Platforms : Remote vehicle diagnostics and fleet analytics

### Security Solutions : Enhanced authentication and anti-hacking mechanisms

### Specialized Tools : Industry-specific diagnostics (heavy trucks, motorcycles, agriculture)

### Training and Consulting : Helping OEMs and suppliers adopt latest diagnostic standards

## Appendices

## A. UDS Service ID Reference Table (ISO 14229-1)


```
SID Hex Service Name Purpose Common Uses
```
```
19 0x38 Request File Transfer File-based operations Optional file transfers
```
```
20 0x3D Write Memory By Address Write to memory address Advanced programming
```
```
21 0x3E Tester Present Keep session alive Prevent S3 timeout
```
```
22 0x85 Control DTC Setting Enable/disable DTC
recording
```
```
Temporary DTC suppression
```
```
23 0x86 Response On Data Type Configure response behavior Session-specific handling
```
```
24 0x87 Link Control Manage ECU communication Control network bandwidth
```
```
NRC Hex Name Cause Solution
```
- 0x10 generalReject Unknown error Retry; check ECU status
- 0x11 serviceNotSupported Service notimplemented Use valid service for thisECU
- 0x12 subFunctionNotSupported Sub-function variantinvalid Verify sub-function value
- 0x13 incorrectMessageLengthOrInvalidFormat Wrong messagestructure Check message format inspec

```
1 0x21 busyRepeatRequest ECU busy Wait and retry
```
```
2 0x22 conditionsNotCorrect Preconditions not met Check engine status,
speed, gear
```
```
3 0x24 requestSequenceError Wrong operation
sequence
```
```
Perform Security Access
first
```
```
4 0x31 requestOutOfRange Parameter invalid Verify DID/value is valid
```
```
5 0x33 securityAccessDenied Not unlocked Perform Seed/Keyauthentication
```
```
6 0x35 invalidKey Wrong key sent Use correct algorithm
```
```
7 0x36 exceedNumberOfAttempts Too many failedattempts Wait lockout period
```
```
8 0x81 rpmTooHigh Engine speed too high Turn off engine
```
```
9 0x83 engineIsRunning Engine not off Stop engine
```
```
10 0x88 vehicleSpeedTooHigh Vehicle moving Park vehicle
```
## B. Common Negative Response Codes (NRC) and Solutions


### Powertrain System

```
0xF186: Active Diagnostic Session
0xF187: ECU Hardware Version (HW)
0xF188: ECU Software Version (SW)
0xF189: ECU Software Version (Additional)
0xF1A0: Kit Assembly Part Number
0xF1A1: Calibration Identification
0xF1A2: Calibration Date/Time
0xF1B0: ECU Serial Number
0xF1F1: System Supplier (make/part/serial)
```
### Vehicle Identification

```
0xF190: Vehicle Identification Number (VIN) - 17 bytes
0xF191: Manufacturer Name
0xF192: Vehicle Model
0xF194: Transmission Type
0xF195: Fuel Type
0xF1C0: Odometer/Mileage
```
### Real-Time Data

```
0xF200: Engine Speed (RPM)
0xF202: Vehicle Speed (km/h)
0xF204: Engine Temperature (°C)
0xF206: Fuel Rail Pressure (bar)
0xF208: Throttle Position (%)
0xF20A: Intake Air Pressure (bar)
0xF20C: Lambda (O2) Sensor Voltage
```
### Commercial Tools

### Open-Source Tools

## C. Example DIDs by System (Common Across OEMs)

## D. Tools and Software for UDS Development

### Vector CANoe : Industry-leading diagnostic tool with UDS stack

### PEAK PCAN-View : CAN message analysis and simulation

### DTS (Diagnostic Tool Set) : OEM-specific diagnostic software

### Softing DiagRA : ODX-based diagnostic tool framework

### python-uds : Python library for UDS protocol implementation

### can-bus-tools : CAN frame analysis and transmission

### RAMN : Rust-based automotive middleware with UDS support

### CANdela : Open-source CAN diagnostic tool


### Development Platforms

### ISO Standards

### Recommended Books

### Online Resources

### This comprehensive training guide provides everything needed to understand and implement UDS

### from foundational concepts through advanced programming techniques. Mastery of UDS is essential

### for anyone working in automotive diagnostics, ECU development, or diagnostic tool creation.

### ⁂

### Wireshark : Network protocol analyzer supporting CAN

### AUTOSAR : Automotive standard software architecture

### MicroSAR : Embedded AUTOSAR implementation

### Embedded Linux : Vehicle gateway platform

### POSIX RTOS : Real-time OS for ECU bootloaders

## References and Further Learning

### 1. ISO 14229-1:2020 - Unified Diagnostic Services (UDS) - Part 1: Application Layer

### 2. ISO 14229-2:2020 - UDS - Part 2: Session Layer Services

### 3. ISO 15765-2:2016 - Diagnostic Communication Over CAN (DoCAN)

### 4. ISO 13400-2:2019 - Diagnostics Over Internet Protocol (DoIP)

### 5. ISO 22900 series - ODX (Open Diagnostic Data Exchange) standards

### 6. ISO 26262 - Functional Safety for Automotive Systems

### "CAN Bus Explained" - by Kennedy/Heyes

### "Automotive Embedded Systems" - by Navet/Simonot-Lion

### "UDS Protocol Training Course" - Vector Informatik

### "ODX Standard Overview" - ASAM

### ASAM MCD-2 D (ODX) Standard

### Vector CANoe Documentation

### AUTOSAR Specification

### NXP/ARM Microcontroller References

```
[1][2][3][4][5][6][7][8][9][10][11][12][13][14][15][16][17][18][19][20][21][22][23][24][25][26][27][28][29][30][31][32]
[33][34][35][36][37][38][39][40][41][42][43][44][45][46][47][48][49]
```
1. https://www.iso.org/standard/72439.html
2. https://stackoverflow.com/questions/47365883/defaultsession-programmingsession-extendeddiagnosticsession


3. https://www.linkedin.com/pulse/security-access-mechanism-ecus-using-uds-guide-imane-bougrine-5kkhe
4. https://ramn.readthedocs.io/en/latest/userguide/diag_tutorial.html
5. https://www.youtube.com/watch?v=I3wYYUGHqNs
6. https://piembsystech.com/security-access-service-identifier-0x27-uds-protocol/
7. https://piembsystech.com/read-data-by-identifier-0x22-service-in-uds-protocol/
8. https://embetronicx.com/tutorials/automotive/uds-protocol/diagnostics-and-communication-management/
9. https://stackoverflow.com/questions/46538665/calculating-key-from-seed-for-uds-service-27security-access
10. https://www.youtube.com/watch?v=Wf_4J2n2iYc
11. https://www.linkedin.com/pulse/uds-unified-diagnostic-services-iso-14229-vivek-maurya-1e
12. https://piembsystech.com/clear-diagnostic-information-0x14-service-in-uds-protocol/
13. https://www.youtube.com/watch?v=K_9_3HJiUPs
14. https://www.csselectronics.com/pages/uds-protocol-tutorial-unified-diagnostic-services
15. https://piembsystech.com/routinecontrol-0x31-service-uds-protocol/
16. https://en.wikipedia.org/wiki/ISO_15765-2
17. https://www.youtube.com/watch?v=_NxcIPlRhVs
18. https://www.youtube.com/watch?v=TMGMI9rYYt4
19. https://www.autopi.io/blog/what-is-wwh-obd/
20. https://www.linkedin.com/posts/ahmed-mounir-37979415b_understanding-uds-service-0x14-clear-diagnostic-ac
tivity-7314412176604078080-90Ow
21. https://www.influxtechnology.com/post/understanding-uds-unified-diagnostic-services-a-simple-breakdown
22. https://www.autopi.io/blog/diagnostics-over-internet-protocol-explained/
23. https://www.scribd.com/document/660478044/UDS-NRC-codes
24. https://piembsystech.com/uds-protocol/
25. https://www.rapidseasuite.com/blog/next-gen-vehicle-diagnostics-over-ethernet-with-uds-&-doip
26. https://www.youtube.com/watch?v=jkw_m1EZUIE
27. https://excelfore.com/blog/doip-intelligent-vehicle-connectivity
28. https://www.rfwireless-world.com/terminology/uds-nrc-codes
29. https://www.slideshare.net/slideshow/timer-handling-in-uds-s3-server-timer-p2-and-p2-start-timer/268974327
30. https://www.sorion-group.com/support/technologies/doip-diagnostics-over-ip/
31. https://www.youtube.com/watch?v=TTkLe61N6nc
32. https://www.slideshare.net/slideshow/kwp-2000-and-uds-protocols-analysis-comparison/91403724
33. https://www.windhilltech.com/content/articles/20250519/1747646318208/
34. https://piembsystech.com/ecu-reset-service-identifier-0x11-uds-protocol/
35. https://www.asam.net/standards/detail/mcd-2-d/
36. https://www.linkedin.com/pulse/uds-unified-diagnostic-services-iso-14229-ecu-reset-0x11-vivek-maurya
37. https://automotive.softing.com/fileadmin/sof-files/pdf/de/ae/poster/odx_poster4_softing_ae_01.pdf
38. https://www.scribd.com/document/831004906/UDS-Bootloader-The-Backbone-of-Secure-ECU-Software-Updat
e
39. https://rt-labs.com/component/uds-unified-diagnostic-services/


40. https://www.ijfmr.com/papers/2024/2/17526.pdf
41. https://www.ijert.org/a-comprehensive-review-of-unified-diagnostic-services-uds-in-automotive-systems-architec

```
ture-capabilities-limitations-and-future-perspective
```
42. https://www.srmtech.com/knowledge-base/blogs/automotive-unified-diagnostic-services-uds-a-comprehensive-o

```
verview/
```
43. https://pievcore.com/questions/what-are-some-best-practices-for-implementing-uds-protocol-in-automotive-syst
    ems
44. https://www.apex.ai/post/automotive-diagnostic-support-in-apex-grace-using-iso-14229
45. https://www.scribd.com/document/337705989/UDS-Protocol-Implementation-in-an-ECU
46. https://www.dgtech.com/wp-content/uploads/2019/08/WP1903_UDS_V01.pdf
47. https://nvdungx.github.io/unified-diagnostic-protocol-overview/
48. https://www.simmasoftware.com/uds-protocol/
49. https://piembsystech.com/negative-response-codes-nrc-uds-protocol/


