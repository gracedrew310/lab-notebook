# lab-notebook
Lab Notebook for Gen711 

4/21

cp /tmp/gen711_project_data/fastp-single.sh ~/fastp-single.sh
chmod +x ~/fastp-single.sh

./fastp-single.sh 150 /tmp/gen711_project_data/FMT_3/fmt-tutorial-demux-2 trimmed_fastqs
./fastp-single.sh 150 /tmp/gen711_project_data/FMT_3/fmt-tutorial-demux-1 trimmed_fastqs

4/28

qiime tools import --type "SampleData[SequencesWithQuality]" --input-format CasavaOneEightSingleLanePerSampleDirFmt --input-path trimmed_fastqs --output-path combined_fastqs

qiime cutadapt trim-single --i-demultiplexed-sequences combined_fastqs.qza --p-front TACGTATGGTGCA --p-discard-untrimmed --p-match-adapter-wildcards --verbose --o-trimmed-sequences combined_fastqs_with_primers 

--i-data combined_fastqs_with_primers.qza --o-visualization third_folder

qiime dada2 denoise-single --i-demultiplexed-seqs combined_fastqs_with_primers.qza --p-trunc-len 100 --p-trim-left 13 --p-n-threads 4 --o-denoising-stats denoising-stats.qza --o-table feature_table.qza --o-representative-sequences rep-seqs.qza

qiime metadata tabulate --m-input-file denoising-stats.qza --o-visualization denoising-stats.qzv

qiime feature-table tabulate-seqs --i-data rep-seqs.qza --o-visualization rep-seqs.qzv

no merge step needed because our files are already combined 

next step: qiime feature classifier 
