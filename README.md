# TheHive + Cortex SOC Lab — Live IOC Triage Walkthrough

A hands-on run of a TheHive + Cortex incident-triage lab, but instead of using
the typical sample data (`8.8.8.8`, `example.com`), I pulled a **real, currently
active malware indicator from [URLhaus](https://urlhaus.abuse.ch/)** and ran it
through the full case → enrichment → closure workflow.

Goal: make the lab feel like an actual Tier 1 triage instead of a tutorial.

📄 Full writeup with all 13 screenshots: [Medium post](#) <!-- replace # with your Medium link once published -->

---

## Environment

- TheHive 3 — case and task management
- Cortex — automated observable enrichment
- Elasticsearch — backing datastore
- All three running as Docker containers in a local lab VM

## The Indicator

Pulled live from `urlhaus.abuse.ch/browse/`, filtered to `status: online`:

```
http://46.200.4.39:45202/i
```

URLhaus is a free, community-run feed maintained by abuse.ch specifically for
looking up and blocking malware infrastructure — safe to query because that's
what it's built for. The URL itself was never opened in a browser; only the
text value was used as a TheHive observable.

## Workflow

1. Created a case (`Severity: High`, `TLP/PAP: Amber`) with a description written
   like an actual proxy-log finding, not a lab placeholder.
2. Logged two observables — the full URL and the bare IP — both flagged `Is IOC: true`.
3. Ran Cortex analyzers (`GoogleDNS_resolve`, `Abuse_Finder`) — both free, no API
   key required.
4. Cross-checked the two analyzer outputs against each other before writing the
   case summary.
5. Closed the case as `True Positive` with a one-paragraph summary a Tier 2
   analyst could pick up cold.

## Key Screenshots

| Step | Screenshot |
|---|---|
| Case creation | `images/02-create-case.png` |
| Observables logged (auto-defanged) | `images/06-observables-list.png` |
| Abuse_Finder result — hosting provider + abuse contact | `images/11-abuse-finder.png` |
| GoogleDNS_resolve result — PTR record | `images/12-googledns.png` |
| Case closure with summary | `images/13-close-case.png` |

*(Full set of 13 screenshots in `/images`, numbered in workflow order.)*

## What This Demonstrated

- **TheHive hierarchy**: Case → Task → Observable → Task log, and how tasks
  stay unassigned until claimed even on a case you created yourself.
- **TLP/PAP matter operationally** — not decoration. They control who a case
  or indicator can be shared with.
- **Free analyzers are enough** to build a real enrichment workflow without a
  paid threat intel subscription — relevant for anyone building a home lab.
- **Cross-referencing two independent analyzer outputs** (PTR hostname vs.
  abuse-contact registrant) as a basic confidence check before writing up an IOC.

See [`notes.md`](notes.md) for the raw analyzer output and takeaways.

---

**Note:** This was built as a self-directed practice exercise on TheHive +
Cortex, adapted with a live indicator from `urlhaus.abuse.ch` in place of
typical sample data, as part of ongoing SOC analyst skill-building.
