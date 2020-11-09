
## Mapping using hisat2
```bash
## example command
hisat2 -p 4 --summary-file hisat.summary  -x /home/lakhanp/database/Mouse/GRCm38.99/hisat2_index/Mus_musculus.GRCm38.99.dna.primary_assembly.chr.fa -1 /home/lakhanp/analysis/core_projects/39_ZhuBO_RNAseq5_ApoE_diet/raw_data/20345_1.fq.gz -2 /home/lakhanp/analysis/core_projects/39_ZhuBO_RNAseq5_ApoE_diet/raw_data/20345_2.fq.gz | samtools view -bS - | samtools sort  -O bam -o M20345_hisat2.bam
```


## Create BAM file index
```bash
for i in `cat sample_name.list`
do
cd $i
printf "samtools index %s_hisat2.bam
samtools flagstat %s_hisat2.bam > alignment.stats\n\n" $i $i >> generalJob.sh
cd ..
done
```


## Run stringTie in estimation mode
```bash
for i in `cat sample_name.list`
do
cd $i
printf "##Run stringtie: just counting the transcripts and no assembly
stringtie %s_hisat2.bam -p 4 -e -B -G /home/lakhanp/database/Mouse/GRCm38.99/annotation/Mus_musculus.GRCm38.100.chr.gtf -o stringTie_%s/%s.gtf
error_exit \$?\n\n" $i $i $i >> generalJob.sh
cd ..
done
```
