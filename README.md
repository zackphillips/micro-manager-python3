# micro-manager-py36
Files for running Micro-manager 2.0 with python 3.6 support

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

