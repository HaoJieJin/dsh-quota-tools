# DSH Quota Tools

Two companion CLI tools for the **DeepSeek Harness (DSH)** agent platform.

| Tool | Purpose |
|---|---|
| `dsh-quota-check` | Pre-flight check for sub-agent model routing: reads the DSH quota-plugin snapshot plus a local "exhausted memory" file, then recommends a route or reports that a model is dry for today |
| `dsh-cost` | Calculates how much a DSH session has spent, using real token counts × official prices, with **DeepSeek peak/off-peak pricing** (weekdays 9-12 & 14-18 = peak ×2; weekends/holidays = off-peak half price) |

## Requirements

- Node.js 18+
- Designed for a running DSH instance, but both tools degrade gracefully:
  - `dsh-quota-check` reads `http://127.0.0.1:${DSH_PORT||3080}/dsh-quota/snapshot`
  - `dsh-cost` reads session log files under `$DSH_HOME`

## Usage

```bash
# dsh-quota-check
dsh-quota-check                          # human-readable summary
dsh-quota-check --json                   # raw snapshot
dsh-quota-check --pick default|long|short|vision   # recommended route
dsh-quota-check --check <provider>/<model>         # check one route
dsh-quota-check --dry <provider>/<model> [reason]  # mark as dry today
dsh-quota-check --undry <provider>/<model>         # undo dry
dsh-quota-check --clear                  # clear today's memory

# dsh-cost
dsh-cost                                 # most recent active session
dsh-cost --session <id>                  # a specific session
dsh-cost --all                           # all sessions
dsh-cost --today                         # today's total
dsh-cost --json                          # machine readable
dsh-cost --balance                       # account balance only
dsh-cost --no-net                        # local only (no network)
dsh-cost --peak                          # peak or off-peak now
```

Exit codes (`dsh-quota-check`): `0` = available, `2` = dry, `3` = unknown.

## Environment

| Variable | Description |
|---|---|
| `DSH_HOME` | DSH home path (default resolved relative to the script) |
| `DSH_QUOTA_MEM` | Path to the quota-memory JSON file |
| `DSH_PORT` | DSH web server port (default `3080`) |
| `CURL` | curl binary path (default `curl`) |

## License

MIT
