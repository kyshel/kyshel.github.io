---
title: Compile OpenCV3.2 with VS2017 on Windows8.1
---

## Prepare
1. Download [opencv-3.2.0.zip](https://github.com/opencv/opencv/archive/3.2.0.zip)
1. Download [CMake](https://cmake.org/download/)
1. (if add contrib repo) Download [opencv_contrib-3.2.0.zip](https://github.com/opencv/opencv_contrib/archive/3.2.0.zip)


## Compile
1. start cmake-gui.exe, select the opencv source code folder and the folder where binaries will be built (the 2 upper forms of the interface)
1. press the configure button. you will see all the opencv build parameters in the central interface
1. (if add contrib repo)browse the parameters and look for the form called OPENCV_EXTRA_MODULES_PATH (use the search form to focus rapidly on it)
1. (if add contrib repo)complete this OPENCV_EXTRA_MODULES_PATH by the proper pathname to the <opencv_contrib>/modules value using its browse button.
1. press the configure button followed by the generate button (the first time, you will be asked which makefile style to use)
1. build the opencv core with the method you chose 
## Config
1. Right click START BUTTON & Choose `Command Prompt (Admin)`
1. Type `setx -m OPENCV320_INSTALL_DIR D:\opencv3_2\build_manual\install`
1. windows+q, search 'sys env', Click Environment variables -> System Variables, find Path and click edit
1. At the end of the line, add: `%OPENCV320_INSTALL_DIR%\x64\vc15\bin`
1. VS2017 -> File, New, Project...(Create an empty project) 
1. Click Property Manager on right side vs panel -> right click Debug|x64 -> Add New Property Sheet -> Change name to `opencv3_2_manual_x64.props` -> Add
1. Double click **opencv3_2_manual_x64** -> VC++ Directories -> Include Directories -> Click the dropdown arrow to the right then click Edit
1. Click the new folder icon and type: `$(OPENCV320_INSTALL_DIR)\include`
1. In the same properties window,  -> Linker -> General -> Click the dropdown arrow to the right then click Edit
1. Click the new folder icon and type: `$(OPENCV320_INSTALL_DIR)\x64\vc15\lib`
1. In the same properties window, -> Linker > Input 
1. Choose Additional Dependencies and add [Dependencies List](#Dependencies List)
1. Hit OK to close the properties window.
1. Replace the code in your main file with the following:
``` c++
#include <opencv2/core/core.hpp>
#include <opencv2/highgui/highgui.hpp>
#include <iostream>

using namespace cv;
using namespace std;

int main() {

	Mat image;
	image = imread("Path to image", IMREAD_COLOR); // Read the file

	namedWindow("Display window", WINDOW_AUTOSIZE); // Create a window for display.

	if (!image.data) // Check for invalid input
		{
			cout << "Could not open or find the image" << std::endl;
		} else {		// Image is good!

			imshow("Display window", image); // Show our image inside it.
		}

	waitKey(0);
	return 0;
}
```
The project is now ready! Change "Path to Image" to a valid image path, and run it.


## Dependencies List
opencv_surface_matching320d.lib
opencv_text320d.lib
opencv_tracking320d.lib
opencv_video320d.lib
opencv_videoio320d.lib
opencv_videostab320d.lib
opencv_xfeatures2d320d.lib
opencv_ximgproc320d.lib
opencv_xobjdetect320d.lib
opencv_xphoto320d.lib
opencv_aruco320d.lib
opencv_bgsegm320d.lib
opencv_bioinspired320d.lib
opencv_calib3d320d.lib
opencv_ccalib320d.lib
opencv_core320d.lib
opencv_datasets320d.lib
opencv_dnn320d.lib
opencv_dpm320d.lib
opencv_face320d.lib
opencv_features2d320d.lib
opencv_flann320d.lib
opencv_fuzzy320d.lib
opencv_highgui320d.lib
opencv_imgcodecs320d.lib
opencv_imgproc320d.lib
opencv_line_descriptor320d.lib
opencv_ml320d.lib
opencv_objdetect320d.lib
opencv_optflow320d.lib
opencv_phase_unwrapping320d.lib
opencv_photo320d.lib
opencv_plot320d.lib
opencv_reg320d.lib
opencv_rgbd320d.lib
opencv_saliency320d.lib
opencv_shape320d.lib
opencv_stereo320d.lib
opencv_stitching320d.lib
opencv_structured_light320d.lib
opencv_superres320d.lib
