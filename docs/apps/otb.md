# Orfeo ToolBox (Open Source processing of remote sensing images) 

[Orfeo ToolBox](https://www.orfeo-toolbox.org/) or OTB is an open source application for processing high resolution optical, multispectral and radar images at the terabyte scale.

## Available

Orfeo ToolBox is available in the following versions:

* 7.2.0
* 6.6 

## Usage

### Loading Orfeo ToolBox

Orfeo ToolBox can be loaded with

`module load otb`

### Using the command line tools

You can find the numerous **OTB applications** and examples of them from the [OTB CookBook](https://www.orfeo-toolbox.org/CookBook/Applications.html)

For example, you can calculate NDVI from a Sentinel virtual raster by using the **otbcli_RadiometricIndices** application. This requires NIR band to be in the first channel and RED band in the second

`otbcli_RadiometricIndices -in <virtual raster> -channels.nir 1 -channels.red 2 -list Vegetation:NDVI -out <output_file>`

### Using the OTB Python tools

The applications included in OTB can also be run by using the Python bindings included in the module.

Here's an example how to run mean raster calculation on a test virtual raster with Python.

```
import otbApplication as otb

fp = <INPUT RASTER FILEPATH>

out = <OUTPUT RASTER FILEPATH>

app = otb.Registry.CreateApplication("Smoothing")
app.SetParameterString("in", fp)
app.SetParameterString("type", "mean")
app.SetParameterString("out", out)
app.ExecuteAndWriteOutput()
```


### Running OTB applications in parallel 

OTB applications seem to scale the number of processing threads automatically which means that the applications generally run faster when given more CPU cores. 

Here is an example batch job script with 4 CPU cores

```
#!/bin/bash
#SBATCH --job-name=<name_of_your_job>
#SBATCH --account=<your_project>
#SBATCH --time=00:03:00
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=4
#SBATCH --mem-per-cpu=2000
#SBATCH --partition=small

module load otb

otbcli_RadiometricIndices -in test_vrt.vrt -channels.nir 1 -channels.red 2 -list Vegetation:NDVI -out NDVI.tif
```

!!! note
    It is also possible to run OTB in parallel in several computing nodes [using MPI](https://www.orfeo-toolbox.org/CookBook/CliInterface.html#parallel-execution-with-mpi) but it has not been tested yet on Puhti

## License and citing

Orfeo ToolBox is licensed under the Apache License, Version 2.0. The full licence can be found [here](https://github.com/orfeotoolbox/OTB/tree/develop/Copyright)

In your publications please acknowledge also oGIIR and CSC, for example “The authors wish to acknowledge for computational resources CSC – IT Center for Science, Finland (urn:nbn:fi:research-infras-2016072531) and the Open Geospatial Information Infrastructure for Research (oGIIR, urn:nbn:fi:research-infras-2016072513).”

### References

* [Orfeo ToolBox homepage](https://www.orfeo-toolbox.org/)
* [Orfeo ToolBox general documentation](https://www.orfeo-toolbox.org/CookBook/)
* [Orfeo ToolBox Python API documentation](https://www.orfeo-toolbox.org/PythonDoc/)

