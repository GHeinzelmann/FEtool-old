# FEtool.py

FEtool.py is a python tool for fully automated absolute binding free energy calculations. It encompasses the creation of the system, generation of parameters using AM1-BCC/GAFF, preparation of the simulation files, and analysis to retrieve the binding free energy. By using the pmemd.cuda software from Amber, it is able to perform several calculations at a reduced computational cost.

FEtool.py can perform binding free energy calculations by a physical route, through the attach-pull-release (APR) method, as well as an alchemical route, using a double decoupling (DD) procedure in the presence of restraints. The program is compatible with AMBER18 (both DD and APR), as well as AMBER16 (APR only). It also requires a few installed programs such as VMD, which are listed in the next section. 

# Getting started

To use FEtool.py, download the files from this repository, which already contain an example for the 5uf0 structure. In order to perform all the steps from FEtool.py, the following programs must be installed and in your path:

VMD (Visual Molecular Dynamics) - https://www.ks.uiuc.edu/Development/Download/download.cgi?PackageName=VMD

Openbabel - http://openbabel.org/wiki/Category:Installation

MUSTANG v3.2.3 (MUltiple (protein) STructural AligNment alGorithm) - http://lcb.infotech.monash.edu.au/mustang/

AmberTools16 or later - http://ambermd.org/AmberTools.php

The folder ./all-poses contains an example of system input files, with a docked receptor from the 5uez crystal structure (hiTanimoto-5uf0_5uez_docked.pdb), as well as 9 poses for the ligand with the 5uf0 crystal structure (pose0.pdb to pose8.pdb). The docking files were generated and converted to .pdb using Autodock Vina and AutodockTools, using the protocol from the CELLP challenge (https://github.com/drugdata/d3r/wiki). The ./all-poses folder also contains the crystal structure file for 5uf0. Below we show an example of using these files to calculate the standard binding free energies the top 5 docked poses and the crystal structure, with all the necessary steps in the calculation. 

# Running a sample calculation

The calculations are divided in three steps, equilibration (folder ./equil), preparation (folder ./prep) and free energy calculation (folder ./fe). The input file with all the needed parameters is called input.in, with the meaning of each parameter explained in more detail in the user guide. For our sample calculation, we will use the values already provided in the input.in file included in this distribution. The poses_list parameter sets up the calculation for the first 5 poses from Autodock Vina, all in the ./all-poses folder. The input.in file can be modified to perform the calculations in the 5uf0 crystal structure, by changing the calc_type option to "crystal", the celpp_receptor option to "5uf0", and the ligand_name option to "89J", which is the ligand residue name in the 5uf0 pdb structure. 

## Equilibration

The equilibration starts from the docked complex or the crystal structure, gradually releasing restraints applied on the ligand and then performing a final simulation with an unrestrained ligand. The necessary parameters for the ligand are also generated in this stage, using the General Amber Force Field (GAFF), and the AM1-BCC charge model. To run this step, inside the program main folder type:

python FEtool.py -i input.in -s equil

FEtool.py is compatible with python 2.7 versions. If you have another version, or you find that this command gives an error, you can use the python version included in the Ambertools distribution:

$AMBERHOME/miniconda/bin/python FEtool.py -i input.in -s equil

This command will create an ./equil folder, with one folder for each of the docked poses. In order to run the simulations inside each folder, you can use the run-local.bash script (to run them locally), or the PBS-run script, which is designed to run in a queue system. Both of these files might have to be adjusted, depending on your computer or server configuration. The number of simulations depend on the release_eq array from the input file, which is described in more detail in the user guide. 

## Preparation

The second stage starts from the equilibrated system, rebuilding the latter and redefining the dummy/anchor atoms and the restraints for use in the free energy calculation. If the APR method is to be used, the ligand in this stage is pulled from the binding site towards the solvent, generating states that will be used in the APR procedure. If only double decoupling will be performed, no pulling is needed, and only one simulation is performed with the ligand in the bound state in the presence of restraints. This is defined in the input.in file, with more details in the user guide. To run this stage, type in the program main folder:

python FEtool.py -i input.in -s prep

, or use the miniconda option as shown in the previous section. It is possible that the ligand has left the binding site during equilibration, due to an unstable docked pose. In this case, the preparation is not performed for this pose, and a warning message appears after running the command above. The same way as the equil stage, there is a folder for each pose (given that it did not unbind during equilibration) in the ./prep directory, and the simulations can be run locally or in a server such as tscc. Once the preparation simulations are concluded, the systems are ready for the binding free energy calculations. 

## Free energy calculation 

### Simulations

Starting from the states created in the prep stage, we can now perform the binding free energy calculations, which will be located inside the ./fe folder. In this example we will perform both APR and DD, so the results can be directly compared using the two routes. Again in the program main folder, type:

python FEtool.py -i input.in -s fe

For each pose, a folder will be created inside ./fe, and inside there will be three folders: ./pmf, ./restraints and ./dd. The restraints folder contains all the simulations needed for the application/removal of restraints. The pmf folder contains the folders for the "pull" process of APR, calculated using umbrella sampling. The dd folder contains the coupling/decoupling of the ligand electrostatic/LJ interactions, both in the binding site and in bulk. A script called run-all.bash, inside the run_files folder, can be used to run these simulatons quickly using the PBS scripts. A similar script can be written to do the same, using your particular running protocol. 

### Analysis

Once all of the simulations are concluded, it is time to process the output files and obtain the binding free energies using the two methods. Here a few parameters can be set concerning the analysis, such as using TI or MBAR for double decoupling, number of blocks for block data analysis, and the Gaussian weights if TI is used for double decoupling. More details on these parameters can be found in the user guide. Inside the main folder type:

python FEtool.py -i input.in -s analysis

You should see a ./Results directory inside each ./fe/pose folder, containing the final results using the two methods in the Results.dat file. This folder also contains the results for each of the chosen data blocks, which is useful to check for convergence. This fully automated procedure can be readily applied for any other ligand that binds to the second BRD4 bromodomain, and with minimal adjustments it can be extended to several other proteins.




