# Create a Catalog of Model Files on Keeling with Intake-ESM

This tutorial will demonstrate how to create a most up-to-date catalog in the form of a pandas table with Jupyter notebook. 

To see how to downlaod CMIP model output from ESGF nodes, please see [Downloading CMIP5 and CMIP6 Models .nc Files from ESGF](../documentation/downloading_CMIP5_and_CMIP6_Models_.nc_Files_from_ESGF.md). 
To see how to downlaod CESM model output, please see **(in progress)**. 

You will be downloading the files to the following directory (referred as `path`): 

    /data/keeling/a/cristi/a/data_tmp/cmip6

, where organized model output are located at (referred as `rootpath`): 

    /data/keeling/a/cristi/a/esm_data/cmip6

You do not need to do the organizing manually, as the notebook will sort files in the `data_tmp` folder automatically. In other words, the `data_tmp` folder should only contain `.nc` files. 

Links to the sample Jupyter Notebook: 

- CESM: [Here](../tutorials/create_catalog_cesm.ipynb)
- CMIP6: [Here](../tutorials/create_catalog_cmip6.ipynb)

>Make sure you are referring to the correct notebook when building the catalog. 

This tutorial is based on the content contributed by professionals who developed intake_esm and ecgtools. Check out more details about what intake_esm is and its additional functions at their websites - [intake_esm](https://intake-esm.readthedocs.io/en/stable/how-to/build-a-catalog-from-timeseries-files.html) and [ecgtools](https://ecgtools.readthedocs.io/en/latest/how-to/use-a-custom-parser.html). 

## Prerequisites
It is required to have following packages installed. 
   
    import intake
    from ecgtools import Builder
    from ecgtools.parsers.cesm import parse_cesm_timeseries
    import pandas as pd 
    import xarray as xr
    import numpy as np
    import itertools

- Install [`ecgtools`](https://ecgtools.readthedocs.io/en/latest/how-to/install-ecgtools.html)
- Install [`intake`](https://intake-esm.readthedocs.io/en/stable/how-to/install-intake-esm.html)

## Introduction 
The main goal of doing this step is to update the existing catalog with the newly downloaded files that are rearranged in the organized folders (e.g. `./cmip6/` and `./cesm/`). It is most likely that there are new folders created to store the newly downloaded datasets, for example, model output for a new member of an exisiting model is downloaded; or, datasets of a new model is downloaded. 

Therefore, we would have to 

0. **Organize the newly-downloaded files from the `data_tmp` folder and move them to the `esm_data` folder** 
1. **Get the most recent list of all subdirectories in the root folder (`cesm2` or `cmip6`)**
2. **Load the list into a Jupyter Notebook**
3. **Create the Catalog Builder and Build the Catalog**
4. **Check and Save the new Catalog**

## Step 0: Move Files from the `data_tmp` to the `esm_data` Folder
This step aims to sort all the newly-downloaded model output files in the `data_tmp` folder to follow the structure of the organized `esm_data` folder:

>root path/model/experiment/member

We first define ‘`rootpath`’ and ‘`path`’ as the path of the parent folder to-be-moved-to and the current parent folder of newly downloaded files.
The underlying assumption when using this tutorial is that, the downloaded files do not exist in the catalog: it may be an additional model, or a new experiment  from one of the existing models, or another member from a model and experiment that is already included in the catalog. These all require making a new folder.

To do so, we loop through all the new files and do the following:

A. Create the path that the file should locate in the organized parent folder

B. Double check if the (model/experiment/member) folders exist

C. Create the folder if the folders do not exist

D. Move the file to the folders
    
    def target_location_cmip(fname,rootpath): #generates target location 
    if rootpath[-1]!='/':
        rootpath=rootpath+'/'
    fname_set=fname.split('_')     
    location=rootpath+fname_set[2]+'/'+fname_set[3]+'/'+fname_set[4]+'/'    
    return location

    # move files from current folder to target folders
    k_moved=0
    for j in listdir(path): #read all downlaoded files in data_tmp
        current_dir=os.path.join(path, j) 
        target_dir= target_location_cmip(j,rootpath)
        if target_dir!=current_dir:
            if os.path.exists(target_dir)==False:
                os.makedirs(target_dir) #make directories 
            shutil.move(current_dir,target_dir) #move files 
            k_moved+=1

Step A is done using the function `target_location_cmip` , and the indices `2,3,4` of `fname_set` represents the model, experiment and member accessed from the full path of the new files respectively.

## Step 1: Get Updated List of Subdirectories 
Go to the targetted folder through the Terminal: 
- If you need to update the **CMIP6** catalog, go to `/data/keeling/a/cristi/a/esm_data/cmip6`. 
- If you are updating the **CESM** catalog, go to `/data/keeling/a/cristi/a/esm_data/cesm`. 

**CMIP6**

You should be able to see a similar scene when you reached the `./cmip6/` folder.  
![cmip6_folder_dir](../images/cmip6_folder_dir.png)

Run the following line of code: 
    
    readlink -f $(find . -type d -path '*/r*' -prune) > subdir_list.csv

This is a nested code, where all subdirectories (all members within the model folders) are included, '`readlink -f`' part instructs to get the full filepath. The latter part '`> subdir_list.csv`' saves the output as a `.csv` file in the `./cmip6/` folder. 

**CESM1/CESM2** 

(To be updated)

## Step 2: Load List of all Subdirectories in `./cesm` or `./cmip6`

    filepathlist = pd.read_csv('/data/keeling/a/cristi/a/esm_data/cmip6/subdir_list.csv',header=None,names=['dirpaths']).values.tolist()
    
    filepath=list(itertools.chain.from_iterable(filepathlist))

> Change the filepath `./cmip6/...` to `./cesm/...` for reading the subdirectory list file for CESM2. 

We load the `.csv` file from step 0 into the Jupyter Notebook and get all the directory paths. Since it is a pandas object, we need to change it as a list. After the first line of code, we will get a list of lists; therefore, the second line combines the list of lists and return a list with strings of directory paths. 

## Step 3: Create the Catalog Builder and Build the Catalog 

    cat_builder = Builder(paths=filepath)
    catalog = cat_builder.build(parsing_func=parse_cmip6)

The `Builder` object is used directly from [Intake_ESM](https://intake-esm.readthedocs.io/en/stable/how-to/build-a-catalog-from-timeseries-files.html), which has more detailed discussion about the arguments for `Builder`. Here, `paths` is the only argument included, which is the list of directory paths we get from step 1. 

Then, we will build the catalog with `.build()`, where the `parsing_func` is dependent on the files that the catalog includes:

- **`parse_cmip6`** for CMIP6 files 
- **`parse_cesm_timeseries`** for CESM Timeseries files*
- **`parse_cesm_history`** for CESM History files*

<font size = 1> *Works for both CESM1 and CESM2. 

<font size =2>

These are the exisisting parsing functions. For files other than these types, such as CMIP5 files, you can build your own parsing function with the guidelines from [ecgtools](https://ecgtools.readthedocs.io/en/latest/how-to/use-a-custom-parser.html). To check what the parsing function is, you can run

    ?parse_cmip6

The run time for this step may be long, especially for the CMIP6 catalog, due to the large amount of files. 


## Step 4: Check and Save the Catalog

    catalog.df

The variable `catalog` is a `ecgtools.builder.Builder` object. To view the content, we add `.df` to read it as a dataframe. 

    catalog.df.to_csv('/data/keeling/a/cristi/a/esm_data/cmip6_catalog.csv', index=False)

`index=False` is required to avoid an additional column of index when loading the catalog. 

The path for saving the catalogs is as follow: 

    /data/keeling/a/cristi/a/esm_data/


And now you are done! 













## Resources 
1. [Intake_ESM page](https://intake-esm.readthedocs.io/en/stable/how-to/build-a-catalog-from-timeseries-files.html)
2. [ECGtools (Changing Parsers)](https://ecgtools.readthedocs.io/en/latest/how-to/use-a-custom-parser.html)
