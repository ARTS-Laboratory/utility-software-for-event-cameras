# Utility Software for Event Cameras
Utility software and artifacts for event cameras

Created by: Charlie Buren

- Date of last change: 
    - Updated: 8/08/2025
    - Turned into markdown file: 8/3/2026

- **Notes**: 
    <!-- - If you have any questions or concerns please email me at: cburen@email.sc.edu -->
	- If you don't want to do these steps and just want the .exe's then look in the metavision_executables folder on GitHub. Just make sure to copy them to a new folder so you don't overwrite them on accident. 

## Using GUI
1) Copy metavision_executables folder into a new folder not in the github
	- Ensures that any errors don't change the exe files
2) When GUI asks for exe folder select that new folder that you just created
3) Select the exe folder first before doing anything else

## Video Exporting
### Option 1: Using Metavision Studio
1) Open the recording that you desire to export to video
2) Trim the recording using the options on the toolbar
3) If desired change the frame rate of video
    - 250 for regular slowed down video
		- If slower is desired use 300 or 500. 
	-  1 for real time speed

### Option 2: Using GUI
	- Not Done Yet

# Installation Guide
<details>
<summary><strong>Table of Contents</strong></summary>

- [1. Prerequisites](#1-prerequisites)
  - [1.1 Git](#11-git)
  - [1.2 CMake](#12-cmake)
  - [1.3 MSYS2](#13-msys2)
- [2. Clone the Repositories](#2-clone-the-repositories)
  - [2.1 OpenEB](#21-openeb)
  - [2.2 vcpkg](#22-vcpkg)
  - [2.3 dirent](#23-dirent)
- [3. Set Up vcpkg](#3-set-up-vcpkg)
  - [3.1 Configure vcpkg](#31-configure-vcpkg)
  - [3.2 Configure OpenEB](#32-configure-openeb)
  - [3.3 Install Boost.Thread](#33-install-boostthread)
  - [3.4 Troubleshooting](#34-troubleshooting)
- [4. Download and Set Up Python](#4-download-and-set-up-python)
  - [4.1 Download Python](#41-download-python)
  - [4.2 Virtual Environment](#42-virtual-environment)
- [5. Compile Using CMake](#5-compile-using-cmake)
  - [5.1 Compiling](#51-compiling)
  - [5.2 Deploying](#52-deploying)
  - [5.3 Environment Variables](#53-environment-variables)
- [6. Compile Using Visual Studio](#6-compile-using-visual-studio)
  - [6.1 Build Visual Studio 17 2022](#61-build-visual-studio-17-2022)
  - [6.2 Setup](#62-setup)
  - [6.3 Change Environment Variables](#63-change-environment-variables)
- [7. Drivers](#7-drivers)
- [8. Running](#8-running)

</details>

## 1. Prerequisites

Refer to the official OpenEB Windows installation guide:

https://docs.prophesee.ai/4.6.2/installation/windows_openeb.html#chapter-installation-windows-openeb

### 1. Install Required Packages 
#### 1.1 Git 
Link to download: https://git-scm.com/downloads/win

#### 1.2 CMake Steps
1. Download the x64 Installer
    - Newest version of CMake: https://cmake.org/download/
	- Downloading zip is recommended for the ReadMe
2. Run the installer
3. Ensure that CMake is added to your PATH in environmental variables
	- If not then add it
	- This done by finding the install location and copying the location into a new PATH
		- ex: C:\Program Files\CMake\bin
4. Verify the installation by opening a new terminal and running:
```powershell
cmake --version
```

#### 1.3 Download MSYS2
1. Link to download: https://www.msys2.org/#installation
2.  Once the exe is downloaded run it and enter:

```bash 
$ pacman -S --needed git base-devel mingw-w64-x86_64-gcc 
```

3. Then enter `y` when it asks if you want install

<p align="right"><a href="#installation-guide">(back to table of contents)</a></p>

## 2. Clone the Repos
### 2.1 OpenEB
```bash 
git clone https://github.com/prophesee-ai/openeb
```
- The install location will be referred to `<openeb>`
	- Ex: C:\Users\CBUREN\Documents\GitHub\openeb

### 2.2 vcpkg
```bash
git clone https://github.com/microsoft/vcpkg
```
- The install location will be referred to `<vcpkg>`
	- Ex: C:\Users\CBUREN\Documents\GitHub\vcpkg

### 2.3 dirent
```bash
git clone https://github.com/tronkko/dirent
```
- The install location will be referred to `<dirent>`
	- Ex: C:\Users\CBUREN\Documents\GitHub\dirent

<p align="right"><a href="#installation-guide">(back to table of contents)</a></p>

## 3. Set up vcpkg
### 3.1 Configure vcpkg
1. Open Powershell

2. cd into `<vcpkg>`
    ```powershell 
    cd "<vcpkg>"
    ```

3. Bootstrap vcpkg
    ```powershell
    .\bootstrap-vcpkg.bat
    ```

4. Update vcpkg
    ```powershell
    .\vcpkg update
    ```

5. Pull again
    ```powershell
    git pull
    ```

6. Install hdf5
    ```powershell
    .\vcpkg install hdf5:x64-windows
    ```

### 3.2 Configure OpenEB 
1. Go to
    ```text
    <openeb>\utils\windows
    ```
2. Copy vcpkg-openeb.json into `<vcpkg>` 
3. Change the name to `vcpkg.json` 
    - Open the file and copy the `COPYTHIS.txt` into `vcpkg.json`

### 3.3 Install boost-thread
1. Using PS enter
    ```powershell
    .\vcpkg install boost-thread:x64-windows
    ```
	
### 3.4 (Skip if no errors) 
1. If failed then try the following
    - Note: 
        - The error logs can be found:
            ```text
            <vcpkg>\buildtrees
            ```
        - Then go to the file that corresponds to the error.

2. If you do any of these steps remove vcpkg.json 
3. Then open powershell
    ```powershell 
    .\vcpkg remove boost-thread --recurse
    .\vcpkg remove boost --recurse
    Remove-Item -Recurse -Force .\buildtrees
    Remove-Item -Recurse -Force .\packages
    Remove-Item -Recurse -Force .\installed
    ```
4. Then use update and git pull
    ```powershell
    .\vcpkg update
    git pull
    ```
    - If these steps didn't work then find error that what was listed and go to the error logs

<p align="right"><a href="#installation-guide">(back to table of contents)</a></p>

## 4. Download and setup Python
### 4.1 Download Python
1. Python version should be 3.12: 
    - Link: https://www.python.org/downloads/release/python-3120/
2. Add the directories to you paths during installation. 
3. Verify PATH was correctly setup
    - Should look something like
        ```text
        C:\Users\pc1\AppData\Local\Programs\Python\Python312\python.exe
        C:\Users\<username>\AppData\Local\Programs\Python\Python312\Scripts
        ```
4. Verify installation in PS
    ```powershell
    python --version
    ```
    - Python version should be 3.12 

### 4.2 Virtual Environment
1. Launch Visual Studio and open the terminal
2. In the terminal enter: 
    1. Create virtual environment
        ```bash 
        python -m venv C:\tmp\prophesee\py3venv --system-site-packages
        ```
    2. Install pybind11
        ```bash
        pip install "pybind11[global]"
        ```
    3. Prevent Python from adding user-level packages
        ```bash
        set PYTHONNOUSERSITE=true
        ```
    4. Upgrade pip
        ```bash
        C:\tmp\prophesee\py3venv\Scripts\python -m pip install pip --upgrade 
        ```
    5. Install OpenEB requirements
        ``` bash
        C:\tmp\prophesee\py3venv\Scripts\python -m pip install -r <openeb>\utils\python\requirements_openeb.txt
        ```
        - Replace `<openeb>` with your path

<p align="right"><a href="#installation-guide">(back to table of contents)</a></p>

## 5. Compilation using CMake
> **NOTE: ITS OK IF YOU CAN'T GET THIS DONE. JUST SKIP TO THE NEXT SECTION**

### 5.1 Compiling
1. Open cmd:
    1. CD into `<openeb>` 
        ```cmd
        cd "<openeb>"
        ```
    2. Create new folder
        ```cmd
        mkdir build
        ```
    3. CD new folder
        ```cmd
        cd build
        ```
2. Open PS:
    1. Install libtiff
        ```powershell
        <vcpkg>\vcpkg.exe install libtiff:x64-windows
        ```
    2. Install boost
		```powershell
        <vcpkg>\vcpkg.exe install boost-program-options:x64-windows boost-timer:x64-windows boost-chrono:x64-windows boost-thread:x64-windows
        ```
	3. Install pybind
        ```powershell
        <vcpkg>\vcpkg.exe install pybind11:x64-windows
        ```
    4. Install tiff
        ```powershell
        <vcpkg>\vcpkg.exe install tiff:x64-windows
        ```
    5. CD back into `<openeb>\build`
        ```powershell
        cd "<openeb>\build"
        ```
    6. Build with CMake
        ```powershell
        cmake .. -A x64 -DCMAKE_BUILD_TYPE=Release -DCMAKE_TOOLCHAIN_FILE="<OPENEB_SRC_DIR>\cmake\toolchains\vcpkg.cmake" -DVCPKG_DIRECTORY="<VCPKG_SRC_DIR>" -DBUILD_TESTING=OFF
        ```
		- **Note**: Ensure to change the paths in command
	7. Build CMake
        ```powershell
        cmake --build . --config Release --parallel 4
        ```

### 5.2 Deploying
1. Run CMD as admin
    1. CD into build folder
        ```cmd
        cd "<openeb>\build"
        ```
	2. Deploy: 
        ```cmd
        cmake .. -A x64 -DCMAKE_TOOLCHAIN_FILE=<OPENEB_SRC_DIR>\cmake\toolchains\vcpkg.cmake -DVCPKG_DIRECTORY=<VCPKG_SRC_DIR> -DCMAKE_INSTALL_PREFIX=<OPENEB_INSTALL_DIR> -DPYTHON3_SITE_PACKAGES=<PYTHON3_PACKAGES_INSTALL_DIR> -DBUILD_TESTING=OFF
        ```
		- Notes:
			- Ensure to change the paths in command
			- `<PYTHON3_PACKAGES_INSTALL_DIR>` can be found by running PS:
                ```cmd
                FindPython3
                ```
### 5.3 Environment variables
- **Note**: If the variable exists add a semicolon to the path
1. Add `<openeb>\bin` to PATH
    ```text
    <openeb>\bin
    ```
2. Edit MV_HAL_PLUGIN_PATH to 
    ```text
    C:\Program Files\Prophesee\lib\metavision\hal\plugins
    ```
3. Edit HDF5_PLUGIN_PATH to
    ```text
    <openeb>\lib\hdf5\plugin
    ```
4. PYTHONPATH to
    ```text
    <PYTHON3_PACKAGES_INSTALL_DIR>
    ```
<p align="right"><a href="#installation-guide">(back to table of contents)</a></p>

## 6. Compilation using Visual Studio 
- **Notes**: 
    - If CMake compiling failed delete the build folder
    - If you are getting errors:
        1. cd into <vcpkg> and enter: git pull & git update
            ```cmd
            cd "<vcpkg>"
            ```
        2. Pull & Update
            ```bash
            git pull
            .\vcpkg update
            ```
### 6.1 Build Visual Studio 17 2022
1. Open CMD
2. CD into `<openeb>`
    ```cmd
    cd "<openeb>"
    ```
3. Create build folder
    ```cmd
    mkdir build
    ```
4. CD into new folder
    ```cmd
    cd build
    ```
5. Use CMake to build
    ```cmd
    cmake .. -G "Visual Studio 17 2022" -A x64 -DCMAKE_BUILD_TYPE=Release -DCMAKE_TOOLCHAIN_FILE="<openeb>\cmake\toolchains\vcpkg.cmake" -DVCPKG_DIRECTORY="<vcpkg>" -DBUILD_TESTING=OFF
    ``` 
    - **Note: ensure to change `<vcpkg>` & `<openeb>`**
- **Errors**: 
    1. Error 1: HDF5 Targets Already Defined
        - Location: 
            ```text
            build/vcpkg_installed/x64-windows/share/hdf5/hdf5-targets.cmake:42
            ```
  		- Message:
            ```text 
            Some (but not all) targets in this export set were already defined.
            Targets Defined: hdf5::h5diff
            ```
		- Fix 
            1. cd into `<openeb>` 
                    ```cmd 
                    cd <openeb>
                    ```
            2. Enter:
                ```cmd 
                findstr /s /n /i "HDF5" *.txt *.cmake
                ```
            3. Review the files and line numbers returned by the command to locate where HDF5 is being loaded more than once.
	2. Error 2: pybind11 Could Not Be Found
        - DCMAKE_TOOLCHAIN_FILE needs to be an absolute path
        - Message:
            ```text
            "CMake Error at cmake/custom_functions/python3.cmake:178 (find_package): By not providing "Findpybind11.cmake" in CMAKE_MODULE_PATH this project has asked CMake to find a package configuration file provided by "pybind11", but CMake did not find one." 
            ```
        - Fix:
            1. CD into `<vcpkg>`
                ```cmd
                cd "<vcpkg>"
                ```
            2. Enter:
                ```cmd
                .\vcpkg install pybind11:x64-windows
                ```
            3. If you get this error:
                ```text
                "CMake Error at C:/Program Files/Prophesee/third_party/share/hdf5/hdf5-config.cmake:25(message):
  	    		File or directory C:/Program Files/Prophesee/third_party/tools/hdf5 referenced by variable HDF5_TOOLS_DIR does not exist !"
                ```
                - If not present skip section
            4. Install hdf5 into `<vcpkg>`
                ```cmd
                .\vcpkg install hdf5:x64-windows
                ```

### 6.2 Setup
1. Open metavision.sln in Visual Studio
	- Mine was located at: 
        ```text 
        C:\Users\CBUREN\Documents\GitHub\openeb\build\metavision.sln
        ```
2. At the top near the green play button at the top select `RELEASE` from the drop down menu and hit the play button. 
3. Go to Solution Explorer right click `ALL_BUILD` and hit build and then do the same for `INSTALL`

### 6.3 Change environment variables
1. Append to PATH
    ```text
    C:\Program Files\Prophesee\bin
    ```
2. Append to MV_HAL_PLUGIN_PATH 
    ```text
    C:\Program Files\Prophesee\lib\metavision\hal\plugins
    ```
3. Append to HDF5_PLUGIN_PATH
    ```text
    C:\Program Files\Prophesee\lib\hdf5\plugin
    ```

<p align="right"><a href="#installation-guide">(back to table of contents)</a></p>

## 7. Drivers
1. Install: wdi-simple.exe
    - https://kdrive.infomaniak.com/app/share/975517/cb164518-e68f-49fd-a6a1-eea693783bd2/preview/unknown/90270
2. Run CMD as admin 
3. CD into downloads folder
    ```cmd
    cd ~/Downloads
    ```
4. Enter: 
    ```cmd
    wdi-simple.exe -n "EVK" -m "Prophesee" -v 0x04b4 -p 0x00f4
    wdi-simple.exe -n "EVK" -m "Prophesee" -v 0x04b4 -p 0x00f5
    wdi-simple.exe -n "EVK" -m "Prophesee" -v 0x04b4 -p 0x00f3
    ```

<p align="right"><a href="#installation-guide">(back to table of contents)</a></p>

## 8. Running
1. Add new folder in `<openeb>` called datasets
    ```cmd
    cd "<openeb>"
    mkdir datasets
    ```
2. CD into `<openeb>\build`
    ```cmd
    cd "<openeb>\build"
    ```
3. Build
    ```cmd
    cmake .. -A x64 -DCMAKE_BUILD_TYPE=Release -DCMAKE_TOOLCHAIN_FILE="<openeb>\cmake\toolchains\vcpkg.cmake" -DVCPKG_DIRECTORY="<vcpkg>" -DBUILD_TESTING=OFF
    ```
4. Enter
    ```cmd
    cmake --build . --config Release --parallel 4
    ```

<p align="right"><a href="#installation-guide">(back to table of contents)</a></p>
