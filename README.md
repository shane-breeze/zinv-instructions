# zinv-instructions

How to run the zinv python framework.

It is advised to run this framework in python 3.7 since the end of life for python 2 is on it's way and consistent results may not be possible. It is also dependent on python packages primarily on pypi and installable through pip (or any other means). If root access isn't available on your platform a virtual environment is recommended, particularly that of miniconda.

## Miniconda environment

Follow the [instructions](https://docs.conda.io/en/latest/miniconda.html) to install miniconda, i.e. select the relevant executable with the python version currently installed on your system. Follow the onscreen instructions and add lines to your `.bashrc` as needed.

If you're using miniconda, create a python 3.7 environment (updating conda if it asks you to)
```bash
conda create -n zinv37 python=3.7
```

Activate the environment
```bash
conda activate zinv37
```

Next you need to enter the environment you can skip the installation procedure and proceed to activating the environment. We will install the required packages later. Some of which depend on some mathematical tools in ROOT which have no simple alternatives. We cannot install ROOT through pip so we'll use conda instead:
```
conda install root -c conda-forge
```
Test this by trying to import ROOT in python
```
python -c "import ROOT"
```


## Analysis documentation

The analysis documentation is currently in a standalone package: https://github.com/shane-breeze/AN-17-313. This allows integration with overleaf for a live update of the document while writing. Within this repository sits a `notebooks` directory for jupyter notebooks to create the results included in the analysis using the tools from the https://github.com/shane-breeze/zdb-analysis repository.

Clone the analysis note repository and move into the `notebooks` directory to run some results.

```
git clone git@github.com:shane-breeze/AN-17-313.git
cd AN-17-313/notebooks
```

### Jupyter notebook

From the notebooks directory perform the following setup.

The notebooks can be used locally on the system, or preferably through an ssh tunnel for Imperial (where the input datasets are located):

1. Open a terminal and log into either lx02 or lx03 (lx00 and lx01 have old glibc libraries and may result in issues)

2. Activate your environment and install jupyter:
```bash
conda install pip
conda install -c conda-forge nodejs
pip install jupyterlab ipywidgets widgetsnbextension
jupyter labextension install @jupyter-widgets/jupyterlab-manager
#jupyter labextension install jupyterlab_vim # vim bindings in jupyterlab
```

3. Run a jupyter notebook server:
```bash
jupyter lab --no-browser --port=8889
```
Note that with the `--no-browser` option, the notebook will appear, however, might be terribly slow. Now take note of the http URL that can be used to access the notebook.

4. Open a terminal on your laptop and start an SSH tunnel:
```bash
ssh -N -f -L localhost:8889:localhost:8889 remote_user@remote_host
```
where `remote_user` and `remote_host` are replaced by those where the notebook server is running.

5. Open a browser and paste the URL from step 3

Now you can run some of the notebooks found here. For example, navigate to `040_triggers/met/met_triggers.ipnb`. Currently, you don't have the required python package, but these can be installed within the notebook and will persist in the `zinv37` environment you've created. Try running the first block to see what's missing or simply install all the packages you can see in a block
```
!pip install zdb numpy pandas matplotlib seaborn dftools numdifftools scipy iminuit auto_tqdm pysge
```

Now the environment is setup try running the whole notebook.

### Reopening the environment

Open two terminals: one for your laptop and one ssh'd into lx02 or lx03.

In lx0? change into the relevant directory and run
```
jupyter lab --no-browser --port=8889
```

On your laptop's terminal run:
```
ssh -N -f -L localhost:8889:localhost:8889 remote_user@remote_host
```
and open a browser with the URL taken from the command run in lx0?
