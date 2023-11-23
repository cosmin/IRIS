# IRIS

IRIS is a solution created by EA to identify video footage that could potentially cause photosensitive epileptic risks. IRIS is not intended to guarantee, certify or otherwise validate visual content's compliance with legal, regulatory or other requirements. 

The cross-platform library analyses video content and checks for flashes and spatial patterns that could be harmful for people with photosensitive epilepsy, or cause discomfort in viewers.

IRIS detects the following issues:
- Luminance flashes.
- Red saturation flashes.
- Spatial patterns (with clear contrast and regularity between pattern components).

According to IRIS' photosensitive epilepsy criteria:
- For luminance and red saturation, whenever there are more than 3 flashes per second in any given second of the video, it is marked as a flash failure.
- For luminance and red saturation, whenever there are between 2-3 flashes per second in any given 5 consecutive seconds, it is marked as an extended flash failure.
- For spatial patterns, whenever there is a harmful pattern present for half a second or more, it is marked as a pattern failure.

The library outputs the results of the analysis in a CSV file  with the following columns:
- Frame: the frame index at this moment of the video.
- Time Stamp: video time stamp.
- Flash Area Luminance: the area proportion of the luminance change.
- Average Luminance Difference Accumulation: this is the value that’s used to determine whether a new luminance transition has occurred or not. If it’s equal or greater than 0.1 (or equal or lower than -0.1) a new transition would have occurred.
- Flash Area Red: the area proportion of the red change between this frame and the one before.
- Average Red Difference Accumulation: this is the value that’s used to determine whether a new red transition has occurred or not. If it’s equal or greater than 20 (or equal or lower than -20) a new transition would have occurred.
- Luminance Transitions: the amount of luminance transitions that have occurred from this moment up to one second before.
- Red Transitions: the amount of red transitions that have occurred from this moment up to one second before.
- Luminance Extended Fail Count: the amount of frames that were luminance flashing between 4 and 6 transitions from this moment up to 5 seconds before. 
- Red Extended Fail Count: the amount of frames that were red flashing between 4 and 6 transitions from this moment up to 5 seconds before.
- Pattern Area: area of the bounding rect. of the pattern.
- Pattern Detected Lines: number of detected pattern lines.
- Luminance Frame Result: frame luminance status.
  - 0: pass.
  - 1: flash warning (transitions have surpassed the 2 flashes, 4 transitions, per second threshold).
  - 2: extended flash failure.
  - 3: flash failure.
- Red Frame Result: frame red status.
  - 0: pass.
  - 1: flash warning (transitions have surpassed the 2 flashes, 4 transitions, per second threshold).
  - 2: extended flash failure.
  - 3: flash failure.
- Pattern Frame Result: pattern frame status.
  - 0: pass.
  - 1: pattern failure.


## Example app
The example app project provides a console application in which to run IRIS. The application expects videos to be set in a "TestVideos" directory to analyse and generate the results in the "Results" directory. Both these directories can be automatically generated by the example app by executing it.

### Optional arguments
When running IRIS in the example app, the following command line arguments can be passed:
- `-j`: when passing true/1 generates the results in a json file. 
- `-v`: the path to a video can be specified as to. 
- `-p`: enabled/disable the pattern detection (true/1 or false/0).
- `-l`: specify the luminance type for the luminance calculations (CD || RELATIVE), relative luminance is the default and standard.


## Configuration 
The [appsettings.json](config/appsettings.json) is a file where values used by IRIS are defined and can be modified to alter the execution of the analysis. These default values are configured to detect photosensitive content based on publicly available guidelines. IRIS is not intended to guarantee, certify or otherwise validate video content’s photosensitivity compliance. Modifying IRIS’s default values should be done at your own risk, understanding that doing so may impact IRIS’s results and its ability to detect photosensitivity issues.  


## How to build
IRIS uses by default CMake, Ninja and the vcpkg package manager for handling dependencies. All the dependencies are specified in the [vcpk.json](vcpkg.json) file. If you want CMake to automatically run vcpkg and compile all dependencies for you, you must set your VCPKG root folder as an environment variable called "VCPKG_ROOT" or add it as a variable in the CMakePresets.json. 

IRIS uses cmake presets to build with different configurations  e.g.: 

`>cmake --preset windows-release`   
`>cmake --build --preset windows-release`

More presets can be added to the CMakePresets.json file or defined in a CMakeUserPresets.json file.

Build options:
- BUILD_EXAMPLE_APP: build Iris example console application
- BUILD_SHARED_LIBS: build iris as a shared library
- EXPORT_IRIS: export and install library
- BUILD_TESTS: build library unit tests
- BUILD_COVERAGE: build code coverage (only available for Linux)



## CONTRIBUTING
Before you can contribute, EA must have a Contributor License Agreement (CLA) on file that has been signed by each contributor. You can sign [here](https://electronicarts.na1.echosign.com/public/esignWidget?wid=CBFCIBAA3AAABLblqZhByHRvZqmltGtliuExmuV-WNzlaJGPhbSRg2ufuPsM3P0QmILZjLpkGslg24-UJtek*).