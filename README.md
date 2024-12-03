# Final Repository
Parts of this final repository are Copyright Joshua Rest. Do not post online.

### Introduction
The following code follows code given by Professor Joshua Rest during the duration of the Fall 2024 semester. His code was altered to fit a specific gene family and its evolution through time. All outputs are given within this repository, including all intermediates, labelled under "StartingGene Textfile Intermediates," as well as the pdfs that were eventually generated, labelled under "StartingGene Final PDFS."

The following was given as the very beginning of the project, being the FASTA file:
```
NP_060818.4
```

# Lab 3 code

The following was given during our third bioinformatics lab, altered to fit the FASTA file above.

```
ncbi-acc-download -F fasta -m protein "NP_060818.4"
```

```
blastp -db ../allprotein.fas -query NP_060818.4.fa -outfmt 0 -max_hsps 1 -out StartingGene.blastp.typical.out

```
blastp -db ../allprotein.fas -query NP_060818.4.fa  -outfmt "6 sseqid pident length mismatch gapopen evalue b

```
itscore pident stitle"  -max_hsps 1 -out StartingGene.blastp.detail.out
```

```
grep -c H.sapiens StartingGene.blastp.detail.out
```

```
awk '{if ($6< 1e-15)print $1 }' StartingGene.blastp.detail.out > StartingGene.blastp.detail.filtered.out
```

```
wc -l StartingGene.blastp.detail.filtered.out
```

```
grep -o -E "^[A-Z]\.[a-z]+" StartingGene.blastp.detail.filtered.out  | sort | uniq -c
```


# Lab 4 Code
The following was given during our fourth bioinformatics lab, altered to fit the FASTA file above.

```
seqkit grep --pattern-file ~/lab03-$MYGIT/StartingGene/StartingGene.blastp.detail.filtered.out ~/lab03-$MYGIT/allprotein.fas | seqkit grep -v -p "carpio" > ~/lab04-$MYGIT/StartingGene/StartingGene.homologs.fas
```

```
muscle -align ~/lab04-$MYGIT/StartingGene/StartingGene.homologs.fas -output ~/lab04-$MYGIT/StartingGene/StartingGene.homologs.al.fas
```

```
alv -kli  ~/lab04-$MYGIT/StartingGene/StartingGene.homologs.al.fas | less -RS (this one lags, do not speed through)
```

```
alv -kli --majority ~/lab04-$MYGIT/StartingGene/StartingGene.homologs.al.fas | less -RS (same with this one, only left to right)
```

```
Rscript --vanilla ~/lab04-$MYGIT/plotMSA.R  ~/lab04-$MYGIT/StartingGene/StartingGene.homologs.al.fas
```

```
alignbuddy  -al  ~/lab04-$MYGIT/StartingGene/StartingGene.homologs.al.fas (1095)
```

```
alignbuddy -trm all  ~/lab04-$MYGIT/StartingGene/StartingGene.homologs.al.fas | alignbuddy  -al (293)
```

```
alignbuddy -dinv 'ambig' ~/lab04-$MYGIT/StartingGene/StartingGene.homologs.al.fas | alignbuddy  -al (1050)
```

```
t_coffee -other_pg seq_reformat -in ~/lab04-$MYGIT/StartingGene/StartingGene.homologs.al.fas -output sim (TOTTOT 42.66)
```

```
alignbuddy -pi ~/lab04-$MYGIT/StartingGene/StartingGene.homologs.al.fas | awk ' (NR>2)  { for (i=2;i<=NF  ;i++){ sum+=$i;num++} }
     END{ print(100*sum/num) } ' (35.683)
```


# Lab 5 Code
The following was given during our fifth bioinformatics lab, altered to fit the FASTA file above.

```
sed 's/ /_/g'  ~/lab04-$MYGIT/StartingGene/StartingGene.homologs.al.fas | seqkit grep -v -r -p "dupelabel" >  ~/lab05-$MYGIT/StartingGene/StartingGene.homologsf.al.fas
iqtree -s ~/lab05-$MYGIT/StartingGene/StartingGene.homologsf.al.fas -bb 1000 -nt 2
nw_display ~/lab05-$MYGIT/StartingGene/StartingGene.homologsf.al.fas.treefile
Rscript --vanilla ~/lab05-$MYGIT/plotUnrooted.R  ~/lab05-$MYGIT/StartingGene/StartingGene.homologsf.al.fas.treefile ~/lab05-$MYGIT/StartingGene/StartingGene.homologsf.al.fas.treefile.pdf 0.4 15
gotree reroot midpoint -i ~/lab05-$MYGIT/StartingGene/StartingGene.homologsf.al.fas.treefile -o ~/lab05-$MYGIT/StartingGene/StartingGene.homologsf.al.mid.treefile
nw_order -c n ~/lab05-$MYGIT/StartingGene/StartingGene.homologsf.al.mid.treefile  | nw_display -
nw_order -c n ~/lab05-$MYGIT/StartingGene/StartingGene.homologsf.al.mid.treefile | nw_display -w 1000 -b 'opacity:0' -s  >  ~/lab05-$MYGIT/StartingGene/StartingGene.homologsf.al.mid.treefile.svg -
convert  ~/lab05-$MYGIT/StartingGene/StartingGene.homologsf.al.mid.treefile.svg  ~/lab05-$MYGIT/StartingGene/StartingGene.homologsf.al.mid.treefile.pdf
nw_order -c n ~/lab05-$MYGIT/StartingGene/StartingGene.homologsf.al.mid.treefile | nw_topology - | nw_display -s  -w 1000 > ~/lab05-$MYGIT/StartingGene/StartingGene.homologsf.al.midCl.treefile.svg -
convert ~/lab05-$MYGIT/StartingGene/StartingGene.homologsf.al.midCl.treefile.svg ~/lab05-$MYGIT/StartingGene/StartingGene.homologsf.al.midCl.treefile.pdf
```

