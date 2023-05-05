# lab-notebook
Lab Notebook for Gen711 

4/21

cp /tmp/gen711_project_data/fastp-single.sh ~/fastp-single.sh
chmod +x ~/fastp-single.sh

./fastp-single.sh 150 /tmp/gen711_project_data/FMT_3/fmt-tutorial-demux-2 trimmed_fastqs
./fastp-single.sh 150 /tmp/gen711_project_data/FMT_3/fmt-tutorial-demux-1 trimmed_fastqs

4/28

qiime tools import --type "SampleData[SequencesWithQuality]" --input-format CasavaOneEightSingleLanePerSampleDirFmt --input-path /tmp/gen711_project_data/FMT_3/fmt-tutorial-demux-1 --output-path demux1
qiime tools import --type "SampleData[SequencesWithQuality]" --input-format CasavaOneEightSingleLanePerSampleDirFmt --input-path /tmp/gen711_project_data/FMT_3/fmt-tutorial-demux-2 --output-path dumux2

qiime cutadapt trim-single --i-demultiplexed-sequences combined_fastqs.qza --p-front TACGTATGGTGCA --p-discard-untrimmed --p-match-adapter-wildcards --verbose --o-trimmed-sequences cutadapt1
qiime cutadapt trim-single --i-demultiplexed-sequences combined_fastqs.qza --p-front TACGTATGGTGCA --p-discard-untrimmed --p-match-adapter-wildcards --verbose --o-trimmed-sequences cutadapt2 

qiime demux summarize --i-data /home/users/ged1022/cutadapt1.qza --o-visualization  /home/users/ged1022/summarize1.qzv
qiime demux summarize --i-data /home/users/ged1022/cutadapt2.qza --o-visualization  /home/users/ged1022/summarize2.qzv

qiime dada2 denoise-single --i-demultiplexed-seqs /home/users/ged1022/cutadapt1.qza --p-trunc-len 100 --p-trim-left 13 --p-n-threads 4 --o-denoising-stats denoising-stats1.qza --o-table feature_table.qza --o-representative-sequences rep-seqs1.qza
qiime dada2 denoise-single --i-demultiplexed-seqs /home/users/ged1022/cutadapt2.qza --p-trunc-len 100 --p-trim-left 13 --p-n-threads 4 --o-denoising-stats denoising-stats2.qza --o-table feature_table.qza --o-representative-sequences rep-seqs1.qza

qiime metadata tabulate --m-input-file /home/users/ged1022/denoising-stats1.qza --o-visualization /home/users/ged1022/denoising-stats1.qzv
qiime metadata tabulate --m-input-file /home/users/ged1022/denoising-stats2.qza --o-visualization /home/users/ged1022/denoising-stats2.qzv

qiime feature-table tabulate-seqs --i-data /home/users/ged1022/rep-seqs1.qza --o-visualization /home/users/ged1022/rep-seqs1.qzv
qiime feature-table tabulate-seqs --i-data /home/users/ged1022/rep-seqs.qza --o-visualization /home/users/ged1022/rep-seqs2.qzv

qiime feature-table merge-seqs --i-data /home/users/ged1022/rep-seqs1.qza --i-data /home/users/ged1022/rep-seqs.qza --o-merged-data /home/users/ged1022/merged.rep-seqs.qza

qiime feature-classifier classify-sklearn --i-classifier /tmp/gen711_project_data/reference_databases/classifier.qza --i-reads /home/users/ged1022/merged.rep-seqs.qza --o-classification /home/users/ged1022/FMT-taxonomy.qza

qiime taxa barplot --i-table /home/users/ged1022/feature_table.qza --i-taxonomy /home/users/ged1022/FMT-taxonomy.qza --o-visualization /home/users/ged1022/barplot-1.qzv

qiime taxa barplot --i-table <output path>/feature_table-2.qza --i-taxonomy <output path>/FMT-taxonomy.qza --o-visualization <output path>/barplot-2.qzv
