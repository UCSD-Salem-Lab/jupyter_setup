# Jupyter Notebook Setup
Tutorial to set up Jupyter Notebook on Narrows computer cluster

Author: Wanjun Gu

Date: Aug 17th, 2023



### Step1: Installing Jupyterlab in the conda base environment

Log in the compute cluster head node and spin up an interactive session:

```bash
module load slurm; srun --partition=salem-compute --mem=8G --pty bash
```

If have not done so already， install `jupyterlab` in conda

```bash
conda activate base
conda install jupyterlab
```

Before doing this step, make sure that `conda` is installed and initiated in the interactive session. If `conda` is initiated, you will see something like:

```bash
Last login: Thu Aug 17 15:16:57 2023 from 70.95.214.144
[wagu@narrows-login-1 ~]$ module load slurm; srun --partition=salem-compute --mem=8G --pty bash
(base) bash-4.4$
```

where `(base)` shows that the user is currently in the base environment of conda.



### Step2: Locate the absolute path of the user's conda directory 

In the interactive session, find out the absolute path of your conda installation using:

```bash
conda info --base
```

If this snippet runs as expected, you will see something like:

```bash
/salemlab/users/wagu/miniconda3
```

where `wagu` should instead be your own user name.

In this case, knowing my conda directory, the script to initialize conda for me would be in:

```bash
/salemlab/users/wagu/miniconda3/etc/profile.d/conda.sh
```



### Step3: Initialize conda and load jupyter-submit module

Restart another cluster head node and run:

```bash
# Load slurm
module load slurm
# Load conda
# Replace this path with your own conda path
source /salemlab/users/wagu/miniconda3/etc/profile.d/conda.sh 
# Load the jupyter-submit module, which works with slurm to launch jupyter instances
module load jupyter-submit
# Activate the conda base environment
conda activate base
# Launch jupyter notebook
jupyter-submit -p salem-compute -A salem-compute
```

Once these commands are successfully run, you should have a `.url` file generated in your `~/` directory that looks like:

```bash
(base) bash-4.4$ cat 120940.url
http://salem-cn-01.sdsc.edu:8890/lab?token=9b713b40630aa8bec6c9f2f43c3bbe02
```

You can then use this url in the file to access the jupyter instance from your local browser. 

**NOTE**: When accessing the notebook from outside UCSD, you will need to connect to the UCSD VPN first.



### ONE-STEP-PROCESS: From now on

If you are able to successfully install the conda environment and launch the instance once, from now on, you should be able to easily launch more jupyter notebook instances from the head node using these commands:

```bash
module load slurm
# Replace this path with your own conda path
source /salemlab/users/wagu/miniconda3/etc/profile.d/conda.sh 
module load jupyter-submit
conda activate base
jupyter-submit -p salem-compute -A salem-compute
```

