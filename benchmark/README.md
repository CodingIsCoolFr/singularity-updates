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

Singularity 0.8.30 and Discord app 1.0.9259, both already open, neither in a
call. The average of two 60-second runs, Intel Core i9-13900K, 32 GB RAM,
Windows 11 Pro, 27 September 2026.

Singularity had an animated background chosen, but its own log recorded zero
redraws a second during the samples, so the CPU figure is the cost of sitting
idle, not of playing that picture.

|                        | Singularity | Discord  |
|------------------------|------------:|---------:|
| Private memory         |  **178 MB** | 1,012 MB |
| Working set            |   **75 MB** | 1,202 MB |
| CPU average            |   **0.12%** |    0.32% |
| CPU peak               |   **0.52%** |    2.10% |
| Processes              |       **1** |        6 |
| Threads                |      **27** |      260 |
| Size on disk           |  **203 MB** |   490 MB |

Discord keeps six processes and 1,012 MB of its own memory just to stay open.
Singularity is one process and 178 MB. That is nearly six times the memory,
about ten times the threads, and more than twice the disk.

An earlier run on 0.8.4, both sitting in the same text channel with the
picture actually moving, measured 330 MB private, 35 threads, and 0.60% CPU
against Discord's 0.23%. Redrawing the GIF was most of that CPU.

An earlier run on Singularity 0.8.0 (639 MB private, 107 threads, 1.27% CPU)
had a bug in how chat videos were played: each one started dozens of threads.
0.8.1 fixed it.

An earlier result (Singularity 0.7.7: 336 MB private, 0.10% CPU) was
withdrawn. It was described as "both idle", but Singularity was in a voice
call at the time, so it did not measure what it said. Its raw numbers are
still in `benchmark-results.json`, with every other run.

Calls and screen share have not been compared yet.
