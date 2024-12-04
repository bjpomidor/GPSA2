===== GPSA2 v0.1.0 README =====

1. Introduction
2. Installation
3. Configuration
4. Usage
5. Troubleshooting

-----

1. Introduction

Thank you for downloading GPSA2, a surface shape analysis software package. Here you will find instructions for configuring and running the application, which implements the geometric morphometrics methods detailed in Pomidor & Dean 2024 (https://doi.org/10.1101/2024.08.03.604701). GPSA2 is the successor to our previous software, GPSA, based on Pomidor et al. 2016 (https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0150368). GPSA2 has been built from the ground up to efficiently provide improved analysis compared to the original GPSA. However, this software is new and still under development. If you experience bugs or difficulty configuring your analysis settings, please check the Troubleshooting section or contact us directly.

-----

2. Installation

1) Download & install the 64-bit version of Java 7 or later (found at https://www.java.com/en/download/manual.jsp).
2) Download the GPSA2-0.1.0.jar Java application file from https://github.com/bjpomidor/GPSA2/
3) Optional: Download the sample data & configuration files from https://github.com/bjpomidor/GPSA2/

-----

3. Configuration

The configuration files (ending in .cfg) contain several parameters necessary for GPSA2 to run correctly. We strongly recommend modifying an existing config file from the sample data for simplicity. Some of these parameters may need to be modified to obtain optimal results for your data set. Note that some parameters are exposed in this file for development purposes and may be removed or renamed in later updates. The parameters are as follows, with suggested ranges and recommended starting values that may need to be adjusted where applicable:

	- threadCount (integer > 0): Determines the number of threads to use for parallel processing (recommended value = 4 to 16).
	- shapeDescriptors (true/false): Determines whether basic point-wise local shape descriptors (mean curvature, Gaussian curvature, curvature index) will be used for determining homology (recommended value = true).
	- shapeDescriptorWeight(0.0 to 0.5): Assigns a weighting value to the local shape descriptors (recommended  value = 0.01).
	- landmarkDescriptors (true/false): Determines whether point-wise, landmark-based shape descriptors will be used for determining homology.  Requires an .nts landmark file to be provided in the landmarkFilename parameter (only set to true if you will be using additional landmark data). 
	- landmarkDescriptorWeight (0.0 to 0.5): Assigns a weighting value to the landmark-based shape descriptors (recommended value = 0.01)
	- landmarkCount (integer > 0): The number of landmarks per individual (e.g., for N individuals with M landmarks, this value should be set to M).
	- resistantFit (true/false): Determines whether resistant-fitting of surfaces (similar to Slice 1996 for landmarks) is applied (recommended value = false, unless the sample exhibits strong variation or partially missing features).
	- prototypeSmoothing (true/false): Determines whether Taubin smoothing is applied to the reference surface after each full superimposition loop (recommended value = true to avoid instability)
	- prototypeSmoothingKpb (0.0 to 0.5): Control parameter for reference object Taubin smoothing, determines resolution of detail retained (recommended starting value = 0.01).
	- prototypeSmoothingN (integer > 0): The number of Taubin smoothing iterations applied to the reference object (recommended starting value = 50).
	- homologizationSmoothing (true/false): Determines whether Taubin smoothing is applied to the homologized surface output after analysis is complete (recommended value = true)
	- homologizationSmoothingKpb (0.0 to 0.5): Control parameter for reference object Taubin smoothing, determines resolution of detail retained (recommended starting value = 0.1).
	- homologizationSmoothingN (integer > 0): The number of Taubin smoothing iterations applied to the homologized surface output after analysis is complete (recommended starting value = 25)
	- terminationCondition (0.0 to 1.0): Threshold value that determines when the superimposition is complete, based on the difference between reference objects between subsequent iterations (recommended value = 0.005).
	- initialization (integer 0 to 8): Determines initialization scheme used for preliminary positioning of the scans relative to one another (recommended starting value = 4).
			0 - none 
			1 - translation to overlap centroids at the origin 
			2 - translation to overlap centroids at the origin, with scaling applied for uniform size
			3 - translation to overlap centroids at the origin and rotation to align major axes with the X axis
			4 - translation to overlap centroids at the origin and rotation to align major axes with the X axis, with scaling applied for uniform size
			5 - translation to overlap centroids at the origin and rotation along major axes to align a single landmark
			6 - translation to overlap centroids at the origin and rotation along major axes to align a single landmark, with scaling applied for uniform size
			7 - translation and rotation terms taken from Procrustes superimposition of landmarks applied to surface objects
			8 - translation and rotation terms taken from Procrustes superimposition of landmarks applied to surface objects, with scaling applied for uniform size
	- projectionCutoff (0.0 to 1.0): Ordination axis variance proportion cutoff limit for generating projections; no projections will be generated for ordination axes below this value (recommended value = 0.01).
	- projectionWeightA (float > 0): Weighting term for first set of ordination projections (recommended value = 1.0).
	- projectionWeightB (float > 0): Weighting term for second set of ordination projections (recommended value = 5.0).
	- areaWeightColorMaps (true/false): Applies an alternative area-weighting scheme to heat map generation for better visualization results in some cases (recommended value = false).
	- colorPalette (viridis/cividis/inferno): Determines color palette used for heat maps (recommended value = viridis).
			viridis - yellow/green/blue/purple
			cividis - color blind friendly blue/yellow 
			inferno - yellow/orange/red/purple/black
	- storeExtraSuperimpositionOutput (true/false): Whether to save extra output about the superimposition process, useful for diagnosing issues (recommended value = false).
	- storeSuperImposedSurfaces (true/false): Whether to save the final set of superimposed sample surfaces (recommended value = true).
	- storeHomologizedSurfaces (true/false): Whether to save the final set of homologized sample surfaces (recommended value = true).
	- storeMeanSurface (true/false): Whether to save the final averaged reference surface (recommended value = true).
	- storeVarianceHeatmap (true/false): Whether to save a heat map of the shape variance visualized on the reference surface (recommended value = true).
	- heatMapLowerLimit (integer between 1 and 99): Lower percentile limit for converting point values to heat map color bins (recommended value = 1).
	- heatMapUpperLimit (integer between 1 and 99): Upper percentile limit for converting point values to heat map color bins (recommended value = 99).
	- storeProjectionsA (true/false): Whether to generate and store surfaces projected at +/- projectionWeightA for each ordination axis (recommended value = true).
	- storeProjectionsB (true/false): Whether to generate and store surfaces projected at +/- projectionWeightB for each ordination axis (recommended value = true).
	- storeOrdinationData (true/false): Whether to generate and store ordination values & scores (recommended value = true).
	- storeSuperimposedSurfacesDistanceMatrix (true/false): Whether to generate and store the shape distance matrix between the superimposed original surfaces in the sample (recommended value = true).
	- storeHomologizedSurfacesDistanceMatrix (true/false): Whether to generate and store the shape distance matrix between the homologized surfaces in the sample (recommended value = true).
	- storeSuperimposedToMeanDistances (true/false): Whether to generate and store the shape distances between the superimposed original surfaces and the averaged reference surface (recommended value = false).
	- storeHomologizedToMeanDistances (true/false): Whether to generate and store the shape distances between the homologized surfaces and the averaged reference surface (recommended value = false).
	- storeSuperimposedToHomologizedDistances (true/false): Whether to generate and store the shape distances between the superimposed original surfaces and their homologized counterparts (recommended value = false).
	- storeMeanCentroidSizeValues (true/false): Whether to store the shape mean centroid size estimator value for each surface (recommended value = true).
	- storeDataLabels (true/false): Whether to store the shape mean centroid size estimator value for each surface (recommended value = true).
	- storeHomologizedPointsArray (true/false): Whether to generate and store a NxM shape variable matrix containing all of the homologized surfaces (recommended value = true).
	- storeMeanLandmarksArray (true/false): Whether to store the shape mean centroid size estimator value for each surface (recommended value = true).
	- storeSuperimposedSampleLandmarksArray (true/false): Whether to store the landmark configuration after the sample has been superimposed (recommended value = false).
	- dataInputDirectory (file path): Location of the input data directory (e.g., /home/USERNAME/data/input/ or .\sample_data\GPSA2_TEST_DATA-CEBUS_20K\ and so on, depending on OS).
	- dataOutputDirectory (file path): Location where the output should be stored (e.g., /home/USERNAME/data/output/ or .\output\ and so on, depending on OS).
	- prototypeFilename (ASCII PLY filename): Filename of the reference surfaces for the data set (e.g., reference_surface.ply).
	- sampleFilenames (ASCII PLY filenames array): Array of filenames for the surfaces in the data set in brackets, separated by commas (e.g., {sample1.ply, sample2.ply, sample3.ply, ..., sampleN.ply}).
	- landmarkFilename (NTS filename): NTS file containing landmark data for the sample (e.g., sample_landmarks.nts).
	- prototypeLandmarkIndex (integer > 0): The index of the reference object in the NTS file (e.g., 3).
	- landmarkFileIndexArray (integer array): Array of integers that act as pointers from the sequentially-read NTS file to their position in the PLY filename array; useful for re-indexing data {e.g., {0, 1, 2, 4, 5, 3}).

