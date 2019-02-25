# FEtool.py

FEtool.py is a python tool for fully automated absolute binding free energy calculations. It encompasses the creation of the system, generation of parameters, preparation of the simulation files, and analysing the simulations to retrieve the binding free energy. By using the pmemd.cuda software from Amber, it is able to perform several calculations at a reduced computational cost.

It is able to perform the calculations through a physical route, using the attach-pull-release (APR) method, as well as an alchemical route, using a double decoupling (DD) procedure in the presence of restraints. The program is compatible with AMBER18 (both DD and APR), as well as AMBER16 (APR only). It also requires a few installed programs such as VMD, which are explained in detail below. 

# Getting started

To use FEtool.py, download the files from this repository, which already contain an example for the 5uf0 structure. In order to perform all the steps from FEtool.py, the following programs must be installed and in your path:

VMD (Visual Molecular Dynamics) - https://www.ks.uiuc.edu/Development/Download/download.cgi?PackageName=VMD

Openbabel - http://openbabel.org/wiki/Category:Installation

MUSTANG v3.2.3 (MUltiple (protein) STructural AligNment alGorithm) - http://lcb.infotech.monash.edu.au/mustang/

AmberTools18 (http://ambermd.org/AmberTools.php)



