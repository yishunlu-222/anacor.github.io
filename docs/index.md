![Flow chart](https://github.com/yishunlu-222/anacor.github.io/blob/main/docs/whole%20process.jpg)
## anacor.init
This command is used to create the initialization for the AnACor. This creates the following three default input files for the user to enter commands/flags to the commands. They contain the possible flags for the anacor.preprocess, anacor.mp and anacor.postprocess. The users can use this template to create a new input files for the commands.
```markdown
├── current_folder
│   ├── default_mpprocess_input.yaml
│   ├── default_postprocess_input.yaml
│   ├── default_preprocess_input.yaml
```

## anacor.preprocess
![anacor.preprocess](https://github.com/yishunlu-222/anacor.github.io/blob/main/docs/anacor.preprocess.jpg)
This command is used to create 3D model, calculate absorption coefficient and preprocess data from dials (.expt and .refl).  To execute `anacor.preprocess` , the user can either change the flags/parameters in `default_preprocess_input.yaml`  or create a new one .yaml file, where the user needs to source the path like below:

```anacor.preprocess --input-file "input flag file path" ```

### Parameters/Flags in the .yaml file
	"--dataset" ,
	type = str ,
	help = "dataset reference number/name " ,
	
        "--store-dir" ,
	type = str ,
	default = "./" ,
	help = "the store directory " ,
	
	"--segimg-path" ,
	type = str ,
	help = "the path of segmentation images" ,
         
	"--rawimg-path" ,
	type = str ,
	default = None ,
	help = "the path of raw flat-field images" ,
		 
	"--refl-filename" ,  
	type = str ,  
	help = "the path of the reflection table" , 
	    
	"--expt-filename" ,
        type = str ,
        help = "the path of the experimental file" ,
        
	"--create3D" ,	
        type = bool ,
        default = True ,
        help = "whether the reconstruction slices need to be vertically filpped to match that in the real experiment" ,
        
	"--cal-coefficient" ,
        type = bool ,
        default = False ,
        help = "whether need to calculate coefficients" 
        
        "--coefficient-auto-orientation" ,
        type = bool ,
        default = True ,
        help = "whether automatically match the orientation of 3D model with the flat-field image to calculate absorption coefficient "
        
        "--coefficient-auto-viewing" ,
        type = bool ,
        default = True ,
        help = "whether automatically calculating the largest area of the flat-field image to calculate absorption coefficient "
	    
	"--coefficient-orientation" ,
        type = int ,
        default = 0 ,
        help = "the orientation offset of the flat-field image to match the 3D model in degree. normally this is 0 degree" ,
	    
	"--coefficient-viewing" ,
        type = int ,
        default = 0 ,
        help = "the viewing angle of the 3D model to have the best region to determine absorption coefficient in degree" ,
        
        "--flat-field-name" ,
        type = str ,
        default=None,
        help = "the flat-field image selected to determine the absorption coefficient, "
               "when you use this flag, you should also fill the angle in coefficient_viewing"
               "to allow the 3D model to rotate to match it"
        
        "--coefficient-thresholding" ,
        type = str ,
        default = None ,
        help = "thresholding method to extract the region of interest"
               "options are: 'triangle', 'li', 'mean,'minimum','otsu','yen','isodata'" ,
        
        "--full-reflection" ,
        type = bool ,
        default = False ,
        help = "whether cutting some unwanted data of the reflection table"
               "before calculating based dials.scale outlier removing algorithm" ,
        
        "--dials-dependancy" ,
        type = str ,
        help = "the path to execute dials package"
               "e.g. module load dials"
               "e.g. source /home/yishun/dials_develop_version/dials" ,
        
        "--model-storepath" ,
        type = str ,
        default = None ,
        help = "the storepath of the 3D model built by other sources in .npy" ，
        
        "--coe_li" ,
        type = float ,
        Optional =True,
        help = "pre-measured absorption coefficient for liquor, if this is given,"
        " liquor will be fixed in this process " ,
        
        "--coe_cr" ,
        type = float ,
        Optional =True,
        help = "pre-measured absorption coefficient for crystal, if this is given,"
        " crystal will be fixed in this process " ,
        
        "--coe_lo" ,
        type = float ,
        Optional =True,
        help = "pre-measured absorption coefficient for loop, if this is given,"
        " loop will be fixed in this process " ,



### Example results on the store directory
```markdown
├── current_folder
|   ├──{dataset}_save_data
│   │   ├──ResultData
	│   │   ├──absorption_coefficient
		│   │   ├──**.png**
	│   │   ├──absorption_factors
	│   │   ├──reflections
	│   │   ├──dials_output
│   │   ├──Logging
│   │   ├──**preprocess_script.sh**
│   │   ├──**{dataset}_.npy**
│   │   ├──** *.expt.json**
│   │   ├──** *.refl.json**
│   ├── default_mpprocess_input.yaml
│   ├── default_postprocess_input.yaml
│   ├── default_preprocess_input.yaml
```

## anacor.mp
This command is used to calculate the absoprtion factors across different nodes on the cluster (Single core command is not optimized yet).  To execute `anacor.mp` , the user can either change the flags/parameters in `default_mpprocess_input.yaml`  or create a new one .yaml file, where the user needs to source the path like below:

```anacor.mp --input-file "input flag file path" ```

### Parameters/Flags in the .yaml file
		"--dataset" ,
         type = str ,
         help = "dataset reference number/name " ,
         
        "--store-dir" ,
         type = str ,
         default = "./" ,
         help = "the store directory " ,
           
		 "--refl-filename" ,  
	    type = str ,  
	    help = "the path of the reflection table" , 
	    
		"--expt-filename" ,
        type = str ,
        help = "the path of the experimental file" ,
        
        "--dials-dependancy" ,
        type = str ,
        help = "the path to execute dials package"
               "e.g. module load dials"
               "e.g. source /home/yishun/dials_develop_version/dials" ,
        
        "--model-storepath" ,
        type = str ,
        default = None ,
        help = "the storepath of the 3D model built by other sources in .npy" ,
        
        "--hpc-dependancies" , nargs = '*' , type = str ,  
                     help = "List of hpc_dependancies to execute, they can be entered at the same time e.g. 'module load dials' 'module load global/cluster' " 
                     
         "--num-cores" ,
        type = int ,
        default = 20 ,
        help = "the number of cores to be distributed" ,
        
        "--hour" ,
        type = int ,
        default = 3 ,
        help = "cluster request time: hour" ,
        
        "--minute" ,
        type = int ,
        default = 10 ,
        help = "cluster request time: minute" ,
        
        "--second" ,
        type = int ,
        default = 10 ,
        help = "cluster request time: second" ,
        
        "--crac" ,
        type = float , required = True ,
        help = "the absorption coefficient of the crystal and it is needed" ,
   
        "--loac" ,
        type = float , required = True ,
        help = "the absorption coefficient of the loop and it is needed" ,
    
        "--liac" ,
        type = float , required = True ,
        help = "the absorption coefficient of the liquor and it is needed" ,
    
        "--buac" ,
        type = float , default = 0 ,
        help = "the absorption coefficient of the bubble and it is not necessarily needed" ,
        
        "--sampling" ,
         type = int ,
         default = 5000 ,
         help = "sampling for picking crystal point to calculate" ,
        
        "--post-process" ,
         type = bool ,
         default = False ,
         help = "doing the post process in the cluster after the calculation,
	 if you set this true, after the calculation is finished, you can go to
	 the directory ./ResultData/dials_output/*.log to check the results" ,
        
        "--offset" ,
        type = float ,
        default = 0 ,
        help = "the orientation offset" ,
	
	"--auto-sampling" ,
        type = bool ,
        default =True,
        help = "automatically determine sampling number",
        
        "--full-iteration",
        type=int,
        default=0,
        help="whether to do full iteration(break when encounter an air point)",
        
		"--pixel-size-x",
        type=float,
        default=0.3,
        help="overall pixel size of tomography in x dimension",
       
       "--pixel-size-y",
        type=float,
        default=0.3,
        help="overall pixel size of tomography in y dimension",
        
       "--pixel-size-z",
        type=float,
        default=0.3,
        help="overall pixel size of tomography in z dimension
		
       "--by-c",
        type=str2bool,
        default=True,
        help="calculate by c instead of python",
        
        "--slicing",
        type=str,
        default='z',
        help="slicing sampling direction",
        
         "--num-workers",
        type=int,
        default=4,
        help="number of workers",
        
        "--test-mode",
        type=str2bool,
        default=False,
        help="test mode",

        "--bisection" ,
        type = str2bool,
        default = True ,
        help = "activate bisection method" ,


### Example results on the store directory
```markdown
├── current_folder
|   ├──{dataset}_save_data
│   │   ├──ResultData
	│   │   ├──absorption_coefficient
	│   │   ├──absorption_factors
		│   │   ├──**.json**
	│   │   ├──reflections
	│   │   ├──dials_output
│   │   ├──Logging
│   │   ├──preprocess_script.sh
│   │   ├──**mpprocess_script.sh**
│   │   ├──{dataset}_.npy
│   │   ├──*.expt.json
│   │   ├──*.refl.json
│   ├── default_mpprocess_input.yaml
│   ├── default_postprocess_input.yaml
│   ├── default_preprocess_input.yaml
```
## anacor.post_process
This command is used to combine the calculated absorption factors into dials file and then `dials.scale`.  To execute `anacor.post_process` , the user can either change the flags/parameters in `default_postprocess_input.yaml`  or create a new one .yaml file, where the user needs to source the path like below:

```anacor.post_process --input-file "input flag file path" ```

The results are stored in `{dataset}_save_data/ResultData/dials_output/..`
