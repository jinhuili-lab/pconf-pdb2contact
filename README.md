PConPy
======
## This is a copy of the PConPy. However, the raw version has many issues. I changed it. If useful for you, add a star, please.
```
1. DSSP executable is not available in the provided ftp server. But I can get it using conda install -c salilab dssp, and the command is mkdssp instead of dssp.
2. from Bio._py3k import StringIO should be removed in python3. Without StringIO, the output can be read as str instead of file.
3. without chain_ids defined residues = get_residues(opts["--pdb"], chain_ids=chain_ids) will throw an error. Just add chain_ids=None.
```
## Overview

![1ubq CA-CA distance map](images/1ubq-dmap-CA.png)
![1ubq CA-CA contact map](images/1ubq-cmap-CA.png)

## About

A [_contact map_](http://en.wikipedia.org/wiki/Protein_contact_map) is a 2D
representation of protein structure that illustrates the presence or absence of
contacts between individual amino acids. This enables the rapid visual
exploratory data analysis of structural features without 3D rendering software.
Additionally, the underlying 2D matrix of a contact map, known as a _contact
matrix_, can naturally be used as numeric input in subsequent automated
knowledge discovery or machine learning tasks. PConPy generates
publication-quality renderings of contact maps, and the related distance- and
hydrogen bond maps.

## Installation

### Dependencies

PConPy was developed using **Python 3** using the following libraries:
- NumPy
- BioPython
- Matplotlib
- docopt

which can be installed via ``apt-get`` using Ubuntu:
```
sudo apt-get install python-numpy python-biopython python-matplotlib python-docopt
```  
or via the [Anaconda Python Distribution](http://continuum.io/downloads):
```
conda install numpy biopython matplotlib pip
conda install -c salilab dssp
pip install docopt
```

### DSSP

PConPy uses the DSSP secondary structure assignment program to obtain
inter-residue hydrogen bond information. The DSSP executable needs to be
installed into your system path and renamed to `dssp`, it can be
downloaded from:

- https://anaconda.org/ostrokach/dssp/2.2.1/download/linux-64/dssp-2.2.1-1.tar.bz2
```
wget https://anaconda.org/ostrokach/dssp/2.2.1/download/linux-64/dssp-2.2.1-1.tar.bz2
tar -jxvf dssp-2.2.1-1.tar.bz2
# add env variable manually
# for example: alias dssp='/home/jinhui/software/pconpy/bin/mkdssp'
```

## Example usage
Generate a PDF contact map using the CA-CA distance measure:
```
python ./pconpy/pconpy.py cmap 8.0 --pdb ./tests/pdb_files/1ubq.pdb \
          --chains A --output 1ubqA_cmap.pdf --measure CA 
```
Generate a PNG distance map using the min. VDW distance measure:
```
python ./pconpy/pconpy.py dmap --pdb ./tests/pdb_files/3erd.pdb \
          --chains B,C --output 3erdBC_dmap.png --measure minvdw
```
Generate a plain-text [hydrogen bond matrix](http://en.wikipedia.org/wiki/Protein_contact_map#HB_Plot):
```
python ./pconpy/pconpy.py hbmap --pdb ./tests/pdb_files/1ubq.pdb \
          --chains A --plaintext --output 1ubq.txt
```

