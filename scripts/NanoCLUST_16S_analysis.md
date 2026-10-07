
# Community composition analysis with NanoCLUST

We will use the analysis pipeline [NanoCLUST](https://github.com/genomicsITER/NanoCLUST) to characterise the relative abundance of species in our kefir community.

NanoCLUST takes as input the sequencing reads as fastq files and returns a csv file with the relative abundances of the species which it identified to be present in the sample.

For more information on how NanoCLUST works have a look at the [Applications Note](https://academic.oup.com/bioinformatics/article/37/11/1600/5929694).


First, you will need to set up a git plugin called git-lfs with:
```bash
git lfs install
```

Next, you will need to clone the NanoCLUST repo

```bash
NCROOT="${HOME}/NanoCLUST/"
git clone https://git.embl.de/grp-zimmermann-kogadeeva/NanoCLUST.git ${NCROOT}
```
We also need to create variables to indicate your group and hub. Replace <your_hub> with `HD_hub` or `BA_hub` depending on where you are, and <your_group> with `Group1` or `Group2`.
```bash
HUB=<your_hub> 
GROUP=<your_group>
```

 ## 2. Specify your barcode and locate the data
To save time, we will split up the analysis of the different barcodes amongst you. Set the variable barcode to the barcode that was assigned to you (replace 05 in the code junk with your barcode number):

```bash
barcode=barcode05
```


The data are on a shared directory to which we create a variable as well. For the further analyis we use the reads that passed quality control. You will find them in `fastq_pass`. We change directory to there.
```bash
DATAROOT="/g/teachinglab/data/EMBO_MCD2026/16S/"
DTROOT="${DATAROOT}${HUB}/${GROUP}"
cd $DTROOT/fastq_pass
```
The files containing the reads have the ending .fastq.gz.

## 3. Prepare data for analysis
The files containing the reads have the ending .fastq.gz.

The sequencer partitions the reads for each barcode into files with 4000 reads each or provides all reads in one file per barcode. You first have to concatenate the individual files before you analyse them with NanoCLUST.

To do so, first navigate to the folder that contains the single files
```bash
cd $DTROOT/fastq_pass/$barcode
```
Then, concatenate all files in the folder and save the new file one level up in the path under the name `<your_barcode>_concat_fastq.gz` like this:


```bash
cat * > "${DATAROOT}${HUB}/NanoporeConcat/${barcode}_concat.fastq.gz"
```

## 5. Run NanoCLUST
To run NanoCLUST, first navigate to the root path of the copy of NanoCLUST on your VM.
```bash
cd $NCROOT
```
Define variables for the path to the data and the path that NanoCLUST should write its output to (relative to the NanoCLUST root path)


```bash
data_dir="${DATAROOT}${HUB}/NanoporeConcat"
output_dir="${DATAROOT}/${HUB}/OutputData/${barcode}"
```

Now you are ready to run NanoCLUST. Note that the command below can fail initially, in which case it should be sufficient to start the command a second time.
```bash
pixi run \
nextflow run main.nf -profile conda --umap_set_size 130000 \
                                    --cluster_sel_epsilon 0.25\
                                    --min_cluster_size 50 \
                                    --reads "${data_dir}/${barcode}_concat.fastq.gz" \
                                    --db db/16S_ribosomal_RNA --tax db/taxdb \
                                    --outdir ${output_dir}
```

When NanoCLUST is finished you will find your results in `$output_dir/<your_barcode>/`, where $barcode corresponds to the barcode you got assigned.


## (For completeness, but not to be executed 6. Postprocessing of NanoCLUST results for visualization )


To prepare for the visualization of the NanoCLUST results, the NanoCLUST results for all individual barcodes need to be combined in one file.

**One of the trainers will do that for you** 

```bash
input_dir="${DATAROOT}/${HUB}/OutputData/"
output_file="${DATAROOT}/${HUB}/OutputData/all_barcodes.csv"

> ${output_file}
echo "sample,tax,taxid,rel_abundance" > ${output_file}

for tax in F G O S; do 
    echo ${tax}
    while read f; do
        sample=$(basename ${f} | sed "s/\.fastq_${tax}\.csv//g")
        tail -n +2 ${f} | sed "s/^/${sample},${tax},/g" >> ${output_file}
    done< <(find ${input_dir} -name "*.fastq_${tax}.csv" | sort)
done
```

To visualize the complete prepared dataset, continue with
[`16S_analysis_and_visualization.Rmd`](16S_analysis_and_visualization.Rmd).


