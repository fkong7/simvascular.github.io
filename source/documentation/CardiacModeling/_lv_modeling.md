# Automatic LV Mesh Generation For Cardiac Flow Simulations in svFSI

## Introduction

One application of the **SimVascular Automatic Cardiac Segmentation** tool is to generate left venticle (LV) meshes from the segmentation results. We can use these meshes to perform computational fluid dynamics (CFD) modeling of LV flow combined with patient medical imaging data.  A typical LV model construction pipeline usually starts with segmentation of the LV by manual delineation followed by mesh generation and registration techniques using separate software tools. In SimVascular, we have included an **Automatic LV Mesh Generation** tool to automate each of these steps to generate simulation-ready meshes with no or few human efforts. 

The  **Automatic LV Mesh Generation** tool takes in the generated segmentation and output reconstructed LV surface meshes for CFD simulations. Besides we also provide Python scripts for registering the surface meshes to compute wall motion using [SimpleElastix](https://simpleelastix.github.io/). These registered surface meshes can also be interpolated to obtain sufficient temporal resolution.

## Construct LV Surface Meshes with Tagged Boundary Faces

### Using SimVascular Python Shell

* Similarly, we can use SimVascular's Python Shell to run the prediction script. The SimVascular Python Shell can be invoked from the terminal according to the following instruction: http://simvascular.github.io/docsPythonInterface.html#python_shell. The prediction script `surface_main.py` can be found here in SimVascular's source code: Python/site-packages/sv_auto_lv_modeling/modeling/surface_main.py

    ```
    # Path to SimVascular exectuable
    sv_python_dir=/usr/local/bin
    model_script=modeling/surface_main.py
    # Path to the segmentation results
    input_dir= 02-Segmnts/WS01
    # Path to the outputed surface meshes
    output_dir= 03-Surfaces/WS01
    
    # Construct LV surface meshes with tagged boundary faces
    for file in ${input_dir}/*.nii.gz
    do 
        echo ${file}
        ${sv_python_dir}/simvascular --python \
            -- ${model_script} \
            --input_dir ${input_dir} \
            --output_dir ${output_dir} \
            --seg_name ${file##*/} \
            --edge_size 3.5
    done
    ```

## Volumetric Meshing using SimVascular 
* Update `volume_mesh.sh` with correct file/folder names and mesh edge size.
*  Run `volume_mesh_main.py` through the shell script.

## Construct Point Corresponded LV Meshes from 4D Images

Building point-corresponded LV meshes require segmentations from all time frames. One surface mesh will be created at one time frame and propagated to the others by registering the corresponding segmentations.

* Update `surface_registration.sh` with correct file and folder names. Specify the time phase id to construct LV surface mesh.
* Run `elastix_main.py` through the shell script.
