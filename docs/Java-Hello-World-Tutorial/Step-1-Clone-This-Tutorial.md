# Clone This Tutorial

The very first step to any programming project is getting version control set up. Check out [this guide](//TODO) if you are not familiar with why version control is so important or you do not understand any of the terms. In this section, we will clone this tutorial project from GitHub to our local computer, pull any new changes, checkout a new branch to develop on, make our first commit on this new branch, and then push our changes back to GitHub.

## Cloning The Project

The source code for this guide that you are reading is actually contained in a [GitHub repository](https://github.com/JHS-Viking-Robotics/Java-Hello-World), just like the code we use for the official competition robot. When we want to use or edit the code, we first need to ```clone``` the repository to our local machine. To do this, we will need to copy down the remote project URL on the main page for our ```GitHub``` repository.

Go to the [JHS-Viking-Robotics/Java-Hello-World](https://github.com/JHS-Viking-Robotics/Java-Hello-World) project on ```GitHub``` and click the big green ```Code``` button. Then, copy the URL that begins with ```https://``` (alternatively, check out SSH access for quicker access in the future). The URL should look like this:

//TODO: Add link for setting up SSH access, as well as a section in Learn-Git.md

```URL
https://github.com/JHS-Viking-Robotics/Java-Hello-World.git
```

Now, we can tell ```Sourcetree``` to clone the repository to a local folder. In ```Sourcetree```, select ```File >> New >> Clone from URL```. Paste the URL you just found, and choose a new local folder to store the repository inside of.

//TODO: Add a tip layout.
It is usually a good idea to keep a ```Code``` or ```Coding-Projects``` folder for storing your ```GitHub``` repositories.

You should now have a local folder called ```Java-Hello-World```, and you should be able to see the ```git``` history in ```Sourcetree```.

## Pull New Changes

We know that we have the most current version of the ```repository``` because we just ```cloned``` it, but in the future we will want to ```Pull``` new changes so that we are always working on up-to-date code. To do this, we simply click on ```Pull```. This will ```fetch``` new changes, and then ```merge``` them into our local branch. For this tutorial, go ahead and just click ```Fetch```, and select ```Fetch and store all tags locally```.

If you are working on the same remote branch as someone else, you may end up with a ```merge conflict```. This happens when there is more than one new commit, and ```Sourcetree``` doesn't know which to use. Head on over to the [Learn Git](//TODO:-add-git-guide-link) guide for more help.

## Checkout A New Development Branch

We now have a local copy of our remote GitHub repository, and we want to create a new ```branch``` for keeping track of our work. Our ```Java-Hello-World``` project currently has a ```main``` branch and a ```development``` branch which contains new proposed changes. For this tutorial, we will create a new branch on our local computer called ```tutorial/<your-name-here>```.

//TODO: Add picture of git branch structure and a tip
To create a new branch, we first need to select a parent commit for our branch. Remember, a ```git``` project is like a really big tree with lots of big branches that keep growing outward, and like a tree branch, our ```git``` branch needs to be connected to the original tree somewhere.

In ```Sourcetree```, double click the commit that has a colored tag labelled ```Start-Here```. We just ```checked out``` the version of the code with that tag, so any of the files that we see are in the state that they were in when the author ```committed``` the changes. Now, select ```Branch``` at the top, enter ```tutorial/<your-name-here>``` for the name, and click ```Create Branch```. We just created a new ```branch```, which is connected to the rest of the project by the commit we checked out earlier. This becomes more important [later on](#merge-changes), but for now it is just important to understand that our new branch is a separate line of changes than the other branches, and later on we can combine those changes together.

## Commit New Changes

We have a local copy of our remote GitHub repository, and we have a local branch which we can make changes to. Now we can start making changes. Go ahead and delete the following files from your project:

```plaintext
src/main/java/frc/robot/commands/ExampleCommand.java
src/main/java/frc/robot/subsystems/ExampleSubsystem.java
```

Now delete the following lines of code from ```src/main/java/frc/robot/RobotContainer.java```

```java
import frc.robot.commands.ExampleCommand;
import frc.robot.subsystems.ExampleSubsystem;
```

```java
  private final ExampleSubsystem m_exampleSubsystem = new ExampleSubsystem();

  private final ExampleCommand m_autoCommand = new ExampleCommand(m_exampleSubsystem);
```

```java
    return m_autoCommand;
```

VS Code will not be happy about that last one, but for now we will ignore the warnings. We removed the example command and subsystem from our project, so we can ```commit``` our new changes to our ```branch```. For reference, a ```commit``` is basically a snapshot of our project at one point in time. To create a new ```commit```, click ```Commit``` at the top of the ```Sourcetree``` window. ```Sourcetree``` notices anytime you change a file, and it will list these changes in ```Unstaged Files```. Stage the following files by double clicking them:

```plaintext
src/main/java/frc/robot/commands/ExampleCommand.java
src/main/java/frc/robot/subsystems/ExampleSubsystem.java
src/main/java/frc/robot/RobotContainer.java
```

Now, ```Sourcetree``` knows that you actually intend to record these changes.

//TODO: Add a tip here.
If for some reason you changed a file without knowing it, or do not want to include some change on your commit, you can always leave the ```file```, the ```hunk```, or the ```line``` unstaged and ```Sourcetree``` will ignore it this time.

Now that ```Sourcetree``` knows what changes to remember, we need to write a quick message to explain to other developers what our commit is doing. For now, we can use the following message:

```plaintext
Delete example command and subsystem

New FRC Java projects contain exmple code for reference, but we do not need these right now.

Remove the class files and their references in RobotContainer.java
```

Now when someone looks at our changes, they can understand not only what we changed, but how and why we changed the code. Check out the [git tutorial](//TODO:-add-git-tutorial-link) for more help writing these commit messages. When you are done, click ```Commit``` at the bottom of the screen.

## Push To GitHub

Eventually, we need to send our changes back up to GitHub so that the rest of our team can see our progress. When we are on our own branch, we can simply hit ```Push``` at the top of ```Sourcetree```, and make sure that our ```branch``` is the only one selected. For this tutorial, you can choose to skip this step if you do not want anyone else to see your work.
