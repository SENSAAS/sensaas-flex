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


