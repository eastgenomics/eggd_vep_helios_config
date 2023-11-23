# eggd_vep_helios_config

This repo contains a JSON configuration file for the implementation of VEP for the TSO500 assay.

## What does the JSON config do?

Configuration file required to annotate a vcf using [Variant Effect Predictor](https://github.com/Ensembl/ensembl-vep) implementation [eggd_vep](https://github.com/eastgenomics/eggd_vep).

A variable level of annotation can be achieved by different combinations of custom annotations and vep plugins, in addition to the required VEP resources.

## What does the JSON config version contain?

This json file provides information about annotations,plugins, required fields and the genome version.

* Genome build: GRCh37
* VEP required files:
  * vep_v105.0.tar.gz
  * homo_sapiens_refseq_vep_105_GRCh37.tar.gz
  * Homo_sapiens.GRCh37.dna.toplevel.fa.gz
  * Homo_sapiens.GRCh37.dna.toplevel.fa.gz.fai
  * Homo_sapiens.GRCh37.dna.toplevel.fa.gz.gzi
  * hs37d5.fasta-index.tar.gz
* Custom Annotation sources:
  * clinvar_20231112_b37.vcf.gz
  * gnomad.genomes.r2.0.1.sites.noVEP_normalised_decomposed_PASS.vcf.gz
  * gnomad.exomes.r2.0.2.sites.noVEP_normalised_decomposed_PASS.vcf.gz
* Plugin annotations:
  * REVEL (version May 2022)
    * revel_b37.tsv.gz
  * CADD (v1.6)
    * cadd_whole_genome_SNVs_GRCh37.tsv.gz
    * gnomad.genomes.r2.1.1.indel.tsv.gz
    * InDels_GRCh37.tsv.gz
  * SpliceAI
    * spliceai_scores.masked.snv.hg19.vcf.gz
    * spliceai_scores.masked.indel.hg19.vcf.gz

## Notes
  How to check the names of all the files included in the config:

```bash
config_file=you_file_name.json

# Get the Vep Resources filenames
for file in  $(jq -r ' .vep_resources | .[]' $config_file);
do dx describe $file --json | jq -r '.name';
done

# Get Custom Annotation filenames
for file in  $(jq -r ' .custom_annotations[]|.resource_files[]|.file_id' $config_file);
do dx describe $file --json | jq -r '.name';
done

# Get Plugin Annotation filenames
for file in  $(jq -r ' .plugins[]|.resource_files[]|.file_id' $config_file);
do dx describe $file --json | jq -r '.name';
done
```
