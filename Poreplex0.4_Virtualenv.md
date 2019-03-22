## Poreplex0.4_Virtualenv
Create a virtualised environment for Poreplex-0.4

Download miniconda and create Miniconda environment:
```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
chmod +x Miniconda3-latest-Linux-x86_64.sh
./Miniconda3-latest-Linux-x86_64.sh
```

Create new virtualenv with spevific python version:
```bash
conda create -n poreplex-0.4 python=3.6
```

Install Poreplex and its dependencies:
```bash
conda install -n poreplex-0.4 pip
conda install -n poreplex-0.4 numpy
conda install -n poreplex-0.4 tensorflow
conda install -n poreplex-0.4 poreplex
```

Activate the virtual environment and install Albacore-2.3.4
```bash
conda activate poreplex-0.4

wget https://mirror.oxfordnanoportal.com/software/analysis/ont_albacore-2.3.4-cp36-cp36m-manylinux1_x86_64.whl
pip install ont_albacore-2.3.4-cp36-cp36m-manylinux1_x86_64.whl
pip install inotify
```

Get kmer models:
```bash
cd /home/lp471/bin/miniconda3/envs/poreplex-0.4/lib/python3.6/site-packages/poreplex/
git clone https://github.com/nanoporetech/kmer_models.git
```
## Run poreplex

Setup environment
```bash
__conda_setup="$(CONDA_REPORT_ERRORS=false '/home/lp471/bin/miniconda3/bin/conda' shell.bash hook 2> /dev/null)"
if [ $? -eq 0 ]; then
    \eval "$__conda_setup"
else
    if [ -f "/home/lp471/bin/miniconda3/etc/profile.d/conda.sh" ]; then
        . "/home/lp471/bin/miniconda3/etc/profile.d/conda.sh"
        CONDA_CHANGEPS1=false conda activate base
    else
        \export PATH="/home/lp471/bin/miniconda3/bin:$PATH"
    fi
fi
unset __conda_setup

export POREPLEX_ENV=~/bin/miniconda3/envs/poreplex-0.4

conda activate poreplex-0.4

```
Run poreplex command
```bash
poreplex -i raw/ -o demultiplexed/ --trim-adapter --barcoding --fast5 --basecall --parallel 14 --align $POREPLEX_ENV/minimap2_indexes/Homo_sapiens.GRCh38.90.mmidx --dashboard --contig-aliases $POREPLEX_ENV/minimap2_indexes/contigs_db.txt --live
```
Deactivate the virtual environment:
```bash
conda deactivate
```