-----

4. Usage

After downloading the software and configuring your analysis settings, simply navigate in your command line to the appropriate directory and run the software using the following Java command, replacing "YOUR_CONFIG.cfg" with the name of your configuration file:

	java -Xmx4g -jar GPSA2-0.1.0.jar YOUR_CONFIG.cfg
	
The "-Xmx4g" parameter controls the amount of memory available to the program. we suggest starting at 4 gigabytes, but you may find it necessary to increase this value (e.g., "-Xmx16g" for 16 gigabytes).
 
For example, to run the the CEBUS test data set on Windows, launch the Command Prompt (cmd.exe), navigate to the GPSA2 folder, and type:

	java -Xmx4g -jar GPSA2-0.1.0.jar sample_data\GPSA2_TEST_DATA-CEBUS_20K.cfg
	
Then press the "Enter" key.

-----

5. Troubleshooting

If the  software fails to run or the analysis provides nonsensical results, please check the following potential issues and solutions:
	
	- Unable to read PLY files: All PLY files must be formatted as ASCII with appropriate headers.  Importing and exporting your PLY files in MeshLab as ASCII will likely resolve any formatting issues.
	- Java out of memory: This software strives to be memory efficient, but depending on the size of the data set and number of parameters, you may need to increase the maximum heap size using the -Xmx option described in the Usage section.
	- Surfaces are jagged/surfaces are too smooth/surfaces look wavy: This is typically a result of incorrectly-parameterized Taubin smoothing.  We are working on a configuration tool to make identifying the ideal parameters easier, but in the interim, try making small adjustments to the prototypeSmoothingKpb, prototypeSmoothingN, homologizationSmoothingKpb, and homologizationSmoothingN parameters.
	- Color maps seem skewed or don not show much variation:  This can be an issue with the distribution and amount of shape variation in the sample and how it is interpreted by the color mapping method.  Try adjusting the heatMapLowerLimit and heatMapUpperLimit values (e.g., 1->5, 99->95) and toggling the areaWeightColorMaps option for better visualization.
	- Objects are flipped the wrong way after superimposition: This is often an issue with initialization, try some of the other initialization options, but if that also fails, you can roughly pre-align the objects in MeshLab and turn initialization off.

If your issue isn't listed here, please turn on all configuration parameters starting with the word "store" (e.g, storeExtraSuperimpositionOutput = true, storeSuperimposedToMeanDistances = true, etc.) and re-run your data, then email/message us with the "console.log" file located in the same directory as the "GPSA2-0.1.0.jar" executable.  In the event we are unable to diagnose the issue from the log file alone, we may request a copy of your input and output data to find the problem, and if necessary, to write a fix for the software.  As fellow scientists, we know collecting and curating data is a tremendous amount of work, and the decision to share it is not taken lightly.  We promise we will not share/publish your data/results with any third parties without your explicit knowledge and consent.