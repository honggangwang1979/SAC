## Introduction
	1. This package provide interface to run supported apps without first installing them. The only thing the users need to do is to "git clone https://github.com/honggangwang1979/SAC.git"

	2. There are three parties in this game: the Client app, the account server, and the simulation server. The users only need to run the client app under this SAC repository. 

	    2.1 For the linux (ubuntu) users, just go to the ubuntu_20.04 folder and run, for example, : 

           ./SAC_CLI FDS HPC.fds Jame SAC_TEST L 1 
where FDS is the app name, HPC.fds is the input file to the app of FDS, Jame is the user name, SAC_TEST is the access code (free for test), L is the priority level (H,M,L), 1 is the number of cores the user requests (for FDS, this number needs to be same as the number of meshes).

	    The exe file, SAC_CLI, is compatible with higher versions of ubuntu (>20.04), may not work for lower versions of ubuntu (<20.04)

	Note: 
	    1) In case of you batch runnings, you may want to supress the ssh prompts which may ask you to answer "yes/no", you can add the following two lines to your /etc/ssh/ssh_config file just following the host * line:

		UserKnownHostsFile /dev/null
		StrictHostKeyChecking no

	    Remember to comment them out when you are done with the batch runnings.

	    2) Since the whole data tar ball may be quite big, you may not want to download it frequently. In this case, you can set a time interval between 2 downloads of the whole data tar ball by, for example : 
		export SAC_REQTAR_INT=10
	    This means the tar ball will be downloaded every 10s of the wall time. 

	    2.2 For the windows users, there are some dependencies you need to install: 

              	1) python 3.12.4 for windows, see: https://www.python.org/downloads/
	      	2) git for windows, see: https://git-scm.com/download/win
		3) launch git bash by : "C:\Program Files\Git\bin\sh.exe" --login
                   or in powershell, use: & 'C:\Program Files\Git\bin\sh.exe' --login
		4) go to the window_10 folder under SAC and run SAC_CLI in the same way as for the linux version.

	3. A user will need a user name and access code to run the client app. There is a free test access code, SAC_TEST, which can be used by anyone for free. However, the longest simulation time of a case for a free account is limited to a value depending on the dynamic server resource. In general, A free account can expect 100 core hours for a single simulation. 

