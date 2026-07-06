[GitHub - sbousseaden/EVTX-ATTACK-SAMPLES: Ejemplos de ataque de eventos de Windows · GitHub](https://github.com/sbousseaden/EVTX-ATTACK-SAMPLES/)
- Cada `.evtx` → fue nombrado según una técnica
- Splunk usa la ruta del archivo como `source`

**Estructura Fields EVTX RAW en Splunk**
```bash
[Hora] (Time): Momento exacto del evento.
[Proceso] (Process / Image): Ejecutable que se lanzó.
[Versión / descripción]: Info del binario (nombre, versión, vendor).
[CommandLine]: Comando completo con argumentos usados al ejecutar.
[WorkingDir]: Carpeta desde donde se ejecutó el proceso.
[User]: Usuario que ejecutó el proceso.
[Integrity]: Nivel de privilegio (Low, Medium, High, System).
[Hashes]: Huellas del archivo (MD5, SHA1, SHA256).
[Parent] (ParentImage):** Proceso padre que inició este proceso.
```

Mindset Theath Hunting
```bash
- ¿Está destruyendo algo?
- ¿Está descargando algo?
- ¿Está ejecutando algo?
- ¿Está ocultando algo?
```

|                                                                                                                                                        |       |
| ------------------------------------------------------------------------------------------------------------------------------------------------------ | ----- |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\AutomatedTestingTools\Malware\DE_timestomp_and_dll_sideloading_and_RunPersist.evtx *                 | 23    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\AutomatedTestingTools\Malware\rundll32_cmd_schtask.evtx *                                            | 50    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\AutomatedTestingTools\PanacheSysmon_vs_AtomicRedTeam01.evtx *                                        | 565   |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\AutomatedTestingTools\panache_sysmon_vs_EDRTestingScript.evtx *                                      | 122   |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Command and Control\DE_RDP_Tunnel_5156.evtx *                                                        | 101   |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Command and Control\DE_RDP_Tunneling_TerminalServices-RemoteConnectionManagerOperational_1149.evtx * | 228   |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Command and Control\DE_sysmon-3-rdp-tun.evtx *                                                       | 73    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Command and Control\bits_openvpn.evtx *                                                              | 1537  |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Credential Access\ACL_ForcePwd_SPNAdd_User_Computer_Accounts.evtx *                                  | 55    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Credential Access\CA_PetiPotam_etw_rpc_efsr_5_6.evtx  NONE                                           | 29107 |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Credential Access\CA_hashdump_4663_4656_lsass_access.evtx                                            | 2     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Credential Access\CA_keepass_KeeThief_Get-KeePassDatabaseKey.evtx                                    | 2     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Credential Access\babyshark_mimikatz_powershell.evtx                                                 | 33    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Credential Access\discovery_sysmon_1_iis_pwd_and_config_discovery_appcmd.evtx                        | 40    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Credential Access\etw_rpc_zerologon.evtx                                                             | 415   |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Credential Access\remote_sam_registry_access_via_backup_operator_priv.evtx                           | 31    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Credential Access\sysmon_3_10_Invoke-Mimikatz_hosted_Github.evtx                                     | 2     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Defense Evasion\DE_1102_security_log_cleared.evtx                                                    | 112   |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Defense Evasion\Sysmon 7 Update Session Orchestrator Dll Hijack.evtx                                 | 64    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Defense Evasion\Win_4985_T1186_Process_Doppelganging.evtx                                            | 2     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Defense Evasion\de_unmanagedpowershell_psinject_sysmon_7_8_10.evtx                                   | 84    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Discovery\Discovery_Remote_System_NamedPipes_Sysmon_18.evtx                                          | 20    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Discovery\dicovery_4661_net_group_domain_admins_target.evtx                                          | 63    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Discovery\discovery_meterpreter_ps_cmd_process_listing_sysmon_10.evtx                                | 52    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Execution\Sysmon_Exec_CompiledHTML.evtx                                                              | 2     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Execution\exec_driveby_cve-2018-15982_sysmon_1_10.evtx                                               | 2     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Execution\exec_sysmon_1_11_lolbin_rundll32_zipfldr_RouteTheCall.evtx                                 | 2     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Execution\exec_sysmon_1_lolbin_renamed_regsvr32_scrobj.evtx                                          | 2     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Execution\exec_sysmon_1_lolbin_rundll32_advpack_RegisterOCX.evtx                                     | 2     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Execution\execution_evasion_visual_studio_prebuild_event.evtx                                        | 29    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Execution\rogue_msi_url_1040_1042.evtx                                                               | 351   |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Execution\sysmon_exec_from_vss_persistence.evtx                                                      | 17    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Execution\sysmon_zipexec.evtx                                                                        | 8     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Lateral Movement\DFIR_RDP_Client_TimeZone_RdpCoreTs_104_example.evtx                                 | 21    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Lateral Movement\LM_5145_Remote_FileCopy.evtx                                                        | 869   |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Lateral Movement\LM_ScheduledTask_ATSVC_target_host.evtx                                             | 34    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Lateral Movement\LM_renamed_psexecsvc_5145.evtx                                                      | 22    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Lateral Movement\LM_sysmon_3_12_13_1_SharpRDP.evtx                                                   | 38    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Lateral Movement\dfir_rdpsharp_target_RdpCoreTs_168_68_131.evtx                                      | 40    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Lateral Movement\net_share_drive_5142.evtx                                                           | 2     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Lateral Movement\remote_file_copy_system_proc_file_write_sysmon_11.evtx                              | 2     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Lateral Movement\smbmap_upload_exec_sysmon.evtx                                                      | 27    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Other\rdpcorets_148_mst120_bluekeep_rpdscan_full.evtx                                                | 733   |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Persistence\DACL_DCSync_Right_Powerview_ Add-DomainObjectAcl.evtx                                    | 28    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Persistence\Network_Service_Guest_added_to_admins_4732.evtx                                          | 2     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Persistence\persistence_sysmon_11_13_1_shime_appfix.evtx                                             | 237   |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Persistence\sysmon_13_1_persistence_via_winlogon_shell.evtx                                          | 25    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Persistence\sysmon_20_21_1_CommandLineEventConsumer.evtx                                             | 17    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\4624 LT3 AnonymousLogon Localhost - JuicyPotato.evtx                            | 3     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\4765_sidhistory_add_t1178.evtx                                                  | 3     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\CVE-2020-0796_SMBV3Ghost_LocalPrivEsc_Sysmon_3_1_10.evtx                        | 4     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\EfsPotato_sysmon_17_18_privesc_seimpersonate_to_system.evtx                     | 20    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\NTLM2SelfRelay-med0x2e-security_4624_4688.evtx                                  | 11    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\Runas_4624_4648_Webshell_CreateProcessAsUserA.evtx                              | 3     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\Sysmon_13_1_UACBypass_SDCLTBypass.evtx                                          | 5     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\Sysmon_UACME_22.evtx                                                            | 19    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\Sysmon_UACME_30.evtx                                                            | 11    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\Sysmon_UACME_32.evtx                                                            | 8     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\Sysmon_UACME_33.evtx                                                            | 9     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\Sysmon_UACME_34.evtx                                                            | 6     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\Sysmon_UACME_36_FileCreate.evtx                                                 | 13    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\Sysmon_UACME_38.evtx                                                            | 8     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\Sysmon_UACME_41.evtx                                                            | 4     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\Sysmon_UACME_43.evtx                                                            | 5     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\Sysmon_UACME_45.evtx                                                            | 8     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\Sysmon_UACME_53.evtx                                                            | 14    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\Sysmon_UACME_54.evtx                                                            | 10    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\Sysmon_UACME_56.evtx                                                            | 15    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\Sysmon_UACME_63.evtx                                                            | 3     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\Sysmon_UACME_64.evtx                                                            | 530   |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\Sysmon_uacme_58.evtx                                                            | 6     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\System_7045_namedpipe_privesc.evtx                                              | 1     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\UACME_59_Sysmon.evtx                                                            | 7     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\UACME_61_Changepk.evtx                                                          | 2     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\privesc_KrbRelayUp_windows_4624.evtx                                            | 1     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\privesc_registry_symlink_CVE-2020-1377.evtx                                     | 30    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\privesc_spoolfool_mahdihtm_sysmon_1_11_7_13.evtx                                | 4     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\privesc_spoolsv_spl_file_write_sysmon11.evtx                                    | 37    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\privesc_sysmon_cve_20201030_spooler.evtx                                        | 8     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\privesc_unquoted_svc_sysmon_1_11.evtx                                           | 87    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\privexchange_dirkjan.evtx                                                       | 8     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\samaccount_spoofing_CVE-2021-42287_CVE-2021-42278_DC_securitylogs.evtx          | 18    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\sysmon_11_1_15_WScriptBypassUAC.evtx                                            | 15    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\sysmon_11_7_1_uacbypass_windirectory_mocking.evtx                               | 10    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\sysmon_13_1_compmgmtlauncherUACBypass.evtx                                      | 4     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\sysmon_13_1_meterpreter_getsystem_NamedPipeImpersonation.evtx                   | 4     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\sysmon_1_13_UACBypass_AppPath_Control.evtx                                      | 3     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\sysmon_1_7_elevate_uacbypass_sysprep.evtx                                       | 15    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\sysmon_privesc_psexec_dwell.evtx                                                | 12    |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Privilege Escalation\win10_4703_SeDebugPrivilege_enabled.evtx                                        | 1     |
| C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\UACME_59_Sysmon.evtx                                                                                 |       |
