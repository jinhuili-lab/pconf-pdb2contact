PConPy/PDB2ContactMap
======
## Update: 2025, the code is rabish. I abandan it and create a new, refer: https://github.com/jinhuili-lab/pdb2contact
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
conda create -n pdb2contact python=3.6
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
# for example:
alias dssp='/home/jinhui/software/pconpy/bin/mkdssp'  #change it to your path
```

## Usage
```
Usage:
    pconpy.py cmap <dist> -p <pdb> -o <file> [options]
    pconpy.py dmap -p <pdb> -o <file> [options]
    pconpy.py hbmap -p <pdb> -o <file> [options]

Options:
    -p, --pdb <pdb>             The PDB file.
    -c, --chains <chain-ids>    Comma-separated list of chain identifiers
                                (defaults to the first chain).
    -o, --output <file>         Save the plot to a file. The file format is
                                determined by the file extension.
    -m, --measure <measure>     The inter-residue distance measure [default: CA].
    -M, --mask-thresh <dist>    Hide the distances below a given threshold (in
                                angstroms).
    --plaintext                 Generate a plaintext distance/contact matrix
                                and write to stdout (recommended for
                                piping into other CLI programs).
    --asymmetric                Display the plot only in the upper-triangle.

    --title TITLE               The title of the plot (optional).
    --xlabel <label>            X-axis label [default: Residue index].
    --ylabel <label>            Y-axis label [default: Residue index].

    --font-family <font>        Font family (via matplotlib) [default: sans].
    --font-size <size>          Font size in points [default: 10].

    --width-inches <width>      Width of the plot in inches [default: 6.0].
    --height-inches <height>    Height of the plot in inches [default: 6.0].
    --dpi <dpi>                 Set the plot DPI [default: 80]

    --greyscale                 Generate a greyscale distance map.
    --no-colorbar               Hide the color bar on distance maps.
    --transparent               Set the background to transparent.
    --show-frame

    -v, --verbose               Verbose mode.

Distance measures:
    "CA" -- Conventional CA-CA distance, this is the default distance measure.
    "CB" -- The CB-CB distance.
    "cmass" -- The distance between the residue centers of mass.
    "sccmass" -- The distance between the sidechain centers of mass
    "minvdw" -- The minimum distance between the VDW radii of each residue.

```
## Example usage
Generate a PDF contact map using the CA-CA distance measure:
```
python pconpy.py cmap 8.0 --pdb ./tests/pdb_files/1ubq.pdb \
          --chains A --output 1ubqA_cmap.pdf --measure CA 
```
Generate a PNG distance map using the min. VDW distance measure:
```
python  pconpy.py dmap --pdb ./tests/pdb_files/3erd.pdb \
          --chains B,C --output 3erdBC_dmap.png --measure minvdw
```
Generate a plain-text [hydrogen bond matrix](http://en.wikipedia.org/wiki/Protein_contact_map#HB_Plot):
```
python pconpy.py hbmap --pdb ./tests/pdb_files/1ubq.pdb \
          --chains A --plaintext --output 1ubq.txt
```

