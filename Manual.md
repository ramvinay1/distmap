


# 1. Introduction #
`DistMap` provides a scalable and portable framework to map short reads on a Hadoop cluster. It is a complete pipeline especially designed for non-experts. Currently,  `DistMap ` supports 9 mappers:

**BWA** (http://soap.genomics.org.cn/soapaligner.html)<br>
<b>GSNAP</b> (<a href='http://soap.genomics.org.cn/soapaligner.html'>http://soap.genomics.org.cn/soapaligner.html</a>)<br>
<b><code>TopHat</code></b> (<a href='http://tophat.cbcb.umd.edu/'>http://tophat.cbcb.umd.edu/</a>)<br>
<b>Bowtie</b> (<a href='http://bowtie-bio.sourceforge.net/index.shtml'>http://bowtie-bio.sourceforge.net/index.shtml</a>)<br>
<b>Bowtie2</b> (<a href='http://sourceforge.net/projects/bowtie-bio/files/bowtie2/2.0.6/'>http://sourceforge.net/projects/bowtie-bio/files/bowtie2/2.0.6/</a>)<br>
<b>SOAP</b> (<a href='http://soap.genomics.org.cn/soapaligner.html'>http://soap.genomics.org.cn/soapaligner.html</a>)<br>
<b>STAR</b> (<a href='http://gingeraslab.cshl.edu/STAR/'>http://gingeraslab.cshl.edu/STAR/</a>)<br>
<b>Bismask</b> (<a href='http://www.bioinformatics.babraham.ac.uk/projects/bismark/'>http://www.bioinformatics.babraham.ac.uk/projects/bismark/</a>)<br>
<b>BSMAP</b> (<a href='http://code.google.com/p/bsmap/'>http://code.google.com/p/bsmap/</a>)<br>

<code>DistMap</code> supports paired-end and single-end read mapping in FASTQ file format. It returns the final output as a SAM or BAM file.<br>
<br>
<h2><code>DistMap</code> components</h2>
The <code>DistMap</code> workflow consists of seven main modules which can be executed either end-to-end by a single command, or each module can be executed by giving appropriate command line flags..<br><br>


<h3>Module1: Genome indexing:</h3>
The first module of <code>DistMap</code> creates genome indices and uploads these into the HDFS file system. Genome indices from previous <code>DistMap</code> runs are maintained as <b>tgz</b> files such that the user can re-use these via the option –reference-index-archive on the <code>DistMap</code> command line. The user can execute <i>Genome indexing</i> as a stand-alone module by giving the flag –only-index in the <code>DistMap</code> command line.<br>

<h3>Module2: Data processing:</h3>
The second module of <code>DistMap</code> takes all input parameters, FASTQ formatted reads and mapper executables from the local computer and creates a final archive. This archive is then uploaded to the cluster. The user can execute <i>Data processing</i> as a stand-alone module by giving the flag –only-process in the <code>DistMap</code> command line.<br>

<h3>Module3: Data upload:</h3>
This module loads the processed reads and the archive created in module 1 into the HDFS file system. The user can execute <i>Data upload</i> as a stand-alone module by giving the flag –only-hdfs-upload in the <code>DistMap</code> command line.<br>

<h3>Module4: Data mapping:</h3>
This module facilitates the mapping of the reads to the reference genome. The user can execute <i>Data mapping</i> as a stand-alone module by giving the flag –only-map in the <code>DistMap</code> command line.<br>

<h3>Module5: Data download:</h3>
This module implements data transfer from the HDFS on the local computer after the completion of mapping. The user can execute <i>Data download</i> as a stand-alone module by giving the flag –only-hdfs-download in the <code>DistMap</code> command line.<br>

<h3>Module6: Post processing output:</h3>
This module results in the combination of all outputs into a single SAM or BAM file in the local output directory. The user can execute <i>Post processing</i> output as a stand-alone module by giving the flag –only-merge in the <code>DistMap</code> command line.<br>

<h3>Module7: Data cleanup:</h3>
The final module deletes all intermediate input and output files stored within the HDFS and on the local computer. The <i>Data cleanup</i> module is optional and it can be executed as a stand alone module with the flag –only-delete-temp in the <code>DistMap</code> command line.<br>




<h1>2. System Requirements</h1>
<code>DistMap</code> is implemented in Perl and runs on all Unix operating systems. It requires Perl 5.8 or higher, <code>MergeSamFiles.jar</code> and <code>SortSam.jar</code> from PICARD (<a href='http://picard.sourceforge.net'>http://picard.sourceforge.net</a>).<br>
<br>
<h1>3. How to run <code>DistMap</code>?</h1>

Download source code of  <code>DistMap </code> from <a href='http://distmap.googlecode.com/files/DistMap_v1.0.tar.gz'>http://distmap.googlecode.com/files/DistMap_v1.0.tar.gz</a> and run following command on terminal to uncompress the source code.<br>
<br>
<pre><code>tar -xzvf DistMap_v1.0.tar.gz<br>
<br>
</code></pre>

Run following command to get <code>DistMap</code> command line parameters.<br>
<pre><code>perl DistMap_v1.0/distmap --help<br>
</code></pre>


<h3><code>DistMap</code> command line parameters</h3>


<pre><code>Usage: perl DistMap_v1.0/distmap<br>
<br>
--reference-fasta      		Reference fasta file full path. MANDATORY parameter.<br>
  <br>
--input                		Provide input fastq files, either as a pair or single Fastq file.<br>
				For Paired-end data --input 'read1.fastq,read2.fastq'<br>
				For Single-end data --input 'read.fastq'<br>
				Multiple inputs are possible by repeating --input parameter<br>
					<br>
				--input 'sample1_1.fastq,sample1_2.fastq' --input 'sample2_1.fastq,sample2_2.fastq'<br>
				It is necessary to give input file(s) within<br>
				single or double quotes. Paired files must be comma<br>
				separated. MANDATORY parameter.<br>
					<br>
--output              		Full path of output folder where final output will be kept. MANDATORY parameter.<br>
<br>
--only-process    		Step1: This option will 1) convert FASTQ files into 1 line format, 2) create a genome index<br>
			        and 3) create a job archive and exit. OPTIONAL parameter<br>
				<br>
--only-hdfs-upload          	Step2: This option assumes that the data processing is complete. It uploads the reads and the archive created in the<br>
				data process step into HDFS file system. OPTIONAL parameter<br>
<br>
--only-map            		Step3: This option assumes that data is already loaded into the HDFS and will<br>
				run map on the cluster.<br>
<br>
--only-hdfs-download          	Step4: This option assumes that mapping has been completed on the cluster. Files will be downloaded<br>
				from the cluster to a local output directory. OPTIONAL parameter<br>
				<br>
--only-merge          		Step5: This option assumes that the data is already in the local directory.<br>
				Data will be merged to create a single SAM or BAM output file. OPTIONAL parameter<br>
<br>
--only-delete-temp          	Step6: This is the last step of the pipeline. This option deletes the<br>
				mapping data from HDFS file system as well as from local temp directory. Useful to clean up after large mapping jobs!<br>
				OPTIONAL parameter<br>
				<br>
--hadoop-home         		Gives the full path of the hadoop folder. MANDATORY parameter.<br>
				The hadoop home path should be identical in master, secondary namenode and<br>
				all slaves (nodes).<br>
				Example: --hadoop-home /usr/local/hadoop<br>
					<br>
--mapper              		Mapper name [bwa,tophat,gsnap,bowtie,soap]. MANDATORY parameter.<br>
				Example: --mapper bwa<br>
					<br>
--mapper-path         		Mapper executable full path. MANDATORY parameter.<br>
				Example: --mapper-path /usr/local/hadoop/bwa<br>
--gsnap-output-split		GSNAP has a feature to split different types of mapping output into different<br>
                                SAM files.<br>
				<br>
				For detail input: gsnap --help, read --split--output.<br>
				  --split-output=STRING   Basename for multiple-file output, separately for nomapping,<br>
                                   halfmapping_uniq, halfmapping_mult, unpaired_uniq, unpaired_mult,<br>
                                   paired_uniq, paired_mult, concordant_uniq, and concordant_mult results (up to 9 files,<br>
                                   or 10 if --fails-as-input is selected, or 3 for single-end reads)<br>
				   					<br>
--picard-mergesamfiles-jar     PICARD MergeSamFiles.jar full path. It will be used to merge all<br>
				SAM or BAM files. MANDATORY parameter.<br>
				Example: --picard-mergesamfiles-jar /usr/local/hadoop/picard-tools-1.56/MergeSamFiles.jar<br>
				<br>
--picard-sortsam-jar     	PICARD SortSam.jar full path. It will be used for SAM BAM conversion. MANDATORY parameter.<br>
				Example: --picard-sortsam-jar /usr/local/hadoop/picard-tools-1.56/SortSam.jar<br>
				<br>
--mapper-args        		Arguments for mapping:<br>
				BWA mapping for aln command:<br>
					Example --mapper-args "-o 1 -n 0.01 -l 200 -e 12 -d 12"<br>
					Note: Define BWA parameters correctly according to the version used here.<br>
				TopHat:<br>
					Example: --mapper-args "--mate-inner-dist 200 --max-multihits 40 --phred64-quals"<br>
					Note: Define TopHat parameters correctly according to the version used here.<br>
				GSNAP mapping:<br>
					Example: --mapper-args "--pairexpect 200 --quality-protocol illumina"<br>
					Note: Define gsnap parameters correctly according to the version used here.<br>
					For detail about parameters visit [http://research-pub.gene.com/gmap/]<br>
				bowtie mapping:<br>
					Example: --mapper-args "--sam"<br>
					Note: Define gsnap parameters correctly according to the version used here.<br>
					For detail about parameters visit [http://bowtie-bio.sourceforge.net/index.shtml]<br>
				SOAPAlinger:<br>
					Example: --mapper-args "-m 400 -x 600"<br>
					Note: Define SOAPaligner parameters correctly according to the version used here.<br>
					For detail about parameters visit [http://soap.genomics.org.cn/soapaligner.html]<br>
Please note that processor parameters are not required in arguments. For example BWA mapping does not require -t parameter.<br>
This parameter is given by DistMap internally.<br>
<br>
--bwa-sampe-args      	Arguments for BWA sampe or samse module.<br>
			bwa sampe for paired-end reads<br>
			bwa samse for single-end reads<br>
			Example --bwa-sampe-args "-a 500 -s"<br>
					<br>
--output-format    		Output file format either SAM or BAM.<br>
				Default: BAM<br>
					<br>
--job-desc    		Give a job description which will be displayed in JobTracker webpage.<br>
				Default: &lt;mapper name&gt; mapping.<br>
Hadoop streaming Parameters:<br>
<br>
--hadoop-scheduler    	Give the scheduler name:<br>
				1) Fair Scheduler (need to configure. http://hadoop.apache.org/docs/stable/fair_scheduler.html)<br>
				2) FIFO (it is default comes with Hadoop)<br>
				3) Capacity Scheduler (need to configure. http://hadoop.apache.org/docs/stable/capacity_scheduler.html)<br>
				If your hadoop has Fair Scheduler then provide a pool name with the parameter --queue-name<br>
				If your hadoop has Capacity Scheduler then provide  queue name with the parameter --queue-name.<br>
				Example: --hadoop-scheduler Fair<br>
					 --hadoop-scheduler Capacity<br>
					 --hadoop-scheduler FIFO<br>
<br>
--queue-name    		If your hadoop has Capacity Scheduler then provide --queue-name.<br>
				Example: --queue-name pg1<br>
--job-priority    		Give the job priority to run your job.<br>
				Possible options: [ VERY_LOW | LOW | NORMAL | HIGH | VERY_HIGH ]<br>
				Example: --job-priority VERY_HIGH<br>
				<br>
--verbose/--help                 	Print inputs on screen.  <br>
<br>
<br>
</code></pre>

<h1>4 BWA mapping</h1>
Burrows-Wheeler Aligner (BWA) is an efficient program that aligns relatively short nucleotide sequences against a long reference sequence such as the available human reference sequence.<br>
<br>
<b>Step1:</b> Download latest BWA source code from <a href='http://sourceforge.net/projects/bio-bwa/files/'>http://sourceforge.net/projects/bio-bwa/files/</a>


<b>Step2:</b> Uncompress the downloaded file bwa-0.6.2.tar.bz2 with following command:<br>
<br>
<pre><code>tar -xvjf bwa-0.6.2.tar.bz2<br>
</code></pre>

This command will return the folder/directory <b>bwa-0.6.2</b>

Step3: Enter into the uncompressed folder <b>bwa-0.6.2</b> with following command:<br>
<br>
<pre><code>cd /home/user/bwa-0.6.2<br>
</code></pre>

Step4: To make BWA executable run the <b>sudo make</b> command:<br>
<br>
<pre><code>sudo make<br>
</code></pre>

After running the make command an executable called bwa will be created in <b>/home/user/bwa-0.6.2</b> folder.<br>
<br>
Give the full path for <code>DistMap</code> command parameter<br>
<br>
<pre><code>perl DistMap_v_1.0/distmap --mapper-path /home/user/bwa-0.6.2/bwa<br>
</code></pre>



<h1>5 GSNAP mapping</h1>

GSNAP (Genomic Short-read Nucleotide Alignment Program) is a mapper for RNA-seq data. It has the advantages that it can detect splicing events and is capable of SNP tolerant alignments (Wu & Nacu 2010).<br>
<br>
<b>Step1:</b> Download the GSNAP source code from<br>
<a href='http://research-pub.gene.com/gmap/src/gmap-gsnap-2012-07-20.tar.gz'>http://research-pub.gene.com/gmap/src/gmap-gsnap-2012-07-20.tar.gz</a>

<b>Step2:</b> Uncompress the downloaded file “gmap-gsnap-2012-07-20.tar.gz” with following command:<br>
<pre><code>tar -xzvf gmap-gsnap-2012-07-20.tar.gz<br>
</code></pre>

<b>Step3:</b> Enter into the uncompressed folder <b>gmap-2012-07-20</b> with following command:<br>
<br>
<pre><code>  cd /home/user/gmap-2012-07-20<br>
</code></pre>


<b>Step4:</b> To install the gmap package run these four commands:<br>
<pre><code>./configure --prefix=/home/user/gmap-2012-07-20<br>
make<br>
make check   (optional)<br>
make install<br>
</code></pre>

The above four commands will build GSNAP and other executables in <b>/home/user/gmap-2012-07-20/bin</b>

<b>Step5:</b> To check the GSNAP installations run this command:<br>
<pre><code>/home/user/gmap-2012-07-20/bin/gsnap –help<br>
</code></pre>

This command will return the detailed help manual for various GSNAP parameters if the installation was successful.<br>
<br>
<b>Note:</b> For more detailed information how to install GSNAP please read the gmap-2012-07-20/README file.<br>
<br>
<br>
In <b>/home/user/gmap-2012-07-20</b> folder you should get following executables:<br>
<br>
<br>
<pre><code>/home/user/gmap-2012-07-20/bin/gsnap<br>
/home/user/gmap-2012-07-20/bin/gmap_build<br>
<br>
</code></pre>

Give the full path for <code>DistMap</code> command parameter<br>
<br>
<pre><code>perl DistMap_v_1.0/distmap --mapper-path /home/user/gmap-2012-07-20/bin/gsnap<br>
</code></pre>




<h1>6 <code>TopHat</code> mapping</h1>
Download TopHat binary from <a href='http://tophat.cbcb.umd.edu/downloads/tophat-2.0.6.OSX_x86_64.tar.gz'>http://tophat.cbcb.umd.edu/downloads/tophat-2.0.6.OSX_x86_64.tar.gz</a> for Macintosh or from <a href='http://tophat.cbcb.umd.edu/downloads/tophat-2.0.6.Linux_x86_64.tar.gz'>http://tophat.cbcb.umd.edu/downloads/tophat-2.0.6.Linux_x86_64.tar.gz</a> for Linux operating system.<br>
<br>
<h2>Get <code>Bowtie for TopHat</code> mapping</h2>
As <code>TopHat</code> requires bowtie for mapping internally. Bowtie binaries can be found at <a href='http://sourceforge.net/projects/bowtie-bio/files/bowtie/0.12.9/'>http://sourceforge.net/projects/bowtie-bio/files/bowtie/0.12.9/</a> and copy following bowtie executables from <b>/home/user/bowtie-0.12.9</b> folder into <b>/home/user/tophat-2.0.6</b> folder.<br>
<pre><code>cp /home/user/bowtie-0.12.9/bowtie /home/user/tophat-2.0.6/<br>
cp /home/user/bowtie-0.12.9/bowtie-build /home/user/tophat-2.0.6/<br>
cp /home/user/bowtie-0.12.9/bowtie-build-debug /home/user/tophat-2.0.6/<br>
cp /home/user/bowtie-0.12.9/bowtie-debug /home/user/tophat-2.0.6/<br>
cp /home/user/bowtie-0.12.9/bowtie-inspect /home/user/tophat-2.0.6/<br>
cp /home/user/bowtie-0.12.9/bowtie-inspect-debug /home/user/tophat-2.0.6/<br>
</code></pre>




In <b>/home/user/tophat-2.0.6</b> folder you should get following executables:<br>
<br>
<br>
<pre><code>/home/user/tophat-2.0.6/prep_reads<br>
/home/user/tophat-2.0.6/tophat<br>
<br>
/home/user/tophat-2.0.6/bowtie<br>
/home/user/tophat-2.0.6/bowtie-build<br>
/home/user/tophat-2.0.6/bowtie-build-debug<br>
/home/user/tophat-2.0.6/bowtie-debug<br>
/home/user/tophat-2.0.6/bowtie-inspect<br>
/home/user/tophat-2.0.6/bowtie-inspect-debug<br>
</code></pre>


Give the full path for <code>DistMap</code> command parameter<br>
<br>
<pre><code>perl DistMap_v_1.0/distmap --mapper-path /home/user/tophat-2.0.6/tophat<br>
</code></pre>

<b>Note:</b> Since TopHat was developed on Python 2.6, all slaves should have Python 2.6. This uses the python package called <b>getopt</b> which is called in <b>/home/user/tophat-2.0.6/tophat</b>


<h1>7 Bowtie mapping</h1>

Download Bowtie binaries from <a href='http://sourceforge.net/projects/bowtie-bio/files/bowtie/0.12.9/'>http://sourceforge.net/projects/bowtie-bio/files/bowtie/0.12.9/</a>


In <b>/home/user/bowtie-0.12.9</b> folder you should get following executables:<br>
<br>
<pre><code>/home/user/bowtie-0.12.9/bowtie<br>
/home/user/bowtie-0.12.9/bowtie-build<br>
/home/user/bowtie-0.12.9/bowtie-build-debug<br>
/home/user/bowtie-0.12.9/bowtie-debug<br>
/home/user/bowtie-0.12.9/bowtie-inspect<br>
/home/user/bowtie-0.12.9/bowtie-inspect-debug<br>
</code></pre>

Give the full path for <code>DistMap</code> command parameter<br>
<br>
<pre><code>perl DistMap_v_1.0/distmap --mapper-path /home/user/bowtie-0.12.9/bowtie<br>
</code></pre>



<h1>8 Bowtie2 mapping</h1>

Download Bowtie binaries from <a href='http://sourceforge.net/projects/bowtie-bio/files/bowtie2/2.0.6/'>http://sourceforge.net/projects/bowtie-bio/files/bowtie2/2.0.6/</a>


In <b>/home/user/bowtie2-2.0.6</b> folder you should get following executables:<br>
<br>
<pre><code>/home/user/bowtie2-2.0.6/bowtie2<br>
/home/user/bowtie2-2.0.6/bowtie2-build<br>
/home/user/bowtie2-2.0.6/bowtie2-build-debug<br>
/home/user/bowtie2-2.0.6/bowtie2-debug<br>
/home/user/bowtie2-2.0.6/bowtie2-inspect<br>
/home/user/bowtie2-2.0.6/bowtie2-inspect-debug<br>
</code></pre>

Give the full path for <code>DistMap</code> command parameter<br>
<br>
<pre><code>perl DistMap_v_1.0/distmap --mapper-path /home/user/bowtie2-2.0.6/bowtie2<br>
</code></pre>


<h1>9 SOAP mapping</h1>

<h2>SOAPaligner/soap2 installation</h2>
SOAPaligner/soap2 is a short read mapper that allows gapped and un-gapped mapping of short reads.<br>
<br>
<b>Step1:</b> Download latest SOAPaligner/soap2 source code from http:// soap.genomics.org.cn/down/SOAPaligner-v2.20-src.tar.gz<br>
<br>
<b>Step2:</b> Uncompress the downloaded file SOAPaligner-v2.20-src.tar.gz with following command:<br>
<pre><code>tar -xzvf SOAPaligner-v2.20-src.tar.gz<br>
</code></pre>

This command will return the folder/directory <b>soap2.20</b>
<b>Step3:</b> Enter into the uncompressed folder soap2.20 with following command:<br>
<pre><code>	cd /home/user/soap2.20<br>
</code></pre>

<b>Step4:</b> To make soap executable run the make command:<br>
<pre><code>	make<br>
</code></pre>
After running the make command, an executable called soap will be created in <b>/home/user/soap2.20</b> folder.<br>
<br>
<h2>SOAPaligner_builder installation</h2>
Running SOAPaligner requires index files for the reference genome, reads can then be searched against the formatted index files.<br>
<br>
<b>Step1:</b> Download latest SOAPaligner_builder source code from <a href='http://soap.genomics.org.cn/down/SOAPaligner-v2.20-src_builder.tar.gz'>http://soap.genomics.org.cn/down/SOAPaligner-v2.20-src_builder.tar.gz</a>


<b>Step2:</b> Uncompress the downloaded file SOAPaligner-v2.20-src_builder.tar.gz with following command:<br>
<br>
<pre><code>tar –xzvf SOAPaligner-v2.20-src_builder.tar.gz<br>
</code></pre>

This command will return the folder/directory <b>soap_builder</b>

<b>Step3:</b> Enter in the uncompressed folder <b>soap_builder</b> with following command:<br>
<pre><code>	cd /home/user/soap_builder<br>
</code></pre>

<b>Step4:</b> To make soap executable run the make command:<br>
<pre><code> make<br>
</code></pre>

After running the make command an executable called <b>2bwt-builder</b> will be created into folder <b>/home/user/soap_builder</b>.<br>
<br>
<b>Step5:</b> Copy <b>2bwt-builder</b> file into /home/user/soap2.20 folder with following command:<br>
<pre><code>cp /home/user/soap_builder/2bwt-builder /home/user/soap2.20/<br>
</code></pre>


<h2>soap2sam.pl installation:</h2>
This Perl script is required to convert soap output format into common SAM format.<br>
<br>
<b>Step1:</b> Download latest soap2sam.pl source code from http:// soap.genomics.org.cn/down/soap2sam.tar.gz<br>
<br>
<b>Step2:</b> Uncompress the downloaded file soap2sam.tar.gz with following command:<br>
<pre><code>tar –xzvf soap2sam.tar.gz<br>
</code></pre>

This command will return the perl script soap2sam.pl<br>
<br>
<b>Step3:</b> Copy <b>soap2sam.pl</b> file into /home/user/soap2.20 folder with following command:<br>
<pre><code>cp soap2sam.pl /home/user/soap2.20/<br>
</code></pre>

In <b>/home/user/soap2.20</b> folder you should get following executables:<br>
<br>
<pre><code>/home/user/soap2.20/soap2<br>
/home/user/soap2.20/2bwt-builder<br>
/home/user/soap2.20/soap2sam.pl<br>
</code></pre>


Give the full path for <code>DistMap</code> command parameter<br>
<br>
<pre><code>perl DistMap_v_1.0/distmap --mapper-path /home/user/soap2.20/soap2<br>
</code></pre>


<h1>10 STAR mapping</h1>

Download binary of STAR from <a href='ftp://ftp2.cshl.edu/gingeraslab/tracks/STARrelease/2.2.0/'>ftp://ftp2.cshl.edu/gingeraslab/tracks/STARrelease/2.2.0/</a>


Give the full path for <code>DistMap</code> command parameter<br>
<br>
<pre><code>perl DistMap_v_1.0/distmap --mapper-path /home/user/STAR_2.2.0c.Linux_x86_64/STAR<br>
</code></pre>

<h1>11 Bismak mapping</h1>
Bismark A bisulfite read mapper and methylation caller<br>
<br>
www.bioinformatics.babraham.ac.uk/projects/bismark/bismark_v0.7.7.tar.gz<br>
<br>
<br>
<br>
<b>Step1:</b> Download the Bismark source code from www.bioinformatics.babraham.ac.uk/projects/bismark/bismark_v0.7.7.tar.gz<br>
<br>
<b>Step2:</b> Uncompress the downloaded file “bismark_v0.7.7.tar.gz” with following command:<br>
<pre><code>tar -xzvf bismark_v0.7.7.tar.gz<br>
</code></pre>
<b>Step3:</b> Enter the uncompressed folder “bismark_v0.7.7” with following command:<br>
<pre><code>cd bismark_v0.7.7<br>
</code></pre>

<h2>Get Bowtie for Bismark mapping</h2>
As Bismark uses bowtie for mapping internally. Bowtie can be downloaded from <a href='http://sourceforge.net/projects/bowtie-bio/files/bowtie/0.12.9/'>http://sourceforge.net/projects/bowtie-bio/files/bowtie/0.12.9/</a>, copy following bowtie executables from <b>/home/user/bowtie-0.12.9</b> folder into <b>/home/user/bismark_v0.7.7</b> folder.<br>
<pre><code>cp /home/user/bowtie-0.12.9/bowtie /home/user/bismark_v0.7.7/<br>
cp /home/user/bowtie-0.12.9/bowtie-build /home/user/bismark_v0.7.7/<br>
cp /home/user/bowtie-0.12.9/bowtie-build-debug /home/user/bismark_v0.7.7/<br>
cp /home/user/bowtie-0.12.9/bowtie-debug /home/user/bismark_v0.7.7/<br>
cp /home/user/bowtie-0.12.9/bowtie-inspect /home/user/bismark_v0.7.7/<br>
cp /home/user/bowtie-0.12.9/bowtie-inspect-debug /home/user/bismark_v0.7.7/<br>
</code></pre>




In <b>/home/user/bismark_v0.7.7</b> folder you should get following executables:<br>
<br>
<br>
<pre><code>/home/user/bismark_v0.7.7/bismark<br>
/home/user/bismark_v0.7.7/bismark_genome_preparation<br>
/home/user/bismark_v0.7.7/bismark_methylation_extractor<br>
/home/user/bismark_v0.7.7/bowtie<br>
/home/user/bismark_v0.7.7/bowtie-build<br>
/home/user/bismark_v0.7.7/bowtie-build-debug<br>
/home/user/bismark_v0.7.7/bowtie-debug<br>
/home/user/bismark_v0.7.7/bowtie-inspect<br>
/home/user/bismark_v0.7.7/bowtie-inspect-debug<br>
</code></pre>


Give the full path for <code>DistMap</code> command parameter<br>
<br>
<pre><code>perl DistMap_v_1.0/distmap --mapper-path /home/user/bismark_v0.7.7/bismark<br>
</code></pre>


<h1>12 BSMAP mapping</h1>

BSMAP is a short reads mapping software for bisulfite sequencing reads. Bisulfite treatment converts unmethylated Cytosines into Uracils (sequenced as Thymine) and leave methylated Cytosines unchanged, hence provides a way to study DNA cytosine methylation at single nucleotide resolution. BSMAP aligns the Ts in the reads to both Cs and Ts in the reference.<br>
<br>
<br>
<br>
<b>Step1:</b> Download the BSMAP source code from <a href='http://bsmap.googlecode.com/files/bsmap-2.73.tgz'>http://bsmap.googlecode.com/files/bsmap-2.73.tgz</a>

<b>Step2:</b> Uncompress the downloaded file “bsmap-2.73.tgz” with following command:<br>
<pre><code>tar -xzvf bsmap-2.73.tgz<br>
</code></pre>
<b>Step3:</b> Enter the uncompressed folder “bsmap-2.73” with following command:<br>
<pre><code>cd bsmap-2.73<br>
</code></pre>
<b>Step4:</b> To install the BSMAP package run these two commands:<br>
<br>
<pre><code>sudo make<br>
sudo make install<br>
</code></pre>

The above four commands will build BSMAP executable in “/usr/bin”<br>
<br>
<b>Step5:</b> To check the BSMAP installations run this command:<br>
<pre><code><br>
bsmap –h<br>
</code></pre>

This command will return the detailed help manual for various BSMAP parameters if the installation was successful.<br>
<br>
<b>Note:</b> For more detailed information how to run BSMAP, please read the bsmap-2.73/README.txt file.<br>
<br>
<br>
Give the full path for <code>DistMap</code> command parameter<br>
<br>
<pre><code>perl DistMap_v_1.0/distmap --mapper-path /home/user/bsmap-2.73/bsmap<br>
</code></pre>


