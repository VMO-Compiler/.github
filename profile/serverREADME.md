# How to Use Server Scheduler (Guava)

# Index
1. [Allocate **GPU**s](#allocate-gpus)
2. [Allocate **CPU**s](#allocate-cpus)
3. [Show job queue](#show-job-queue)
4. [Cancel job](#cancel-job)
* [Servers](#servers)

# Allocate GPUs
`srun --gres=gpu:<n> <my_executable>`  
---
The server is using slurm scheduler.
Therefore, you can use slurm client tools such as `sinfo`, `salloc`, `srun`, and `sbatch`.
Note that `salloc` does not change environment variables while scheduling your jobs.
For example, if you check `nvidia-smi` via `salloc`,
```bash
salloc --gres=gpu:1 nvidia-smi
```
it will show you all the nvidia gpus on the server, not just one.  
On the other hand, if you use `srun` or `sbatch`, the environment will be changed.
```bash
srun --gres=gpu:1 nvidia-smi
```
Also, you can submit your job batch to the slurm with `sbatch`.
For example, create an shell script and write,
```bash
#!/bin/bash
#
#SBATCH --ntasks=16      # CPU cores
#SBATCH --mem=2gb        # Memory limit
#SBATCH --time=00:10:00  # Time limit (hh:mm:ss)
#SBATCH --gres=gpu:2     # GPUs
./my_executable
```
Then,
```bash
sbatch my_sbatch.sh
```

# Allocate CPUs
`srun --ntasks=<n> <my_executable>`  
---
You can specify the number of CPU cores, which will run your program.
Use `--ntasks` option to specify the number of cores.  

You only can use 48 cores at maximum.
The rest of cores are used for OS and other light tasks.

# Show job queue
`sq`
---
You can check the slurm execution queue with `squeue`.
```bash
squeue -o "%.18i %.9P %.8j %.8u %.2t %.10M %.6D %.16R %.16b"
```
The command above is registered as **`sq`**.

# Cancel job
`scancel -u <user_name>`
---
You can cancel submitted jobs in the queue.
```bash
scancel <job-id>
```
Also, you can cancel all of your jobs via
```bash
scancel -u <my_user_name>
```

---

# Servers
Only the servers in the cluster (scheduler network) are listed.
|Name|CPU|GPU|
|---|---|---|
|Guava|AMD 5975wx|4 * Nvidia RTX 4090|
