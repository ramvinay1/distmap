

# About the project #
`DistMap` is a user-friendly pipeline designed to map short reads in a `MapReduce` framework on a local Hadoop cluster. It is designed to be easily implemented by researchers who do not have expert knowledge of bioinformatics. As it does not have any dependencies, `DistMap` provides full flexibility and control to the user. The user can use any version of a compatible mapper and any reference genome assembly. There is no need to maintain the mapper, reference or DistMap source code on each of the slaves (nodes) in the Hadoop cluster, making maintenance extremely easy.

# Basic Concepts #
## Input ##
- FASTQ formatted, paired-end and single-end raw sequences.<br>
- Mixture of paired-end and single-end sequences also supported.<br>
- FASTA formatted reference genome sequence.<br>

<h2>Output</h2>
- A single SAM or BAM file of mapped reads. This SAM or BAM output file can be directly used for downstream analysis.<br>
- <code>DistMap</code> returns initial mapping output, such that no information is lost.<br>


<h2>Prerequisites</h2>
- Perl 5.8 or higher<br>
- Mapper executable<br>
- <code>MergeSamFiles.jar</code> and <code>SortSam.jar</code> from PICARD (<a href='http://picard.sourceforge.net'>http://picard.sourceforge.net</a>).<br>
- A working Hadoop cluster.<br>


<h2>Features</h2>
- The whole <code>DistMap</code> pipeline can be run at once or in step-by-step mode. The user can run any individual step or can start from any step onwards.<br>
- Easily runs from the command line like any other Perl script.<br>
- No manual steps required unlike other available tools.<br>
- <code>DistMap</code> uses all input files and parameters stored on the local computer and returns final output into the local computer output directory.<br>
- Prior knowledge of Hadoop cluster or <code>MapReduce</code> is not required (though will be useful).<br>
- Once a Hadoop cluster is available, any non-expert researcher can use <code>DistMap</code> to map an enormous amount of sequencing reads onto a reference genome in a time-efficient manner.<br>
- There is no limitation of data size in <code>DistMap</code>; it is specifically designed to handle Gigabytes or Terabytes of data.<br>


<h2>Distribution</h2>
<code>DistMap</code> is distributed as command line pipeline that runs locally on a Hadoop cluster.<br>
<br>
<h2>Contact</h2>
Ram Vinay Pandey (ramvinay.pandey'at'vetmeduni.ac.at)<br>

<h1>Usage</h1>
<h2>How to use <code>DistMap</code>?</h2>
<a href='http://code.google.com/p/distmap/wiki/Manual'>http://code.google.com/p/distmap/wiki/Manual</a><br><br>

<h2>How to setup Hadoop cluster on Macintosh?</h2>
<a href='http://code.google.com/p/distmap/wiki/SetupHadoopMacintosh'>http://code.google.com/p/distmap/wiki/SetupHadoopMacintosh</a><br>


<h2>How to setup Hadoop cluster on Linux?</h2>
<a href='http://code.google.com/p/distmap/wiki/SetupHadoopLinux'>http://code.google.com/p/distmap/wiki/SetupHadoopLinux</a><br><br>


<h1>How to cite <code>DistMap</code>?</h1>

Please cite the following <code>DistMap</code> paper.<br>
<br>
Pandey RV, Schlötterer C. (2013) <code>DistMap</code>: a toolkit for distributed short read mapping on a Hadoop cluster. PLoS One. 8(8):e72614.<br>
<br>
<br>
