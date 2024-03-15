---
title: "Pipeline to Annotate the Transcriptome of <em>Pongo pygmaeus</em>"
# <img align="right" src="/images/drugdiscoveryml.png" style="width:100px;height:100px" />
excerpt: This comprehensive pipeline was designed for annotating the transcriptome of the Bornean orangutan, Pongo pygmaeus, with a focus on the utilization of Iso-seq data. It aims to provide a generalizable solution applicable to other mammalian eukaryotes.
collection: Projects
---

# <span style="color:#007ea7"> Introduction

<p style='text-align: justify;'> 
This pipeline is focused on annotating the transcriptome of the Bornean orangutan, <em>Pongo pygmaeus</em>. In addition to the main goal, one of our subgoals was to design a pipeline that had a more generalizable solution. Iso-seq data was chosen because it uses long reads which improves accuracy and completeness, and eliminates the need for extra assembly steps compared to other RNA sequencing methods. This however limits the pipeline to organisms where Iso-seq data is available.
</p>

<p style='text-align: justify;'> 
The workflow encompasses various stages, including data pre-processing, aligning Iso-seq data with the reference genome, obtaining exome boundaries, identifying coding sequences, and functional annotation using tools like SRA Toolkit, FastQC, Minimap2, Samtools, StringTie, GFFREAD, TransDecoder, and Blast2GO via OmicsBox.
</p>

# <span style="color:#007ea7"> Data

<p style='text-align: justify;'>
The pipeline begins with the retrieval and pre-processing of RNA-seq data from the Sequence Read Archive (SRA) using the SRA Toolkit and quality assessment with FastQC. FASTQC can reveal any issues with the raw NGS data. If errors are found, steps can be taken to filter out the problematic reads in the data. A per base sequence quality plot was obtained, where the y-axis indicates the quality score, the x-axis the position in the read, and the blue line the average quality score for the nucleotide in that position (See Figure 1). The overall quality score for each read was quite good, which indicated the sequencing data was reliable enough to proceed with. 
</p>

    ![Figure 1: Quality control results obtained from FASTQC tool](/images/QCResults.jpg)

<p style='text-align: justify;'>
Subsequently, the Iso-seq data is aligned with the reference genome using Minimap2, followed by the conversion of the alignment file to the .bam format using Samtools. The exome boundaries are obtained using StringTie, and the coding sequences are identified with TransDecoder. This tool scans transcript sequences to identify possible ORFs that have the potential to encode proteins based on several factors such as length and start and stop codons and is capable of filtering out false positive ORFs. These sequences are then subjected to functional annotation using Blast2GO via OmicsBox. By building on the regular BLAST tool, Blast2GO not only provides the homologous sequence searching feature that BLAST provides, but it also provides additional tools for the functional annotation of the coding sequences. It builds on the BLAST sequence similarity results and provides functional information, including best match, annotation, as well as the gene ontology (GO) terms. The GO terms are a particular feature of interest as they describe functions and attributes of the genes.This approach was chosen since the use of specific tools like AUGUSTUS for gene prediction was hindered by root-user privilege restrictions.
</p>

<p style='text-align: justify;'>
The operation of this pipeline showed that the chosen tools and data could be used in conjunction to obtain annotation results. <em>Pongo pygmaeus</em> is an organism that is already annotated, which provided us with an opportunity to check the validity of our approach when annotating its transcriptome. Sequences used in the BLAST process mapped back to protein sequences found in <em>Pongo pygmaeus</em> with a high percent identity for each.
</p>

![Figure 2: Results from Blast2GO](/images/Blast2GOResults.jpg)

# <span style="color:#007ea7"> Results

<p style='text-align: justify;'>
The results obtained through OmicsBox was an indication that this pipeline could obtain accurate results with our chosen data, methods, and tools. Comparing this pipeline to others published, found during our planning process, shows many similarities to our final solution. This pipeline offers promise for future analyses. It can be used for downstream analysis, such as differential gene expression studies, to understand biological processes in orangutans. Additionally, the pipeline's design could be adapted to analyze RNA-sequencing data from other eukaryotic organisms, although some modifications might be necessary depending on the data source and organism.

The next steps with this pipeline could be to work on expanding its functionality so that it can be used for a wider range of organisms or different sources of data. A possible expansion could include a sub-workflow that utilizes shorter read RNA-seq data using an aligner such as STAR, or potentially a sub-workflow that allows for using data for organisms other than mammals.

</p>
