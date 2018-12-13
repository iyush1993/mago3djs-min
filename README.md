# Mago3d - Community Edition (F4D Converter + Mago3dJS)

### Project Folders

| Folder          | Description                                       |
| --------------- | ------------------------------------------------- |
| css             | Basic Styling                                     |
| **<u>data</u>** | Configuration Files                               |
| **<u>dist</u>** | Mago3d and Cesium distribution files (JavaScript) |
| externlib       | External Libraries                                |
| images          | UI Images                                         |
| f4d             | Contains F4D files of projects.                   |

<u>*Note: **data** and **dist** are the folders that contain the necessary files for 3D visualization. More detail about the folders below*</u>

### Folder Description

#### - data

The data folder is for maintaining the operation policy and data management. This folder contains necessary configuration files related to particular project. There are two configuration files we need to render 3D using Mago3d:

1. policy.json (3D rendering Policy)
2. data.json (List of 3D data to be visualized along with their spatial data)

*Note: The name of the files are not required to be* `policy.json` *and* `data.json`. *Meaning the file names can be anything (but preferably name the files on the basis of the project)*

We need to manually create these files. However, the structure is same. We just need to edit some key values.

1. policy.json

It stores the information related to 3D rendering like: Initial Camera Latitude and Longitude, viewing library, default project, **data path**, LOD information, etc.

**Important keys:**

| Key                       | Description                                              |
| ------------------------- | -------------------------------------------------------- |
| geo_data_path             | Path to f4d symbolic link (explained below)              |
| geo_data_default_projects | Array of default projects. we specify the data.json here |
| geo_init_latitude         | Initial camera latitude                                  |
| geo_init_longitude        | Initial camera longitude                                 |
| geo_init_height           | Initial camera elevation                                 |
| geo_init_duration         | Camera pan animation duration                            |

1. data.json

It stores the information of 3D data to be visualized. We can consider it as **One `data.json` file per project**. One `data.json` can list multiple 3D data (contents).

**Important keys:**

| Key       | Description                                                  |
| --------- | ------------------------------------------------------------ |
| data_key  | - data_key of the **root node** of json should match project name inside f4d<br />- data_key of **child node** of json should match F4D folder name. |
| data_name | Name of the 3D data. Can be same as data_key                 |
| latitude  | Latitude of 3D data                                          |
| longitude | Longitude of 3D data                                         |
| height    | Specifies at what height to place 3D data                    |
| heading   | Specifies which direction does the front of 3D data faces    |

*Note: We can use same policy for multiple projects. It is not required to have individual policy for each project.*

Since these `.json` files serve different purposes, the data folder is has two sub folders, policies and projects to separate them.

#### - dist

This folder contains the final releases of Cesium and Mago3d along with css files required for Cesium.

#### - f4d

This is the symbolic link to the folder that holds the 3D data which has been converted from IFC rather than an actual folder. Mago3D finds the 3D data inside this folder which is referenced in `policy.json` and renders it on the basis of its respective `data.json`.

### Data Conversion - IFC to F4D

The Mago3D team has developed a new file format for efficient rendering of 3D data. They have developed a converter called F4DConverter which converts 3D files (IFC, COLLAD, JT, 3DS, etc.) to F4D format.

#### - F4D Converter

First, we need to download the F4D Converter from Mago3D Website. Go [here](http://www.mago3d.com/homepage/download.do), download installer from <u>F4D Converter Section</u> and Install it.

- Create a folder to store the converted files (**#outputFolder**)

  - For e.g. **D:\f4d\project_name**

    *Separate folder for each project inside f4d.*

- Create a folder to store input files (.ifc files) (**#inputFolder**)

- Place your IFC files inside that folder. Try separating IFC files according to project.

  - For e.g. **D:\data\project_name**

    *Put all the IFC files related to a project inside a particular folder*

- Run Command Line Prompt (cmd.exe) with Administrative Privileges

- cd to where F4D Converter is installed

- Run

  - `C:\F4DConverter>F4DConverter.exe #inputFolder D:\data\project_name #outputFolder D:\f4d\project_name #log D:\F4DConverter\logs\log.txt #indexing y #meshType 0 `

    **[Detail about the parameters and how to use.](https://github.com/Gaia3D/F4DConverter/blob/master/README.md#how-to-use)**

- Once the conversion is complete, it will output F4D folder for each IFC inside **outputFolder**. For e.g. if the input file was school.ifc, F4DConverter will create F4D_school folder with all the files required inside outputFolder.



#### - Symbolic Link

Next, we need to **create symbolic link to output folder** inside project root.

To create a symbolic link,

- Run Command Link Prompt (cmd.exe) with Administrative Privileges.

- cd to project root

- Run

  `D:\mago>mklink /d "D:\mago\f4d" "D:\f4d"`

  Here, the first path is where we want to create symbolic link and second path is the actual path where we convert and store converted files.



### Configuration File

We use json files in order to reference the 3D data into our project. Following are two configuration files we need in order for Mago3D to render our 3D data.

#### data.json

```json
{
    "attributes" : {
      "isPhysical" : false,
      "nodeType": "root",
      "projectType": "project Type"
    },
    "children" : [
    ],
    "data_key" : "Project name",
    "data_name" : "Project name"
}
```

**Important Key: data_key**

Make sure the value of data_key matches the project folder name. Same for data_name

```json
"children" : [
   {
     "attributes" : {
       "isPhysical" : true,
       "nodeType" : "..."
     },
     "children" : [
     ],
     "data_key" : "Unique identifier",
     "data_name" : "Data name",
     "latitude" : "Enter latitude",
     "longitude" : "Enter longitude",
     "height" : "Enter height",
     "heading" : "Enter heading",
     "pitch" : "Enter pitch",
     "roll" : "Enter roll"
  }
]
```

**Important Key: data_key**

Make sure the value of data_key matches the F4d folder name. For e.g. if the output of F4DConverter is F4D_SCHOOL, data_key value should be SCHOOL. Suffix F4D is added by the converter.



#### policy.json

```json
{
    "geo_data_default_projects": [
    	"data.json"
	],
	"geo_init_latitude": "Enter latitude",
	"geo_init_longitude": "Enter longitude"
}
```



[Complete API for Mago3D](www.mago3d.com/homepage/api.do)