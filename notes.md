# Raw Notes — Analyzer Output & Takeaways

## Indicator

```
URL: http://46.200.4.39:45202/i
IP:  46.200.4.39
Source: urlhaus.abuse.ch (status: online at time of pull)
```

## Abuse_Finder_2_0 — Report

```
Names: AGGREGATE BLOCK FOR UKRTELECOM
       # NCC2011125583 Approved IP assignment
Abuse addresses: postmaster@ukrtelecom.ua
```

## GoogleDNS_resolve_1_0_0 — Report

```
Question: 39.4.200.46.in-addr.arpa. IN *
Answer:
  Name: 39.4.200.46.in-addr.arpa.
  Type: PTR
  Data: 39-4-200-46.pool.ukrtel.net.
  TTL:  21600
```

## Reading the results together

- Both analyzers independently point to the same provider (Ukrtelecom) —
  agreement between two unrelated lookups is a basic confidence signal.
- The PTR hostname follows a generic `pool.*` naming pattern, which usually
  indicates dynamic/residential-leaning IP space rather than dedicated
  hosting — common for botnet or compromised-host infrastructure.
- Abuse contact (`postmaster@ukrtelecom.ua`) captured during triage, so no
  second lookup is needed if/when escalating for takedown.

## Case closure summary (as written in TheHive)

> Investigated IOC 46.200.4.39:45202/i, sourced live from URLhaus. Confirmed
> active malware distribution host via Cortex enrichment. Observables logged
> and flagged as IOC. Recommended firewall/proxy block. Case closed as True
> Positive.

## Takeaways

- Total time from case creation to closure: under 15 minutes.
- Free analyzers (no API key) were sufficient for a real triage — useful
  reference for anyone setting up a personal SOC lab without paid TI feeds.
- Next iteration: feed the IOC in via TheHive's Alert API instead of manual
  case creation, to better simulate a SIEM → TheHive handoff.
