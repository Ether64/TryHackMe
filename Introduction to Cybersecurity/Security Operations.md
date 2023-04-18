
The main purpose of security operation centers is to:

- Find vulnerabilities within a network.
- Detect unauthorized activity
- Discover policy violations
- Detect Intrusions
- Support with the incident response process 


- A good security operations center uses many data sources to monitor a network for signs of intrusion:

	- **Server logs** - will show authentication attempts.
	- DNS activity - You can track DNS queries of certain domains and link them to unknown IPs to identify attempted attacker communication.
	- Firewall logs - can see blocked traffic or attempted connections
	- DHCP logs - can use these logs to see what kind of devices (rogue?) joined our network.

- SOC analyts often use a SIEM Security Information and Event Management tool to aggregate data from all types of log sources so that data can be correlated  to specific attackers/locations/event types.

**SOC Services**

- SOCs can provide for:
		- Security posture monitoring
		- Vulnerability management
		- Malware Analysis - recovering malicious programs within a network and analyzing it
		- Intrusion detection - detect log intrusions and suspicious packets
		- Reporting - report incidents and alarms, meet compliance requirements.

- On top of this, most modern SOCs provide *Proactive services* such as:
		- Network security monitoring: focuses on monitoring the network data nad analyzing traffic within customers environments.
		- Threat hunting: SOC assumes an intrusion has taken place, and begin to hunt to confirm this assumption.
		- Threat intelligence - focuses on mapping backstories and idnetities to potential adversaries and recording their tactics and techniques for future preventative measures.