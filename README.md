# Daz To Unreal Bridge
A Daz Studio Plugin based on Daz Bridge Library, allowing transfer of Daz Studio characters and
props to the Unreal Engine.

# Table of Contents
1. About the Bridge
2. How to Install
3. How to Use
4. How to Build
5. How to QA Test
6. How to Develop


# 1. About the Bridge
This is a refactored version of the original DazToUnreal Bridge using the Daz Bridge Library as a foundation. Using the Bridge Library allows it to share source code and features with other bridges such as the refactored DazToUnity and DazToBlender bridges. This will improve development time and quality of all bridges.

The DazToUnreal Bridge consists of two parts: a Daz Studio plugin which exports assets to an Unreal Engine Project and an Unreal Engine Plugin which contains shaders, scripts and other resources to help recreate the look of the original Daz Studio asset in the Unreal Engine.


# 2. How to Install
### Windows ###
On the [Release page](https://github.com/daz3d/DazToUnreal/releases), the **dzunrealbridge.dll** can be downloaded, unzipped and copied into the Daz Studio plugins folder (example: "\Daz 3D\Applications\64-bit\DAZ 3D\DAZStudio4\plugins").
Then select one of the `UE***.zip` files and unzip the contents into a folder named `plugins` in your Unreal Project folder. If the plugins folder does not exist, create one. You should then have a folder named DazToUnreal inside the plugins folder,
> example: \MyUnrealProject\plugins\DazToUnreal\

### Mac ###
On the [Release page](https://github.com/daz3d/DazToUnreal/releases), download, unzip and copy the **libdzunrealbridge.dylib** into the Daz Studio plugins folder.
Then select one of the `UE***-mac.zip` files to download, depending on your version of Unreal. Then unzip and copy the contents into a folder named `plugins` in your Unreal Project folder. If the plugins folder does not exist, create one. You should then have a folder named DazToUnreal inside the plugins folder,
> example: \myMacUnrealProject\plugins\DazToUnreal\

Daz Studio can then be started, and the bridge can be accessed from the main menu: File-> Send To-> Daz To Unreal.


# 3. How to Use
1. Open your character in Daz Studio.
2. At the same time you have your Unreal Project open for an automated process
3. Make sure any clothing or hair is parented to the main body.
4. From the main menu, select File -> Send To -> Daz to Unreal.
5. A dialog will pop up, choose the desired name for the desintation folder inside the Unreal Contents Folder, and choose what type of conversion you wish to do, “Static Mesh” (no skeleton), “Skeletal Mesh” (Character or with joints), “Animation” (character must already be transferred), or "Environment".
6. To enable Morphs or Subdivision levels, click the CheckBox to Enable that option, then click the "Choose Morphs" or "Choose Subdivisions" button to configure your selections.
7. Click Accept, then wait for a dialog popup to notify you when to switch to the Unreal Engine Window.


# 4. How to Build
Requirements: Daz Studio 4.5+ SDK, Qt 4.8.1, Autodesk Fbx SDK, Pixar OpenSubdiv Library, CMake, C++ development environment

Download or clone the DazToUnreal github repository to your local machine. The Daz Bridge Library is linked as a git submodule to the DazBridge repository. Depending on your git client, you may have to use `git submodule init` and `git submodule update` to properly clone the Daz Bridge Library.

Use CMake to configure the project files. Daz Bridge Library will be automatically configured to static-link with DazToUnreal. If using the CMake gui, you will be prompted for folder paths to dependencies: Daz SDK, Qt 4.8.1, Fbx SDK and OpenSubdiv during the Configure process.


# 5. How to QA Test
The Test folder contains a `QA Manual Test Cases.md` document with instructions for performaing manual tests.  The Test folder also contains subfolders for UnitTests, TestCases and Results. To run automated Test Cases, run Daz Studio and load the `Test/testcases/test_runner.dsa` script, configure the sIncludePath on line 4, then execute the script. Results will be written to report files stored in the `Test/Reports` subfolder.

To run UnitTests, you must first build special Debug versions of the DazToUnreal and DzBridge Static sub-projects with Visual Studio configured for C++ Code Generation: Enable C++ Exceptions: Yes with SEH Exceptions (/EHa). This enables the memory exception handling features which are used during null pointer argument tests of the UnitTests. Once the special Debug version of DazToUnreal dll is built and installed, run Daz Studio and load the `Test/UnitTests/RunUnitTests.dsa` script. Configure the sIncludePath and sOutputPath on lines 4 and 5, then execute the script. Several UI dialog prompts will appear on screen as part of the UnitTests of their related functions. Just click OK or Cancel to advance through them. Results will be written to report files stored in the `Test/Reports` subfolder.

For more information on running QA test scripts and writing your own test scripts, please refer to `How To Use QA Test Scripts.md` and `QA Script Documentation and Examples.dsa` which are located in the Daz Bridge Library repository: https://github.com/daz3d/DazBridgeUtils.

Special Note: The QA Report Files generated by the UnitTest and TestCase scripts have been designed and formatted so that the QA Reports will only change when there is a change in a test result.  This allows Github to conveniently track the history of test results with source-code changes, and allows developers and QA testers to notified by Github or their git client when there are any changes and the exact test that changed its result.

# 6. How to Modify and Develop
The Daz Studio Plugin source code is contained in the `DazStudioPlugin` folder. The Unreal Engine Plugin source code and resources are available in the `UnrealPlugin` folder.

The DazToUnreal Bridge uses a branch of the Daz Bridge Library which is modified to use the "DzUnrealNS" namespace. This ensures that there are no C++ Namespace collisions when other plugins based on the Daz Bridge Library are also loaded in Daz Studio. In order to link and share C++ classes between this plugin and the Daz Bridge Library, a custom `CPP_PLUGIN_DEFINITION()` macro is used instead of the standard DZ_PLUGIN_DEFINITION macro and usual .DEF file. NOTE: Use of the DZ_PLUGIN_DEFINITION macro and DEF file use will disable C++ class export in the Visual Studio compiler.
