---
title: Install Software
nav_order: 2
---
# Install Software

In this section, we will install the following programs on our computer:

- [git](#git) - the world's most popular version control program
- [Sourcetree](#sourcetree) - a user-friendly ```git``` GUI
- [WPI Library](#wpi-library) - the official software library and development environment for FRC
- [CTRE Phoenix Library](#ctre-phoenix-library) - software library for CTRE hardware

## git

---

```git``` is one of the world's most popular and most powerful Version Control System (VCS). ```git``` is awesome for keeping track of file history and enabling collaboration between teammates. Check out their [official website](https://git-scm.com/) for more info.

For this tutorial, we will be using ```git``` on our local computer with Sourcetree as a GUI, and we will use GitHub for hosting our code in the cloud.

### Windows/macOS/Linux

1. Create a free [GitHub account](https://github.com/)
2. [Install ```git```](https://git-scm.com/downloads) on your computer. During installation, select ```Override default branch name``` to ```main``` (read more [here](https://github.com/github/renaming/) about this decision). Leave all other settings as default

## Sourcetree

---

Awesome, you should now have ```git``` installed on your computer! ```git``` itself is a command line program, which means you have to use a Windows CMD shell or macOS/Linux Bash shell to access it. Using the command line might look something like this:

```bash
git clone https://github.com/JHS-Viking-Robotics/Java-Hello-World.git
git add *
git commit -m "Add some files"
git push
```

If you are used to this, feel free to use ```git``` this way! However, most people are not comfortable on a command line, which is why Atlassian made a really neat GUI for ```git``` called ```Sourcetree``` which we will use instead.

### Windows

1. Download and run the installer for [Sourcetree](https://www.sourcetreeapp.com/) on your computer
2. Skip Bitbucket registration
3. Uncheck Mercurial download (we already have ```git``` installed)
4. Enter the name and email address you used for ```GitHub```
5. Open ```Git Bash``` from the Windows start menu, and paste in the following:

```bash
cd \
&& cat << EOF > ./.gitmessage.txt
# Subject line, try to keep under 50 characters
#
# Why is this commit necessary?
#
# How does this commit fix said problem?
#
# Bullet points are fine for extra paragraphs
# - This is a point
# - Here's another
#
# Address any issues, articles, projects, etc.
# Resolves: #123
# See also: #456, #789
EOF
git config --global commit.template ./.gitmessage.txt \
&& exit
```

### macOS/Linux

1. Download and run the installer for [Sourcetree](https://www.sourcetreeapp.com/) on your computer
2. Skip Bitbucket registration
3. Uncheck Mercurial download (we already have ```git``` installed)
4. Enter the name and email address you used for ```GitHub```
5. In ```Sourcetree```, go to ```Preferences >> Commit``` and copy/paste the text below into the text box

```plaintext
# Subject line, try to keep under 50 characters
#
# Why is this commit necessary?
#
# How does this commit fix said problem?
#
# Bullet points are fine for extra paragraphs
# - This is a point
# - Here's another
#
# Address any issues, articles, projects, etc.
# Resolves: #123
# See also: #456, #789
```

## WPI Library

---

We now have ```git``` and ```Sourcetree``` installed. We need to install the ```WPI Library```, which is the official codebase for FRC projects (read more [on the official docs](https://docs.wpilib.org/en/stable/docs/software/what-is-wpilib.html)). This installation will come with a copy of Microsoft's ```Visual Studio Code```  pre-configured with ```Java```, ```Gradle```, ```Maven```, and other tools for ```Java``` development.

### Windows

1. Go to the [WPILib GitHub page](https://github.com/wpilibsuite/allwpilib/releases), scroll down, and download the newest installer labeled ```WPILib_Windows64-<VERSION>.iso```
2. Right click the ```.iso``` file, choose ```Mount```, and launch the ```WPILibInstaller.exe``` installer
3. Choose ```Download VS Code for Single Install``` and follow the rest of the instructions

### macOS/Linux

1. Go to the [WPILib GitHub page](https://github.com/wpilibsuite/allwpilib/releases), scroll down, and download the newest installer labeled ```WPILib_macOS-<VERSION>.dmg``` or ```WPILib_Linux-<VERSION>.tar.gz```, and launch the installer
2. Choose ```Download VS Code for Single Install``` and follow the rest of the instructions

## CTRE Phoenix Library

---

The final piece of software to install is the ```Phoenix Library``` from [Cross The Road Electronics](https://docs.ctre-phoenix.com/en/stable/index.html). This library allows us to easily integrate CTRE hardware like Talon SRX motor controllers or Pigeon IMU's into our code. While we could do all of these things with just the WPI Library, it is much simpler and recommended to use the official ```Phoenix Library```.

Note that the Windows version will also install the ```Phoenix Tuner``` and other really useful tools not available for macOS/Linux. These tools are not necessary for writing code, but are important for testing and debugging and will be available on the team's driver station laptop.

### Windows

1. Go to the [CTRE GitHub page](https://github.com/CrossTheRoadElec/Phoenix-Releases/releases) and download the newest installer labeled ```CTRE_Phoenix_Framework_<VERSION>.exe```
2. Run the installer, and make sure that ```Phoenix Tuner``` and ```C++/Java``` are selected

### macOS/Linux

1. Go to the [CTRE GitHub page](https://github.com/CrossTheRoadElec/Phoenix-Releases/releases) and download the newest installer labeled ```CTRE_Phoenix_FRC_macOS_<VERSION>.zip``` or ```CTRE_Phoenix_FRC_Linux_<VERSION>.zip```
2. Open the folder and follow the instructions in the ```README.txt``` file. This involves copying the ```vendordeps``` and ```maven``` folders into ```~/wpilib/2021```, and the contents of ```Robotbuilder```  into the corresponding folder in ```~/wpilib/2021```

## Next Steps

---

You should now have Git, Sourcetree, the WPI Library, and the CTRE Phoenix Library installed on your computer.

Head over to the next section [Learn Java](https://github.com/JHS-Viking-Robotics/Java-Hello-World/wiki/Learn-Java) to start learning how to use these tools.
