# SLURM-array-example

A `SLURM` job array is a convenient way of submitting dozens, hundreds, or thousands of jobs. Conveniently, every job in the array will be assigned the bash variable `$SLURM_ARRAY_TASK_ID` which is an integer equal to that job's Nth position in the array.
 
Consider the example submission command `sbatch --array=1-100%20 myscript.sh`

The above would submit a job array of length 100. `%20` tells `SLURM` to only let at most 20 jobs run at a time, and to keep the rest pending. This helps limit your usage so you don't individually take up the entire cluster.

The first job in the arry has `$SLURM_ARRAY_TASK_ID` set to `1`, the 30th job has `$SLURM_ARRAY_TASK_ID` set to `30` and so on.

We can take advantage of this variable to extract variables from a file that defines how each job in the array should run. For example, if we want to submit a job with 100 pairs of `fastq` files, we would create a 100-row file, where each row provides the parameters for a single job.

Then, within each job, we can extract the row corresponding to our job array number.

I've created [sample_ids.txt](sample_ids.txt) which contains 1000 sample IDs. 

I then created a `bash` script that should run once per sample, [jobscript.sh](jobscript.sh). Check it out to see how it uses the `$SLURM_ARRAY_TASK_ID` variable to run a sample-specific job for every job in the array.

The 1000-job array would be submitted as follows:

```bash
sbatch --array=1-1000%20 jobscript.sh 
```