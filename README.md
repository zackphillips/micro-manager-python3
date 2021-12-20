# NOTE: Depricated Repository!
Please use [pycro-manager](https://github.com/micro-manager/pycro-manager) instead. This repository is no longer maintained.

# micro-manager-py36
Files for running Micro-manager 2.0 with python 3.X support on Windows

# Compiling 
0. Download [Anaconda](https://www.anaconda.com/download/) (or your preffered python distro)

1. Set PYTHONPATH environment variable to the location you installed anadonda

```
set PYTHONPATH=%HOMEPATH%\anaconda\
```

2. Check out micro-manager source from [https://github.com/micro-manager](https://github.com/micro-manager).

3. Download [Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/)

4. Open Micro-manager.sln in Visual Studio Community 2017

5. Right-click MMCorePy_wrap in the Solution Explorer and click "Properties". Select "VC++ Directories" in the right pane and append this line to the Include Directories box:

```
;%PYTHONPATH%\include;%PYTHONPATH%\lib\site-packages\numpy\core\include
```

Then, append this line to the "Library Directories" box:
```
;%PYTHONPATH%\libs;
```

6. Fix common erroes:
- If you get an error:
```
Error	C1083	Cannot open include file: 'inttypes.h': No such file or directory
```
Open "pyport.h" and replace "inttypes.h" with "stdint.h". This means modifying a python header file to accomidate VS2017. I don't really like this solution, but it does seem to work, and does not mess up your python install.

- If you get an error: 
```
Error	LNK1104	cannot open file 'python37_d.lib
```

Ensure you are building the release build. Note that the lib and include settings you updated above will need to be updated for the release configuration.

7. Close the property browser, right-click MMCorePy_wrap, and click build.

8. Open the output directory and copy the files [TODO] to a recent Micro-manager nightly install, such as:
```
C:\Program Files\Micro-Manager-2.0beta
```

# Installation
0. Install [Micro-manager 2.0](https://micro-manager.org/wiki/Version_2.0) (a recent nightly should work)

1. Download or clone the files inside the MMCorePy directory into your micro-manager directory (Usually ```C:\Program Files\Micro-Manager2.0beta\``` or similar)

2. Ensure you are running Python 3.6 (Using other verisons will NOT work as I have built this against python 3.6)

3. Run ``` pip install numpy ``` and optionally ``` pip install jupyter ``` if you will be using the notebook interface

4. In jupyter (or your own .py file), run the following:

```python
# Append MM directory to path
import sys
sys.path.append('C:\\Users\\Zack\\Desktop\\Micro-Manager-2.0beta')
system_cfg_file = '\\path\\to\\your\\MM\\cfg\\file.cfg'

# For most devices it is unnecessary to change to the MM direcotry prior to importing, but in some cases (such as the pco.de driver), it is required.

prev_dir = os.getcwd()
os.chdir(mm_directory) # MUST change to micro-manager directory for method to work
import MMCorePy

# Get micro-manager controller object
mmc = MMCorePy.CMMCore()

# Load system configuration (loads all devices)
mmc.loadSystemConfiguration(system_cfg_file)
os.chdir(prev_dir)

# Success!
print("Micro-manager was loaded sucessfully!")
```

