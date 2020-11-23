## A [list](http://mmcif.wwpdb.org/docs/software-resources.html) of mmCIF parser from wwpdb 
### py-mmcif
* No tutorial 
* Problematic pip installation 
### Python PDBx 
* Used in [a wwpdb tutorial](http://mmcif.wwpdb.org/docs/sw-examples/python/html/)
* Incompatible with Python3
### CIFPARSE-OBJ  
* Used in [a wwpdb tutorial](http://mmcif.wwpdb.org/docs/sw-examples/cpp/html/)
* Lightning fast: 0.1s to process "1JGC"
* My favorite
### PyCifRW
* Pure python ver 3.0 lib. 
* Extremely slow: 30mins to process "1JGC". 
### BioPython
* Easy to install
* Fast
## PyCifRW vs BioPython
1. Get mmCIF of 1PWC and 1JGC to use PyCifRW and BioPython. 
2. Process them with the same function (see source code in [Appendix I](https://github.com/xg590/PDB_Cov/new/master#appendix-i))
```
mmcif_1PWC_BioPython = get_mmCIF_by_pdbid('1PWC', 'BioPython')
mmcif_1PWC_PyCifRW   = get_mmCIF_by_pdbid('1PWC', 'PyCifRW')
mmcif_1JGC_BioPython = get_mmCIF_by_pdbid('1JGC', 'BioPython')
mmcif_1JGC_PyCifRW   = get_mmCIF_by_pdbid('1JGC', 'PyCifRW')
```
* It takes 3 milliseconds to process mmcif_1PWC_BioPython
```
get_covalent_bond_record_in_mmcif(mmcif_1PWC_BioPython) 
```
* It takes 2.14 seconds to process mmcif_1PWC_PyCifRW
```
get_covalent_bond_record_in_mmcif(mmcif_1PWC_PyCifRW) 
```
* It takes 236 milliseconds to process mmcif_1JGC_BioPython
```
get_covalent_bond_record_in_mmcif(mmcif_1JGC_BioPython) 
```
* It will take 30 minutes to process mmcif_1JGC_PyCifRW
```
get_covalent_bond_record_in_mmcif(mmcif_1JGC_PyCifRW) 
``` 

## Appendix I
```
import io, os, urllib, Bio.PDB, CifFile

def get_mmCIF_by_pdbid(pdbid, parser):
    if parser=='BioPython': 
        r = urllib.request.urlopen(f'http://files.rcsb.org/download/{pdbid}.cif')  
        f = io.StringIO(r.read().decode()) 
        blc = Bio.PDB.MMCIF2Dict.MMCIF2Dict(f) 
    elif parser=='PyCifRW':
        blc = CifFile.ReadCif(f'http://files.rcsb.org/download/{pdbid}.cif').first_block() 
    return blc
    
def get_covalent_bond_record_in_mmcif(blc): 
    if "_struct_conn.id" not in blc: 
        return f'{blc["_entry.id"]}:struct_conn_IS_NOT_FOUND'
    cbr = ''
    for i, conn_type_id in enumerate(blc["_struct_conn.conn_type_id"]): 
        if conn_type_id == 'covale':
            covalent_bond_record = []
            for j, label_entity_id in enumerate(blc['_atom_site.label_entity_id']): 
                if  blc["_atom_site.label_asym_id"    ][j] == blc["_struct_conn.ptnr1_label_asym_id"     ][i] and \
                    blc["_atom_site.auth_asym_id"     ][j] == blc["_struct_conn.ptnr1_auth_asym_id"      ][i] and \
                    blc["_atom_site.label_comp_id"    ][j] == blc["_struct_conn.ptnr1_label_comp_id"     ][i] and \
                    blc["_atom_site.auth_comp_id"     ][j] == blc["_struct_conn.ptnr1_auth_comp_id"      ][i] and \
                    blc["_atom_site.label_seq_id"     ][j] == blc["_struct_conn.ptnr1_label_seq_id"      ][i] and \
                    blc["_atom_site.auth_seq_id"      ][j] == blc["_struct_conn.ptnr1_auth_seq_id"       ][i] and \
                    blc["_atom_site.pdbx_PDB_ins_code"][j] == blc["_struct_conn.pdbx_ptnr1_PDB_ins_code" ][i]: 
                    for k, id_ in enumerate(blc["_entity.id"]):
                        if label_entity_id == id_: 
                            covalent_bond_record.append(blc["_struct_conn.ptnr1_label_asym_id"     ][i])
                            covalent_bond_record.append(blc["_struct_conn.ptnr1_auth_asym_id"      ][i])
                            covalent_bond_record.append(blc["_struct_conn.ptnr1_label_comp_id"     ][i])
                            covalent_bond_record.append(blc["_struct_conn.ptnr1_auth_comp_id"      ][i])
                            covalent_bond_record.append(blc["_struct_conn.ptnr1_label_seq_id"      ][i])
                            covalent_bond_record.append(blc["_struct_conn.ptnr1_auth_seq_id"       ][i])
                            covalent_bond_record.append(blc["_struct_conn.pdbx_ptnr1_PDB_ins_code" ][i])
                            covalent_bond_record.append(blc["_struct_conn.ptnr1_label_atom_id"     ][i])
                            covalent_bond_record.append(blc["_struct_conn.pdbx_ptnr1_label_alt_id" ][i]) 
                            covalent_bond_record.append(blc["_entity.type"][k])  
                            break
                    break
            for j, label_entity_id in enumerate(blc['_atom_site.label_entity_id']):
                if  blc["_atom_site.label_asym_id"    ][j] == blc["_struct_conn.ptnr2_label_asym_id"     ][i] and \
                    blc["_atom_site.auth_asym_id"     ][j] == blc["_struct_conn.ptnr2_auth_asym_id"      ][i] and \
                    blc["_atom_site.label_comp_id"    ][j] == blc["_struct_conn.ptnr2_label_comp_id"     ][i] and \
                    blc["_atom_site.auth_comp_id"     ][j] == blc["_struct_conn.ptnr2_auth_comp_id"      ][i] and \
                    blc["_atom_site.label_seq_id"     ][j] == blc["_struct_conn.ptnr2_label_seq_id"      ][i] and \
                    blc["_atom_site.auth_seq_id"      ][j] == blc["_struct_conn.ptnr2_auth_seq_id"       ][i] and \
                    blc["_atom_site.pdbx_PDB_ins_code"][j] == blc["_struct_conn.pdbx_ptnr2_PDB_ins_code" ][i]:
                    for k, id_ in enumerate(blc["_entity.id"]):
                        if label_entity_id == id_: 
                            covalent_bond_record.append(blc["_struct_conn.ptnr2_label_asym_id"     ][i])
                            covalent_bond_record.append(blc["_struct_conn.ptnr2_auth_asym_id"      ][i])
                            covalent_bond_record.append(blc["_struct_conn.ptnr2_label_comp_id"     ][i])
                            covalent_bond_record.append(blc["_struct_conn.ptnr2_auth_comp_id"      ][i])
                            covalent_bond_record.append(blc["_struct_conn.ptnr2_label_seq_id"      ][i])
                            covalent_bond_record.append(blc["_struct_conn.ptnr2_auth_seq_id"       ][i])
                            covalent_bond_record.append(blc["_struct_conn.pdbx_ptnr2_PDB_ins_code" ][i])
                            covalent_bond_record.append(blc["_struct_conn.ptnr2_label_atom_id"     ][i])
                            covalent_bond_record.append(blc["_struct_conn.pdbx_ptnr2_label_alt_id" ][i]) 
                            covalent_bond_record.append(blc["_entity.type"][k])  
                            break                    
                    break 
            cbr += ','.join(covalent_bond_record)
            cbr += '\n'
    return cbr
```
