# Genomic diversity pipeline

Inbreeding creeps up quietly. Pedigrees only show part of it, and in many flocks they are incomplete or recorded too shallowly. With SNP data you can measure it directly, and that is what this pipeline does.

I built it for breeding decisions: which animals carry the most useful variation, which pairs are too closely related, and how diversity is changing over time. I used it on a Texel flock with 910 genotyped animals and 27 autosomes.

## What it measures

- **Heterozygosity**, as a basic picture of how much variation each animal carries.
- **Runs of homozygosity (ROH) and F_ROH**, the stretches of the genome inherited identical from both parents. They give a direct, genome-based estimate of inbreeding, and also hint at how recent it is.
- **Genomic relationships** between animals, from a genomic relationship matrix (GRM).
- **Animal ranking** by genetic profile, to spot the individuals that are most valuable for keeping diversity.
- **Genetic compatibility** between potential mates, to support mating plans that keep inbreeding under control.

## What you get

Three main tables, plus plots (distributions of heterozygosity and F_ROH, ROH patterns along the chromosomes, relationship heatmaps and rankings). The idea is that a breeder can read the output and make a decision, without needing to be a geneticist.

| File | What it is |
|---|---|
| `animal_genetic_profile.csv` | One row per animal. Its genetic profile: heterozygosity, inbreeding from ROH (F_ROH) and its position in the ranking. Use it to see which animals carry the most useful variation and which are the most inbred. |
| `all_genetic_pairs.csv` | One row per pair of animals. How genetically related and compatible each pair is. It is the full picture, so it can be large. |
| `candidate_matings.csv` | A shortlist of suggested matings drawn from the pairs above, favoring combinations that keep inbreeding low. This is the file to open when planning matings. |

## Running it

The pipeline takes SNP genotypes and, optionally, a pedigree `.txt` (animal, sire, dam, year, sex), and writes everything to an output folder. When the genotypes were renumbered by BLUPF90, an ID map built from the `renadd*.ped` file brings the original names back.

```bash
python genomic_diversity_pipeline.py
```

## Keep in mind

ROH and relationship results depend on SNP density and on how well the markers are cleaned, so a quality control step comes first. Small samples make the estimates noisier.

This is part of the SelectaGen toolkit, developed as a commercial project, so the code stays private. The visualizations and documentation are public, and I'm happy to talk if you'd like to try it on your own flock.
