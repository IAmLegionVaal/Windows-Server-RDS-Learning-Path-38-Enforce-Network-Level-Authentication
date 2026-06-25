# Windows Server RDS Learning Path 38 — Enforce Network Level Authentication

**Level:** Advanced · **Module:** 38/70

## Goal
Require Network Level Authentication for RDP and verify approved Windows clients connect correctly.

## Setup
1. Record the current RDP listener configuration.
2. In the RDS server GPO enable **Require user authentication for remote connections by using Network Level Authentication**.
3. Confirm the policy applies only to the RDS Servers OU.
4. Refresh computer policy.
5. Start a new connection from CL01 using an approved RDS user.
6. Review RemoteConnectionManager and Security events.

```powershell
Get-ItemProperty 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' |
 Select-Object UserAuthentication,SecurityLayer,MinEncryptionLevel
gpupdate.exe /force
gpresult.exe /scope computer /r
Get-WinEvent -LogName 'Microsoft-Windows-TerminalServices-RemoteConnectionManager/Operational' -MaxEvents 50
```

## Evidence
Store listener state before/after, the GPO report, successful client connection and relevant events under `evidence/`.

## Troubleshooting
For a failed approved client, verify Windows updates, DNS, time, certificate trust and user rights before changing NLA policy.

## Security
NLA reduces pre-authentication exposure and should remain required for supported clients.

## Rollback
Restore the recorded policy only through the RDS GPO while maintaining console access.

## Next
`Windows-Server-RDS-Learning-Path-39-Configure-RDS-Account-Lockout-Controls`
