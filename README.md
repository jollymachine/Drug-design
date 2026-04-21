Project Overview: This project involves the retrieval, binding pocket prediction, and structural preparation of the BACE complex for in silico drug discovery

### Retrieval of 3D structure
The proein chosen as an example for assessment is BACE-1 (Beta Secretase-1) which functions in the 
The 3D structure of Beta secretase (BACE) protein imported from Protein Data Bank into PyMol. The pdb id 5QCU was chosen becuase of its resolution 1.95 A. Lower resolutions (<2A) helps to visualize individual atoms clearly. 2-2.5A provides good detail but >3A makes it harder to place atoms precisely. PyMol is a tool used for visualizing secondary structures, identify binding pockets visually.

![3D structure](./images/5QCU_pymol.png)

*Figure 1: Beta-secretase protein structure visualized in PyMol.*

### Determining binding pockets
The binding pockets on the protein was detected using DoG site scorer program in Protein Plus (https://proteins.plus/). The pdb structure was uploaded to Proteins plus and binding pocket with p_val = 0 was identified in the chain A. 

![Binding Pocket Prediction](./images/binding_pocket.png)

*Figure 2: Binding site detection in Chain A using the DoGsite scorer on Proteins Plus.*

### Protein clean-up
Next, the protein pdb structure was uploaded to Chimera to prepare the protein for docking. Chains B and C, the co-crystallized ligands and the water molecules were removed. The structure was saved in a dockprep format. This preparation is important to add any missing atoms, assign charges, remove bound ligands and water molecules.

### Protein Preparation Results
| Before Cleaning | After Dockprep |
| :---: | :---: |
| ![Before](./images/Before_dockprep.png) | ![After](./images/After_dockprep.png) |
| *Initial 5QCU complex with water, ligands, and extra chains.* | *Final prepared Chain A isolated and cleaned in Chimera.* |

### Assessing physicochemical properties
Physicochemical properties like molecular weight, lipophilicity, GI aborption, pharmacokinetics determine the drug-likeness of a compound. Three compounds were used for testing.
1) Galantamine- an acetylcholinesterase inhibitor used to treat dementia. It is isolated from the plant Galanthis nivalis
2) Coumarin - an aromatic compound isolated from plants like beans and lavender. It has anti-inflammatory functions
3) Geraniol - another aromatic compound found in the essential oils of many plants. It has anti-inflammatory functions

The SMILES structures of these compounds was obtained from PubChem (See data folder). The SMILES was fed into SWISS-ADME server to determine ADMET properties. 

### Druglikeness screening comparison
| Property | Coumarin | Galantamine | Geraniol |
| :--- | :---: | :---: | :---: |
| **Molecular Weight** | 146.14 g/mol | 287.35 g/mol | 154.25 g/mol |
| **Log P (Lipophilicity)** | 1.82 | 1.92 | 2.74 |
| **GI Absorption** | High | High | High |
| **Lipinski Rule Violations** | 0 | 0 | 0 |
| **BBB Permeant** | Yes | Yes | Yes |

More details are in the images folder containing screenshots and in the data folder containing the csv files.

### Docking using PyRx
Docking was performed using PyRx. The structures of the ligands were downloaded in sdf format from PubChem and opened in PyRx. Using Vina wizard, docking was performed on the binding site residues determined from DoG site scorer. The definition of the search space (Grid Box):
```
Grid Center: 
  X: 28.6668
  Y: 6.4795
  Z: 20.2951

Grid Size (Dimensions):
  X: 31.8003 Å
  Y: 26.6275 Å
  Z: 25.1095 Å
  ```
Docking gives a numerical value which represents the binding affinity between the ligand and protein. PyRx calculates binding energy in kcal/mol. It measures the Gibbs free energy of binding, which is the amount of energy released when the ligand binds to the protein. Since it is energy released, it has a negative sign. The lower the value, the lower the energy required for binding and the stronger the affinity.

* -9.0 kcal/mol: very strong binding suggesting a high affinity hit for further investigation
* -7.0 to -8.0 kcal/mol: Good binding. Most drug-like molecules are in this range
* -5.0 to -6.0 kcal/mol: Moderate binding
* -4.0 kcal/mol: Very weak or no binding

### Docking results 

| Name | Ligand | Target | Binding energy kcal/mol |
| :--- | :---: | :---: | :---: |
| Galantamine | 9651_mmff94_E=69.95 | 5QCU_dockprep | -8.3 |
| Coumarin | 323_mmff94_E=33.85 | 5QCU_dockprep | -6.0 |
| Geraniol | 637566_mmff94_E=17.24 | 5QCU_dockprep | -5.8 |

### Interpretation
The results show that Galantamine, the clincally used drug demonstrates strong binding affinity. It has the strongest binding affinity compared to the plant based compounds - coumarin and geraniol, which show moderate binding affinity. Still their drug-likeness properties makes them good candidates for further investigation.

### 2D visualization

