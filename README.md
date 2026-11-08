https://dash0-migrator.vercel.app

Expanded the functionalities of our web GUI to an actual CLI called **intern0**.

## Installation

Python 3.9 or later, nothing else.

```
git clone -b feature/intern0 https://github.com/eric134422/dash0-dashboard-migrator
cd dash0-dashboard-migrator
uv tool install .          # or: pipx install .
intern0 migrate            # add --dry-run to only preview, nothing is sent
```

Without installing, run `python3 -m intern0 migrate` inside the folder.

The first run opens your browser twice: sign in to Datadog and to Dash0 and click Allow. There are no keys to create.

## Initial limitation, solved

A migration has two parts: **dashboards**, the numbers such as requests per second, and **alerts**, the workflow that warns you. At first the website only converted: you copied the result and imported it in Dash0 by hand. That works for dashboards, but Dash0 has no file import for alerts.

**Now** the website sends both straight to Dash0 through its import API ([dashboards](https://www.dash0.com/docs/api-reference/operations/post/api/import/dashboard), [alerts](https://www.dash0.com/docs/api-reference/operations/post/api/import/check-rules)). Paste a Dash0 auth token, pick the dataset, send. Copy and download still work.

## Files

```
pyproject.toml               makes intern0 an installable command
intern0/
├── cli.py                   intern0: sign-ins, picking, plan, import
└── converter/
    ├── datadog_to_perses.py     Datadog dashboard → Dash0 dashboard (Perses)
    ├── monitors_to_dash0.py     Datadog monitor → Dash0 alert (Prometheus rule)
    └── datadog_metrics.csv.gz   type and unit of 36,948 Datadog metrics, used by both
```
