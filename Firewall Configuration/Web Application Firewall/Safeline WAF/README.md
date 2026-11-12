# DVWA and SafeLine WAF Test Project

This project demonstrates SQL injection and reflected cross-site scripting (XSS) tests against DVWA, with SafeLine WAF monitoring the requests.
> **What the evidence shows:** In Defense mode, the SQL injection and reflected XSS result screenshots show **Access Forbidden**. SafeLine logs also contain both **Blocked** and **Audited** entries, so the evidence shows that some requests were blocked and some were recorded without blocking. Offline mode is a separate setting that blocks all visitors.

## Tools used

- **SafeLine WAF** — inspected and filtered web requests; recorded attack events and blocked requests.
- **DVWA** — deliberately vulnerable web application used as the authorized test target.
- **Apache 2.4.58** — served DVWA in the lab.
- **Ubuntu Desktop 24.04 and Docker** — lab platform for the application and SafeLine.
- **Oracle VirtualBox on Windows 11** — ran the isolated Ubuntu lab.
- **Web browser** — used to access DVWA and observe test results.

## Step-by-step evidence and image placement

### 1. Start at the SafeLine homepage

[View screenshot: SafeLine dashboard](images/01-safeline-homepage.png)

The dashboard is set to a 24-hour view. In this screenshot, it shows 74.9k requests, 43.7k page views, 15 visitors, 2 unique IPs, 14 blocked requests, and 1 IP address in the displayed summary. These are the values visible for that time filter, not a promise of current or general traffic.

- **Requests:** individual web calls received by the protected application.
- **Views (PV):** page-view count. One person can create many views.
- **Visitors (UV):** estimated distinct visitors for the selected period.
- **Unique IP:** distinct source IP addresses seen in the selected period.
- **Blocked:** requests SafeLine stopped.
- **IP Addr:** the source address associated with traffic. In the logs below, the visible address is `192.168.56.1` and is labelled LAN IP, meaning it is an address on the lab network.

### 2. Confirm the protected DVWA application

[View screenshot: DVWA application in SafeLine](images/02-dvwa-application.png)

The Applications page shows a DVWA application registered in SafeLine, with Defense mode and its listener details.
The **BOT PRO...** and **AUTH** controls are visible here.
**Bot protection** helps identify or manage automated bot traffic.
**Auth** adds an access challenge for visitors.

### 3. Review the SQL injection and XSS security rules

[View screenshot: SafeLine security settings](images/03-security-settings.png)

The screenshot shows SQL injection and XSS set to **Strict Mode** in the semantic analysis settings. Strict mode applies stricter inspection to those attack categories. Other rule categories have their own settings.

### 4. Select Defense mode for the attack tests

[View screenshot: Defense mode selection](images/04-defense-mode.png)

Defense mode is selected. In simple terms, SafeLine aims to block requests it identifies as attacks while allowing ordinary traffic. Audited mode records suspicious requests without blocking them. Offline mode blocks all users and is demonstrated separately at the end.

### 5. Run the SQL injection test and record its result

[View screenshot: DVWA SQL injection test input](images/05-sql-injection-test.png)

The screenshot shows a SQL injection test entered in DVWA’s SQL Injection page.

[View screenshot: SQL injection blocked result](images/06-sql-injection-blocked.png)

The result page displays **Access Forbidden**, indicating the request was blocked at the WAF in this Defense-mode test. The SafeLine log screenshot also includes a SQL injection entry marked **Blocked**. Other SQL injection entries in the log are marked **Audited**, so the evidence does not mean every attempt was blocked.

### 6. Run the reflected XSS test and record its result

[View screenshot: Reflected XSS test input](images/07-reflected-xss-test.png)

The screenshot shows a script test string entered into DVWA’s Reflected XSS field.

[View screenshot: Reflected XSS blocked result](images/08-reflected-xss-blocked.png)

The result displays **Access Forbidden**, indicating that this request was blocked. SafeLine’s log also contains XSS entries marked **Blocked** and an earlier entry marked **Audited**. An audited entry means the request was recorded but not blocked in that event.

### 7. Review detailed SafeLine logs

[View screenshot: SafeLine attack logs](images/09-attack-logs.png)

The Logs page lists individual requests with an action, URL, attack type, source IP, and time. **Blocked** means SafeLine stopped the request. **Audited** means SafeLine recorded it but did not stop it. The shown rows include SQL injection and XSS events. The IP shown is `192.168.56.1` (LAN IP).

### 8. Review grouped attack events

[View screenshot: SafeLine event summary](images/10-event-summary.png)

The Events page groups related activity by source IP and application, and summarizes attack counts and duration. Use Logs for individual request details and Events for a grouped view of activity.

### 9. Demonstrate Offline mode separately

[View screenshot: Offline mode selection](images/11-offline-mode.png)

Offline mode is selected in this screenshot. It is an availability control, not an attack-specific result: it blocks all visitors to the protected site.

[View screenshot: Offline mode result](images/12-offline-result.png)

The browser shows **Website is Offline, Please Visit Later**, confirming that the site was unavailable while Offline mode was active. Return the application to Defense mode after this demonstration.

## Conclusion

The screenshots document the SafeLine dashboard, a DVWA application, Strict Mode settings for SQL injection and XSS, and test outcomes. The Defense-mode result pages and SafeLine logs support that the shown SQL injection and reflected XSS requests were blocked. The logs also contain audited events, which should be reported separately from blocks. Offline mode blocked all visitors as expected in its separate demonstration.

## Lab safety

These tests were performed against DVWA in an isolated lab. Keep DVWA off the public internet and only test systems for which you have authorization.

