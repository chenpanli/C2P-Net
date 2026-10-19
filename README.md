# C2P-Net: Two-Stage Non-Rigid Point Cloud Registration for Middle Ear Diagnostics

## Installation
All required packages are listed in requirements.txt. <br>
You also need to follow the installation instructions of the sub-repositories [NgeNet](https://github.com/zhulf0804/NgeNet), [RegTR](https://github.com/yewzijian/RegTR) and [Neural Deformation Pyramid](https://github.com/rabbityl/DeformationPyramid).

## Dataset
Training and testing require a dataset, which must be placed under mesh_dataset/ear_dataset.

```text
mesh_dataset\
│
├── ear_dataset\
│   ├── 000000\
│   │   ├── intra_surface.stl
│   │   └── pre_surface_with_displ.vtp
│   │
│   ├── 000001\
│   │   ├── intra_surface.stl
│   │   └── pre_surface_with_displ.vtp

...

│   ├── 999999\
│   │   ├── intra_surface.stl
        └── pre_surface_with_displ.vtp
```

Each *pre_surface_with_displ.vtp* file contains the source model, represented as points and the connections between them, <br>
together with the displacement field (one vector per point, describing the transformation from source to target).
Each *intra_surface.stl* file contains the target 3D model.

Next, cache the data in a pickle file by running these two lines:<br>

    cd mesh_dataset
    python ear_dataset_setup.py

The script also creates a train/val/test split (80%, 15%, 5%) of the dataset.
Unfortunately, the dataset cannot be made available yet, because legal matters with the institute still need to be resolved.

## Training
To train *NgeNet*:<br>

    python trainNgeNet.py

The results are saved under *trainResults/eardataset_nonrigid_randSurface_perm*. <br>

To train *RegTR*:<br>

    python trainRegTR.py

You can use TensorBoard to visualize the loss curves and other metrics from this data. The weights are stored in the */checkpoints* subfolder.

## Testing
To test the method, run:

    python test_pipeline.py

Among other outputs, the script prints the Chamfer distance of the result and the wall time.<br>
The results are saved as STL files in *test_output_folder*, along with other outputs needed to create the animations.<br>

To plot how the method's accuracy depends on the number of points in the target, run:

    python surfaceamount_test.py

## Creating Images and Animations

The images in the presentation were created with this script:

    python make_animations.py [TEST_INDEX]

TEST_INDEX selects the test sample. The maximum index is the number of test samples minus 1.

All diagrams and images used in the BeLL and the written documentation are in figures/.

### Acknowledgements
I thank **Liu Peng from NCT Dresden** and **Steffen Oßmann from ERCD Dresden** for providing the dataset. <br>
I also thank the authors of [NgeNet](https://github.com/zhulf0804/NgeNet), [RegTR](https://github.com/yewzijian/RegTR) and [Neural Deformation Pyramid](https://github.com/rabbityl/DeformationPyramid).
