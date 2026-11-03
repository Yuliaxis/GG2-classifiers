# GG2-classifiers

Pre-trained Naive Bayes taxonomic classifiers for **QIIME 2 (qiime2-2026.10)**, built on the **Greengenes2 2024.09** backbone and trained specifically for the primer pairs used in our 16S rRNA studies.

## Available classifiers

All files are in the [`classifier/`](classifier/) folder.

| File | Region | Primers | Study | Size |
|---|---|---|---|---|
| `GG2-202409-V4-classifier.qza` | V4 | 515F / 806R (Caporaso et al., 2011) | IRTA | 47 MB |
| `GG2-202409-V4-YB-classifier.qza` | V4 | 515F-Y / 806R-B (Parada et al., 2016; Apprill et al., 2015) | SOWS | 47 MB |
| `GG2-202409-V3V4-classifier.qza` | V3–V4 | 341F / 805R (Klindworth et al., 2013) | Korean pigs, UPV | 68 MB |

### Primer sequences (5'→3')

| Primer | Sequence |
|---|---|
| 515F | `GTGCCAGCMGCCGCGGTAA` |
| 806R | `GGACTACHVGGGTWTCTAAT` |
| 515F-Y | `GTGYCAGCMGCCGCGGTAA` |
| 806R-B | `GGACTACNVGGGTWTCTAAT` |
| 341F | `CCTACGGGNGGCWGCAG` |
| 805R | `GACTACHVGGGTATCTAATCC` |

## How they were built

1. Full-length 16S rRNA reference sequences and taxonomy from **Greengenes2 2024.09**.
2. In silico extraction of the target region with each primer pair.
3. Training of a Naive Bayes classifier (`qiime feature-classifier fit-classifier-naive-bayes`) on the extracted region.

### In silico extraction summary

Out of 337,506 Greengenes2 reference sequences:

| Classifier | Extracted | % extracted | Median amplicon length |
|---|---|---|---|
| V4 (IRTA) | 336,170 | 99.60% | 253 bp |
| V4-YB (SOWS) | 336,162 | 99.60% | 253 bp |
| V3–V4 | 335,076 | 99.28% | 422 bp |

- 334,776 sequences (99.2% of the database) were extracted by all three primer pairs.
- The two V4 primer pairs share 99.99% of their extracted sequences.

## Usage

Download the classifier that matches the primers used in your study and run:

```bash
qiime feature-classifier classify-sklearn \
  --i-classifier GG2-202409-V4-classifier.qza \
  --i-reads rep-seqs.qza \
  --o-classification taxonomy.qza

qiime metadata tabulate \
  --m-input-file taxonomy.qza \
  --o-visualization taxonomy.qzv
```

To download a single file from the command line:

```bash
wget https://github.com/Yuliaxis/GG2-classifiers/raw/main/classifier/GG2-202409-V4-classifier.qza
```

## Important notes

- **Use the classifier with the same primers as your sequencing data.** A classifier trained on a different region will give poorer taxonomic assignments.
- **QIIME 2 version compatibility:** these classifiers were trained with **qiime2-2026.10**. Naive Bayes classifiers depend on the scikit-learn version, so they may fail to load in other QIIME 2 releases. If that happens, they need to be retrained with your version.

## References

- McDonald D, et al. (2024). Greengenes2 unifies microbial data in a single reference tree. *Nature Biotechnology* 42, 715–718.
- Bolyen E, et al. (2019). Reproducible, interactive, scalable and extensible microbiome data science using QIIME 2. *Nature Biotechnology* 37, 852–857.
- Bokulich NA, et al. (2018). Optimizing taxonomic classification of marker-gene amplicon sequences with QIIME 2's q2-feature-classifier plugin. *Microbiome* 6, 90.
- Caporaso JG, et al. (2011). Global patterns of 16S rRNA diversity at a depth of millions of sequences per sample. *PNAS* 108, 4516–4522.
- Parada AE, et al. (2016). Every base matters: assessing small subunit rRNA primers for marine microbiomes with mock communities, time series and global field samples. *Environmental Microbiology* 18, 1403–1414.
- Apprill A, et al. (2015). Minor revision to V4 region SSU rRNA 806R gene primer greatly increases detection of SAR11 bacterioplankton. *Aquatic Microbial Ecology* 75, 129–137.
- Klindworth A, et al. (2013). Evaluation of general 16S ribosomal RNA gene PCR primers for classical and next-generation sequencing-based diversity studies. *Nucleic Acids Research* 41, e1.

