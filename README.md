# Modidec - RNA modification detector and classifier
ModiDeC is a customizable neural network to identify RNA modifications from Oxford Nanopore Technology (ONT) based direct RNA sequencing data. ModiDeC combines LSTM and newly designed inception-res-net blocks for multi-modification-classification. ModiDec is composed of three Epi2ME integratable tools (data curation, network training and analysis). It allows researchers to train the multi-modification-classification model on synthetic RNA strands mimicking physiologically relevant motifs and modification patterns on transcripts of interest. The latter can be utilized to investigate modification ratios of transcripts derived from physiological data. During the data curation step of ModiDec, data derived from ONT based direct RNA sequencing experiments (RNA002 or RNA004) can be preprocessed to suit the succeeding model training step. During model training the network can be trained on the preprocessed data to optimally learn motif and modification patterns of the transcript of interest. The trained model can then be used in the analysis step of ModiDec to investigate modification ratios in physiological derived data.

![Modidec schema](./figures/ModiDec_Epi2ME_schema.png)

Here the model training part is implemented. Please visit [wf-modidec_data-curation](https://github.com/Nanopore-Hackathon/wf-modidec_data-curation) and [wf-modidec_analysis](https://github.com/Nanopore-Hackathon/wf-modidec_analysis) to complete the toolset. 

## Requirements

Install dependencies on your system:
   -  Install [`Epi2Me Desktop`](https://labs.epi2me.io) (v5.1.14 or later)
   -  Install [`Miniconda`](https://conda.io/miniconda.html)
   -  Install [`Docker`](https://conda.io/miniconda.html)
   -  Install [`Nextflow`](https://www.nextflow.io/docs/latest/getstarted.html#installation) (`>=23.10.0`)
   -  Install samtools and minimap
   -  Make sure your [nvidia GPU drivers](https://docs.nvidia.com/datacenter/tesla/driver-installation-guide/#ubuntu-installation) are installed and functional.
   -  Install the [nvidia-container-toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html) to enable GPU usage from within a docker container. 

Import the workflow in Epi2Me:
   -  Open Epi2Me
   -  Navigate to Launch
   -  Click on import workflow -> Import from Github
   -  Paste https://github.com/Nanopore-Hackathon/wf-modidec_training into the Edit line
   -  Click on Download
   -  The repository should now appear on the workflow list in Launch

## Instructions for Network Training

### Preparation
Network training can be started with tensors in npz format obtained by the [wf-modidec_data-curation](https://github.com/Nanopore-Hackathon/wf-modidec-data_curation) workflow. Once data curation is performed the produced data should be split in a training and validation folder with distinct and therefore independent npz files. Independence of the training and validation datasets is important to prevent overfitting of the model. As a recommendation the training folder should contain ~80% of the data whereas the validation folder should contain ~20% of the data. Navigate to Launch on Epi2Me and click on the wf-modidec_data-curation workflow. Press Launch on the right side of the page and you will be linked to the menu. 

### Input Options 
1. Training Data Folder (Path to the folder containing training data as mentioned above)
2. Validation Data Folder (Path to the folder containing training data as mentioned above)


### Training Options

1. Batch size (Define how many training instances should be used for training the network at once. Larger batch sizes require more memory (VRAM) of the GPU.)
2. Kmer model (Which chemistry did you use ? RNA004 uses 9-mers -> 9 and RNA002 2-mers -> 2)
3. Epochs (For how many iterations should the network be trained ? 1 Epoch corresponds to one iteration through all batches obtained form training dataset. For each iteration the training dataset will be shuffled to prevent overfitting. Several iterations are recommended. Out of experience ~4 Epochs often converge. It is also recommended to use a reasonable amount of training instances for a given motif. We recommend to use at least 30000 instances per training motif and modification pattern.)
4. Model name (The name of the output model)

### Training Output
The output of the network training will be written to the output directory determined by Epi2ME. The path to the output directory can be found in the Logs section of the launched pipeline. (Parameters: out_dir folder)
The Reports section of the pipeline provides two result plots. The plot "report_accuracy" shows the training (blue) and validation (red) accuracy performance. "Report_loss" shows the training (blue) and validation (red) loss.


### Acknowledgements

This project has been initiated in a [hackathon event](https://epi2me.nanoporetech.com/mainz-hackathon/) in 2024. We want to thank the Institute for [Quantitative and Computational Biosciences (IQCB)](https://iqcb.uni-mainz.de), the [RNA modification and processing consortium](https://www.trr319-rmap.de/) and [Holzer Scientific Consulting](https://www.holzerscientific.com/) for funding and/or organizing this event. Moreover, we want to thank [Oxford Nanopore Technologies](https://nanoporetech.com/) for teaching workflow integration into Epi2ME in an on-site event. Finally, the long-read community kickstarted the development of the here presented workflow and we are very thankful for your participation and engagement.
 
![Modidec schema](./figures/example_accuracy_modidec_training.png)
![Modidec schema](./figures/example_loss_modidec_training.png)

