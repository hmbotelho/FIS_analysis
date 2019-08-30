# Open source forskolin-induced swelling (FIS) assay analysis workflow protocol
7 August 2019

# About
The forskolin-induced swelling (FIS) assay has become the preferential assay of choice to assess the efficacy of established and investigational CFTR-modulating compounds for individuals that suffer from cystic fibrosis (CF). In this step-by-step protocol, we describe a complete workflow using open-source software to perform standardized analysis of CFTR function measurements of intestinal (CF) organoids. The workflow comprises 3 major steps:
1. Renaming of raw microscopy images (include experimental metadata in the file names);
2. Image analysis in CellProfiler or Fiji/ImageJ (measurement of organoid area);
3. Data normalization and statistical analysis (_Organoid Analyst_ web app).

![Summary pannel](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/misc/pannel.gif)

All software tools, together with a demonstration dataset and expected results from the analyses are included in this document as hyperlinks.

# Table of Contents
- [Citation](#markdown-header-Citation)
- [License](#markdown-header-License)
- [1. Setup](#markdown-header-1.-Setup)
  - [1.1. ZEN (blue edition)](#markdown-header-1.1.-ZEN-(blue-edition))
  - [1.2. Leica Application Suite (LAS X)](#markdown-header-1.2.-Leica-Application-Suite-(LAS-X))
  - [1.3. Java](#markdown-header-1.3.-Java)
  - [1.4. R](#markdown-header-1.4.-R)  
    - [1.4.1. Windows](#markdown-header-1.4.1.-Windows)
    - [1.4.2. macOS](#markdown-header-1.4.2.-macOS)
  - [1.5. CellProfiler](#markdown-header-1.5.-CellProfiler)
  - [1.6. Fiji / ImageJ](#markdown-header-1.6.-Fiji-/-ImageJ)
    - [1.6.1. Windows](#markdown-header-1.6.1.-Windows)
    - [1.6.2. macOS](#markdown-header-1.6.2.-macOS)
- [2. Demonstration dataset](#markdown-header-2.-Demonstration-dataset)
- [3. Generating TIF files from raw data](#markdown-header-3.-Generating-TIF-files-from-raw-data)
  - [3.1. Zeiss](#markdown-header-3.1.-Zeiss)
  - [3.2. Leica](#markdown-header-3.2.-Leica)
- [4. Creating well descriptors](#markdown-header-4.-Creating-well-descriptors)
- [5. File Renaming Tools](#markdown-header-5.-File-Renaming-Tools)
  - [5.1. Zeiss](#markdown-header-5.1.-Zeiss)
  - [5.2. Leica](#markdown-header-5.2.-Leica)
- [6. Image analysis](#markdown-header-6.-Image-analysis)
  - [6.1. CellProfiler](#markdown-header-6.1.-CellProfiler)
  - [6.2. Fiji / ImageJ](#markdown-header-6.2.-Fiji-/-ImageJ)
    - [6.2.1. Test Mode](#markdown-header-6.2.1.-Test-Mode)
    - [6.2.2. Batch Analysis Mode](#markdown-header-6.2.2.-Batch-Analysis-Mode)
- [7. Statistical analysis](#markdown-header-7.-Statistical-analysis)
- [8. Interpretation of the results](#markdown-header-8.-Interpretation-of-the-results)


# Citation
Marne C. Hagemeijer, Annelotte M. Vonk, Evelien Kruisselbrink, Johanna F. Dekkers, Nikhil T. Awatade, Jeffrey M. Beekman, Margarida D. Amaral, and Hugo M. Botelho (2019) **An open-source high-content analysis workflow for CFTR function measurements using the forskolin-induced swelling assay.**


# License
This project is licensed under the terms of the GNU General Public License v3.0 (GNU GPLv3).


# 1. Setup
The setup process only needs to be performed when running the workflow for the first time. 

The required software can be downloaded from the following sources:
- ZEN (blue edition, optional). [Download the LITE version](https://www.zeiss.com/microscopy/int/products/microscope-software/zen-lite.html)
- [Java](https://www.oracle.com/technetwork/java/javase/downloads/)
- [R](https://cran.r-project.org/)
- [HTM renamer](https://cirrus.ciencias.ulisboa.pt/owncloud/s/44k3Q8HPQKLzqG9) (R package)
- [XQuartz](https://www.xquartz.org/) (macOS users only)
- [Microscope Infile templates](https://cirrus.ciencias.ulisboa.pt/owncloud/s/xSE7GDZGtELspx8)
- [CellProfiler](https://cellprofiler.org/releases/)
- [Fiji](http://fiji.sc/)
- [Image analysis pipeline (CellProfiler)](https://cirrus.ciencias.ulisboa.pt/owncloud/s/jamiefP6Z4RT7Tm)
- [Image analysis pipeline (Fiji)](https://cirrus.ciencias.ulisboa.pt/owncloud/s/AnQY4FZDpYXbRRk)
- [Organoid Analyst](https://cirrus.ciencias.ulisboa.pt/owncloud/s/r9joKzmFcLLio9J)
- [Demo dataset](https://cirrus.ciencias.ulisboa.pt/owncloud/s/6C9qx5TxoY9imFG)

Detailed instructions on how to use and install each piece of software are provided below.

## 1.1. ZEN (blue edition)
![Zen Blue](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/ZenBlue/zen_logo_100x100.png)
[*Download ZEN (blue edition LITE)*](https://www.zeiss.com/microscopy/int/products/microscope-software/zen-lite.html)

To perform the open source FIS analysis workflow the raw images have to be in the TIF format. Raw datasets acquired with Zeiss microscopes and stored as CZI files need to be exported in the TIF format. This can be done using the ZEN (blue edition) software. Please note that ZEN software is a Windows only application.

## 1.2. Leica Application Suite (LAS X)
![LAS X](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/Leica/lasx_logo_100x100.png)
High-content Leica microscope experiments generated with the LAS X software can be saved as OME-TIFF files during acquisition when using the Leica MatrixScreener software without the need of manual exporting or converting of the raw datasets.

## 1.3. Java
![Java](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/Java/java_logo_81x150.png)
[*Download Java*](https://www.oracle.com/technetwork/java/javase/downloads/)

Java is required by the R analytical tools. Download and install the Oracle JDK version which matches your operating system (Windows, macOS) and system architecture (32 bit, 64 bit).
Users of older CellProfiler versions (up to 2.2.0) will also need the Java JRE. This is not required in the current version (3.1.5).

## 1.4. R
![R](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/R/R_logo_129x100.png)
[*Download R*](https://cran.r-project.org/)
[*Download XQuartz*](https://www.xquartz.org/)

The R software provides a back-end for the file management and statistical analysis tools used for the open source FIS workflow. Download the R version that matches your operating system and install the program. macOS users must also download and install XQuartz.

Analytical tools can be downloaded from the links below:
- [htmrenamer R package](https://cirrus.ciencias.ulisboa.pt/owncloud/s/44k3Q8HPQKLzqG9)
- [Organoid Analyst](https://cirrus.ciencias.ulisboa.pt/owncloud/s/r9joKzmFcLLio9J)

Below are instructions on how to run the tools in Windows and macOS.

### 1.4.1. Windows
1. Run R by clicking the icon on your desktop or Start menu.
2. The R window will appear:
    ![R - Windows 10](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/R/r_windows.png)


3. To install the **`htmrenamer`** package use the menu bar and select `Packages` > `Install package(s) from local files...` and the `htmrenamer_1.0.0.tar.gz` file you downloaded previously. This step is only required in the first time the tool is run.
4. Analytical tools can be run by writing the appropriate commands in the command prompt (the area next to the red `>` symbol at the bottom of the screen). Press Enter/Return after each command line.
5. To run the file renaming tools, write:
    ```
    library(htmrenamer)
    rename_zeiss_gui()
    ```
    or 
    ```
    library(htmrenamer)
    rename_leica_gui()
    ```
    If prompted, select a CRAN mirror to download the required additional libraries.

6. The **Organoid Analyst** data analysis tool does not require an installation. To run it, unzip the [file](https://cirrus.ciencias.ulisboa.pt/owncloud/s/r9joKzmFcLLio9J) you downloaded previously ('extract here') and write:
    ```
    setwd("c:/users/yourname/desktop/organoid_analyst")
    source("run.r")
    ```
    In the first command, use the folder with the contents of the unzipped file. If prompted, select a CRAN mirror to download the required additional libraries.

7. Starting the tools for the first time may take a few minutes.


**Note 1**: Slashes can be written wither as forward slashes ("/") or double reverse slashes ("\\\\")
**Note 2**: If any of the required packages fails to install (error message similar to ` Error: package or namespace load failed for ‘packagename’`), remove the problematic package with `remove.packages(packagename)` and run the software again (steps 5 or 6).
**Note 3**: If JDK is not installed on your computer and you try to run `htmrenamer` or Organoid Analyst, R will throw an error message as the one shown below. In this case, follow step 1.3 to install the JDK and run R again.
```
Error: package or namespace load failed for ‘rJava’:
 .onLoad failed in loadNamespace() for 'rJava', details:
  call: fun(libname, pkgname)
  error: JAVA_HOME cannot be determined from the Registry
In addition: Warning message:
package ‘rJava’ was built under R version 3.5.3 
```

### 1.4.2. macOS
1. Run R by clicking the icon on the Launchpad or Dock.
2. The R window will appear:
    ![R - macOS](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/R/r_macos.png)


3. To install the **`htmrenamer`** package use the menu bar and select `Packages & Data` > `Package Installer`. In the R package installer selest `Local Source Package` as the package location, click `Install...` and select the `htmrenamer_1.0.0.tar.gz` file you downloaded previously. This step is only required in the first time the tool is run.
    ![R - macOS install packages](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/R/mac_install_local_package.png)


4. Analytical tools can be run by writing the appropriate commands in the command prompt (the area next to the red `>` symbol at the bottom of the screen). Press Enter/Return after each command line.
5. To run the file renaming tools, write:
    ```
    library(htmrenamer)
    rename_zeiss_gui()
    ```
    or 
    ```
    library(htmrenamer)
    rename_leica_gui()
    ```
    If prompted, select a CRAN mirror to download the required additional libraries.


6. The **Organoid Analyst** data analysis tool does not require an installation. To run it, unzip the [file](https://cirrus.ciencias.ulisboa.pt/owncloud/s/r9joKzmFcLLio9J) you downloaded previously ('extract here') and write:
    ```
    setwd("/Users/yourname/Desktop/renamer_zeiss")
    source("run.r")
    ```
    In the first command, use the folder with the contents of the unzipped file. If prompted, select a CRAN mirror to download the required additional libraries.

7. Starting the tools for the first time may take a few minutes.

**Note 1**: If XQuartz is not installed, R will freeze when running the file renamer tools or when loading data into Organoid Analyst.
**Note 2**: If any of the required packages fails to install (error message similar to ` Error: package or namespace load failed for ‘packagename’`), remove the problematic package with `remove.packages(packagename)` and run the software again (steps 5 or 6).
**Note 3:** If R thows an error message similar to
```
Error: package or namespace load failed for ‘rJava’:
 .onLoad failed in loadNamespace() for 'rJava', details:
  call: dyn.load(file, DLLpath = DLLpath, ...)
  error: unable to load shared object '/Users/yourname/Library/R/3.6/library/rJava/libs/rJava.so':
  dlopen(/Users/yourname/Library/R/3.6/library/rJava/libs/rJava.so, 6): Library not loaded: /Library/Java/JavaVirtualMachines/jdk-11.0.1.jdk/Contents/Home/lib/server/libjvm.dylib
  Referenced from: /Users/yourname/Library/R/3.6/library/rJava/libs/rJava.so
  Reason: image not found
```
make sure that JDK is installed. If not, install it and run steps 5 or 6 again.

This error arises due to an incorrect linking of the JDK version installed in your computer and the `rJava` package, which both `htmrenamer` and Organoid Analyst depend on. To relink Java correctly open a Terminal window and enter:
`sudo R CMD javareconf`
Check whether steps 5 or 6 are now working.
If the error persists, the only known solution is to replace the currently installed JDK by the one mentioned in the error message (version 10.0.1 in the exampe above). Please be aware that older Java versions may have security weaknesses.

JDK may be uninstalled by pasting the following commands on a Terminal window:

*Uninstall Java*
```
sudo rm -fr /Library/Internet\ Plug-Ins/JavaAppletPlugin.plugin 
sudo rm -fr /Library/PreferencePanes/JavaControlPanel.prefPane 
sudo rm -fr ~/Library/Application\ Support/Oracle/Java
```
*Uninstall JDK (replace `<version>` with the JDK version installed on your system)*
```
sudo rm -rf /Library/Java/JavaVirtualMachines/jdk<version>.jdk
```
*Uninstall Java plugins*
```
sudo rm -rf /Library/PreferencePanes/JavaControlPanel.prefPane
sudo rm -rf /Library/Internet\ Plug-Ins/JavaAppletPlugin.plugin
sudo rm -rf /Library/LaunchAgents/com.oracle.java.Java-Updater.plist
sudo rm -rf /Library/PrivilegedHelperTools/com.oracle.java.JavaUpdateHelper
sudo rm -rf /Library/LaunchDaemons/com.oracle.java.Helper-Tool.plist
sudo rm -rf /Library/Preferences/com.oracle.java.Helper-Tool.plist
```

Any given JDK version can be downloaded from the [Oracle Java Archive](https://www.oracle.com/technetwork/java/javase/archive-139210.html). Select and install the one that matches your operating system (Windows, macOS), system architecture (32 bit, 64 bit) and the version mentioned in the error message.
You should now be able to run steps 5 or 6.


## 1.5. CellProfiler
![CellProfiler logo](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/CellProfiler/cp_logo_400x100.png)
[*Download CellProfiler (CP)*](https://cellprofiler.org/releases/)
[*Download CP pipeline file*](https://cirrus.ciencias.ulisboa.pt/owncloud/s/ZACNbGtBdyibCRM)

CellProfiler is the preferred image analysis software for analysing most FIS image datasets. It is available for both Windows and macOS. Download and install the version which matches the operating system of your computer.

## 1.6. Fiji / ImageJ
![](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/Fiji/fiji_imagej_logo_200x100.png)
[*Download Fiji*](http://fiji.sc/)
[*Download image analysis scripts*](https://cirrus.ciencias.ulisboa.pt/owncloud/s/AnQY4FZDpYXbRRk)

As an alternative to the CellProfiler software, we developed an image analysis algorithm as an ImageJ1 macro. We recommend using the Fiji distribution of ImageJ. Download and install the version for your operating system. Instructions on how to install the ImageJ1 macro on machines running Windows or macOS are described below.

| ![Uneven background](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/misc/uneven_background.jpg) | |  ![Dye dilution](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/misc/bleach.gif) |
|:-----:|:-----:|:-----:|
| Background gradient | | Dye dilution |

The ImageJ analysis is recommended in the following cases:
- **Images with irregular fluorescence background or background gradients:** The script applies a pseudo-flat field background correction to remove these artefacts.
- **Assays with large organoid swelling:** Calcein fluorescence may become dim due to dilution of the dye upon swelling of the organoids. This may significantly complicate proper segmentation. The type of segmentation artefacts depends on whether or not the “fill holes” operation is performed after thresholding:
    - _Without the 'fill holes' operation:_ Highly swollen organoids may be segmented as ring-shaped objects due to calcein dilution in the center.

    - _With the 'fill holes' operation:_ Clusters of densely packed organoids may swell as such that they touch each other and fully encircle organoid-free (_i.e._ background) patches. The fill holes operation will attribute these background pixels to one of the organoid objects resulting in overestimation of the area (see  [this](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/CellProfiler/fillholes.gif) example in 6.1.13).
    The script solves these artefacts by assuming that organoid radial expansion (_i.e._ swelling) is much larger than lateral displacement during the time lapse by applying a ‘conditional fill holes’ procedure:
        1. The fluorescence image is thresholded with holes left unfilled (**B**).
        2. All pixels located in holes in the thresholded image are identified (**C**).
        3. The intersection of hole pixels with the final thresholded image in the previous time point is calculated (**D**). There are no intersecting pixels in the first time point. Since organoids expand radially, this image is expected to highlight the pixels where fluorescence was diluted below the threshold value since the previous frame: true holes in organoids. Background pixels are not expected to be part o0f this image because they should always be below the threshold value.
        4. The final thresholded image is the union of images B and D (**E**).
![ImageJ algorithm](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/Fiji/IJ_algorithm.gif)
_This time lapse corresponds to a portion of [well B8, #20](https://cirrus.ciencias.ulisboa.pt/owncloud/s/QFBPPGPoqMACJio) in the demonstration dataset. Segmentation masks have been re-colored to accurately track objects._

Installation of the ImageJ1 macro on Windows or macOS is described below.

### 1.6.1. Windows

1. Locate the Fiji installation folder. This is usually `C:\Fiji.app`
2. In folder `\Fiji.app\scripts` create a new folder named `FIS`.
3. Copy files `FIS_analysis....ijm` and `FIS_test....ijm` into the `FIS` folder.
4. Run Fiji.
5. Fiji will now have a new “FIS” tool in the menu bar:
![Fiji win10](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/Fiji/window_win10_FISmenu.png)

### 1.6.2. macOS
1. In Finder, go to the Applications folder and locate the Fiji icon
2. Right click on the Fiji icon and select the `Show Package Contents` option. A 
3. In folder `Scripts` folder create a new folder named `FIS`.
4. Copy files `FIS_analysis....ijm` and `FIS_test....ijm` into the `FIS` folder.
5. Run Fiji.
6. Fiji will now have a new “FIS” tool in the menu bar:
![Fiji macOS](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/Fiji/window_macOS_FISmenu.png)

## 2. Demonstration dataset
![](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/misc/code.png)
[*Download demonstration dataset*](https://cirrus.ciencias.ulisboa.pt/owncloud/s/6C9qx5TxoY9imFG)

The open source FIS analysis workflow described in this online protocol will be demonstrated using a demonstration image dataset. By following steps 3 - 7 you will be able to reproduce the results shown in this document and learn how to use this workflow to analyse your own FIS datasets.

**Assay Description of demonstration dataset**
The FIS assay was performed using intestinal organoids homozygous for a class II CFTR mutation in the absence (DMSO) or presence of VX-809 and/or VX-770 (3.2 μM). CFTR was activated by addition of forskolin (Fsk) in a concentration range from 0,008 μM – 5 μM. The layout of the assay plate is depicted below.

![](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/sample_dataset/plate_layout_733x205.png)

- __Number of plates:__ 1
- __Number of wells:__ 64
- __Number of imaging fields per well:__ 1
- __Number of raw images:__ 448
- __Number of timepoints:__ 7
- __Time interval between frames:__ 10 min
- __Total experiment time:__ 60 min
- __Image resolution:__ 512 x 512 pixels  
- __Pixel dimensions:__ 4.991 x 4.991 μm
- __Image bit depth:__ 8 bit
- __Number of fluorescence channels:__ 1 (calcein green)

Several versions of the demonstration dataset are available to download:

- __**Full dataset**__: includes all the data  [[download](https://cirrus.ciencias.ulisboa.pt/owncloud/s/6C9qx5TxoY9imFG)] (298 MB)
  

- Raw images [[download](https://cirrus.ciencias.ulisboa.pt/owncloud/s/mnY3oPZWiTYZdjS)] (91.8 MB)
- Well descriptors [[download](https://cirrus.ciencias.ulisboa.pt/owncloud/s/F6r4n8mB7cntpRZ)] (3 KB)
- Renamed images [[download](https://cirrus.ciencias.ulisboa.pt/owncloud/s/GDWNSqp58GrELwg)] (91.9 MB)
- Image analysis pipelines [[download](https://cirrus.ciencias.ulisboa.pt/owncloud/s/zBQntTHj2GgaRWD)] (152 kB)
- Analyzed images (with CellProfiler) [[download]](https://cirrus.ciencias.ulisboa.pt/owncloud/s/4QgLGw9oZpm3Aae) (16 MB)
- Analyzed images (with Fiji) [[download]](https://cirrus.ciencias.ulisboa.pt/owncloud/s/jZW3Y7kHMnT9Fy7) (17.5 MB)
- Results after statistical analysis (data from CellProfiler) [[download]](https://cirrus.ciencias.ulisboa.pt/owncloud/s/Y3rtii6jtnA45Kk) (40.4 MB)
- Results after statistical analysis (data from Fiji) [[download]](https://cirrus.ciencias.ulisboa.pt/owncloud/s/s46NNJj4xRHz7Gc) (39.7 MB)

# 3. Generating TIF files from raw data
[*Download raw images (Zeiss microscope)*](https://cirrus.ciencias.ulisboa.pt/owncloud/s/mnY3oPZWiTYZdjS)

The FIS analysis pipeline supports fluorescence microscopy images generated by Zeiss or Leica automated microscopes that are saved, or converted into TIF files.

## 3.1. Zeiss

Images acquired using Zeiss software are stored in the native CZI file format and have to be exported in a TIF format using the file exporter function included in the ZEN (blue edition) software:
1. Open a CZI file (`File > Open...`)
2. Open the file exporter  (`File > Export/Import > Export`)
3. Export images as TIF files. LZW compression is advised. A target folder and prefix for all images (_e.g._ the experiment name) are required:
![Zen export settings](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/ZenBlue/zen_export.png)
4. Start the export by clicking "Apply"

## 3.2. Leica
Images should be acquired using Leica’s LAS X software using the MatrixScreener module and exported as OME-TIFF:

![MatrixScreener GUI](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/Leica/MatrixScreener_small_crop.png)

This procedure will generate a folder with a name similar to `demoplate_01--2019_01_01_12_00_00`, which contains all microscopy files and metadata:

![MatrixScreener folders](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/Leica/MatrixScreener_folders.png)

These OME-TIFF files can be used in step [5 (File Renaming Tools)](#markdown-header-5.-File-Renaming-Tools).

# 4. Creating well descriptors
[*Download blank template files*](https://cirrus.ciencias.ulisboa.pt/owncloud/s/xSE7GDZGtELspx8)
[*Download infile for demonstration dataset*](https://cirrus.ciencias.ulisboa.pt/owncloud/s/5DmPjkpNG8YAQjf)

The analysis workflow requires that each experimental treatment of each well (_e.g._ compound(s) and concentration) must be described systematically in a so-called text-based microscope “infile”. The microscope infile has a table-like structure:

**Infile structure:**
```
001--A--01--00--00--fsk--0.008
002--A--02--01--00--fsk--5
003--A--03--02--00--fsk_vx809--0.008
004--A--04--03--00--fsk_vx809--5
005--A--05--04--00--fsk_vx770--0.008
006--A--06--05--00--fsk_vx770--5
```

Columns are separated by a double dash (--) and represent:
- `Well Number (001, 002, 003, ...)`
- `Row Coordinate (A, B, C, ...)`
- `ColumnCoordinate (01, 02, 03, ...)`
- `ColumnCoordinate (00, 01, 02, ...)`
- `Row Coordinate (00, 01, 02, ...)`
- `Compound name (fsk, fsk_vx809, ...)`
- `Compound concentration (0.005, 5, ...)`

Blank infile templates in Excel format have been included for both  [96 well](https://cirrus.ciencias.ulisboa.pt/owncloud/s/tMiaW7C4nyJBdtG) and [384 well](https://cirrus.ciencias.ulisboa.pt/owncloud/s/6QF9kfyJS3JQxXa) assay formats. To generate a customized infile, complete the `Layout` sheet with the contents of each well. Each well can contain two descriptors:
- **Top cell**: Name of the compound(s) being tested
- **Bottom cell**: compound concentration, written as a number and without units  

Following these recommendations will greatly simplify the analysis.

![Infile template - layout](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/infiles/layout.png)

The infile structure will be generated automatically in sheet `WellList`. Copy column `I` to a text file to create a custom infile.
Save the text file with a name corresponding to the experiment name (or plate name).

![Infile template - well list](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/infiles/welllist.png)

# 5. File Renaming Tools
[*Download the htmrenamer R package*](https://cirrus.ciencias.ulisboa.pt/owncloud/s/44k3Q8HPQKLzqG9)
[*Download Zeiss TIF files (before renaming)*](https://cirrus.ciencias.ulisboa.pt/owncloud/s/mnY3oPZWiTYZdjS)
[*Download Zeiss TIF files (after renaming)*](https://cirrus.ciencias.ulisboa.pt/owncloud/s/GDWNSqp58GrELwg)

TIF images from step 3 will be renamed to include relevant metadata in the file name (_e.g._ well contents and timelapse information). This information in conveyed through the infile generated in step 4.

File renaming is achieved with the `htmrenamer` R package, which supports images generated by Zeiss and Leica high-content microscopes. The renaming algorithm is based on a workflow originally implemented at the [EMBL](https://www.embl.de) [Advanced Light Microscopy Facility (ALMF)](https://www.embl.de/services/core_facilities/almf) by [Volker Hilsenstein](https://monash.ilab.agilent.com/service_center/show_external/4442?name=monash-micro-imaging-clayton), Rubén Muñoz and others.

## 5.1. Zeiss

1. Start R and install `htmrenamer` as described in [section 1.4.](#markdown-header-1.4.-R)
2. Run the tool by typing the following commands:

    ```
    library(htmrenamer)
    rename_zeiss_gui()
    ```
    The commands work in Windows and Mac OS.

3. If prompted, select a CRAN mirror to download the required additional libraries.
4. The renaming tool window will appear:
![Renaming tool - Zeiss](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/renamer/renamer_zeiss.png)


5. Specify the following details of the experiment:
    - **Select input folder:** Folder with exported TIF files.
    - **Select output folder:** Folder where renamed images should be saved to.
    - **Select Microscope Infile:** Location of the infile.
    - **Number of rows:** Number of rows in the assay plate, regardless of how many wells were imaged. [96 well plates: 8 rows][384 well plates: 16 rows].
    - **Number of columns:** Number of columns in the assay plate, regardless of how many wells were imaged. [96 well plates: 12 columns][384 well plates: 24 columns].


6. Click the `Start renaming` button to start the file renaming process.
7. The progress of renaming of the files will be shown in the log text box as well as in the R console.
8. The files will be saved in a folder with the same name as the infile:
![Infile template - layout](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/renamer/renamer_files.png)
9. The renaming process will create a folder structure which includes the following metadata in the file and folder names:
    - **Plate name**: `demoplate_01`
    - **Well**: `W0001`, `W0002`, ...
    - **Sub-position within the well**: `P001`
    - **Time number**: `T0000`, `T0001`, ...
    - **Fluorescence channel**: `C00`
    - **Compound identity**: `fsk`, `fsk_809`, ...
    - **Compound concentration**: `0.008`, `5`, ...


## 5.2. Leica

1. Start R and install `htmrenamer` as described in [section 1.4.](#markdown-header-1.4.-R)
2. Run the tool by typing the following commands:

    ```
    library(htmrenamer)
    rename_leica_gui()
    ```
    The commands work in Windows and Mac OS.

3. If prompted, select a CRAN mirror to download the required additional libraries.
4. The renamer tool window will appear:
![Renaming tool - Leica](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/renamer/renamer_leica.png)
5. Specify the following details of the experiment:
- **Select input folder:** Folder with exported TIF files.
- **Select output folder:** Folder where renamed images should be saved to.
- **Select Microscope Infile:** Location of the infile.
- **Output experiment descriptors:** Export experiment metadata in Excel and CSV format.
- **Lossless compression:** Performs lossless image compression using the “deflate” algorithm. Typically, this produces a file size ~20% lower than LZW-compressed images but removes all metadata (_e.g._ pixel size calibration).
6. Click the `Start conversion` button to start the file renaming process.
7. The progress of renaming of the files will be shown in the log text box as well as in the R console.
5. The files will be saved in a folder with the same name as the infile:
![Infile template - layout](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/renamer/renamer_files.png)
6. The renaming process will create a folder structure which includes the following metadata in the file and folder names:
    - **Plate name**: `demoplate_01`
    - **Well**: `W0001`, `W0002`, ...
    - **Sub-position within the well**: `P001`
    - **Time number**: `T0000`, `T0001`, ...
    - **Fluorescence channel**: `C00`
    - **Compound identity**: `fsk`, `fsk_809`, ...
    - **Compound concentration**: `0.008`, `5`, ...

# 6. Image analysis
This step of the workflow processes the renamed microscopy images from section 5 to extract the following features:
- Quantitative measurements (_e.g._ organoid area)
- Experimental metadata (_e.g._ compound and concentration)

The high-content image analysis algorithm stores the extracted data in tabular text files (CSV) together with the segmentation masks in image format (PNG).

Similar analysis algorithms have been implemented in CellProfiler and Fiji / ImageJ. Both image analysis pipelines perform the following consecutive actions to for every image:
1. Background correction
2. Organoid segmentation
3. Quality control: rejection of aberrant organoids (optional)
4. Measurement of organoid area
5. Export of measurements and segmentation masks

## 6.1. CellProfiler
[*Download blank pipeline file*](https://cirrus.ciencias.ulisboa.pt/owncloud/s/jamiefP6Z4RT7Tm)
[*Download demonstration dataset*](https://cirrus.ciencias.ulisboa.pt/owncloud/s/GDWNSqp58GrELwg)
[*Download pipeline file pre-configured for the demonstration dataset*](https://cirrus.ciencias.ulisboa.pt/owncloud/s/kbN8WEmSsWqZQsz)
[*Download CellProfiler analysis of the demonstration dataset*](https://cirrus.ciencias.ulisboa.pt/owncloud/s/4QgLGw9oZpm3Aae)

We recommend using CellProfiler for analysis of most datasets. The Fiji / ImageJ approach is recommended for challenging datasets (see section 1.6). Comprehensive information about all CellProfiler features can be obtained by clicking on the !['?'](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/CellProfiler/help_button.png) buttons throughout the GUI or in the [online documentation](https://cellprofiler.org/manuals/).

1. Open CellProfiler.
2. Load the pipeline file with `File > Open Project...`
3. Click on `Window > Show All Windows On Run`. This option will make CellProfiler display all image processing steps as they occur.
4. The input modules in the left part of the main menu (Images, Metadata, ...) correspond to the individual steps in the image analysis pipeline. Clicking on their names will reveal the settings of each module.
5. In the **Images** module add the renamed microscopy images to the white box named `Drop files and folders here`.
_The pipeline file, which has been pre-configured for the demonstration dataset, already has the image files listed. However, the file location needs to be updated in each computer. Therefore, remove the files with `Right click > Clear File List` and add them from a local folder in your computer._
![LoadImages](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/CellProfiler/01_CP_Images.png)
6. The **Metadata** module requires no changes. It extracts text-based metadata from the file and folder names.
7. The **NamesAndTypes** module requires no changes when the renamed microscopy image filenames end as **--C00.tif** or **--C00.ome.tif**. Otherwise, replace `C00` with the correct channel name as shown in the text box below:
![NamesAndTypes](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/CellProfiler/03_CP_NamesAndTypes_zoom.png)


8. The **Groups** module requires no changes. It defines the time lapse experiment.

    The remaining modules in the bottom left corner (_e.g._ image and object processing) perform image analysis tasks which can be explored interactively by activating `Test > Start Test Mode`

9. Here, we will use [well B8 (well #20)](https://cirrus.ciencias.ulisboa.pt/owncloud/s/QFBPPGPoqMACJio) for optimizing the analysis settings.
10. Go to  `Test > Choose Image Group`
![NamesAndTypes](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/CellProfiler/05_CP_ChooseGroup.png)


In the steps below, the active module will appear underlined (_e.g._ ![active module example](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/CellProfiler/activemodule.png)). click on ![Step](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/CellProfiler/step.png) to see the output of the active module.
11. The **ColorToGray** module requires no changes. It either selects the green channel (RGB images) or does nothing at all (grayscale images).
12. The **RescaleIntensity** module requires no changes. It stretches pixel grey values to the full intensity range to maximize the dynamic range and remove background offsets.
13. The **IdentifyPrimaryObjects** module performs organoid segmentation and should be adjusted for each experiment:
![IdentifyPrimaryObjects test](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/CellProfiler/08_IdentifyPrimaryObjects_test2.png)
    With the exception of `Name the primary objects to be identified` all settings can be adjusted. The following settings are usually the most relevant:
    - **Typical diameter of objects:** In pixel units. The minimum and maximum size of a circle with the same area as the organoids. Organoids outside this range will be discarded.
    - **Threshold smoothing scale:** Controls image smoothing before the thresholding step. Images with noise usually require more smoothing.
    - **Threshold correction factor:** Controls threshold stringency. A factor of 1 means no adjustment, 0 to 1 lowers the threshold value and > 1 increases the threshold value. 
    - **Lower and upper bounds on threshold:** Range: [0 ~ 1]. Defines the range where the threshold value will be in.
    - **Method to distinguish clumpled objects:** Setting to distinguish identified objects as single organoids or several ones touching each other.
    - **Method to draw dividing lines between clumpled objects:** Setting to separate organoids classified as being in a group of touching organoids.
    - **Size of smoothing filter:** Controls image smoothing during the declumping process of clumped objects.
    - **Suppress local maxima that are closer than...:** In pixel units. Defines the maximum radius in which only 1 organoid should be present (the approximate radius of the smallest expected organoid).
    - **Fill holes in identified objects:** Allows for filling holes of the identified objects after thresholding of the image. When calcein labelling is intense across all wells and time points, `Never` should be selected, as this typically results in less artefacts. However, when there is significant organoid swelling calcein fluorescence is frequently low in the lumen producing objects with holes. We recommend selecting `After both thresholding and declumping` when this occurs. Filling holes may produce an overestimation of organoid size if densely packed organoids touch each other and produce voids (see below).
    **Note:** Use the ImageJ analysis pipeline whenever segmentation in CellProfiler is not satisfactory.
    ![Fill holes](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/CellProfiler/fillholes.gif)
    _Fluorescence image before and after thresholding with and without `Fill holes after both thresholding and declumping`. Note that not filling holes produces an unsatisfactory segmentation with ring-shaped organoids (arrows). When Fill Holes is enabled, most organoids are correctly segmented but a background region is wrongly classified as object at the 40 min frame (arrowhead)._
    _To test this time lapse experiment in CellProfiler select `Test > Choose Image Group > Metadata_wellNum=0020` and `Test > Choose Image Set` to select each time point. Segmentation masks have been re-coloured to accurately track objects. Panels show a portion of the entire image._


14. The **MeasureObjectIntensity** module requires no changes. It is required for quality control in step 17.
15. The first **MeasureObjectSizeShape** module requires no changes. It is required for quality control in steps 16 and 17.
16. The **DisplayDataOnImage** module is required for object-level quality control. It is used to overlay data on top of each organoid. As an example, if the user wishes to exclude organoids with irregular outlines, the [FormFactor](http://cellprofiler-manual.s3.amazonaws.com/CellProfiler-3.0.0/modules/measurement.html) feature can be used. Selecting the following drop-down options
![DisplayDataOnImage](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/CellProfiler/GUI_DisplayDataOnImage.png)
will allow the user to inspect the FormFactor for each organoid in the test image and choose a threshold value which discriminates between dead or alive organoids.
17. The **FilterObjects** module allows for exclusion of individual organoids based on fluorescence intensity of morphological features. In the `Category` and `Measurement` boxes select the feature chosen in step 16. In `Minimum value` and `Maximum value` insert the range of allowed values. Organoids with values outside this range will be discarded. Selecting minimum and maximum values outside the range of measured values will not discard any organoids.
![GUI FilterObjects](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/CellProfiler/GUI_FilterObjects.png)
_In this example all organoids will be approved, since [FormFactor](http://cellprofiler-manual.s3.amazonaws.com/CellProfiler-3.0.0/modules/measurement.html)  is always comprised in the range [0 ~ 1]_

    Below is an example where  [FormFactor](http://cellprofiler-manual.s3.amazonaws.com/CellProfiler-3.0.0/modules/measurement.html) allows for a perfect discrimination of live (FormFactor ≥ 0.5) and dead (FormFactor = 0.22) organoids:
![FilterObjects](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/CellProfiler/FilterObjects_pannel_witharrow.png)
        _Organoids with FormFactor > 0.3 were approved thereby excluding irregular structures surrounded by cell clumps from the analysis (arrowhead). Segmentation masks show the identified objects from the segmentation step (`organoids_prelim`) and identified objects by applying the quality control criteria (`organoids`). Panels show a portion of the images from [well H4, #88](https://cirrus.ciencias.ulisboa.pt/owncloud/s/6xnRZsmTZcEBQQe) from the demonstration dataset. To test this image select `Test > Choose Image Group > Metadata_wellNum=0088`._
        **Note:** In the demonstration dataset all organoids were approved in the analysis
 
 
18. The **OverlayOutlines** module requires no changes. It provides an alternative way of visualizing the filtered objects. Outlines of approved organoids are displayed on the fluorescence microscopy image.
19. The **TrackObjects** module assigns a unique numeric label to the same organoid across all time-lapse frames. `Maximum pixel distance to consider matches` should be adjusted to the maximum number of pixels an organoid is expected to drift along two consecutive time-lapse frames. `Minimum lifetime` should be adjusted to be _n_ - 1, where _n_ is the number of time points in the time-lapse.
![TrackObjects GUI](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/CellProfiler/GUI_TrackObjects.png)
20. The second **MeasureObjectSizeShape** module requires no changes. It measures organoid area.
21. The **CalculateMath** module scales the pixel size to micron units and must always be checked. Fill the `Multiply the above operand by` field with the square of the pixel size (_e.g._ if the pixel size is 4.991 μm, the conversion factor is 24.910081).
![CalculateMath GUI](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/CellProfiler/GUI_CalculateMath.PNG)
22. The **SaveImages** module requires no changes. It saves the segmentation masks as PNG files.
23. The **ExportToSpreadSheet** module requires no changes. It exports image features in tabular text files.
    | **Note:** To ensure that the selected analysis settings are suitable for the entire dataset, several images should be tested. Segmentation (step 14) is frequently the step requiring the most adjustments. To test images from another well select `Test > Choose Image Group`. To test a specific time point image select `Test > Choose Image Set`. Test mode must be enabled. |
    |-----------------------|


24. Click the ![View output settings](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/CellProfiler/viewoutputsettings.png) button and in the `Default Output Folder` specify a folder where to store the analyzed data.
![OutputSettings GUI](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/CellProfiler/GUI_OutputSettings.png)
25. Before running the analysis activate `Window > Hide All Windows On Run`
26. Start the analysis of the whole dataset by clicking on the ![Analyze images](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/CellProfiler/analyzeimages.png) button.
27. Analysis of the demo dataset should take about 10 minutes in a computer with a ~2.5 GHz quad core processor.
28. 5.	The CellProfiler analysis will produce a file and folder structure resembling the raw data where images are segmentation masks and CSV files contain quantitative features and metadata The results folder can be identified by the `--cellprofiler` suffix.
![CP analysis results](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/CellProfiler/CP_ResultsFiles.png)


## 6.2. Fiji / ImageJ
[*Download ImageJ scripts*](https://cirrus.ciencias.ulisboa.pt/owncloud/s/AnQY4FZDpYXbRRk)
[*Download demonstration dataset*](https://cirrus.ciencias.ulisboa.pt/owncloud/s/GDWNSqp58GrELwg)
[*Download ImageJ script pre-configured for the demo dataset*](https://cirrus.ciencias.ulisboa.pt/owncloud/s/X3sebQqDR2iji9D)
[*Download ImageJ analysis of the demonstration dataset*](https://cirrus.ciencias.ulisboa.pt/owncloud/s/jZW3Y7kHMnT9Fy7)

The image analysis algorithm implemented in the ImageJ scripts perform the following steps:
1. Duplication of the raw image.
2. Conversion of the raw image into a 16 bit image.
3. Compute a pseudo-flat field image (_e.g._ median filter).
4. Subtract the pseudo-flat field from the raw 16 bit image.
5. Rescale image grayvalues to the [0 ~ 1] range.
6. Subtract the residual background offset.
7. Perform manual thresholding.
8. Removal of salt and pepper noise.
9. Apply per object quality control.
10. Export object features and segmentation masks

The ImageJ workflow comprises of two scripts: the first one is used to test single images and determine the analysis parameters for optimal segmentation. The second one applies these parameters to all images in a dataset and stores the quantification data in the desired location on the computer.

### 6.2.1. Test Mode
1. Open Fiji / ImageJ (scripts should be previously installed as described in section 1.4).
2. Open an image (`File > Open...`) to start optimizing the analysis settings. For this example use the [first time point from well B8 (#20)](https://cirrus.ciencias.ulisboa.pt/owncloud/s/ojZadGYQX5LQ3tM) in the demo dataset.
3. Start the test mode by selecting `FIS > FIS test...`
![FIS test...](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/Fiji/01_IJtest_menu.png)
4. The test mode window will open.
![FIS test window](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/Fiji/02_IJtest_dialog.png)
5. Adjust the analysis parameters for the selected image:
    **Background filter:** This filter generates the pseudo-flat field from the fluorescence image.
    **Radius of filter:** The pixel radius of the background filter in pixel units. The value is disregarded if `No filter (flat background)` is selected in the previous setting.
    **Offset after background correction:** This value will be subtracted from all pixels after subtracting the pseudo-flat field from the fluorescence image.
    **Manual threshold value:** This will be applied after pseudo-flat field subtraction, offset correction and grey value rescaling to [0 ~ 1]. All pixels above this grey value will be assigned to objects (organoids).
    **Fill all holes:**. When unchecked, the ‘conditional fill holes’ is applied. When checked, all holes will be filled after the thresholding step.
    **Remove salt and pepper noise:** When checked, isolated pixels in the thresholded image will be removed.
    **Font size for organoid labels:** Each segmented organoid will be overlaid with a unique label having this font size.
    **Exclude objects touching the image border:** When checked, all objects which touch the image border will be discarded from the analysis.
    **Minimum organoid area:** Minimum allowed size of organoids (in μm² units). Smaller organoids will be discarded from the analysis.
    **Maximum organoid area:** Maximum allowed size of organoids (in μm² units). Larger organoids will be discarded from the analysis.
    **Minimum organoid circularity:** Minimum allowed circularity of organoids. Organoids with lower circularity will be discarded from the analysis. **Note:** 0 ≤ circularity ≤ 1
    **Maximum organoid circularity:** Maximum allowed circularity of organoids. Organoids with higher circularity will be discarded from the analysis. **Note:** 0 ≤ circularity ≤ 1
    **Exclude organoids based on measurement:** An arbitrary measurement can be selected here for additional object-level quality control purposes.
    **Minimum allowed value:**  Minimum allowed value for the additional quality control measurement. Organoids with smaller values will be discarded from the analysis.
    **Maximum allowed value:** Maximum allowed value for the additional quality control measurement. Organoids with larger values will be discarded from the analysis.
    **Pixel width/height:** Pixel size in the raw microscopy image. This is used to set the image scale throughout the test and analysis processes.
6. Click the `OK` button to test the analysis settings in the open image.
7. ImageJ will apply the test settings (as described in 6.2) and will display the results of  each analysis step with images being numbered consecutively according to the sequence of the applied image analysis operations:
![All test windows](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/Fiji/test_allwindows.png)
_These are the results for [this](https://cirrus.ciencias.ulisboa.pt/owncloud/s/ojZadGYQX5LQ3tM) test image._

    Images are numbered according to the sequence of image analysis operations:
    
    7.1. **Raw image:** A copy of the test image selected in step 2.
    ![IJ test raw image](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/Fiji/test_step_by_step_0_Raw.png)
    _The lower plot is the [grayvalue profile](https://imagej.nih.gov/ij/docs/menus/analyze.html#plot) across the red dashed line. Notice the non-homogenous background: higher intensities in the center of the image and lower intensities at the edges._
    
    7.2. **16 bit conversion:** Conversion of the test image into a 16 bit grayscale image. This ensures that single and multichannel images will be appropriately processed.
    ![IJ test 16bit](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/Fiji/test_step_by_step_1_16bit.png)
    _The lower plot is the [grayvalue profile](https://imagej.nih.gov/ij/docs/menus/analyze.html#plot) across the red dashed line._

    7.3. **Pseudo flat field:** A “background-only” image produced by applying the background filter to the 16 bit image. This image is not generated if `No filter (flat background)` is selected.
    ![IJ test background](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/Fiji/test_step_by_step_2_background.png)
    _The lower plot is the grey value profile across the red dashed line. The pseudo-flat field image mirrors the background values of the raw image: higher values in the center of the image and lower values at the edges. The discreteness in the profile plot (grayvalues ∈ {0, 1, 2, 3} arises because of modest background inhomogeneity._
    
    7.4. **Background correction (with offset)**: The result of subtracting the pseudo-flat field of the 16 bit image.
    ![IJ test flat background](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/Fiji/test_step_by_step_3_bgCorr_withOffset.png)
    _The lower plot is the [grayvalue profile](https://imagej.nih.gov/ij/docs/menus/analyze.html#plot) across the red dashed line. Notice how the pseudo-flat field correction produced a homogenous background throughout the image._
    
    7.5. **Background correction (with offset, rescaled)**: The image from step 7.4 with grey values rescaled to the [0 ~ 1] range.
    ![IJ test rescaled](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/Fiji/test_step_by_step_4_bgCorr_withOffset_rescaled_withlabel.png)
    _The lower plot is the [grayvalue profile](https://imagej.nih.gov/ij/docs/menus/analyze.html#plot) across the red dashed line. Notice how the background possesses a slight offset: many background grey values fluctuate around 0.005 (red dashed line in the profile plot)._
    
    7.6. **Background corrected:** The subtraction of the background offset to the previous image in step 7.5.
    ![IJ test background corrected](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/Fiji/test_step_by_step_5_bgCorrected_withlabel.png)
    _The lower plot is the [grayvalue profile](https://imagej.nih.gov/ij/docs/menus/analyze.html#plot) across the red dashed line. Manual thresholding will be applied to this image using the value specified in the test mode dialog box. Notice how a threshold value of e.g. 0.05 (red dashed line in the profile plot) allows an excellent discrimination between background and object pixels._

    7.7. **Thresholded image:** Manual thresholding of the previous image.
    ![IJ test thresholded](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/Fiji/test_step_by_step_6_thresholded.png)
    _Image thresholding produced some isolated "object"/bright pixels (similar to salt and pepper noise). These can be removed if requested._

    7.8. **Salt and pepper noise removed:** Isolated pixels were removed from the previous image, to facilitate visual inspection.
    ![IJ test without salt&pepper](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/Fiji/test_step_by_step_7_withoutSaltPepper.png)
    _All organoids have been segmented but many small structures which should not be included in the analysis are also visible. For individual object analysis, intervals for the size, circularity and other object-level features can be specified here. In this demo analysis, only organoids larger than 500 μm² will be accepted, regardless of all other applied features._
    
    7.9. **Final:** Segmentation masks of accepted organoids.
    ![IJ test final](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/Fiji/test_step_by_step_8_Final.png)
    _Objects touching the image border were accepted. Each object is overlaid with a cyan label. Labels are shown in all images at the same location, to inform the segmentation process. Labels can be hidden by selecting `Image > Overlay > Hide Overlay`._

8. 8.	At this point, all images can be inspected with any of the tools in Fiji / ImageJ. We recommend inspecting pixel values and [profile plots](https://imagej.nih.gov/ij/docs/menus/analyze.html#plot) to judge the quality of background subtraction, segmentation and object/level quality control. All measurements, which can be used for object-level quality control, are displayed in the Results window. Each line corresponds to the cyan label overlaid on each organoid:
    ![IJ test results](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/Fiji/test_step_by_step_results.png)
    The table contains standard [ImageJ measurements](https://imagej.nih.gov/ij/docs/guide/146-30.html#toc-Subsection-30.7), as well as CellProfiler's [FormFactor](http://cellprofiler-manual.s3.amazonaws.com/CellProfiler-3.0.0/modules/measurement.html).
    The log window will show the applied settings:
    ![IJ test log](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/Fiji/test_step_by_step_log.png)
    
9. Click the `OK` button in the box below to return to the test mode tool:
    ![IJ test final](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/Fiji/test_step_by_step_actionrequired.png)
10. The test tool will allow changing the analysis settings. Make adjustments as many times as required to obtain an adequate segmentation.
11. Save the settings in the log window for the batch image segmentation process (see 6.2.2).

    | **Note:** To ensure that the selected analysis settings are suitable for the entire dataset, several images should be tested. Make all required adjustments until a satisfactory analysis is consistently achieved. |
    |-----------------------|
    

### 6.2.2. Batch Analysis Mode

1. Open Fiji / ImageJ.
2. Start the batch analysis mode by selecting `FIS > FIS analysis...`
![FIS analysis...](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/Fiji/window_win10_FISanalysis.png)
3. The batch analysis window will open.
![FIS analysis window](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/Fiji/02_IJ_analysis_dialog.png)
4. All settings are the same as in test mode, except for a new window:
    **Regular expression matching all files being analyzed:** the default is `.*--C00(?:.ome)??.tif$`, which matches the images generated by the renaming tools. When the fluorescence image is not the first channel, replace `C00` with the correct channel name.
5. Input the optimized settings as determined in the test mode. 
    The demo dataset was analyzed using the following settings:

    | **Parameter**                                | **Value**              |
    |----------------------------------------------|------------------------|
    | _Background filter_                          | Median                 |
    | _Radius of filter_                           | 50                     |
    | _Offset after background correction_         | 0.005                  |
    | _Manual threshold value_                     | 0.05                   |
    | _Fill all holes?_                            | No                     |
    | _Remove salt and pepper noise?_              | Yes                    |
    | _Exclude objects touching the image border?_ | No                     |
    | _Minimum organoid size_                      | 500 μm²                |
    | _Maximum organoid size_                      | 99999999 μm²           |
    | _Minimum organoid circularity_               | 0                      |
    | _Maximum organoid circularity_               | 1                      |
    | _Exclude organoids based on measurements?_   | None - Do not exclude  |
    | _Minimum allowed value_                      | _Irrelevant_           |
    | _Maximum allowed value_                      | _Irrelevant_           |
    | _Pixel width/height_                         | 4.991 μm               |
    | _Regular expression_                         | .*--C00(?:.ome)??.tif$ |

6. Click `OK` to start the analysis.
7. Select the folder with the renamed microscopy images.
8. Select the folder where the result files should be saved to.
9. ImageJ will analyze the entire dataset. Analysis of the demo dataset should take about 20 minutes on a computer with a ~2.5 GHz quad core processor.
10. 10.	The ImageJ analysis will produce a file and folder structure resembling that of the raw data where images are saved as segmentation masks, together with CSV files that contain quantitative features (_e.g._ organoid area) and metadata. Each well will contain one CSV file that contains the information from that well. The results folder can be identified by the `--ij` suffix.
![IS analysis results](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/Fiji/IJ_ResultsFiles.png)



# 7. Statistical analysis
![Organoid Analyst logo](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/OrganoidAnalyst/OA_logo_296x100.png)
[*Download Organoid Analyst*](https://cirrus.ciencias.ulisboa.pt/owncloud/s/r9joKzmFcLLio9J)
[*Download CellProfiler analysis files*](https://cirrus.ciencias.ulisboa.pt/owncloud/s/4QgLGw9oZpm3Aae)
[*Download ImageJ analysis files*](https://cirrus.ciencias.ulisboa.pt/owncloud/s/jZW3Y7kHMnT9Fy7)
[*Download normalized CellProfiler data*](https://cirrus.ciencias.ulisboa.pt/owncloud/s/Y3rtii6jtnA45Kk)
[*Download normalized ImageJ data*](https://cirrus.ciencias.ulisboa.pt/owncloud/s/s46NNJj4xRHz7Gc)

We developed a tool for statistical analysis of the FIS data called Organoid Analyst. It allows for an integrated analysis of the FIS data generated by either the CellProfiler or ImageJ analysis pipelines by implementing:
- Interactive data inspection
- Image visualization (using Fiji / ImageJ) 
- Per-object and per-well quality control
- Data normalization
- Generation of publication-ready quantitative FIS data


The normalization algorithm within Organoid Analyst implements the following steps:
- Computation of the total organoid area in each image (_i.e._ the sum of the area of all organoids in an image)
- Computation of kinetic curves for each well: total organoid area _vs_ time
- Normalization of kinetic curves to the area in the first time point (A₀ = 100%)
- Computation of the area under the curve of the normalized kinetic curves (baseline = 100%)
- Exporting of the following measurements: 
    - **AUC (area under the curve):** calculated between the first and the last time points of the experiment (usually: 0 and 60 min).
    - **ISR (initial swelling rate):** the slope of a line fitted to the region of maximal linear swelling as measured in the normalized kinetic curves.
    - **Aₜ/A₀:** normalized area value at the final time point of the FIS assay (Aₜ), using normalization factor A₀ = 100%.
        ![Organoid Analyst measurements](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/OrganoidAnalyst/3measurements_400x347.png)
        _Graphical representation of the three FIS outputs generated by Organoid Analyst: AUC, ISR and Aₜ/A₀. Notice the  ~10 min lag between CFTR stimulation at the beginning of the assay (t = 0 min) and the linear swelling region where ISR is measured (t = 10 ~30 min)._


Below are step-by-step instructions on how to perform the statistical analysis of the FIS data generated with the demonstration dataset using Organoid Analyst. Additional information about most features in Organoid Analyst can be obtained in the online help accessible by clicking the ![!](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/OrganoidAnalyst/helpbutton.png) button.

1. Download Organoid Analyst.
2. Unzip the file (_e.g._ to the desktop).
3. Start R, as described in 1.4.
4. Run the tool by typing the following commands (adapt the folder in the first line to the location of the unzipped file):

    **Windows:**
    ```
    setwd("c:/users/yourname/desktop/organoid_analyst")
    source("run.r")
    ```
    
    **MacOS:**
    ```
    setwd("/Users/yourname/Desktop/organoid_analyst")
    source("run.r")
    ```

5. Organoid Analyst will open in a new browser window.
![Organoid Analyst](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/OrganoidAnalyst/01_OA_loadempty-2.png)
6. Click on `1. Load data`.
7. Click `Choose a '--cellprofiler' or '--ij' folder...` and select the folder `demoplate_01--cellprofiler` or `demoplate_01--ij`.
8. Organoid Analyst will concatenate the `objects.csv` files generated by CellProfiler or Fiji / ImageJ to assemble a data table containing all data of the experiment. The raw data table can be inspected by clicking `View data > Raw data table (concatenated)`.
    ![View raw data](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/OrganoidAnalyst/CP--02_OA_viewraw_menu.png)

    Each row in the table represents a single organoid in a single image. Each column represents one feature (measurement or metadata).
    ![raw data table](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/OrganoidAnalyst/CP--03_OA_viewraw_table.png)

9. Click on `2. Settings`.
10. This section is used to inform Organoid Analyst of which columns in the data table contain relevant data (_e.g._ organoid area), as well as of the type of multi well plate used in the experiment, quality control information and file locations:
**Experiment Settings:** names of columns containing the experiment metadata and the time resolution of the experiment.
**Quality Control Settings:** Coordinates of organoid centroids and labels of organoids which should not be considered in the analysis. In the CellProfiler analysis these organoids are assigned the label `NaN`. This quality control feature can be used to exclude organoids with this label.
**File Remapping Settings:** When image analysis and data analysis are not being performed on the same computer path remapping is required to open images with Organoid Analyst. Here, the location of the folder containing the analyzed TIF images by CellProfiler or Fiji / ImageJ (`demoplate_01`, for the demo dataset) can be specified for _(i)_ the computer that performed the image analysis and _(ii)_ the computer running Organoid Analyst.
**Segmentation Masks Settings:** Determines whether segmentation masks should be updated according to the quality control parameter defined above.
**Interaction with Fiji Settings:** The location of the Fiji / ImageJ executable file of the computer running Organoid Analyst. Only required for Windows and not for MacOS.
Below are the settings which were used to analyze the demo dataset:

    | **Parameter**                                  | **Value**                                                                           |
    |------------------------------------------------|-------------------------------------------------------------------------------------|
    | **Experiment Settings**                        |                                                                                     |
    | _Time resolution (minutes per timepoint)_      | 10                                                                                  |
    | _Name of the column with AREA values_          | Math_area_micronsq                                                                  |
    | _Name of the column with TIME values_          | Metadata_timeNum                                                                    |
    | _Name of the column with WELL values_          | Metadata_wellNum                                                                    |
    | _Name of the column with COMPOUND names_       | Metadata_compound                                                                   |
    | _Name of the column with CONCENTRATION values_ | Metadata_concentration                                                              |
    | _Number of rows_                               | 8                                                                                   |
    | _Number of columns_                            | 12                                                                                  |
    | **Quality Control Settings**                   |                                                                                     |
    | Name of the column with organoid ID **¹**      | TrackObjects_Label_4                                                                |
    | ID of invalid organoids                        | Allow all organoids                                                                 |
    | Name of the column with organoid center (X)    | AreaShape_Center_X                                                                  |
    | Name of the column with organoid center (Y)    | AreaShape_Center_Y                                                                  |
    | **File Remapping Settings**                    |                                                                                     |
    | Column with file path                          | Metadata_FileLocation                                                               |
    | Image root folder name in table                | file:///X:/organoid_HCS/workflow/demo_dataset/03-images_renamed/demoplate_01        |
    | Image root folder name in this computer **²**  | X:/organoid_HCS/workflow/demo_dataset/03-images_renamed/demoplate_01                |
    | **Segmentation Masks Settings**                |                                                                                     |
    | Generate segmentation masks?                   | Yes                                                                                 |
    | Image root folder name in table                | file:///X:/organoid_HCS/workflow/demo_dataset/03-images_renamed/demoplate_01        |
    | Image root folder name in this computer **³**  | X:/organoid_HCS/workflow/demo_dataset/05-images_analysis/demoplate_01--cellprofiler |
    | Length of image suffix **⁴**                   | 9                                                                                   |
    | Suffix for segmentation mask files             | --masks.png                                                                         |
    | Suffix for Organoid Analyst masks file         | --OAmask                                                                            |
    | Suffix for Organoid Analyst labels file        | --OAlabel                                                                           |
    | **Interaction with Fiji Settings**             |                                                                                     |
    | Path to Fiji (Windows) **⁵**                   | C:/Fiji.app/ImageJ-win64.exe                                                        |
    _**¹** This setting is for data analyzed with CellProfiler. For data analysed by ImageJ enter `TrackObjects_Label`_.
    _**²** Select the folder containing the renamed microscopy images on your computer._
    _**³** Select the location of the `demoplate_01--cellprofiler` or `demoplate_01--ij` folder in your computer._
    _**⁴** This setting is for data analyzed with CellProfiler. For data analysed by ImageJ enter `4`._
    _**⁵** Select the location of the Fiji executable file on your computer. Not required when running macOS._

11. Click on `Normalize data`.
12. Organoid Analyst will normalize the data and update the segmentation masks. This may take a few minutes. After normalization is complete, the corresponding data table can be inspected by clicking `View data > Normalized data`.
    ![View normalized data](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/OrganoidAnalyst/CP--05_OA_viewnorm_menu.png)

    ![normalized data table](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/OrganoidAnalyst/CP--06_OA_viewnorm_table.png)
13. Click on  `3. Plotting`.
14. This section allows for interactive data exploration and per well quality control.
    ![plotting section](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/OrganoidAnalyst/CP--07_OA_Plot_top.png)

    The following features are available:
        **Analysis settings:** Defines the final time point for the experiment and the time points to be used for the calculation of the initial swelling speed.
        **Quality control:** Allows for excluding individual wells from the analysis (_e.g._ wells with imaging aberrations or insufficient organoids). Selected wells will be hidden from all generated plots and summary statistics.
        **Timelapse viewer:** Allows for opening and inspection of an arbitrary number of wells as time lapse sequences in Fiji. Start by selecting the wells of interest and clicking the ![Open movies in Fiji](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/OrganoidAnalyst/openmovies.png) button. Fiji will open in a new window as shown below. Raw fluorescence images will be overlaid with segmentation masks and organoid labels from the analyses.
        ![Organoid Analyst Fiji](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/OrganoidAnalyst/CP--08_OA_IJ.png)
        **Plots:** Organoid Analyst visualizes the quantitative FIS data in five different plots:
            * Multi-well plate layout with the normalized kinetic curves being displayed for each well. The plot can also show the ISR.
            * Dose-response plot for AUC measurements. **Important! For this plot to be appropriately generated, the infile must contain strictly numerical values for the compound concentration field**.
            * Bar plots representing summarized AUC, ISR and Aₜ/A₀ measurements (average ± standard deviatio, across identically treated wells).

    The demonstration dataset was analyzed using the following settings:

    | **Parameter**                    | **Value** |
    |----------------------------------|-----------|
    | Select initial data points       | 10 ~ 30   |
    | Select final experiment time     | 60        |
    | Wells excluded from calculations | None      |
    
15. Click the `Export data` button to save the analyzed dataset to the output folder.
16. Organoid Analyst will save the following files:
    - **Updated segmentation masks**
    - **Updated organoid labels**
    - **FIS_normalized.xlsx** Data for individual wells: sum of all organoid areas, normalized areas, normalized areas subtracted of the 100% baseline, and cumulative AUC.
    - **FIS_rawdata.csv** Concatenation of the objects.csv files into a single data table.
    - **FIS_summary_xxmin.xlsx** Per-treatment summary of AUC, ISR and Aₜ/A₀ measurements.
    - **FISanalysis_dd-mm-yy_hh-ss.log** Organoid Analyst settings at the moment of data export.
    - **plot_AtA0_xxmin.png** Bar plot of summarized Aₜ/A₀ measurements.
    - **plot_AUC_xxmin.png** Bar plot of summarized AUC measurements at the final time point of the experiment.
    - **plot_initialslope_xxmin.png** Bar plot of summarized ISR measurements.
    - **plot_overview.png** Plate layout with normalized kinetic curves.
    - **plot_titration_AUC_xxmin.png** Dose-response plot for AUC measurements at the final time point of the experiment.

In the table below we have summarized the analysed AUC data from the demonstration dataset obtained from CellProfiler and ImageJ. The observed differences between the two sets of results are mostly due to the hole filling strategy performed during the thresholding step: standard fill holes (CellProflier) _vs_ conditional fill holes (ImageJ).


**CellProfiler analysis**

| **Compounds**         | **[Fsk] (μM)** | **AUC (mean)** | **AUC (sd)** |
|-----------------------|----------------|----------------|--------------|
| Fsk                   | 0.008          |          16.64 |        29.54 |
| Fsk                   | 0.02           |          -2.07 |        36.18 |
| Fsk                   | 0.05           |          14.76 |         6.87 |
| Fsk                   | 0.128          |         -64.81 |         2.29 |
| Fsk                   | 0.32           |          29.95 |         1.13 |
| Fsk                   | 0.8            |          -1.88 |        14.57 |
| Fsk                   | 2              |           43.7 |         12.3 |
| Fsk                   | 5              |          47.43 |        44.05 |
| Fsk + VX-770          | 0.008          |          51.47 |         9.07 |
| Fsk + VX-770          | 0.02           |          34.49 |         7.05 |
| Fsk + VX-770          | 0.05           |           5.35 |        24.62 |
| Fsk + VX-770          | 0.128          |            4.7 |        40.07 |
| Fsk + VX-770          | 0.32           |          80.22 |        14.19 |
| Fsk + VX-770          | 0.8            |         359.88 |         12.5 |
| Fsk + VX-770          | 2              |         449.02 |        11.39 |
| Fsk + VX-770          | 5              |          513.1 |         9.65 |
| Fsk + VX-809          | 0.008          |          25.54 |        18.77 |
| Fsk + VX-809          | 0.02           |          72.81 |         8.57 |
| Fsk + VX-809          | 0.05           |         -13.86 |         11.9 |
| Fsk + VX-809          | 0.128          |         -25.36 |        27.24 |
| Fsk + VX-809          | 0.32           |          14.76 |        13.59 |
| Fsk + VX-809          | 0.8            |         171.86 |         5.63 |
| Fsk + VX-809          | 2              |         734.43 |       114.55 |
| Fsk + VX-809          | 5              |        1361.16 |        74.95 |
| Fsk + VX-770 + VX-809 | 0.008          |          30.24 |          4.7 |
| Fsk + VX-770 + VX-809 | 0.02           |           52.6 |        19.01 |
| Fsk + VX-770 + VX-809 | 0.05           |         156.59 |        73.21 |
| Fsk + VX-770 + VX-809 | 0.128          |         477.28 |       238.58 |
| Fsk + VX-770 + VX-809 | 0.32           |        1329.92 |       155.75 |
| Fsk + VX-770 + VX-809 | 0.8            |        2522.41 |        63.19 |
| Fsk + VX-770 + VX-809 | 2              |        2620.19 |       289.62 |
| Fsk + VX-770 + VX-809 | 5              |        2998.54 |       149.31 |

![AUC titration - CellProfiler](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/final_data/CP_lowres.png)



**Fiji analysis**

| **Compounds**         | **[Fsk] (μM)** | **AUC (mean)** | **AUC (sd)** |
|-----------------------|----------------|----------------|--------------|
| Fsk                   | 0.02           |          22.95 |        38.08 |
| Fsk                   | 0.05           |         -38.11 |        14.25 |
| Fsk                   | 0.128          |         -84.04 |         0.34 |
| Fsk                   | 0.32           |         -24.70 |        15.25 |
| Fsk                   | 0.8            |         -48.61 |        19.19 |
| Fsk                   | 2              |         -10.84 |        20.23 |
| Fsk                   | 5              |          33.56 |        55.63 |
| Fsk + VX-770          | 0.008          |         -44.72 |         6.45 |
| Fsk + VX-770          | 0.02           |         -25.54 |        42.62 |
| Fsk + VX-770          | 0.05           |         -49.76 |        30.29 |
| Fsk + VX-770          | 0.128          |         -94.97 |        55.25 |
| Fsk + VX-770          | 0.32           |          40.29 |         9.84 |
| Fsk + VX-770          | 0.8            |         196.14 |        43.69 |
| Fsk + VX-770          | 2              |         254.76 |        67.97 |
| Fsk + VX-770          | 5              |         390.52 |         1.80 |
| Fsk + VX-809          | 0.008          |          -0.42 |        47.88 |
| Fsk + VX-809          | 0.02           |          32.71 |         1.53 |
| Fsk + VX-809          | 0.05           |         -21.92 |        15.45 |
| Fsk + VX-809          | 0.128          |         -47.16 |        13.48 |
| Fsk + VX-809          | 0.32           |         -23.64 |         3.66 |
| Fsk + VX-809          | 0.8            |          97.87 |        31.26 |
| Fsk + VX-809          | 2              |         491.27 |        79.05 |
| Fsk + VX-809          | 5              |         997.04 |        84.73 |
| Fsk + VX-770 + VX-809 | 0.008          |          -4.00 |        31.17 |
| Fsk + VX-770 + VX-809 | 0.02           |         -74.63 |        32.17 |
| Fsk + VX-770 + VX-809 | 0.05           |          89.73 |        61.40 |
| Fsk + VX-770 + VX-809 | 0.128          |         379.10 |       231.87 |
| Fsk + VX-770 + VX-809 | 0.32           |        1122.95 |        77.37 |
| Fsk + VX-770 + VX-809 | 0.8            |        1963.72 |        60.83 |
| Fsk + VX-770 + VX-809 | 2              |        2212.82 |       323.37 |
| Fsk + VX-770 + VX-809 | 5              |        2645.44 |       139.45 |

![AUC titration - CellProfiler](http://webpages.fc.ul.pt/~hmbotelho/FIS_analysis_pipeline/final_data/IJ_lowres.png)


# 8. Interpretation of the results
These results suggest that the organoid donor is a cystic fibrosis patient, most likely expressing class II CFTR mutations.
**Cystic fibrosis** is suggested by the fact that CFTR activity (_i.e._ organoid swelling) is not detected in control experiments (DMSO). Lack of CFTR activity is the primary biological defect underlying cystic fibrosis.
**Class II CFTR mutations** produce misfolded CFTR molecules which are retained inside the cell and do not reach their expected localization at the plasma membrane. The data are indicative of such mutations, as CFTR activity was recovered by the combined treatment with a CFTR folding corrector (VX-809) and a potentiator of CFTR's function (VX-770). The fact that treatment with the folding corrector alone is not effective at rescuing CFTR activity - except at high concentrations - indicates that this mutant CFTR has two molecular defects: misfolding and defficient activation.
This behavior is typical of organoids homozygous for the most common CFTR mutation: F508del.

References:
- Dekkers _et al_ (2013) **A functional CFTR assay using primary cystic fibrosis intestinal organoids** _Nat Med_ 19(7):939-45. DOI: [10.1038/nm.3201](https://doi.org/10.1038/nm.3201)
- Dekkers _et al_ (2016) **Characterizing responses to CFTR-modulating drugs using rectal organoids derived from subjects with cystic fibrosis** _Sci Transl Med_ 8(344):344ra84. DOI: [10.1126/scitranslmed.aad8278](https://doi.org/10.1126/scitranslmed.aad8278)
