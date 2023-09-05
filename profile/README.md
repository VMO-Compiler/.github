# Compilers and Optimization Team @VMO
Compiler research group at VMO Lab. ECE. SNU.

# Index
1. [How to use server](#how-to-use-server)
  1. [Allocate and Bind CPU Cores](#allocate-and-bind-cpu-cores)
  2. [Allocate and Bind GPUs](#allocate-and-bind-gpus)
2. [Servers](#servers)

# How to use server (Guava)
## Allocate and Bind CPU Cores
**TLDR;** Use `numactl`.  
You can specify the cores to run your program.
For example, if you want to run your executable on the core 0 to 3 (4 cores)
```bash
numactl --physcpubind=0,1,2,3 <my_executable>
```
or
```bash
numactl --physcpubind=0-3 <my_executable>
```

Those commands will set your program's core affinitiy.

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

You can check the slurm execution queue with `squeue`.
```bash
squeue -o "%.18i %.9P %.8j %.8u %.2t %.10M %.6D %R %b"
```
The command above is registered as **`sq`**.

# Servers
|Name|CPU|GPU|
|---|---|---|
|Eska|Intel i7-9700k|-|
|Falcon|Intel E5-2630v3|Nvidia TitanXp|
|Guava|AMD 5975wx|4 * Nvidia RTX 4090|
