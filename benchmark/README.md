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

Singularity 0.8.4 and Discord app 1.0.9259, both started a few minutes
before, on the same text channel, neither in a call during the runs. The
average of two 60-second runs, Intel Core i9-13900K, 32 GB RAM, Windows 11
Pro, 25 September 2026.

Singularity was playing an animated GIF as its background. Discord has no
animated background. Redrawing that GIF is most of Singularity's CPU in this
run, which is why Discord comes out ahead on average CPU.

|                        | Singularity | Discord  |
|------------------------|------------:|---------:|
| Private memory         |  **330 MB** | 1,065 MB |
| Working set            |  **307 MB** | 2,177 MB |
| CPU average            |       0.60% | **0.23%** |
| CPU peak               |   **1.03%** |    1.15% |
| Processes              |       **1** |        6 |
| Threads                |      **35** |      262 |
| Size on disk           |  **203 MB** |   490 MB |

An earlier run on Singularity 0.8.0 (639 MB private, 107 threads, 1.27% CPU)
is replaced by this one. That version had a bug in how chat videos were
played: each one started dozens of threads. 0.8.1 fixed it.

An earlier result (Singularity 0.7.7: 336 MB private, 0.10% CPU) was
withdrawn. It was described as "both idle", but Singularity was in a voice
call at the time, so it did not measure what it said. Its raw numbers are
still in `benchmark-results.json`, with every other run.

Calls and screen share have not been compared yet.
