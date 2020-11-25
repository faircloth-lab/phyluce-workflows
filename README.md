# phyluce-workflows

## Purpose

These are additional workflows that we use with [phyluce](https://github.com/faircloth-lab/phyluce/tree/master/bin) to accomplish a variety of redundant tasks (e.g. read mapping, contig correct, etc.).

## Installing

1. Check out the code somewhere

    ```bash
    git clone https://github.com/faircloth-lab/phyluce-workflows
    ```

2. Navigate to that location and install dependencies from conda environment

    ```bash
    cd <path to wherever you cloned>
    conda env create -f environment.yml
    ```

3. Activate conda environment

    ```bash
    conda activate phyluce-workflows
    ```

4. Navigate to directory that contains the workflow you want to run, edit config file appropriately, and run snakemake:
        
    ```bash
    cd mapping
    # <edit config file or run using example>
    # change cores to suite your system
    snakemake --cores 1
    ```

5. Right now, the remaining workflows are built off of the `mapping` workflow, meaning that you need to run it first, regardless of other things that you run.

## Workflows

* **Mapping**: Map raw reads to species specific contigs.  Uses `bwa`, `samtools`, `pandas`, and a custom script to map reads, perform duplicate detection and marking, and compute coverage across contigs by a few metrics.

* **Contig-correction**: Using pre-existing BAM files (perhaps from `Mapping`), use `bcftools` and depth of coverage information to call SNPs in contigs, remove bad calls, and output consensus of results.  BAM files may also have been run through [mapDamage](https://ginolhac.github.io/mapDamage/).  Filters for removal are `--IndelGap 5 --SnpGap 5 --exclude 'QUAL<20 | DP<5 | AN>2'`, and sequences that are reduced to `< 50 bp` after filtering.

* **Phasing**: Using pre-existing BAM files (perhaps from `Mapping`), use `samtools` to phase SNPs.  This actually phases the SNPs using samtools, produces `0.BAM` and `1.BAM` files for each haplotype, then converts those to FASTA data representing each haplotype using [pilon](https://github.com/broadinstitute/pilon/wiki).  Probably still needs a little work to deal with low coverage FASTAs that are produced. 