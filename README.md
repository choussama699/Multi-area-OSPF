# Multi-area-OSPF


A multi-area OSPF lab with four routers across Area 10, Area 0 (backbone), and Area 20. R2 and R3 act as Area Border Routers (ABRs), giving full end-to-end connectivity between two remote LANs.


Area Design
Router	Role	Area(s)
R1	    Internal router	Area 10
R2	    ABR	Area 10 & Area 0
R3	    ABR	Area 0 & Area 20
R4	    Internal router	Area 20


Addressing
Device	Interface IPs	                      Connected To
PC0    	192.168.10.10/24	                  Switch0 → R1
R1    	192.168.10.1/24 (LAN), 10.0.12.1/30	R2
R2	    10.0.12.2/30, 10.0.23.1/30	        R1, R3
R3	    10.0.23.2/30, 10.0.34.1/30	        R2, R4
R4	    10.0.34.2/30, 192.168.40.1/24 (LAN)	R3
PC1	    192.168.40.10/24	                  Switch1 → R4



OSPF Configuration

R1

router ospf 1
 network 192.168.10.0 0.0.0.255 area 10
 network 10.0.12.0 0.0.0.3 area 10

R2 (ABR)

router ospf 1
 network 10.0.12.0 0.0.0.3 area 10
 network 10.0.23.0 0.0.0.3 area 0

R3 (ABR)

router ospf 1
 network 10.0.23.0 0.0.0.3 area 0
 network 10.0.34.0 0.0.0.3 area 20

R4

router ospf 1
 network 10.0.34.0 0.0.0.3 area 20
 network 192.168.40.0 0.0.0.255 area 20

 
Verification
show ip ospf neighbor     
show ip route ospf        
show ip ospf border-routers
PC0> ping 192.168.40.10

R1 and R4 should learn each other's LANs as O IA (inter-area) routes, and the ping from PC0 to PC1 should succeed.

Skills Demonstrated
Multi-area OSPF design with a backbone (Area 0)
ABR configuration and inter-area route exchange
Verification and troubleshooting 
