# binf8940
It has scripts for class BINF-8940

#!/bin/bash
#SBATCH --job-name=miniasm
#SBATCH --partition=batch
#SBATCH --ntasks=4
#SBATCH --mem=20gb
#SBATCH --time=24:00:00
#SBATCH --output=miniasm.%j.out
#SBATCH --error=miniasm.%j.err

cd $SLURM_SUBMIT_DIR
ml miniasm/0.3-foss-2016b
ml minimap2/2.10-foss-2016b

minimap2 -t 10 -x ava-pb SRR1200815_subreads.fastq SRR1200815_subreads.fastq|gzip -1 >Pacbio_aln_miniasm.paf.gz
miniasm -h 1000 -f SRR1200815_subreads.fastq Pacbio_aln_miniasm.paf.gz>Pacbio_aln_miniasm.gfa 2>Pacbio_aln_miniasm.log
perl -lane 'print ">$F[1]\n$F[2]" if $F[0] eq "S" Pacbio_aln_miniasm.gfa>Pacbio_consensus_miniasm.fasta

But finally this script did not work!!!!!!!!!!!!!!!!!!


#########CANU Script worked for me. I used teaching cluster and highmem queue;

#!/bin/bash
#SBATCH --job-name=j_canu
#SBATCH --partition=highmem
#SBATCH --ntasks=1
#SBATCH --mem=42gb
#SBATCH --time=60:00:00
#SBATCH --cpus-per-task=4
#SBATCH --output=canu.%j.out
#SBATCH --error=canu.%j.err
cd $SLURM_SUBMIT_DIR
export OMP_NUM_THREADS=4
ml canu/1.7-foss-2016b
canu -p canu -d assembly genomeSize=3.5m -pacbio-raw SRR1200815_subreads.fastq
