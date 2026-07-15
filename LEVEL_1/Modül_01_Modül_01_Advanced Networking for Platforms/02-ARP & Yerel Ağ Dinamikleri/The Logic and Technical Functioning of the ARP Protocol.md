# 🔌 Module 01 / Topic 02: ARP Protocol, Table, and Network Dynamics

If a junior engineer ever comes up to you and asks, *"Why do we need a MAC address and ARP when we already have an IP address on the network?"*, tell them this story:

> 🏢 **Analogy: The Corporate Plaza and Employees**
> 
> Imagine you are working in a massive corporate plaza (local network / switch). In this plaza, every employee has a National ID Number (IP Address) and a physical Desk Number (MAC Address) where they sit.
> 
> You need to deliver an urgent document to Ahmet (IP: 192.168.1.20). You know Ahmet's National ID number, but you have no idea which physical desk he sits at (MAC address). The plaza's security guard (Switch) only knows where the desks are located; they do not keep track of anyone's National ID.
> 
> So, you stand in the middle of the plaza and shout at the top of your lungs (**Broadcast - ARP Request**):
> *"Who is Ahmet with National ID 192.168.1.20? Which desk are you sitting at?!"*
> 
> Everyone in the plaza hears your voice, but only Ahmet takes notice and responds to you quietly (**Unicast - ARP Reply**):
> *"I'm over here! My desk number is Desk-F24"*.
> 
> To make sure you do not forget this, you quickly write it down in your pocket notebook (**ARP Table / Cache**): *"Ahmet = Desk-F24"*. This way, you do not have to shout in the middle of the plaza every time you want to send him a document.
