# FEPrepare

FEP prepare automates the set-up procedure for performing NAMD/FEP simulations. 

## Pre-requirements

Install [VMD](https://www.ks.uiuc.edu/Development/Download/download.cgi?PackageName=VMD)

Align your ligands and protein

## To execute the code follow the steps described below.

a) This script ensures consistency between the atoms of the ligands and it creates the following files: newligandA.txt, newligandB.txt, newligandArtf.txt, newligandBrtf.txt, newligandAprm.txt, newligandBprm.txt, how_many.

`python2 names3.py /path/to/reference.pdb /path/to/mutant.pdb /path/to/reference.rtf /path/to/mutant.rtf /path/to/reference.prm /path/to/mutant.prm` 

b) This script identifies the common atoms between reference and mutant ligands. It returns sortedB, which is the .rtf file of the mutant ligand sorted          according to the reference ligand.

`python2 merge2.py newligandArtf.txt newligandBrtf.txt > sortedB`

c) This script implements the dual topology methodology. It generates files where both ligands coexist (pdb: hybridpdb.txt, rtf: final.txt, prm: updated.prm)

`python2 dual2.py`

d) This script generates a complex file where the hybrid ligand generated above and the protein coexist.

`python2 complex.py /path/to/protein.pdb`

e) This script generates the psfgen file needed from VMD.

`python2 split_chains.py complex newligandA.txt`

### VMD commands:

f) `vmd -dispdev text -e psfgen`


g) if you would you like to add 150μΜ NaCl to your system:

`vmd -dispdev text -e vmd_prepare_complex_after_gui_autopsf_ionize > vmd_log.txt`
        
   otherwise:
   
`vmd -dispdev text -e vmd_prepare_complex_after_gui_autopsf > vmd_log.txt`

h) `vmd -dispdev text -e psfgen_solv`

i) if you would you like to add 150μΜ NaCl to your system:

`vmd -dispdev text -e vmd_prepare_ligand_after_gui_autopsf_ionize > vmd_log.txt`
   
   otherwise:
   
`vmd -dispdev text -e vmd_prepare_ligand_after_gui_autopsf > vmd_log.txt`

j) This script generated the necessary ionized_fep files for the simulation.

`python fep.py path/to/complex/ionized.fep path/to/solvent/ionized.fep path/to/complex/ionized.pdb path/to/solvent/ionized.pdb`


k) This script returns the coordinates of the center of the box.

`min-max.py complex/vmd_log.txt solvent/vmd_log.txt`

