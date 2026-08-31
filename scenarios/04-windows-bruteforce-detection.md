# Scenario 04 — Windows Brute-Force Detection

## Objective
Detect repeated failed Windows logon attempts and correlate them in Wazuh into a higher-severity brute-force alert.

## Systems
- Endpoint: Windows 10
- Windows Security Event ID: `4625`
- Wazuh Agent: `windows-agent`
- SIEM: Wazuh 4.12.0

## Base Detection
Individual failed logon attempts generated Windows Event ID `4625`.

Wazuh detected these events using its built-in rule:

- Rule ID: `60122`
- Description: `Logon Failure - Unknown user or bad password`
- Level: `5`

## Custom Correlation Rule
A custom Wazuh correlation rule was created on the Wazuh Manager:

```xml
<group name="windows,authentication_failed,">

  <rule id="100100" level="10" frequency="5" timeframe="60">
    <if_matched_sid>60122</if_matched_sid>
    <description>Windows: Multiple failed logon attempts detected - Possible brute force attack.</description>
    <mitre>
      <id>T1110</id>
    </mitre>
  </rule>

</group>
```

## Attack Simulation
On Windows, repeated failed authentication attempts were generated using:

```powershell
runas /user:FakeSOCUser cmd
```

An incorrect password was entered repeatedly within 60 seconds.

## Observed Result
The individual failed logon events were detected as Rule `60122`, and Wazuh successfully correlated them into the custom high-severity alert:

- Custom Rule ID: `100100`
- Rule Level: `10`
- Description: `Windows: Multiple failed logon attempts detected - Possible brute force attack.`
- MITRE ATT&CK: `T1110` — Brute Force

## Evidence
- `10-wazuh-windows-bruteforce-detection.png` — Custom Windows brute-force correlation alert in Wazuh