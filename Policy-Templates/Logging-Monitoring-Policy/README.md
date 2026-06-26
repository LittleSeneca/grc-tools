# Logging and Monitoring Policy Template

## What This Is

The Logging and Monitoring Policy defines what to log, how to protect logs, how long to keep them, and how to monitor for security events. It's the foundation of your detection and investigation capabilities — without it, you're operating blind.

## What It Covers

- What events must be logged (authentication, authorization, data access, admin actions, network, security)
- Log content requirements (timestamp, subject, object, action, result)
- Log protection (access controls, immutability, forwarding)
- Log retention (online and archive periods)
- Clock synchronization (NTP/UTC)
- Monitoring and alerting requirements
- Security control failure response

## Gotchas People Get Wrong

**1. Logging everything is expensive and useless.** The policy says "log all access to sensitive data" but your application generates 50 million data access events per day. At that volume, storage costs explode and nobody reviews the logs anyway. Be selective: log access to PII, access by privileged users, and anomalous patterns — not every row returned by a paginated API.

**2. Centralized logging is table stakes.** If logs only exist on the server that generated them, an attacker who compromises that server can delete the evidence. Ship logs to a separate, immutable repository in real time. This is the single most important logging control.

**3. Log retention is a cost optimization problem, not just a compliance checkbox.** 90 days of verbose application logs can cost thousands in storage and ingest costs. Use log levels: DEBUG (don't ship to centralized log system), INFO (ship, retain 30 days), WARN/ERROR/SECURITY (ship, retain 90+ days). Tier your retention by log severity.

**4. "Logs must be reviewed" without automation is meaningless.** Nobody manually reviews gigabytes of logs. The policy should require automated alerting on known-bad patterns. Manual review is for investigation of alerts, not for discovering them.

**5. The monitoring section must define what constitutes an alert-worthy event.** "Unusual activity" is too vague. Define specific thresholds: "10 failed login attempts from the same IP in 5 minutes" or "privileged account used outside business hours." These thresholds should be tuned over time based on your baseline.

## Implementation Advice

- **OpenObserve is cost-efficient for log management.** Unlike Splunk or Datadog (which charge by ingest volume and get expensive fast), OpenObserve offers columnar storage with dramatically lower ingest costs. For organizations watching their budget, it's a strong alternative. The open-source version runs in your own infrastructure.
- **Use log levels strategically.** Don't ship DEBUG and TRACE logs to your centralized system — they'll dominate your ingest volume without providing security value. Ship INFO and above, with WARN and ERROR at higher retention.
- **NTP is deceptively important.** If your servers' clocks are off by even a few seconds, correlating events across systems becomes impossible during an investigation. All servers should sync to the same NTP sources, and clock drift should be monitored.
- **Test your alerting pipeline quarterly.** Generate a known-bad event (a test login failure pattern, a fake malicious IP) and verify that it generates an alert that reaches the right people. Silent alert failures are common and dangerous.