# Lab 6
The following was given during our sixth bioinformatics lab, altered to fit the FASTA file above.

```
cp ~/lab05-$MYGIT/StartingGene/StartingGene.homologs.al.mid.treefile ~/lab06-$MYGIT/StartingGene/StartingGene.homologs.al.mid.treefile (didn't work)
cp ~/lab05-$MYGIT/StartingGene/StartingGene.homologsf.al.mid.treefile ~/lab06-$MYGIT/StartingGene/StartingGene.homologs.al.mid.treefile
java -jar ~/tools/Notung-3.0_24-beta/Notung-3.0_24-beta.jar -s ~/lab05-$MYGIT/species.tre -g ~/lab06-$MYGIT/StartingGene/StartingGene.homologsf.pruned.treefile --reconcile --speciestag prefix --savepng --events --outputdir ~/lab06-$MYGIT/StartingGene/ (didn't work)
java -jar ~/tools/Notung-3.0_24-beta/Notung-3.0_24-beta.jar -s ~/lab05-$MYGIT/species.tre -g ~/lab06-$MYGIT/StartingGene/StartingGene.homologsf.al.mid.treefile --reconcile --speciestag prefix --savepng --events --outputdir ~/lab06-$MYGIT/StartingGene/
grep NOTUNG-SPECIES-TREE ~/lab06-$MYGIT/StartingGene/StartingGene.homologs.al.mid.treefile.rec.ntg | sed -e "s/^\[&&NOTUNG-SPECIES-TREE//" -e "s/\]/;/" | nw_display -
python2.7 ~/tools/recPhyloXML/python/NOTUNGtoRecPhyloXML.py -g ~/lab06-$MYGIT/StartingGene/StartingGene.homologs.al.mid.treefile.rec.ntg --include.species
conda activate my_python27_env
python2.7 ~/tools/recPhyloXML/python/NOTUNGtoRecPhyloXML.py -g ~/lab06-$MYGIT/StartingGene/StartingGene.homologs.al.mid.treefile.rec.ntg --include.species
thirdkind -Iie -D 40 -f ~/lab06-$MYGIT/StartingGene/StartingGene.homologs.al.mid.treefile.rec.ntg.xml -o  ~/lab06-$MYGIT/StartingGene/StartingGene.homologs.al.mid.treefile.rec.svg
convert  -density 150 ~/lab06-$MYGIT/StartingGene/StartingGene.homologs.al.mid.treefile.rec.svg ~/lab06-$MYGIT/StartingGene/StartingGene.homologs.al.mid.treefile.rec.pdf
```


# Lab 8 Code 
The following was given during our eighth bioinformatics lab, altered to fit the FASTA file above.

```
sed 's/*//' ~/lab04-$MYGIT/StartingGene/StartingGene.homologs.fas -db ~/data/Pfam/Pfam -out ~/lab08-$MYGIT/StartingGene/StartingGene.rps-blast.out  -outfmt "6 qseqid qlen qstart qend evalue stitle" -evalue .0000000001
cp ~/lab05-$MYGIT/StartingGene/StartingGene.homologsf.al.mid.treefile ~/lab08-$MYGIT/StartingGene
Rscript  --vanilla ~/lab08-$MYGIT/plotTreeAndDomains.r ~/lab08-$MYGIT/StartingGene/StartingGene.homologsf.al.mid.treefile ~/lab08-$MYGIT/StartingGene/StartingGene.rps-blast.out ~/lab08-$MYGIT/StartingGene/StartingGene.tree.rps.pdf
mlr --inidx --ifs "\t" --opprint  cat ~/lab08-$MYGIT/StartingGene/StartingGene.rps-blast.out | tail -n +2 | less -S
cut -f 1 ~/lab08-$MYGIT/StartingGene/StartingGene.rps-blast.out | sort | uniq -c
cut -f 6 ~/lab08-$MYGIT/StartingGene/StartingGene.rps-blast.out | sort | uniq -c
awk '{a=$4-$3;print $1,'\t',a;}' ~/lab08-$MYGIT/StartingGene/StartingGene.rps-blast.out |  sort  -k2nr
cut -f 1,5 -d $'\t' ~/lab08-$MYGIT/StartingGene/StartingGene.rps-blast.out 
```