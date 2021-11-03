---
title: "Windows 10 IDE and DS"
parent: "Install Software"
nav_order: 1
---
# Windows 10 IDE Setup

These setup instructions will allow you to write and deploy code from your Linux computer, as well as operate and talk to the robot and use the Phoenix Tuner for CTRE equipment.

> **_Note:_**  You might need to set up Windows Subsystem Linux for parts of this configuration to work. Run ```wsl --install``` in Powershell if you do not have this already. See [Microsoft's Documentation](https://docs.microsoft.com/en-us/windows/wsl/install) for help.

## git

---

```git``` is a Version Control Systems (VCS). It keeps track of file changes and versions, and it enables collaboration in programming teams. Throughout this guide, we will be using ```git``` on our local computer with Sourcetree as a GUI, and we will use GitHub for hosting our code in the cloud.

1. Create a free [GitHub account](https://github.com/)
2. [Install ```git```](https://git-scm.com/downloads) on your computer. During installation, select ```Override default branch name``` to ```main``` (read more [here](https://github.com/github/renaming/) about this decision). Leave all other settings as default

## Sourcetree

---

```git``` itself is a command line program, which means you have to use a Windows CMD shell or macOS/Linux Bash shell to access it. Using the command line might look something like this:

```bash
git clone https://github.com/JHS-Viking-Robotics/Java-Hello-World.git
git add *
git commit -m "Add some files"
git push
```

However, ```git``` is sometimes tricky to use and most people are not comfortable on a command line. The company Atlassian made a really neat GUI for ```git``` called ```Sourcetree``` which we will use instead.

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

## WPI Library

---

We now have ```git``` and ```Sourcetree``` installed. We need to install the ```WPI Library```, which is the official codebase for FRC projects (read more [on the official docs](https://docs.wpilib.org/en/stable/docs/software/what-is-wpilib.html)). This installation will come with a copy of Microsoft's ```Visual Studio Code```  pre-configured with ```Java```, ```Gradle```, ```Maven```, and other tools for ```Java``` development.

1. Go to the [WPILib GitHub page](https://github.com/wpilibsuite/allwpilib/releases), scroll down, and download the newest installer labeled ```WPILib_Windows64-<VERSION>.iso```
2. Right click the ```.iso``` file, choose ```Mount```, and launch the ```WPILibInstaller.exe``` installer
3. Choose ```Download VS Code for Single Install``` and follow the rest of the instructions

## CTRE Phoenix Library

---

The final piece of software to install is the ```Phoenix Library``` from [Cross The Road Electronics](https://docs.ctre-phoenix.com/en/stable/index.html). This library allows us to easily integrate CTRE hardware like Talon SRX motor controllers or Pigeon IMU's into our code. While we could do all of these things with just the WPI Library, it is much simpler and recommended to use the official ```Phoenix Library```.

1. Go to the [CTRE GitHub page](https://github.com/CrossTheRoadElec/Phoenix-Releases/releases) and download the newest installer labeled ```CTRE_Phoenix_Framework_<VERSION>.exe```
2. Run the installer, and make sure that ```Phoenix Tuner``` and ```C++/Java``` are selected

## Next Steps

---

You should now have Git, Sourcetree, the WPI Library, and the CTRE Phoenix Library installed on your computer.

Head over to the next section [Learn Java](/Learn-Java) to start learning how to use these tools.
