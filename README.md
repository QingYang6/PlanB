## PlanB: SWD(Self-supervised Waterbody Detection) SAR Edition

## 1 Requirements
```
As the modules used in the code.
```

## 2 Usage
### 2.1 PlanB.py
```
python -u PlanB.py '<Input_SAR_Pol1>' '<Input_SAR_Pol2>' <Output_Folder>
```
This will run PlanB in single mode with direct input from command line.
```
python -u PlanB.py <PATH_JSON>
```
This will run PlanB with the input parameters that specified in <PATH_JSON>.

### 2.2 config.json
PlanB works for single image mode. Still could input multi-SAR files. Input files, and output folder in .json file.
* "OUT_PATH": Folder path for output.
* "filepath_list": Input file list.
  * * (1) Raster files links or paths: [["file1_pol1","file1_pol2"],["file2_pol1","file2_pol2"],...,["fileN_pol1","fileN_pol2"]]. The first group should be the in-event image, and the rest are pre-event images.
  * * (2) ASF RTC urls: [["ASF_url1.zip"],["ASF_url2.zip"],...,["ASF_urlN.zip"]].
* "granules" (optional): Granules name for each image, will extract from input file path if not provided. Refer to 'config.json' for more info.
* 'config.json' shows a demo using ASF RTC30 urls. The urls are valid until 05/27/2023.

### 2.3 Data input requirements
1. Dual polarizations.
   * Currently, only input with dual polarizations is supported, regardless of the polarization mode.

### 2.4 Running template
```
python -u PlanB.py 'https://ebd-clients-w.s3.amazonaws.com/NOAA/RCM/COG/RCM2_OK2535409_PK2538063_SC30MCPB_20230503_124750UTC-EBD_CH_AMP.tif?AWSAccessKeyId=AKIAJFTC5FRGKQV5C5KQ&Signature=j73oGsBfQsOscPBLlpCvv%2FVk6W0%3D&Expires=1769538603' \
'https://ebd-clients-w.s3.amazonaws.com/NOAA/RCM/COG/RCM2_OK2535409_PK2538063_SC30MCPB_20230503_124750UTC-EBD_CV_AMP.tif?AWSAccessKeyId=AKIAJFTC5FRGKQV5C5KQ&Signature=b8s8CaOBDjQIlF%2FjZX5Qo6dLhc0%3D&Expires=1769538603' \
./OUTPUT
```
or
```
python -u PlanB.py ./config.json
```

## 3 Output
### 3.1 Output files
1. 'PlanB_2classes_RGB_<granuele_name>.tif': RGB tif result labeling persistent and flood water, recommend for web display.
2. 'PlanB_3classes_<granuele_name>.tif': RGB tif result labeling persistent, pre-event and flood water, when running normal mode with dry reference.
3. 'PlanB_RiverIce_2classes_RGB_<granuele_name>.tif': RGB tif result labeling persistent, river ice and flood water, under development.
4. 'PlanB_raw_<granuele_name>.tif': Raw binary raster, all water pixels are labeled as 1.
5. '*.png, *.jpg': Quick viualization of the rasters.
6. Folder named by <granuele_name>: Contain intermediate result files.

### 3.2 Legend for RGB tif
{0 92 230 Persistent water<br>
0 197 255 Pre-event water<br>
219 0 0 Flood water<br>
255 215 0 River ice
}