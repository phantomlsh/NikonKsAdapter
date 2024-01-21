# NikonKsAdapter

A MicroManager 1.4 Device Adapter for the Nikon Qi2/Ri2 cameras.

> The code is based on [Gomella's Code](https://github.com/andrewgomella/NikonKsAdapter)

**The stability is not garanteed.**

> For MicroManger 2.0.0, the built adapter `mmgr_dal_NikonKsAdapter.dll` should be capable to run in MicroManager 2. If not, try to recompile it with deleting the Line 28&29 in `NikonKsCam.cpp`.

## Added Features

- [x] Exposure Control
- [x] Gain Control
- [x] Image Format
- [x] ROI (supported by format)

## Guide for Users

### Setup

Copy the built dll file `mmgr_dal_NikonKsAdapter.dll` to the root directory of Micro-Manager 1.4 (or 2) software. The file can be found in the release of this repository.

Install the Nikon's `KsCamInstaller_v2_1_2_3_x64`, and after intalling it, copy `KsCam.dll` from the installation directory to the root directory of Micro-Manager Software as well.

Hopefully, you can now setup the NikonKsAdapter normally in the Micro-Manager 1.4

### Notes

Exposure can be controlled by the panel as well as the group/preset.

Gain, Format and ROI Position can be controlled by group/preset.

> Format and ROI are only adapted for Qi2 Camera.

When selecting a ROI format, you can change the frame position by setting `ROI Position X` and `ROI Position Y`

## Guide for Developers

> This guide was created on 2020.11.03, with the operating system Windows 10.

### Step 1: Download the repositories

First, download and install the tool TortoiseSVN here: <https://tortoisesvn.net/downloads.html>
After installing it, find a good place (at least 5GB spared space), make a folder.
Right click anywhere inside the folder -> SVN Checkout...
There are two repos to checkout: (pay attention to the checkout folder!)
- https://valelab4.ucsf.edu/svn/micromanager2/trunk/ (checkout to `micromanager` in
current folder)
- https://valelab4.ucsf.edu/svn/3rdpartypublic/ (checkout to `3rdpartypublic` in current
folder)
> The sizes of repos. are super large, and there might be some trouble while checking out the second
repo. When such things happen, close TortoiseSVN window, then
> 1. right click in the `3rdpartypublic` folder -> TortoiseSVN -> Clear up... -> tick Break write lock -> OK
> 2. right click in the `3rdpartypublic` folder -> SVN Update
> It will continue to download

### Step 2: Install and Config Visual Studio

Download and install the newest Visual Studio C++ (Community 2019 version) here <https://visualstudio.microsoft.com/vs/features/cplusplus/> (A huge space is required), and then you will get the following items in the start menu:
- Visual Studio 2019
- Developer Powershell for VS 2019
- Visual Studio Installer

open `Visual Studio Installer` -> click `update` -> Individual Components -> tick `.NET 4.8 Targeting Pack` and `.NET Framework 4.8 SDK` -> update

Now we need to fix an error called `HRESULT E_FAIL`, occurring while opening projects: Right click `Developer Powershell for VS 2019` -> Open as an administrator -> use command `cd` to reach
the path: `[VS2019path]\Community\Common7\IDE\PublicAssemblies` -> run command
`gacutil -i Microsoft.VisualStudio.Shell.Interop.11.0.dll` 

Hopefully, now you can open Micromanager projects in the Visual Studio.

### Step 3: Clone and Config NikonKsAdapter

Clone this github repository into `/micromanager/DeviceAdapter/`, then edit `/micromanager/micromanager.sln` and add following segment anywhere in the long list of project links
```
Project("{8BC9CEB8-8B4A-11D0-8D11-00A0C91BC942}") = "NikonKsAdapter", "DeviceAdapters\NikonKsAdapter\NikonKsAdapter\NikonKsAdapter.vcxproj", "{2B5F1E65-6F76-45B3-959B-62BB309A3A6E}"
EndProject
```
these codes should not be changed, but if anything wrong happens, check the vs project files.

Now open Visual Studio and open solutions `/micromanager/micromanager.sln`, you are supposed to see a long list of projects, including `MMCore` and `NikonKsAdapter`.

### Step 4: Compile

Unfortunately, for most chance, you will not be able to successfully compile the device adapters. Several possible problems and solutions:
- `Windows 7.1 SDK not found`: right click problematic projects -> properties -> set the Platform ToolSet to `Visual Studio 2019` and Windows SDK Version to `10`.
- Any other not found: please google it and see if it can be solved by change properties -> `VC++ Directories` -> `Include Directories`

Compile for **Release | x64**, and hopefully you can find the built file at `/micromanager/build/Release/x64/mmgr_dal_NikonKsAdapter.dll`
