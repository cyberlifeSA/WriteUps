`C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Command and Control\DE_RDP_Tunneling_TerminalServices-RemoteConnectionManagerOperational_1149.evtx`

```bash
index=attack_lab source="C:\\Logs\\SAMPLES-SPLUNK\\EVTX-ATTACK-SAMPLES-master\\Command and Control\\DE_RDP_Tunneling_TerminalServices-RemoteConnectionManagerOperational_1149.evtx"
| rex field=Message "(?<RealTime>\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2}\.\d{3})"
| eval _time=strptime(RealTime,"%Y-%m-%d %H:%M:%S.%3N")
| rex field=Message "(?<Process>[A-Za-z]:\\\\[^\s]+\.exe)"
| rex field=Message "(?<WorkingDir>[A-Za-z]:\\\\[^\s]+\\\\)\s+[A-Z0-9]+\\\\"
| rex field=Message "(?<User>[A-Z0-9]+\\\\[A-Za-z0-9]+)\s+(Low|Medium|High|System)"
| rex field=Message "(?<Integrity>Low|Medium|High|System)"
| rex field=Message "IMPHASH=[^\s]+\s+(?<ParentImage>[A-Za-z]:\\\\[^\s]+\.exe)\s+(?<ParentCommandLine>.+)$"
| rex field=Message "(?<RawCmd>Microsoft Corporation\s+(.+?)\s+[A-Za-z]:\\\\[^\s]+\\\\\s+[A-Z0-9]+\\\\)"
| rex field=Message "(?<ParentCommandLine>C:\\\\Windows\\\\[^\s]+\.exe\s+[-/a-zA-Z0-9\s]+)$"
| eval CommandLine=replace(RawCmd,"Microsoft Corporation\s+","")
| rename ComputerName as Host
| sort 0 + _time
| eval Time=strftime(_time,"%Y-%m-%d %H:%M:%S.%3N")
| table Time ParentImage ParentCommandLine Host User WorkingDir Integrity Process CommandLine Message
```

Query adecuada
```bash
index=attack_lab source="C:\\Logs\\SAMPLES-SPLUNK\\EVTX-ATTACK-SAMPLES-master\\Command and Control\\DE_RDP_Tunneling_TerminalServices-RemoteConnectionManagerOperational_1149.evtx"
| eval Time=strftime(_time,"%Y-%m-%d %H:%M:%S.%3N")
| rex field=Message "Usuario:\s(?<User>[^\r\n]+)"
| rex field=Message "Dominio:\s(?<Domain>[^\r\n]+)"
| rex field=Message "Dirección de red de origen:\s(?<SrcIP>[^\r\n]+)"
| rename ComputerName as Host
| sort 0 _time
| table Time Host User Domain SrcIP Message
```

![Pasted image 20260504121750](../Fotos/Pasted%20image%2020260504121750.png)