# Spatial Transcriptomics Pipeline

"" ADD TRAVIS BUILD STATUS HERE ""
"" ADD TEST COVERAGE FLAG HERE ""

ST Pipeline contains the tools and scripts needed to process and analyze the raw files generated with the Spatial Transcriptomics method in FASTQ format. 

The following files/parameters are required :
- FASTQ files (Read 1 containing the spatial information and the UMI and read 2 containing the real sequence) 
- A genome index generated with STAR 
- An annotation file in GTF or GFF format
- The file containing the barcodes and array coordinates (look at the folder "ids" and chose the correct one. 
- A name for the dataset

The ST pipeline has multiple parameters mostly related to trimming, mapping and annotation but generally the default values are good enough. You can see a full description of the parameters typing "st_pipeline_run.py --help" after you have installed the ST pipeline.

Basically what the ST pipeline does is :
- Quality trimming (read 1 and read 2) :
	- Remove low quality bases
	- Sanity check
	- Check quality UMI
	- Remove artifacts (PolyT, PolyA, PolyG and PolyC)
	- Check for AT content
	- Discard reads with a minimum number of bases of that failed any of the checks above
- Mapping with STAR (only read 2)
- Demultiplexing with taggd (https://github.com/JoelSjostrand/taggd) (only read 1)
- Keep reads (read 2) that contain a valid barcode and are correctly mapped. 
- Annotate the reads with htseq-count
- Group annotated reads by barcode(spot position) and gene to get a read count
- In the grouping/counting only unique molecules (UMIs) are kept. 

You can see a graphical more detailed description of the workflow in the documents workflow.pdf and workflow_extended.pdf

The output will be a JSON file containing
the aggregated transcripts (spot coordinates, gene and count) and a BED file containing the transcripts (Read name, coordinate, gene, etc..)
The ST pipeline will also output a log file and a stats file with useful information.

**Installation**

We recommend you install a virtual environment like Pyenv or Anaconda before you install the pipeline. 
The ST Pipeline works with python 2.7.

First clone the repository or download a tar/zip from the releases section. 
Access the cloned repository folder or the folder where the tar/zip file has been decompressed. 

To install the pipeline type then

    python setup.py build
    python setup.py install

To run a test type (you need internet connection to run the tests)

    python setup.py test

To see the different options type 

    st_pipeline_run.py --help
    
**Example**

An example run would be

	st_pipeline_run.py --ids ids_file.txt --molecular-barcodes --ref-map path_to_index --log-file log_file.txt --output-folder /home/me/results --ref-annotation annotation_file.gtf file1.fastq file2.fastq 

**License**

The ST pipeline is open source under the BSD license which means that you can use it, change it and re-distribute but you must always refer to our license (see LICENSE and AUTHORS).

**Dependencies** 

The ST Pipeline depends on some Python packages that will
be automatically installed during the installation process. 
You can see them in the file dependencies.txt

**Requirements**

The ST Pipeline requires to have installed
in the system the aligner STAR (minimum version 2.5.0) :
https://github.com/alexdobin/STAR

The ST Pipeline requieres between
32GB and 64GB of RAM depending
on the size of the data. 
It is recommended to run it
in a system with at least 8 cpu cores. 