The 2D interaction diagram was generated to visualize the non-covalent bond network between the ligand and the BACE1 active site. Galantamine forms a hydrogen bond with Gln73 and Phe108, and a Pi-Alkyl bond with Tyr91. The 3D interaction diagram shows a "lock and key" binding. The aromatic rings of the ligand are positioned near the grey hydrophobic regions, while the polar groups are touching the green "Acceptor" clouds. It uses hydrophobic effects for stability and hydrogen bonds for specificity.

| Galantamine 2D Interaction | Galantamine 3D Interaction |
| :---: | :---: |
| ![Galantamine 2D Interaction Diagram](./images/5QCU_dockprep-Galantamine_9651_interaction.png) | ![Galantamine 2D Interaction Diagram](./images/5QCU_9651_3D_interaction_crop.png) |

Coumarin forms a Pi-Pi stacked bond with Tyr71 and Phe108. The 3D interaction diagram shows that Coumarin exhibits significant pocket occupancy, aligning with both donor and acceptor regions of the BACE1 catalytic site, suggesting a stable binding orientation

| Coumarin 2D Interaction | Coumarin 3D Interaction |
| :---: | :---: |
| ![Coumarin 2D Interaction Diagram](./images/5QCU_dockprep-Coumarin_323_interaction.png)| ![Coumarin 2D Interaction Diagram](./images/5QCU_323_3D_interaction_crop.png) |

Geraniol 3D interaction diagram indicated minor geometric distortions likely stemming from the SMILES-to-PDBQT conversion process. While docking scores remained competitive, the structural integrity of this lead requires further force-field refinement (e.g., using AMBER or CHARMM) to ensure a stable bioactive conformation.

| Geraniol 3D Interaction |
| :---: |
| ![Geraniol 3D Interaction Diagram](./images/5QCU_637566_3D_interaction_crop.png)|

### ADMET properties
ADMET stands for Absorption, Distribution, Metabolism, Excretion, Toxicity. Critical parts of ADMET readout are:
1) Absorption - Testing in colon cancer cell line Caco2, human intestinal absorption (HIA), P-glycoprotein substrate
2) Distribution - Blood brain barrier (BBB), Plasma protein binding (PPB),
3) Metabolism – CYP450 (1A2, 2C9, 2D6, 3A4) are enzymes that clean out toxins in the liver. An inhibitor of these enzymes would cause accumulation of toxic-drugs. A substrate would cause faster clearance.
4) Excretion – Half-life in the blod (T-1/2), clearance (CL)
5) Toxicity – hERG blockade is am important indicator of blockade of heart rhythms,  Ames mutagenicity indicates ability to mutate DNA, carcinogenicity, drug induced liver toxicity (DILI)

Lipinski rule of five consists of four rules. These four rules have thresholds set in the multiples of five, hence the name. The rules are:
* Lipophilicity (LogP) Octanol:water partition coefficient must be less than 5: If a drug likes oil too much, it get stuck in the cell membrane. 
* Molecular weight must be less than 500 Da (MW): Larger compounds wont get through the blood brain barrier
* Hydrogen bond donors must be less than 5 (HBD): Groups like OH and NH based groups tend to form hydrogen bonds with water. More HBD means the compounds will likely never leave water
* Hydrogen bond acceptors (HBA) must be less than 10: This is the count of oxygen and nitrogen atoms. Too many HBAs also preclude ability to pass through a cell membrane 

For a lead, 0-1 violations are accepted while 2+ violations are considered a low quality lead. But there are some exceptions to this rule such as the antibiotics & antifungals, PROTACs and peptides. 
A Veber’s rule is considered alongside Lipinski rule. Veber’s rule talks about the number of possible rotating bonds (<=2) and polar surface area (<= 140A֯ 2).

ADMET properties were assessed using ADMETLab 3.0

| Name | PubChem CID | MW | lipinski score | Caco2 | HIA | BBB | t0.5 | CYP2D6-sub | hERG | 
| :--- | :---: | :---: | :---: | :--- | :---: | :---: | :---: | :--- | :---: |
| Coumarin | 323 | 146.04 | 0 | -4.10965 | 0.002706 | 0.00684 | 0.777017 | 0.003664 | 0.101742 |
| Geraniol | 637566 | 154.14 | 0 | -4.42556 | 0.926977 | 0.036598 | 1.497328 | 0.40996 | 0.048563 |
| Galantamine | 9651 | 287.15 | 0 | -4.31261 | 0.057924 | 0.998645 | 5.088033 | 0.982549 | 0.35779 |

### Alphafold predicted structure vs cleaned protein
The Alphafold predicted structure of 5QCU is in aquamarine and the cleaned 5QCU protein is in orange color. The two proteins have an RMSD of 0.28 A֯ which is very identical. 
RMSD measures the average distance between atoms on the alpha-carbon backbone. Distances less than 1 A are considered nearly identical. 


## Acknowledgments
This project was completed as part of the 3-Week Virtual Research Training in CADD (https://www.linkedin.com/company/the-insilico-lab/posts/?feedView=all) organized by **The InSilico Lab**. 
