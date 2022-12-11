### CovBinderInPDB: A Structure-Based Covalent Binder Database
* My PhD advisor Yingkai Zhang and I authored a covalent binder database. X. Guo, Y. Zhang, <i>J. Chem. Inf. Model.</i> <b>2022</b>, DOI: [10.1021/acs.jcim.2c01216](https://doi.org/10.1021/acs.jcim.2c01216)
* I developed a [website](https://yzhang.hpc.nyu.edu/CovBinderInPDB/) that visulized the database.
* This depo archives the codes I used to curate the database.
* I began the project PDBcov since 2017. Because I alone was working on it, the curation dragged for years. Before its publication, other authors has take the name "CovPDB" although their method is not comprehensive. Anyway, I still keep using PDBcov throughout this depo, although the database is published under the name of CovBinderInPDB. 
### File
```
PDBcov
|--1.Genesis.ipynb               # How I get covalent bond records (pdb_cov_20220118.csv), recover the adduct, and recover the binder
|--2.Drugbank.ipynb              # Connect PDBcov with DrugBank through InChIKey matching
|--3.SIFTS.ipynb                 # Get UniProt ID for each covalently modified residue
|--4.PostProcessing.ipynb        # 
|--5.BindingDB.ipynb             # Connect PDBcov with BindingDB through InChIKey and UniProt ID matching
|--6.DrawReactionInSVG.ipynb     # Draw reaction pattern for the website
|--7.ForWeb.ipynb                # Other data to prepare the website 
|--Run.ipynb                     # Run script 1~7
|--SI_maker.ipynb                # Create supportting information of the publication
|--data                          # 
   |--db_id.json                 # Track of used binder identifier in PDBcov 
   |--doi.json                   # DOIs of original publications that report PDB structures (data source: mmCIF, RCSB PDB, Google)
   |--drugbank.csv               # Processed result of drugbank
   |--error.svg                  # So obvious...
   |--faulty_adduct.txt          # I manually checked these adducts and confirmed they are not covalent modifications (artifacts?)
   |--knowledge_base.csv         # Curation note
   |--pdb_cov_20220118.csv       # Covalent Bond Records
   |--pdb_title_chain_name.json  # Data source is mmCIF
   |--sifts.matched.csv          # For each covalently modified residue, there is a record in UniProt Database.
   |--warhead_smarts.json        # Handcrafted SMARTS for each warhead        
```
