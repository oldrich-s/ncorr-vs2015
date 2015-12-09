[NCORR](http://www.ncorr.com/index.php/c-port) digital image correlation c++ port for Windows using Microsoft Visual Studio C++ 2015.

You can download the Visual Studio 2015 sollution in the [release](https://github.com/czb/ncorr-vs2015/releases) section or you can follow the following description on how to do it from scratch:
 
1. Download and install assets
	1. Install Visual Studio 2015 (update 1)
	2. Install [CMAKE](https://cmake.org/)
	3. Download [NCORR](http://www.ncorr.com/index.php/c-port)
	4. Download [OpenCV for Windows](http://opencv.org/downloads.html)
	5. Download [FFTW for Windows DLLs](http://www.fftw.org/install/windows.html)
	6. Download [suitesparse-metis-for-windows](https://github.com/jlblancoc/suitesparse-metis-for-windows)
2. Compile OpenCV (follow [this tutorial](http://funvision.blogspot.dk/2015/11/install-opencv-visual-studio-2015.html))
	1. Run Cmake GUI
	2. `Where is the source code` set e.g. to `{path}\opencv\sources`
	3. `Where to build the binaries` set e.g. to `{path}\opencv\build\x86\vc14`
	4. In `Configure` set `Visual Studio 14 2015` (or x64) and hit `Finish`
	5. Hit `Generate`
	6. Open `{path}\opencv\build\x86\vc14\OpenCV.sln` in VS2015
	7. Set to `Release`
	8. Build the solution
	9. In `Solution Explorer` right click `INSTALL` project and choose `Build`
3. Compile FFTW to *.lib
	1. Open CMD from `Start menu / Visual Studio 2015 / Visual Studio Tools / Windows Desktop Command Prompts / VS2015 x86 x64 Cross Tools Command Prompt` or x64 version for x64 builds
	2. Go to `{path}\fftw\` folder
	2. Execute `lib.exe /def:libfftw3-3.def`
4. Compile Suitesparse
	1. Run Cmake GUI
	2. `Where is the source code` set e.g. to `{path}\suitesparse`
	3. `Where to build the binaries` set e.g. to `{path}\suitesparse\build`
	4. In `Configure` set `Visual Studio 14 2015` and hit `Finish`
	5. Hit `Generate`
	6. Open `{path}\build\SuiteSparseProject.sln` in VS2015
	7. Set to `Release`
	8. Build the solution
	9. In `Solution Explorer` right click `INSTALL` project and choose `Build`
6. NCORR Visual Studio 2015 Solution
	1. Create new solution `Visual C++ / Win32 / Win32 Console Application`
	2. Turn off `Precompiled header` and `Security Development Lifecycle`
	3. Copy all header files from NCORR (`{path}\ncorr\include`) to the project folder
	4. Copy all source files from NCORR (`{path}\ncorr\src`) to the project folder
	5. In `Solution Explorer` choose `Show all files`
	6. Select all copied files, right click and `Include into project`
	7. Open `Project / Properties`
		1. In `C/C++ / Additional Include Directories` add
			* `$(SolutionDir)..\fftw`
			* `$(SolutionDir)..\opencv\build\x86\vc14\install\include\`
			* `$(SolutionDir)..\suitesparse\build\install\include\`
		2. In `Linker / Additional Library Directories` add
			* `$(SolutionDir)..\fftw`
			* `$(SolutionDir)..\opencv\build\x86\vc14\install\x86\vc14\lib`
			* `$(SolutionDir)..\suitesparse\build\install\lib\`
			* `$(SolutionDir)..\suitesparse\build\install\lib64\`
			* `$(SolutionDir)..\suitesparse\build\install\lib\lapack_blas_windows\`
		3. In `Linker / Input / Additional Dependencies` add

		```
		libblas.lib
		liblapack.lib
		opencv_core300.lib
		opencv_highgui300.lib
		opencv_imgcodecs300.lib
		opencv_imgproc300.lib
		opencv_videoio300.lib
		libamd.lib
		libbtf.lib
		libcamd.lib
		libccolamd.lib
		libcolamd.lib
		libcxsparse.lib
		libcholmod.lib
		libklu.lib
		libldl.lib
		libspqr.lib
		libumfpack.lib
		suitesparseconfig.lib
		metis.lib
		libfftw3-3.lib
		```
		
		4. In `Array2D.h`
			* On line 38 replace `#include "SuiteSparseQR.hpp"` with `#include "suitesparse/SuiteSparseQR.hpp"`
			* On line 307 remove `= PAD::ZEROS`
			* On line 3222 replace `typename std::enable_if<std::is_same<typename container_traits<T_container2>::nonconst_container, typename base_region<T_container>::nonconst_container>::value, base_region<T_container>&>::type base_region<T_container>::operator=(const base_region<T_container2> &reg) {`
			* with
			* `auto base_region<T_container>::operator=(const base_region<T_container2>& reg) -> typename std::enable_if<std::is_same<typename container_traits<T_container2>::nonconst_container, nonconst_container>::value, base_region&>::type {`
		5. Now you should be able to compile the solution. Remember that you have to redo the procedure for every of `debug`, `release`, `x86` and `x64` options.
