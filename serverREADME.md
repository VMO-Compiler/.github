# Index
1. [How to use server](#how-to-use-server-guava)
    1. [Allocate and Bind CPU Cores](#allocate-and-bind-cpu-cores)
    2. [Allocate and Bind GPUs](#allocate-and-bind-gpus)
2. [Servers](#servers)

# How to use server (Guava)
## Allocate and Bind CPU Cores
**TLDR;** `srun --ntasks=<n> <my_executable>`  

You can specify the number of CPU cores, which will run your program.
Use `--ntasks` option to specify the number of cores.  

You only can use 48 cores at maximum.
The rest of cores are used for OS and other light tasks.

## Allocate and Bind GPUs
**TLDR;** `srun --gres=gpu:<n> <my_executable>`  

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

---

You can check the slurm execution queue with `squeue`.
```bash
squeue -o "%.18i %.9P %.8j %.8u %.2t %.10M %.6D %.16R %.16b"
```
The command above is registered as **`sq`**.

# Servers
|Name|CPU|GPU|
|---|---|---|
|Eska|Intel i7-9700k|-|
|Falcon|Intel E5-2630v3|Nvidia TitanXp|
|Guava|AMD 5975wx|4 * Nvidia RTX 4090|
