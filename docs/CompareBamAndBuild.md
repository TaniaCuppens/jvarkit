# CompareBamAndBuild

Compare two  BAM files mapped on two different builds. Requires a liftover chain file


## Usage

```
Usage: cmpbamsandbuild [options] Files
  Options:
  * -c, --chain
      ) Lift Over file from bam1 to bam2. REQUIRED
    -d, --distance
      distance tolerance between two alignments
      Default: 10
    -h, --help
      print help and exit
    --helpFormat
      What kind of help
      Possible Values: [usage, markdown, xml]
    -maxRecordsInRam, --maxRecordsInRam
      Max records in RAM
      Default: 50000
    -o, --output
      Output file. Optional . Default: stdout
    -r, --region
      restrict to that region chr:start-end
    -T, --tmpDir
      mp directory
      Default: /tmp
    --version
      print version and exit

```

## Compilation

### Requirements / Dependencies

* java [compiler SDK 1.8](http://www.oracle.com/technetwork/java/index.html) (**NOT the old java 1.7 or 1.6**) and avoid OpenJdk, use the java from Oracle. Please check that this java is in the `${PATH}`. Setting JAVA_HOME is not enough : (e.g: https://github.com/lindenb/jvarkit/issues/23 )
* GNU Make >= 3.81
* curl/wget
* git
* xsltproc http://xmlsoft.org/XSLT/xsltproc2.html (tested with "libxml 20706, libxslt 10126 and libexslt 815")


### Download and Compile

```bash
$ git clone "https://github.com/lindenb/jvarkit.git"
$ cd jvarkit
$ make cmpbamsandbuild
```

The *.jar libraries are not included in the main jar file, [so you shouldn't move them](https://github.com/lindenb/jvarkit/issues/15#issuecomment-140099011 ).
The required libraries will be downloaded and installed in the `dist` directory.

Experimental: you can also create a [fat jar](https://stackoverflow.com/questions/19150811/) which contains classes from all the libraries, on which your project depends (it's bigger). Those fat-jar are generated by adding `standalone=yes` to the gnu make command, for example ` make cmpbamsandbuild standalone=yes`.

### edit 'local.mk' (optional)

The a file **local.mk** can be created edited to override/add some definitions.

For example it can be used to set the HTTP proxy:

```
http.proxy.host=your.host.com
http.proxy.port=124567
```
## Source code 

[https://github.com/lindenb/jvarkit/tree/master/src/main/java/com/github/lindenb/jvarkit/tools/cmpbams/CompareBamAndBuild.java](https://github.com/lindenb/jvarkit/tree/master/src/main/java/com/github/lindenb/jvarkit/tools/cmpbams/CompareBamAndBuild.java)


<details>
<summary>Git History</summary>

```
Fri Jun 2 16:31:30 2017 +0200 ; circos / lumpy ; https://github.com/lindenb/jvarkit/commit/7bddffca3899196e568fb5e1a479300c0038f74f
Thu May 11 10:59:12 2017 +0200 ; samcolortag ; https://github.com/lindenb/jvarkit/commit/dfd3239dc49af52966e2259bf0a5f52dd34aac8e
Tue Apr 18 13:24:50 2017 +0200 ; cont-cleanup ; https://github.com/lindenb/jvarkit/commit/a86c8971fe5ebb3f8de175c75e78f2d0e5325cfd
Tue Dec 6 17:34:29 2016 +0100 ; cont ; https://github.com/lindenb/jvarkit/commit/41a44d15844a7de5fa8ac856b9465454a105e18c
Mon Jun 15 17:24:26 2015 +0200 ; vep as xml base 64 ; https://github.com/lindenb/jvarkit/commit/d629603576c14a970208c9599b39ecb2a8b39994
Fri May 23 15:00:53 2014 +0200 ; cont moving to htsjdk ; https://github.com/lindenb/jvarkit/commit/81f98e337322928b07dfcb7a4045ba2464b7afa7
Mon May 12 10:28:28 2014 +0200 ; first sed on files ; https://github.com/lindenb/jvarkit/commit/79ae202e237f53b7edb94f4326fee79b2f71b8e8
Fri Mar 7 18:02:00 2014 +0100 ; compare bam/build ; https://github.com/lindenb/jvarkit/commit/6897d7ce8013c9e4bf3669218e58a7dfeb87d8ab
```

</details>

## Contribute

- Issue Tracker: [http://github.com/lindenb/jvarkit/issues](http://github.com/lindenb/jvarkit/issues)
- Source Code: [http://github.com/lindenb/jvarkit](http://github.com/lindenb/jvarkit)

## License

The project is licensed under the MIT license.

## Citing

Should you cite **cmpbamsandbuild** ? [https://github.com/mr-c/shouldacite/blob/master/should-I-cite-this-software.md](https://github.com/mr-c/shouldacite/blob/master/should-I-cite-this-software.md)

The current reference is:

[http://dx.doi.org/10.6084/m9.figshare.1425030](http://dx.doi.org/10.6084/m9.figshare.1425030)

> Lindenbaum, Pierre (2015): JVarkit: java-based utilities for Bioinformatics. figshare.
> [http://dx.doi.org/10.6084/m9.figshare.1425030](http://dx.doi.org/10.6084/m9.figshare.1425030)

 

## Example

The following **Makefile** compares two BAMs mapped on hg19 and hg38.

```makefile
BWA=bwa
SAMTOOLS=samtools
.PHONY:all

all: file.diff

file.diff.gz : hg19ToHg38.over.chain hg19.bam hg38.bam
	java -jar jvarkit-git/dist/cmpbamsandbuild.jar -d 20 \
	    -c hg19ToHg38.over.chain hg19.bam hg38.bam | gzip --best > $@

hg38.bam:file1.fastq.gz file2.fastq.gz
	${BWA} mem  index-bwa-0.7.6a/hg38.fa $^ |\
		${SAMTOOLS} view -Sb - |\
		${SAMTOOLS}  sort - hg38 && \
		${SAMTOOLS} index hg38.bam
		
hg19.bam: file1.fastq.gz file2.fastq.gz
		${BWA} mem index-bwa-0.7.6a/hg19.fa $^ |\
		${SAMTOOLS} view -Sb - |\
		${SAMTOOLS}  sort - hg19 && \
		${SAMTOOLS} index hg19.bam


hg19ToHg38.over.chain:
	curl -o $@.gz http://hgdownload.cse.ucsc.edu/goldenPath/hg19/liftOver/$@.gz && gunzip -f $@.gz

```

file.diff :
```tsv
#READ-Name	COMPARE	hg19.bam	hg38.bam
HWI-1KL149:18:C0RNBACXX:3:1101:10375:80749/2	NE	chrMasked:581319279->chrMasked:581415967	chrMasked:581703411
HWI-1KL149:18:C0RNBACXX:3:1101:10394:60111/1	NE	chrMasked:581319428->chrMasked:581416116	chrMasked:581703560
HWI-1KL149:18:C0RNBACXX:3:1101:10394:60111/2	NE	chrMasked:581319428->chrMasked:581416116	chrMasked:581703560
HWI-1KL149:18:C0RNBACXX:3:1101:10413:69955/1	NE	chrMasked:581318674->chrMasked:581415362	chrMasked:581702806
HWI-1KL149:18:C0RNBACXX:3:1101:10413:69955/2	NE	chrMasked:581318707->chrMasked:581415395	chrMasked:581702839
HWI-1KL149:18:C0RNBACXX:3:1101:10484:89477/1	NE	chrMasked:581319428->chrMasked:581416116	chrMasked:581703560
HWI-1KL149:18:C0RNBACXX:3:1101:10484:89477/2	NE	chrMasked:581319428->chrMasked:581416116	chrMasked:581703560
HWI-1KL149:18:C0RNBACXX:3:1101:10527:3241/1	NE	chrMasked:581319279->chrMasked:581415967	chrMasked:581703411
HWI-1KL149:18:C0RNBACXX:3:1101:10527:3241/2	NE	chrMasked:581319279->chrMasked:581415967	chrMasked:581703411
HWI-1KL149:18:C0RNBACXX:3:1101:10580:13030/1	NE	chrMasked:581319331->chrMasked:581416019	chrMasked:581703463
HWI-1KL149:18:C0RNBACXX:3:1101:10580:13030/2	NE	chrMasked:581319331->chrMasked:581416019	chrMasked:581703463
HWI-1KL149:18:C0RNBACXX:3:1101:10618:51813/1	EQ	chrMasked:581318674->chrMasked:581415362	chrMasked:581415362
HWI-1KL149:18:C0RNBACXX:3:1101:10618:51813/2	NE	chrMasked:581318707->chrMasked:581415395	chrMasked:581702839
HWI-1KL149:18:C0RNBACXX:3:1101:10803:23593/1	NE	chrMasked:581319091->chrMasked:581415779	chrMasked:581703223
HWI-1KL149:18:C0RNBACXX:3:1101:10803:23593/2	NE	chrMasked:581319091->chrMasked:581415779	chrMasked:581703223
HWI-1KL149:18:C0RNBACXX:3:1101:11290:76217/1	EQ	chrMasked:581318674->chrMasked:581415362	chrMasked:581415362
HWI-1KL149:18:C0RNBACXX:3:1101:11290:76217/2	NE	chrMasked:581318707->chrMasked:581415395	chrMasked:581702839
HWI-1KL149:18:C0RNBACXX:3:1101:11307:71853/1	NE	chrMasked:581319254->chrMasked:581415942	chrMasked:581703386
HWI-1KL149:18:C0RNBACXX:3:1101:11307:71853/2	NE	chrMasked:581319258->chrMasked:581415946	chrMasked:581703390
HWI-1KL149:18:C0RNBACXX:3:1101:11359:100655/1	EQ	chrMasked:581318923->chrMasked:581415611	chrMasked:581415611
HWI-1KL149:18:C0RNBACXX:3:1101:11359:100655/2	EQ	chrMasked:581318923->chrMasked:581415611	chrMasked:581415611
HWI-1KL149:18:C0RNBACXX:3:1101:11467:8793/1	NE	chrMasked:581319428->chrMasked:581416116	chrMasked:581703560
HWI-1KL149:18:C0RNBACXX:3:1101:11467:8793/2	NE	chrMasked:581319428->chrMasked:581416116	chrMasked:581703560
HWI-1KL149:18:C0RNBACXX:3:1101:11560:69825/1	EQ	chrMasked:581319331->chrMasked:581416019	chrMasked:581416019
HWI-1KL149:18:C0RNBACXX:3:1101:11560:69825/2	EQ	chrMasked:581319335->chrMasked:581416023	chrMasked:581416023

```


## See also

* CmpBams

## History

* 2014: Creation
 
 

