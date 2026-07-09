# Learning Report: MAC Address, Ethernet Basics, and Switch Security

This report explains the concept of physical addressing at Layer 2 (Data Link Layer), the operation of the Ethernet protocol, and how these structures can be manipulated via Layer 2 cyberattacks.

---

## What is a MAC Address?
A MAC (Media Access Control) address is a unique physical identifier burned into a Network Interface Card (NIC) by the manufacturer. It is a 48-bit address, typically written in hexadecimal format and separated by colons.

*   **Example Format:** `00:1A:2B:3C:4D:5E`

A MAC address is structurally divided into two halves (3 bytes + 3 bytes):
1.  **First 3 Blocks (e.g., `00:1A:2B`):** Known as the **OUI (Organizationally Unique Identifier)**. This part is purchased by manufacturers (like Apple, Intel, or Samsung) from the IEEE. Tools like [MAC Vendors](https://macvendors.com/) look at these first 3 blocks to identify the vendor of a device.
2.  **Last 3 Blocks (e.g., `3C:4D:5E`):** A unique serial number assigned to that specific device by the manufacturer.

---

## Ethernet and Switch Operations

### The Ethernet Protocol
Ethernet is not just a physical cable; it is a set of rules (a protocol) that operates at **Layer 1 (Physical)** and **Layer 2 (Data Link)** of the OSI model. It defines how data is formatted and transmitted as **Frames** over wired or wireless mediums within a Local Area Network (LAN).

### How a Switch Works
A Switch is an intelligent device that directs traffic within a local network using a **MAC Address Table (CAM Table)**. 
*   When a computer sends data to another device on the same network, the switch checks its CAM table.
*   Instead of sending the data to everyone, it delivers the frame directly to the specific port where the destination MAC address is connected.
*   If the destination MAC address is unknown, the switch sends the traffic to all ports; this process is called a **Broadcast**.

---

## Cybersecurity Perspective (Layer 2 Manipulations)

Attackers often exploit the implicit trust built into Layer 2 protocols to compromise network privacy.

### 1. MAC Spoofing
*   **Scenario:** Many corporate networks or hotels use "MAC Filtering" to ensure that only authorized devices can access the internet or internal resources.
*   **Attack Mechanism:** An attacker sniffs network traffic (especially over Wi-Fi) to capture the MAC address of an authorized user. Then, they configure their own operating system to mimic this authorized address.
*   **Impact:** The network security controls recognize the attacker as a legitimate user and grant network access.

### 2. MAC Flooding
*   **Scenario:** Network switches have a limited memory capacity for storing MAC addresses in their CAM table.
*   **Attack Mechanism:** The attacker sends thousands of fake, randomly generated MAC addresses to the switch within seconds, completely filling up the CAM table.
*   **Impact:** Once the memory is exhausted, the switch fails open and begins to act like a **Hub**. This means it starts broadcasting all incoming traffic to every single port. Consequently, the attacker can sniff and capture sensitive data, such as passwords, from other users on the network.
