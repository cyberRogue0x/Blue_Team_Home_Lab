# Windows Event Log Analysis: Interactive Logon Review

A beginner SOC analysis of a short sequence of Windows Security events: failed logon (4625), explicit credential use (4648), and successful logon (4624). The activity was reviewed in the context of the user's confirmation that they made the login attempt. The available evidence supports a **likely benign, user-attributed activity** conclusion; it does not establish an attack or a confirmed security incident.


## Project goals

- Identify and interpret Windows Security events related to authentication.
- Build a timeline across failed, explicit-credential, and successful logon events.
- Distinguish observed facts from interpretation and uncertainty.
- Record a cautious disposition and practical follow-up steps.

## Environment and scope

- **Data source:** Windows Security event log, as reviewed by the analyst.
- **Host:** `DESKTOP-Q61AG76$` appears as the subject account in event 4648.
- **Account:** local account `cyber`
- **Time:** 19 October 2026, approximately 10:14:55–10:14:59 PM.
- **Scope limitation:** The supplied evidence covers three events and a later process inspection. No broader host, network, or account review is claimed.

## Event timeline

| Time (as recorded) | Event ID | Observed details | Interpretation |
|---|---:|---|---|
| 10:14:55 PM | 4625 | Failed logon for `cyber`; logon type 2; source address `127.0.0.1`; status `0xC000006D`; substatus `0xC000006A`; process `C:\Windows\System32\svchost.exe` | A local interactive logon attempt failed. The status/substatus are consistent with an incorrect password. |
| 10:14:59 PM | 4648 | Subject `DESKTOP-Q61AG76$`; credentials account `cyber`; target `localhost`; process `svchost.exe`; network address `127.0.0.1` | Explicit credentials were used in a local context shortly after the failure. This event alone does not identify the human or prove malicious behavior. |
| 10:14:59 PM | 4624 | Successful interactive logon for `cyber`; logon type 2; source `127.0.0.1`; authentication package `Negotiate`; elevated token: yes; account is an administrator | A successful local interactive logon followed. The user confirmed they were the person attempting to log in. |

### Process context

The process ID `0x624` (decimal `1572`) was associated with `svchost.exe` hosting `UserManager` when inspected later. That later observation does **not** conclusively establish which service or process caused the historical logon sequence. Do not describe `UserManager` as the confirmed cause.

## Analysis and disposition

The sequence shows one failed local interactive logon followed within seconds by explicit credential use and a successful local interactive logon for the same account. The loopback address and logon type 2 fit a local interaction. The user's confirmation directly supports benign attribution for this observed attempt.

**Disposition:** likely benign, user-attributed activity; no confirmed security incident based on the evidence reviewed.

This is a bounded conclusion, not proof that the host had no other suspicious activity. The supplied event set does not establish whether other logons occurred, whether the account's credentials were exposed, or the historical service-level cause of `svchost.exe` activity.

## Method

1. Reviewed the supplied Windows Security event details for IDs 4625, 4648, and 4624.
2. Ordered the events by recorded timestamp and correlated account, logon type, process, and network fields.
3. Asked the account holder whether they initiated the activity; the user confirmed that they did.
4. Considered later process inspection as context only, not as proof of historical causation.
5. Recorded a disposition that separates event facts, user attribution, and unresolved limitations.

## Key learning

- Event IDs are most useful when correlated into a timeline rather than interpreted alone.
- `127.0.0.1` indicates loopback context, not a remote source.
- A failed logon followed by a success can have a benign explanation; confirm with the user and surrounding evidence.
- A current process/service mapping is not sufficient to prove what a process was doing at an earlier timestamp.
- Elevated/admin context increases the importance of careful review but does not by itself indicate compromise.


