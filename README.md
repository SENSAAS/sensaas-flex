# sensaas-flex
Flexible alignment of molecules using SENSAAS

[![badgepython](https://forthebadge.com/images/badges/made-with-python.svg)](https://www.python.org/downloads/release/python-370/)  [![forthebadge](https://forthebadge.com/images/badges/built-with-science.svg)](https://chemoinfo.ipmc.cnrs.fr/)

**SENSAAS** and **SENSAAS-Flex** are a shape-based alignment softwares which allows to superimpose molecules in a rigid or flexible manner, respectively. It is based on the publications [SenSaaS: Shape-based Alignment by Registration of Colored Point-based Surfaces](https://onlinelibrary.wiley.com/doi/full/10.1002/minf.202000081) and [SENSAAS-Flex: a joint optimization approach for aligning 3D shapes and exploring the molecular conformation space](https://doi.org/10.1093/bioinformatics/btae105)


![example](/images/alignment.png)


**Documentation**: Full documentation is available at [https://github.com/SENSAAS/sensaas-flex/docs/SENSAAS-Flex-documentation.pdf](https://github.com/SENSAAS/sensaas-flex/blob/main/docs/SENSAAS-Flex-documentation.pdf)

**Website**: A web demo to use SENSAAS or SENSAAS-Flex is available at https://chemoinfo.ipmc.cnrs.fr/SENSAAS/index.html

**Tutorial**: These [videos](https://www.youtube.com/channel/UC3cjM1j8cQ-95ev0DNxRMOA) on YouTube provide tutorials


## Main features

 - Uses a surface-based representation that also displays pharmacophore features

 - Allows matchings and submatchings e.g. aligning a fragment on a large molecule

 - Provides the superimposition of 3D molecular graphs

 - Provides Similarity scores

 - Is free and open source


## Requirements

SENSAAS relies on the open-source library [Open3D](http://www.open3d.org). The current release of SENSAAS uses **Open3D version 0.12.0 along with Python3.7**.

Visit the following URL for using Python packages distributed via PyPI: [http://www.open3d.org/docs/release/getting_started.html](http://www.open3d.org/docs/release/getting_started.html) or conda: [https://anaconda.org/open3d-admin/open3d/files](https://anaconda.org/open3d-admin/open3d/files). For example, for windows-64, you can download *win-64/open3d-0.12.0-py37_0.tar.bz2*


## Virtual environment for python with conda (for Windows for example)

Install conda or Miniconda from [https://conda.io/miniconda.html](https://conda.io/miniconda.html)  
Launch Anaconda Prompt, then complete the installation:

	conda update conda
	conda create -n sensaas
	conda activate sensaas
	conda install python=3.7 numpy
	conda install perl

Once Open3D downloaded:
  
 	conda install open3d-0.12.0-py37_0.tar.bz2

Install RDKit (Open-Source Cheminformatics Software) that is compatible with Python 3.7:

	pip install rdkit-pypi
	
(Optional) Additional packages for visualization with PyMOL that is compatible with the current environnment:

  	conda install conda-forge::pymol-open-source==2.4.0
	
Retrieve and unzip sensaas-flex repository in your desired folder. See below for running the programs **sensaas.py** or **meta-sensaas.py** for using the rigid version or **sensaasflex.py** or **meta-sensaasflex.py** for using the flexible version. The directory containing executables is called sensaas-flex-main.

## Linux

Install:

1. Python3.7 and numpy
2. Open3D version 0.12.0 (more information at [http://www.open3d.org/docs/release/getting_started.html](http://www.open3d.org/docs/release/getting_started.html))

The open-Source Cheminformatics Software RDKit must be installed. More information on RDKit can be found at [https://www.rdkit.org/docs/Install.html](https://www.rdkit.org/docs/Install.html) 

3. RDKit

(Optional) Install additional packages for visualization with PyMOL:

4. PyMOL (a molecular viewer; more information at [https://pymolwiki.org](https://pymolwiki.org))
  
Retrieve and unzip sensaas-flex repository. The directory containing executables is called sensaas-flex-main.

## MacOS

	Not tested


## Run Sensaas
To align a Source molecule on a Target molecule, the syntax is:
	
	sensaas.py sdf molecule-target.sdf sdf molecule-source.sdf slog.txt optim
	
Example:

	sensaas.py sdf examples/IMATINIB.sdf sdf examples/IMATINIB_mv.sdf slog.txt optim
	
You may have to run the script as follows:

	python sensaas.py sdf examples/IMATINIB.sdf sdf examples/IMATINIB_mv.sdf slog.txt optim


Don't worry if you get the following warning from Open3D: "*Open3D WARNING KDTreeFlann::SetRawData Failed due to no data.*". It is observed with conda on windows.

Here, the source file IMATINIB_mv.sdf is aligned (**moved**) on the target file IMATINIB.sdf (**that does not move**). The output **tran.txt** contains the transformation matrix allowing the alignment of the source file (result in **Source_tran.sdf**). The **slog.txt** file details results with final scores of the aligned molecule (Source) on the last line. In the current example, the last line must look like:

	gfit= 1.000 cfit= 0.999 hfit= 0.996 gfit+hfit= 1.996

There are three different fitness scores but we only use 2 of them, gfit and hfit, to calculate gfit+hfit. More about [Fitness scores](https://github.com/SENSAAS/sensaas-py/blob/main/docs/index.rst#fitness-scores)

- gfit score estimates the geometric matching of point-based surfaces - it ranges between 0 and 1

- hfit score estimates the matching of colored points representing pharmacophore features - it ranges between 0 and 1

Thus, we calculate a hybrid score = gfit + hfit scores - **gfit+hfit ranges between 0 and 2**

   - A gfit+hfit score close to 2.0 means a perfect superimposition.
   - A gfit+hfit score close to 0.0 means that there are no similarities between molecular structures.


## Run meta-sensaas.py

**1. Virtual Screening** 

This script is suited for performing virtual screenings of sdf files containing several molecules (database mode). For example, if you want to process a sdf file containing several conformers for Target and/or Source. A similarity matrix is provided along with a sdf file that contains all aligned Sources. The syntax is:

 	meta-sensaas.py molecules-target.sdf molecules-source.sdf
 
**Example**
 
 The following example works with 2 files from the directory examples/

	meta-sensaas.py examples/IMATINIB.sdf examples/IMATINIB_parts.sdf
	
You may have to run the script as follows:

	python meta-sensaas.py examples/IMATINIB.sdf examples/IMATINIB_parts.sdf

Here, the source file IMATINIB_parts.sdf contains 3 substructures that are aligned (**moved**) on the target file IMATINIB.sdf (**that does not move**). Outputs are:
- the file **bestsensaas.sdf** that contains the best ranked aligned Source
- the file **catsensaas.sdf** that contains all aligned Sources
- the file **matrix-sensaas.txt** that contains gfit+hfit scores (rows=Targets and columns=Sources)

**Option -l**

When executing meta-sensaas.py, you can iterate the alignment by using the option -l:

	meta-sensaas.py molecules-target.sdf molecules-source.sdf -l 2

here the calculation is performed twice and only the highest-scoring alignment is selected.

**Post-processing**

Then, to ease the analysis of the results, the script utils/ordered-catsensaas.py can be used to generate files in descending order of score.

	utils/ordered-catsensaas.py matrix-sensaas.txt catsensaas.sdf

You may have to run the script as follows:

	python utils/ordered-catsensaas.py matrix-sensaas.txt catsensaas.sdf
	
or if you want to only retrieve solutions having a gfit+hfit score above a defined cutoff (here the cutoff 1.1 is chosen):

	python utils/ordered-catsensaas.py matrix-sensaas.txt catsensaas.sdf 1.1
	

- the file **ordered-catsensaas.sdf** contains all aligned Sources in descending order of score
- the file **ordered-scores.txt** contains gfit+hfit scores in descending order

**Option -s**

When executing meta-sensaas.py, you can also select the score type by using the option -s (default is the score of the Source (**-s source**)):

	meta-sensaas.py molecules-target.sdf molecules-source.sdf -s mean

here the mean of the score of the target and of the aligned source will be used to rank solutions and to fill matrix-sensaas.txt. The option '-s mean' is interesting to favor source molecules that have the same size of the Target. More about [Options](https://github.com/SENSAAS/sensaas-py/blob/main/docs/index.rst#Tutorials)


**2. Finding alternative alignments and Clustering**

This option allows to repeat in order to find alternative alignments when they exist as for example when aligning a fragment on a large molecule. The syntax is:

  	meta-sensaas.py target.sdf source.sdf -r 10
 
 here 10 alignments of the Source will be generated and clustered. Outputs are:
 
 - the file **sensaas-1.sdf** with the best ranked alignment - it contains 2 molecules: first is Target and second the aligned Source
 - the file **sensaas-2.sdf** (if exists) with the second best ranked alignment - it contains 2 molecules: first is Target and second the aligned Source
 - ...
 - file **cat-repeats.sdf** that contains all aligned Sources

**Example** 

The following example works with 2 files from the directory examples/

	meta-sensaas.py examples/VALSARTAN.sdf examples/tetrazole.sdf -r 20
	
You may have to run the script as follows:

	python meta-sensaas.py examples/VALSARTAN.sdf examples/tetrazole.sdf -r 20

As described in the publication, outputs are:

- sensaas-1.sdf contains the self-matching superimposition
- sensaas-2.sdf contains the bioisosteric superimposition
- sensaas-3.sdf contains the geometric-only superimposition

## Visualization 

You can use any molecular viewer. For instance, you can use PyMOL if installed (see optional packages) to load the Target and the aligned Source(s):

after aligning IMATINIB_mv.sdf on IMATINIB.sdf using sensaas.py:

	pymol examples/IMATINIB.sdf Source_tran.sdf 

or after executing meta-sensaas.py with several molecules:

	pymol examples/IMATINIB.sdf bestsensaas.sdf catsensaas.sdf
	
or after the post-processing:

	pymol examples/IMATINIB.sdf ordered-catsensaas.sdf


or after executing meta-sensaas.py with the repeat option (State 1 is Target and State 2 is the aligned Source):
	
	pymol examples/VALSARTAN.sdf sensaas-1.sdf


## Run sensaasflex.pl


## RMSD calculation

If two molecules are exactly the same then they possess the same 3D graph. In such case, the root-mean-square distance (RMSD) of corresponding atom pairs in 3D graphs can be calculated using two RDKit based tools (both are present in the folder utils).

**SymmFit** (author: Paolo Tosco; the code can be found at [https://www.mail-archive.com/rdkit-discuss@lists.sourceforge.net/msg04915.html](https://www.mail-archive.com/rdkit-discuss@lists.sourceforge.net/msg04915.html) which allows the minimization of RMSD value but only when the atoms in the two structure files are arranged in the same order. 

Syntax (‘in place’ RMSD calculation):

	rdkit-symmFitRMSD.py -r mol1.sdf mol2.sdf
	
Syntax (minimizes RMSD and writes transformation matrix called rdkit-tran.txt):

	rdkit-symmFitRMSD.py -s mol1.sdf mol2.sdf

**CalcLigRMSD** (author: Carmen Esposito; the code can be found at [https://github.com/cespos/rdkit/tree/add-CalcLigRMSD-for-prealigned-compounds/Contrib/CalcLigRMSD](https://github.com/cespos/rdkit/tree/add-CalcLigRMSD-for-prealigned-compounds/Contrib/CalcLigRMSD) which allows the calculation of RMSD value of  ‘in place’ structures (without minimization) whatever the arrangement of atoms in the two structure files.

In the present case study, we use that one because our atoms are not in the same order in the sdf files. We obtain:

	rdkit-CalcLigRMSD.py P04035-7.sdf Source_tran.sdf

RMSD= 3.36


## Licenses
SENSAAS code is released under [the 3-Clause BSD License](https://opensource.org/licenses/BSD-3-Clause)

## Copyright
Copyright (c) 2018-2021, CNRS, Inserm, Université Côte d'Azur, Dominique Douguet and Frédéric Payan, All rights reserved.

## Reference
[SENSAAS-Flex: a joint optimization approach for aligning 3D shapes and exploring the molecular conformation space](https://doi.org/10.1093/bioinformatics/btae105)

Bibtex format :

	@article{10.1093/bioinformatics/btae105,
    author = {Biyuzan, Hamza and Masrour, Mohamed-Akram and Grandmougin, Lucas and Payan, Frédéric and Douguet, Dominique},
    title = {SENSAAS-Flex: a joint optimization approach for aligning 3D shapes and exploring the molecular conformation space},
    journal = {Bioinformatics},
    volume = {40},
    number = {3},
    pages = {btae105},
    year = {2024},
    month = {02},
    doi = {10.1093/bioinformatics/btae105},
    url = {https://doi.org/10.1093/bioinformatics/btae105},
    eprint = {https://academic.oup.com/bioinformatics/article-pdf/40/3/btae105/56886864/btae105.pdf},
	}

[Douguet D. and Payan F., SenSaaS: Shape-based Alignment by Registration of Colored Point-based Surfaces, *Molecular Informatics*, **2020**, 8, 2000081](https://onlinelibrary.wiley.com/doi/full/10.1002/minf.202000081)
   
Bibtex format :

	@article{10.1002/minf.202000081,
	author 		= {Douguet, Dominique and Payan, Frédéric},
	title 		= {sensaas: Shape-based Alignment by Registration of Colored Point-based Surfaces},
	journal 	= {Molecular Informatics},
	volume 		= {39},
	number 		= {8},
	pages 		= {2000081},
	keywords 	= {Shape-based alignment, molecular surfaces, point clouds, registration, molecular similarity},
	doi 		= {https://doi.org/10.1002/minf.202000081},
	url 		= {https://onlinelibrary.wiley.com/doi/abs/10.1002/minf.202000081},
	eprint 		= {https://onlinelibrary.wiley.com/doi/pdf/10.1002/minf.202000081},
	year 		= {2020}
	}
	
