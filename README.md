# micro-manager-py36
Files for running Micro-manager 2.0 with python 3.X support on Windows

# Compiling 
0. Download [Anaconda](https://www.anaconda.com/download/) (or your preffered python distro)

1. Check out micro-manager source from [https://github.com/micro-manager](https://github.com/micro-manager).

2. Download [Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/)

3. Open Micro-manager.sln in Visual Studio Community 2017

4. Right-click MMCorePy_wrap in the Solution Explorer and click "Properties". Select "VC++ Directories" in the right pane and append this line to the Include Directories box:

```
;C:\Users\zack\Anaconda3\include;C:\Users\zack\Anaconda3\lib\site-packages\numpy\core\include
```

Then, append this line to the "Library Directories" box:
```
C:\Users\zack\Anaconda3\libs;
```

5. If you get an error related to "inttypes.h", open "pyport.h" and replace "inttypes.h" with "stdint.h". This means modifying a python header file to accomidate VS2017. I don't really like this solution, but it does seem to work, and does not mess up your python install.

6. Close the property browser, right-click MMCorePy_wrap, and click build.

7. Open the output directory and copy the files [TODO] to a recent Micro-manager nightly install, such as:
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