## Supported Applications

	1. FDS/GFDS 
		This version of FDS (cpu based) and GFDS (GPU based) is checked out from : 
			https://github.com/honggangwang1979/FDS_GPU_PUBLISH.git

	2. SWMM
		This version of SWMM is checked out from: 
			https://github.com/USEPA/Stormwater-Management-Model.git
		SWMM supports openmp which by default use maximum number of available cores (?)

         To build the SWMM 5.2 engine library and its command line executable using CMake and the Microsoft Visual Studio C compiler on Windows (as shown in readme.txt):

	1). Open a console window and navigate to the directory where this Readme file resides (which should have 'src' as a sub-directory underneath it).

	2). Issue the following commands:
     		mkdir build
     		cd build

	3). Then enter the following CMake commands:
     		cmake -G <compiler> .. -A <platform>
     		cmake --build . --config Release

	where <compiler> is the name of the Visual Studio compiler being used in double quotes (e.g., "Visual Studio 15 2017" or "Visual Studio 16 2019") and <platform> is Win32 for a 32-bit build or x64 for a 64-bit build.
	The resulting engine DLL (swmm5.dll) and command line executable (runswmm.exe) will appear in the build\Release directory.

	For other platforms, such as Linux or MacOS, Step 3 can be replaced with:
      		cmake ..
      		cmake --build .

	The resulting shared object library (libswmm5.so) and command line executable (runswmm) will appear in the build directory. 


	3. CAFLOOD

               This is a 2D Cellular Automata flood analysis model from : https://github.com/FluiditLtd/caddies-caflood.git

		1) First check out : git clone https://git.exeter.ac.uk/caddies/caddies-caflood.git
		2) cd to /caddies-caflood/apps
		3) then check out: git clone https://github.com/FluiditLtd/caddies-caflood.git
		
		To build, first cd caddies-api, then:

	Step 1: generate the Makefiles:

      		Step 1.a:  For normal compilation:
   			<home-dir>$ cmake -B ./bin 

      		Step 1.b:  for debug purpose, normal compilation:
   			<home-dir>$ cmake -DCMAKE_BUILD_TYPE=Debug -B ./bin

      		Step 1.c:  for openmp multi-threading compilation:
   			<home-dir>$ cmake -DCAAPI_SPECIFIC_IMPL_DIR=$(pwd)/impls/square-cell/vn-neighbours/1-levels/openmp -B build-openmp

      		Step 1.d:  for opencl compilation:  
     			1) install nvidia-phc-sdk-24.1 (optional)
     			2) for OpenCL library and header files:
			 <home-dir>$ sudo apt install opencl-headers ocl-icd-opencl-dev -y
     			3) for GL/gl.h not found error: 
       			  <home-dir>$ sudo apt-get install mesa-common-dev -y
     			4) generate Makefiles: 
       			 <home-dir>$ cmake -DOPENCL_INCLUDE_DIRS=/usr/include -DOPENCL_LIBRARIES=/usr/lib/x86_64-linux-gnu -DCAAPI_OCL_EVENTS=disable -DCAAPI_SPECIFIC_IMPL_DIR=$(pwd)/impls/square-cell/vn-neighbours/1-levels/opencl -B build-opencl

   			Note: in step 1, if errors like "The current CMakeCache.txt directory /notebooks/caddies-api/bin/CMakeCache.txt is different than the directory /root/upwork/caddies-api/bin where CMakeCache.txt was created. This may result in binaries being created in the wrong place." occur, do the following things:
   				<home-dir>$ rm ./bin/CMakeCache.txt
    					or 
   				<home-dir>$ rm -r ./build-openmp

   
   	Step 2: build caflood:

      		Step 2.a:  For normal compilation:
      		Step 2.b:  for debug purpose, normal compilation:
   			<home-dir>& cmake --build ./bin

      		Step 2.c:  for openmp multi-threading compilation:
   			<home-dir>& cmake --build ./build-openmp

		Step 2.d:  for opencl compilation:  
        		1) add  "-L/usr/lib/x86_64-linux-gnu -lOpenCL" to the end of the following 3 files:
            				~/SWMM_Caflood/caddies-api/build-opencl/CMakeFiles/clinfo.dir/link.txt
            				~/SWMM_Caflood/caddies-api/build-opencl/CMakeFiles/convertCA2HPP.dir/link.txt
            				~/SWMM_Caflood/caddies-api/build-opencl/apps/caddies-caflood/CMakeFiles/caflood.dir/link.txt

        		2) build the caflood:
   			<home-dir>& cmake --build ./build-opencl

        		Note: If there are errors related to "game_of_life", just neglect these errors.
               

	4. AERMOD
           This version is based on https://www.epa.gov/scram/air-quality-dispersion-modeling-preferred-and-recommended-models#aermod, we added the support for openmp which may have 10 to 40% higher speed than the origial version. 

	5. FireFoam
           This version is based on https://github.com/fireFoam-dev/fireFoam-v1912

           How to install FireFoam:

		Step 1: Install dependencies of OpenFoam (see https://www.openfoam.com/documentation/system-requirements )

			sudo apt-get update

			sudo apt-get install build-essential flex bison cmake zlib1g-dev libboost-system-dev libboost-thread-dev libopenmpi-dev openmpi-bin gnuplot libreadline-dev libncurses-dev libxt-dev

			(optional): sudo apt-get install qt4-dev-tools libqt4-dev libqt4-opengl-dev freeglut3-dev libqtwebkit-dev

			sudo apt-get install libscotch-dev libcgal-dev

		Step 2: Install FireFoam:

			We will install ESI/ThirdParty, ESI/OpenFOAM and FM-public/FireFOAM in a case-sensitive drive on a Linux machine at a location /path/to/FM-public-FF/.

			Create the containing directory and enter it.

						mkdir -p /path/to/FM-public-FF
						cd /path/to/FM-public-FF

			Copy the setup script to this location and make it executable. Do not clone the repository yet.

						wget https://raw.githubusercontent.com/fireFoam-dev/fireFoam-v1912/main/fireFoam_setup.bash
						chmod +x fireFoam_setup.bash

			Check the top of the file and adjust options as desired. For this project, the defaults should suffice. If debugging capability is desired (at the expense of computational speed), change the BUILD_TYPE setting to Debug.

			Run the installation script. This can take up to three hours to complete. The & puts the job in the background. Foreground and background processes can be toggled using the commands fg and bg.

						./fireFoam_setup.bash > install_log.txt &

			The downloading of ThirdParty from SourceForge sometimes fails. In this case, simply repeat step 4.

			Occasionally there may be an issue midway through the installation process. The script can be re-run without repeating successfully completed sections by specifying the START_STEP parameter based on the steps in the fireFoam_setup.bash script. See comments in the script for details.

                
           
 
           How to run examples of FireFoam:

		Navigate to the directory where you want to run the tutorials. It should not be inside /path/to/FM-public-FF/.

			mkdir -p /path/to/cases/
			cd /path/to/cases/

		Copy a case to this location, e.g.,
			cp -r /path/to/FM-public-FF/fireFoam-v1912/tutorials/poolfireMcCaffrey .
			cd poolfireMcCaffrey

		Prepare the environment.

			source /path/to/FM-public-FF/OpenFOAM-v1912/etc/bashrc

		Set the number of cores to be used in the simulation in system/decomposeParDict. For cases with film and pyrolysis regions, the same must be done for each region, by setting the number of cores in system/filmRegion/decomposeParDict and system/fuelRegion/decomposeParDict.

		Prepare the case.

			./mesh.sh

			Note: sometime you need to change the first line in mesh.sh from #!bin/sh to #bin/bash due to the different default shells in different linux system
			Note: please check the file name of 0/ph_rgh-orig, in mesh.sh, it may be wrongly writen as 0/ph_rgh.orig

		Either run the job locally, or submit the job to a cluster. For the latter, consider the example submission scripts named run.sh.

		Use other scripts, e.g., postproc.sh and reconstruct.sh, to perform initial analysis of the data.
