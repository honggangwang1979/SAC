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
