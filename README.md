# DISC1 In Silico Knockout Study

**Can a protein-interaction network predict what happens when you knock out DISC1?**

A beginner-level, fully in silico project by **Shamreen [add last name]**, built from public databases only. No wet-lab work was done. This is a learning project, and every result comes from existing public data and tools.

---

## The question

DISC1 (Disrupted in Schizophrenia 1) was first linked to mental illness through a balanced chromosomal translocation that segregated with schizophrenia, bipolar disorder and depression in a Scottish family (St Clair et al., 1990). I asked:

1. What does DISC1 look like at the sequence and structure level?
2. How would a CRISPR knockout be designed, and would it break the gene?
3. If I predict knockout effects only from DISC1's protein-interaction network, how well does that match the phenotypes actually measured in knockout mice (IMPC)?

## Short answer

- The network and GO-enrichment analysis **recovered the neurological signal**: a strong neurodevelopment, cytoskeletal-transport, cAMP and dopamine signature.
- It **missed six other affected systems** (eye, immune, hematopoietic, renal, cardiovascular, growth). Only **2 of 8** significantly impacted systems were predicted.
- Both correct predictions were the neurological ones, which also match what the literature already suggests. This was not a blind prediction.
- Takeaway: an interaction network captures a gene's molecular neighbourhood but cannot anticipate pleiotropic effects in unrelated tissues.

## Workflow

| Stage | What I did | Tools |
|---|---|---|
| 1. Target | Confirmed the gene record (MGI:2447658) and pulled IMPC phenotype hits | MGI, IMPC |
| 2. Sequence and structure | Recorded RefSeq IDs, downloaded transcript FASTA, mapped coiled-coil domains, viewed the predicted structure | NCBI Gene, Ensembl, UniProt (Q9NRI5), AlphaFold (AF-Q9NRI5-F1) |
| 3. Knockout modelling | Located mouse exons 2-3, designed guide RNAs, tested a hypothetical frameshift | Ensembl, CRISPOR, Ensembl VEP |
| 4. Network analysis | Mapped DISC1 interactors at high confidence and checked them against the literature | STRING v12, KEGG |
| 5. Phenotype prediction | Ran GO enrichment on the interactor list | g:Profiler |
| 6. Validation | Compared predicted vs actual IMPC phenotypes | IMPC |

## Key findings

**Structure.** The AlphaFold model has a low average pLDDT (about 63). Nearly half the protein (47.1%) is very low confidence, which matches UniProt's disordered-region annotation and the view of DISC1 as a largely unstructured scaffold with four coiled-coil regions (aa 366-394, 452-505, 602-666, 802-830).

**Guide RNA.** On mouse exon 2, the top SpCas9 guide (position 524, forward) had an MIT specificity score of 99 and a CFD specificity score of 98. A one-base insertion at its cut site was called a **frameshift variant** by VEP on the canonical transcript (ENSMUST00000098311.11). An early mistake is worth noting: CRISPOR first defaulted to mm10 while VEP used GRCm39, so I re-ran the design on mm39 to fix the coordinates. The guide itself did not change.

**Network.** Top STRING partners included NDE1, NDEL1, PDE4B, PDE4A, PCNT, GSK3B, DIXDC1, DRD2, PAFAH1B1 (LIS1) and DTNBP1. Three of four literature-predicted partners (NDE1, NDEL1, LIS1) plus PDE4B were present. FEZ1 and Kalirin-7/TRIO were not in the top 10 at the default threshold.

**Enrichment.** The strongest GO term was *telencephalon development* (adjusted p about 1e-6), alongside microtubule transport, centrosome, dopamine signalling and cAMP catabolism terms.

**Predicted vs actual.** IMPC tested 24 physiological systems for the Disc1 knockout and found 8 significantly impacted. See `disc1_predicted_vs_actual_table.html`.

## Limitations

- **Not blind.** I knew the literature before predicting, so the neurological match is expected.
- **Small interactor list.** Enrichment used only 10 interactors, which limits statistical power.
- **Species mix.** Structure and network used human DISC1; knockout design and validation used mouse.
- **Allele differences.** The IMPC allele (`Disc1<em1(IMPC)Mbp>`) is a CRISPR allele, which differs from the exon 2-3 knockout and the ENU point mutants in the older literature.
- **Phenotype table caveat.** Several gross-pathology rows in `Disc1-significant-phenotypes.tsv` show P = 0 and effect size 1 exactly as exported from IMPC. These are categorical calls, and P = 0 should not be read as a literal zero probability. Check IMPC documentation before relying on them.
- **Low-confidence structure.** The AlphaFold model is weak for much of the protein, so I did not draw structural conclusions beyond domain layout.

## Repository contents

| File | Description |
|---|---|
| `AF-Q9NRI5-F1-model_v6.pdb` | AlphaFold model of human DISC1 |
| `disc1_alphafold_structure.png` | Image of the model |
| `ENST00000439617.8.fasta` | Human MANE Select transcript |
| `ENSMUST00000098311.11.fasta` | Mouse canonical transcript |
| `uniprotkb_2026_08_24.fasta.gz` | The four DISC1 coiled-coil segments from UniProt |
| `guides_mm10-chr8-125087453-125088448.xls` | CRISPOR guide output (run on mm10, later confirmed on mm39) |
| `Disc1-significant-phenotypes.tsv` | Significant IMPC phenotypes |
| `disc1_predicted_vs_actual_table.html` | Predicted vs actual comparison |
| `DISC1_project_tracker.xlsx` | Full stage-by-stage log with notes and citations |

## Reproduce it

Everything here uses free web tools: MGI, IMPC, NCBI Gene, Ensembl and VEP, UniProt, AlphaFold DB, CRISPOR, STRING, KEGG and g:Profiler. The tracker records each query, setting and date, so every step can be repeated. Database versions are noted in the tracker (for example STRING v12, Ensembl release 2026-07, and the g:Profiler version string).

## Key references

- St Clair et al. (1990), *Lancet* 336:13-16
- Hikida et al. (2007), *PNAS* 104:14501-14506
- Bradshaw et al. (2008), *Biochem Biophys Res Commun* 377:1091-1096
- Soares et al. (2011), *ACS Chem Neurosci* 2:609-632
- Szklarczyk et al. (2023), *Nucleic Acids Res* 51:D638-D646
- Jumper et al. (2021), *Nature* 596:583-589

The full reference list is in the `References` tab of the tracker.

## About

Built by Shamreen, a bioinformatics learner. Feedback and corrections are very welcome.

**Contact:** [add email or LinkedIn]
