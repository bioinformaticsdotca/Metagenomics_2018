---
layout: workshop_main_3day
permalink: /metagenomics_2018
title: Metagenomics
header1: Workshop Pages for Students
header2: Analysis of Metagenomic Data 2018
image: /site_images/CBW_Metagenome_icon.jpg
---

# Welcome <a id="welcome"></a>

Welcome to Analysis of Metagenomic Data.

Preparation details coming soon.

## Install these tools before the workshop:

1) A file decompression tool. For Windows/PC – 7zip (http://www.7-zip.org/). For Linux – gzip (http://www.gzip.org). For Mac – already there.
 
2) A robust internet browser such as Firefox or Safari (Internet Explorer and Chrome are not recommended because of Java issues).
 
3) Install Cytoscape 3.4.0: http://www.cytoscape.org/
http://chianti.ucsd.edu/cytoscape-3.4.0/
 
4) A PDF viewer (Adobe Acrobat or equivalent).
 
5) Install Java -The visualization program that we will be using (IGV) requires Java. Check if you have Java installed: https://www.java.com/verify/ and download Java if you do not have it installed.
 
6) Integrative Genomics Viewer (IGV) - Once java is installed, go to http://www.broadinstitute.org/igv/ and register in order to get access to the downloads page. Once you have gained access to the downloads page, click on the appropriate launch button that matches the amount of memory available on your laptop (if you have space, 1.2GB is good, more is better). Chrome: Chrome does not launch "java webstart" files by default. Instead, the launch buttons below will download a "jnlp" file. This should appear in the lower left corner of the browser. Double-click the downloaded file to run. Windows users: To run with more than 1.2 GB you must install 64-bit Java. This is often not installed by default even with the latest Windows 7 machines with many GB of memory. In general trying to launch with more memory than your OS/Java combination supports will result in the obscure error "could not create virtual machine".
 
7) SSH client - Mac and Linux users already have a command line ssh program that can be run from the terminal. For Windows users, please download PuTTY:http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html
 
8) SCP/SFTP client - We will be moving data from the servers to the student laptops for visualization. Mac and Linux users already have a command line scp and sftp program. For Windows users, please install WinSCP: http://winscp.net/eng/download.php
 
9) Install STAMP for your machine.

*Windows*
1. Download and install STAMP:
https://github.com/dparks1134/STAMP/releases/download/v2.0.9/STAMP_2_0_9.exe
 
*Mac OSX*
1. Install Xcode/Command line Tools:  https://developer.apple.com/xcode/downloads/
2. Install the Anaconda package for Python v2.7: http://continuum.io/downloads
3. Install PIP by opening a “Terminal” Window (Application->Utilities->Terminal) and then type the following command (and press enter):
sudo easy_install pip
4.Install numpy (again type the following command in the Terminal Window):
pip install numpy 
5. Install a scipy (again type the following command in the Terminal Window):
conda install scipy=0.14.0
6. Install STAMP (again type the following command in the Terminal Window):
pip install STAMP 
7. Open new terminal session and type “STAMP” to launch the graphical interface.
 
*Linux*
1. From a Terminal/Console window type:
sudo apt-get install freetype* python-pip python-dev python-numpy python-scipy python-matplotlib
2. Then type:
sudo pip install STAMP

10) Anvi'o: (http://merenlab.org/2016/06/26/installation-v2/)  

## Do these tutorials before the workshop:

**Command line tutorials**

* [UNIX Bootcamp](http://rik.smith-unna.com/command_line_bootcamp/?id=9xnbkx6eaof)
* [Unix Cheat sheet](http://www.rain.org/~mkummel/unix.html) 

**Cytoscape Preparation tutorials**  

Complete the [introductory tutorial to Cytoscape](http://opentutorials.cgl.ucsf.edu/index.php/Portal:Cytoscape3)

* Introduction to Cytoscape - User Interface

* Introduction to Cytoscape - Welcome Screen

* Filtering and Editing in Cytoscape 

[&uarr;](#back_to_top)

## Before coming to the workshop, read these:

  * [Bioinformatics for the Human Microbiome Project](http://www.ncbi.nlm.nih.gov/pubmed/23209389)
  
  * [Microbiome science needs a healthy dose of scepticism](http://www.ncbi.nlm.nih.gov/pubmed/25143098)
  
  * [Methylotrophic methanogenic Thermoplasmata implicated in reduced methane emissions from bovine rumen](http://www.ncbi.nlm.nih.gov/pubmed/23385573)
