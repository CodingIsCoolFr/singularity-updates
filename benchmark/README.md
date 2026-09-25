# Singularity vs Discord: resource benchmark

`benchmark.ps1` watches Singularity and the official Discord desktop app while
both are running and reports what each costs the machine. It starts nothing
and changes nothing.

It measures, summed over every process each app runs:

- **Private memory**: RAM that belongs to the program alone. The fairest
  memory figure.
- **Working set**: RAM in use. Over-counts Discord, because memory its
  processes share is counted once per process.
- **CPU**: average and peak, as a share of the whole machine.
- **Processes and threads**
- **Size on disk** of the running version's folder

## Run it yourself

1. Open Singularity and Discord, signed in, on the same server and text
   channel. Leave both idle, and not in a call.
2. In PowerShell:

   ```powershell
   powershell -ExecutionPolicy Bypass -File benchmark.ps1 -Seconds 60 -Label "idle in a text channel"
   ```

3. Results print as a table and are added to `benchmark-results.json`.

## Results so far

Both idle on the same text channel, 60 seconds, Intel Core i9-13900K, 32 GB
RAM, Windows 11 Pro, 25 September 2026. Singularity 0.7.7, Discord app
1.0.9258. Second of two runs (the first was taken while Discord was still
settling after start; see `benchmark-results.json` for both).

|                        | Singularity | Discord  |
|------------------------|------------:|---------:|
| Private memory         |  **336 MB** |   863 MB |
| Working set            |  **283 MB** | 1,357 MB |
| CPU average            |       0.10% |    0.10% |
| CPU peak               |   **0.28%** |    0.77% |
| Processes              |       **1** |        6 |
| Threads                |      **33** |      260 |
| Size on disk           |  **181 MB** |   490 MB |

Only the idle case has been measured. Calls and screen share have not been
compared yet.
