# Snort Alert Analysis Report

## 1. Report Overview

**Project:** Snort Network Intrusion Detection Lab  
**Platform:** TryHackMe  
**Tool:** Snort 2.9.7.0  
**Analysis Type:** Network Traffic Monitoring and Rule-Based Detection

### Objective

The objective of this report is to document my analysis of two custom Snort detection rules created during my TryHackMe lab.

The rules focus on ICMP traffic monitoring and TCP traffic directed to destination port 80.

This report explains their detection logic, expected behavior, and limitations.

## 2. Detection Rule 1 – ICMP Traffic

### Rule

```snort
alert icmp any any -> any any (msg:"ICMP Packet Found"; sid:1000001; rev:1;)
```

### Detection Logic

This rule monitors ICMP traffic regardless of source and destination IP addresses.

When matching ICMP traffic is detected, Snort generates an alert with the message:

`ICMP Packet Found`

### Analyst Interpretation

ICMP is commonly used for network diagnostics and communication, including ping requests and replies.

An ICMP alert does not automatically indicate malicious activity.

A SOC analyst should consider the ICMP message type, source and destination addresses, frequency, and network context before determining whether further investigation is necessary.

## 3. Detection Rule 2 – TCP Traffic to Port 80

### Rule

```snort
alert tcp any any -> any 80 (msg:"HTTP Traffic Detected"; sid:1000002; rev:1;)
```

### Detection Logic

This rule monitors TCP traffic directed to destination port 80.

When traffic matches the rule, Snort generates the following alert:

`HTTP Traffic Detected`

### Analyst Interpretation

TCP port 80 is commonly associated with HTTP communication.

However, this rule identifies traffic based on its destination port. It does not inspect the application payload or independently verify HTTP content.

A more detailed investigation would require examining the network traffic and application-layer information.

## 4. Detection Comparison

| Attribute | ICMP Rule | TCP Port 80 Rule |
|---|---|---|
| SID | 1000001 | 1000002 |
| Protocol | ICMP | TCP |
| Destination port | Not applicable | 80 |
| Alert message | ICMP Packet Found | HTTP Traffic Detected |
| Detection method | Protocol-based | Protocol and port-based |

## 5. Investigation Considerations

During a real SOC investigation, I would consider the following information:

- Source IP address
- Destination IP address
- Protocol
- Source and destination ports, when applicable
- Timestamp
- ICMP message type, when applicable
- Frequency of matching traffic
- Relevant network and application context

These indicators help determine whether an alert requires additional investigation.

## 6. Detection Limitations

The two custom rules have important limitations:

- The ICMP rule does not filter specific ICMP message types.
- The TCP rule only matches destination port 80.
- The TCP rule does not inspect HTTP payload contents.
- HTTPS traffic on port 443 is not covered by the TCP port 80 rule.
- A matching alert does not automatically establish malicious activity.

These limitations demonstrate why detection rules should be interpreted alongside other network evidence.

## 7. Evidence Reference

The practical work supporting this report is documented in the repository:

- `evidence/01-Snort-Configuration-Validation.png`
- `evidence/05-Snort-IDS-Mode-No-Logging.png`
- `evidence/08-Snort-ICMP-Traffic-Detection.png`
- `rules/local.rules`

The repository also contains the complete set of eight lab screenshots.

**Evidence limitation:** The original raw alert logs and complete packet records are not included in this report. Exact IP addresses, timestamps, and packet-level findings are therefore not reconstructed.

## 8. Conclusion

This exercise helped me understand how Snort uses custom signatures to identify network traffic matching predefined conditions.

I practiced creating ICMP and TCP port-based detection rules and analyzing their behavior and limitations.

This work strengthened my understanding of network monitoring, signature-based detection, and the importance of contextual alert analysis.

**Project Context:** Educational cybersecurity lab conducted in an authorized TryHackMe environment.
