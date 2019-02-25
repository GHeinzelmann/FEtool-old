# FEtool.py

FEtool.py is a python tool for fully automated absolute binding free energy calculations. It encompasses the creation of the system, generation of parameters, preparation of the simulation files, and analysing the simulations to retrieve the binding free energy. By using the pmemd.cuda software from Amber, it is able to perform several calculations at a reduced computational cost.

Fetool.py can perform binding free energy calculations through a physical route, using the attach-pull-release (APR) method, as well as an alchemical route, using a double decoupling (DD) procedure in the presence of restraints. The program is compatible with AMBER18 (both DD and APR), as well as AMBER16 (APR only). It also requires a few installed programs such as VMD, which are listed in the next section. 

# Getting started

To use FEtool.py, download the files from this repository, which already contain an example for the 5uf0 structure. In order to perform all the steps from FEtool.py, the following programs must be installed and in your path:

VMD (Visual Molecular Dynamics) - https://www.ks.uiuc.edu/Development/Download/download.cgi?PackageName=VMD

Openbabel - http://openbabel.org/wiki/Category:Installation

MUSTANG v3.2.3 (MUltiple (protein) STructural AligNment alGorithm) - http://lcb.infotech.monash.edu.au/mustang/

AmberTools18 (http://ambermd.org/AmberTools.php)

The folder ./all-poses contains an example of system input files, both a docked receptor (hiTanimoto-5uf0_5uez_docked.pdb), as well as 9 poses generated using Autodock Vina, and converted to .pdb using AutodockTools. Below we show an example using these files, with all the necessary steps in the calculation. 

# Running a sample calculation

The calculations are divided in three steps, equilibration (folder ./equil), preparation (folder ./prep) and free energy calculation (folder ./fe). The input file with all the needed paramaters is called input.in, with the meaning of each parameter explained in more detail in the user guide. For our sample calculation, we will use the values already provided in the input.in file included in this distribution. The poses_list parameter sets up the calculation for the first 5 poses from Autodock Vina, included in the ./all-poses folder. 

## Equilibration

The equilibration starts from a docked pose or a crystal structure, gradually releasing restraints applied on the ligand, and then performing a final simulation with an unrestrained ligand. The necessary paramaters for the ligand are also generated in this stage, using the General Amber Force Field (GAFF), and the AM1-BCC charge model. Inside the program main folder, type:

python FEtool.py -i input.in -s equil

FEtool.py is compatible with python 2.7 versions. If you have anopther version, or you find that this command gives an error, you can use the python version included in the Ambertools distribution:

$AMBERHOME/miniconda/bin/python FEtool.py -i input.in -s equil

This command will create an ./equil folder, with one folder for each of the docked poses. In order to run the simulations, you can use the run-local.bash script (to run them locally), or the PBS-run script, which is designed to run in a queue system. Both of these files might have to be adjusted, depending on your computer or server configuration. The number of simulations depend on the release_eq array from the input file, which is described in more detail in the user guide. 



