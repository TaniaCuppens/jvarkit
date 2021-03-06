# Biostar165777

Split a XML file


## Usage

```
Usage: biostar165777 [options] Files
  Options:
    -N, --count
      Number of files to be created
      Default: 100
    -h, --help
      print help and exit
    --helpFormat
      What kind of help
      Possible Values: [usage, markdown, xml]
  * -o, --output
      Output file. Must contains __SPLIT__
    -T, --tag
      XML tag to be split.e.g 'Hit' in blast
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
$ make biostar165777
```

The *.jar libraries are not included in the main jar file, [so you shouldn't move them](https://github.com/lindenb/jvarkit/issues/15#issuecomment-140099011 ).
The required libraries will be downloaded and installed in the `dist` directory.

Experimental: you can also create a [fat jar](https://stackoverflow.com/questions/19150811/) which contains classes from all the libraries, on which your project depends (it's bigger). Those fat-jar are generated by adding `standalone=yes` to the gnu make command, for example ` make biostar165777 standalone=yes`.

### edit 'local.mk' (optional)

The a file **local.mk** can be created edited to override/add some definitions.

For example it can be used to set the HTTP proxy:

```
http.proxy.host=your.host.com
http.proxy.port=124567
```
## Source code 

[https://github.com/lindenb/jvarkit/tree/master/src/main/java/com/github/lindenb/jvarkit/tools/biostar/Biostar165777.java](https://github.com/lindenb/jvarkit/tree/master/src/main/java/com/github/lindenb/jvarkit/tools/biostar/Biostar165777.java)


<details>
<summary>Git History</summary>

```
Wed May 24 17:27:28 2017 +0200 ; lowres bam2raster & fix doc ; https://github.com/lindenb/jvarkit/commit/6edcfd661827927b541e7267195c762e916482a0
Fri May 12 18:07:46 2017 +0200 ; cont ; https://github.com/lindenb/jvarkit/commit/ca96bce803826964a65de33455e5231ffa6ea9bd
Thu May 11 16:20:27 2017 +0200 ; move to jcommander ; https://github.com/lindenb/jvarkit/commit/15b6fabdbdd7ce0d1e20ca51e1c1a9db8574a59e
Fri Apr 14 15:27:32 2017 +0200 ; annotation proc ; https://github.com/lindenb/jvarkit/commit/72b9383a8472e5a91120bab84d15b8acad4db8d4
Sat Nov 14 16:20:44 2015 +0100 ; xml ; https://github.com/lindenb/jvarkit/commit/ab1815e63787ab15d31f4878c8772c74f1376ad0
```

</details>

## Contribute

- Issue Tracker: [http://github.com/lindenb/jvarkit/issues](http://github.com/lindenb/jvarkit/issues)
- Source Code: [http://github.com/lindenb/jvarkit](http://github.com/lindenb/jvarkit)

## License

The project is licensed under the MIT license.

## Citing

Should you cite **biostar165777** ? [https://github.com/mr-c/shouldacite/blob/master/should-I-cite-this-software.md](https://github.com/mr-c/shouldacite/blob/master/should-I-cite-this-software.md)

The current reference is:

[http://dx.doi.org/10.6084/m9.figshare.1425030](http://dx.doi.org/10.6084/m9.figshare.1425030)

> Lindenbaum, Pierre (2015): JVarkit: java-based utilities for Bioinformatics. figshare.
> [http://dx.doi.org/10.6084/m9.figshare.1425030](http://dx.doi.org/10.6084/m9.figshare.1425030)


## Example

```bash
$  java -jar dist-1.139/biostar165777.jar -o out__SPLIT__.xml -T Hit -N 5 ~/blastn.xml

$ ls -la ~/blastn.xml out*.xml
-rw-rw-r-- 1 lindenb lindenb 422606 nov.  14 12:47 /home/lindenb/blastn.xml
-rw-rw-r-- 1 lindenb lindenb  86319 nov.  14 16:17 out001.xml
-rw-rw-r-- 1 lindenb lindenb  83570 nov.  14 16:17 out002.xml
-rw-rw-r-- 1 lindenb lindenb  85096 nov.  14 16:17 out003.xml
-rw-rw-r-- 1 lindenb lindenb  88297 nov.  14 16:17 out004.xml
-rw-rw-r-- 1 lindenb lindenb  87123 nov.  14 16:17 out005.xml


$ grep -cF "<Hit>" ~/blastn.xml out*.xml
/home/lindenb/blastn.xml:100
out001.xml:20
out002.xml:20
out003.xml:20
out004.xml:20
out005.xml:20

```

