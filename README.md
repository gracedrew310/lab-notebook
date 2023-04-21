# lab-notebook
Lab Notebook for Gen711 

4/21

cp /tmp/gen711_project_data/fastp-single.sh ~/fastp-single.sh
chmod +x ~/fastp-single.sh

./fastp-single.sh 150 /tmp/gen711_project_data/FMT_3/fmt-tutorial-demux-2 trimmed_fastqs
./fastp-single.sh 150 /tmp/gen711_project_data/FMT_3/fmt-tutorial-demux-1 trimmed_fastqs

qiime tools import \
   --type "SampleData[PairedEndSequencesWithQuality]"  \
   --input-format CasavaOneEightSingleLanePerSampleDirFmt \
   --input-path trimmed_fastqs \
   --output-path trimmed_fastqs/chimme_files\

qiime cutadapt trim-paired \
    --i-demultiplexed-sequences <path to the file from step 2> \
    --p-cores 4 \
    --p-front-f <the forward primer sequence> \
    --p-front-r <the reverse primer sequence> \
    --p-discard-untrimmed \
    --p-match-adapter-wildcards \
    --verbose \
    --o-trimmed-sequences <path to an output directory>/<name for the output files>.qza

qiime demux summarize \
--i-data <path to the file from step above> \
--o-visualization  <path to an output directory>/<a name for the output files>.qzv 
