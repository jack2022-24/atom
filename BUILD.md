If you're hitting a bug in Atom or just want to experiment with adding a feature to the core of the system, you'll want to run Atom in Dev Mode with access to a local copy of the Atom source.

Fork the atom/atom repository
Follow the GitHub Help instructions on how to fork a repo.

Cloning and bootstrapping
Once you've set up your fork of the atom/atom repository, you can clone it to your local machine:

git clone git@github.com:your-username/atom.git
From there, you can navigate into the directory where you've cloned the Atom source code and run the bootstrap script to install all the required dependencies:

cd where-you-cloned-atom
script\bootstrap
Running in Development Mode
Once you have a local copy of Atom cloned and bootstrapped, you can then run Atom in Development Mode. But first, if you cloned Atom to somewhere other than %USERPROFILE%\github\atom you will need to set the ATOM_DEV_RESOURCE_PATH environment variable to point to the folder in which you cloned Atom. To run Atom in Dev Mode, use the --dev parameter from the terminal:

atom --dev path-to-open
Note: If the atom command does not respond in the terminal, then try atom-dev or atom-beta. The suffix depends upon the particular source code that was cloned.

There are a couple benefits of running Atom in Dev Mode:

When the ATOM_DEV_RESOURCE_PATH environment variable is set correctly, Atom is run using the source code from your local atom/atom repository. This means that you don't have to run script\build every time you change code. Just restart Atom 👍
Packages that exist in %USERPROFILE%\.atom\dev\packages are loaded instead of packages of the same name normally loaded from other locations. This means that you can have development versions of packages you use loaded but easily go back to the stable versions by launching without Dev Mode.
Packages that contain stylesheets, such as syntax themes, will have those stylesheets automatically reloaded by the dev-live-reload package. This does not live reload JavaScript or CoffeeScript files — you'll need to reload the window (window:reload) to see changes to those.
Running Atom Core Tests Locally
In order to run Atom Core tests from the terminal, first be certain to set the ATOM_DEV_RESOURCE_PATH environment variable as mentioned above and then:

cd path-to-your-local-atom-repo
atom --test spec
Building
In order to build Atom from source, you need to have a number of other requirements and take additional steps.

Node.js 10.12 or later (the architecture of node available to the build system will determine whether you build 32-bit or 64-bit Atom)
npm 6.12 or later (run npm install -g npm)
Python v2.6.x, v2.7.x, or v3.5+
Get Python from the Microsoft Store, or
Download Python from https://www.python.org/downloads/.
For Python 2, be sure to install in the default location, or check "Add Python 2.x to PATH" before installing.
For Python 3, check "Add Python 3.x to PATH", or change the install path to [Your_Drive_Letter]:\Python37 e.g. C:\Python37, (even if your version of Python 3 isn't 3.7, that's one place where the scripts will look.)
If python isn't found by the bootstrap script, create a symbolic link to the directory containing python.exe using e.g.: mklink /d %SystemDrive%\Python27 D:\elsewhere\Python27(Links should be set at either %SystemDrive%\Python27 or %SystemDrive%\Python37, regardless of what version of Python you actually have.)
C++ build tools:

Option 1: windows-build-tools - From an elevated Powershell window (right click and "run as Administrator") do: npm install --global windows-build-tools@4 to install
Option 2: Visual C++ Build Tools 2015 or 2017
Option 3: Visual Studio 2015 or 2017 (Community Edition or better)
Also ensure that:

The default installation folder is chosen so the build tools can find it
If using Visual Studio make sure Visual C++ support is selected/installed
If using Visual C++ Build Tools make sure a Windows SDK (Windows 8 SDK or Windows 10 SDK) is selected/installed
A git command is in your path
Set the GYP_MSVS_VERSION environment variable to the Visual Studio/Build Tools version (2015 or 2017.) e.g. [Environment]::SetEnvironmentVariable("GYP_MSVS_VERSION", "2015", "User") in PowerShell (or set it in Windows advanced system settings).
Instructions
script\build
To also install the newly built application, use script\build --create-windows-installer and launch one of the generated installers.

script\build Options
--code-sign: signs the application with the GitHub certificate specified in $WIN_P12KEY_URL.
--compress-artifacts: zips the generated application as out\atom-windows.zip.
--create-windows-installer: creates an .exe and two .nupkg packages in the out directory.
--install[=dir]: installs the application in ${dir}\Atom\app-dev; ${dir} defaults to %LOCALAPPDATA%.
Troubleshooting
Common Errors
node is not recognized

If you just installed Node.js, you'll need to restart Command Prompt before the node command is available on your path.
msbuild.exe failed with exit code: 1

If using Visual Studio, ensure you have the Visual C++ component installed. Go into Add/Remove Programs, select Visual Studio, press Modify, and then check the Visual C++ box.
If using Visual C++ Build Tools, ensure you have the Windows 8 SDK or Windows 10 SDK component installed. Go into Add/Remove Programs, select Visual C++ Build Tools, press Modify and then check the "Windows 8 SDK" or "Windows 10 SDK" box.
script\build stops with no error or warning shortly after displaying the versions of node, npm and Python

Make sure that the path where you have checked out Atom does not include a space. For example, use C:\atom instead of C:\my stuff\atom.
Try moving the repository to C:\atom. Most likely, the path is too long. See issue #2200.
error MSB4025: The project file could not be loaded. Invalid character in the given encoding.

This can occur because your home directory (%USERPROFILE%) has non-ASCII characters in it. This is a bug in gyp which is used to build native Node.js modules and there is no known workaround.
https://github.com/TooTallNate/node-gyp/issues/297
https://bugs.chromium.org/p/gyp/issues/detail?id=393
'node_modules\.bin\npm' is not recognized as an internal or external command, operable program or batch file.

This occurs if the previous build left things in a bad state. Run script\clean and then script\build again.
script\build stops at installing runas with Failed at the runas@x.y.z install script.

See the next item.
error MSB8020: The build tools for Visual Studio 201? (Platform Toolset = 'v1?0') cannot be found.

Try setting the GYP_MSVS_VERSION environment variable to 2015 or 2017 depending on what version of Visual Studio/Build Tools is installed and then script\clean followed by script\build (re-open the Command Prompt if you set the variable using the GUI).
'node-gyp' is not recognized as an internal or external command, operable program or batch file.

Try running npm install -g node-gyp, and run script\build again.
Other node-gyp errors on first build attempt, even though the right Node.js and Python versions are installed.

Do try the build command one more time as experience shows it often works on second try in many cases.
Windows build error reports
If all else fails, use this search to get a list of reports about build errors on Windows, and see if yours has already been reported.
If it hasn't, please open a new issue with your Windows version, architecture (x86 or x64), and a text dump of your build output, including the Node.js and Python versions.
