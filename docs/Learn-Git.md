# Learn The Basics Of git

In this section we will learn about version control and discuss the importance of managing code. We will look at the VCS program Git and a GUI for it called Sourcetree. We will register an account on GitHub, a website which hosts remote Git repositories for collaboration. Finally, we will look at templates for common Git actions such as commits, pull requests, and tagging.

## Register With GitHub

---

The first step to using any of these tools is signing up for a ```GitHub``` account over on the [GitHub website](https://github.com/). Their website is also an excellent source of tutorials and cheat sheets if you are confused with ```git``` at any point.

## Intro To Version Control

---

Version control is super important when working on projects of any size, and especially when working with a team. At some point you have probably seen (and probably created) a project that has a bunch of files like this:

```plaintext
My-Awesome-Presentation.ppt
My-Awesome-Presentation-Revised.ppt
My-Awesome-Presentation-Final.ppt
My-Awesome-Presentation-Resubmitted.ppt
My-Awesome-Presentation-Last-One.ppt
My-Awesome-Presentation-This-Is-Getting-Silly.ppt
My-Notes.docx
My-Notes-Updated.docx
My-Notes-Final.docx
```

Version control programs address a few problems that you might notice about this setup:

1. We need a way to keep track of file versions, and go back to an older version if necessary
2. We need to keep track of the same versions for multiple files that go together
3. We need a way for other people to work on the project, even several years into the future
4. We also need a way for other people to access the files and work on them remotely
5. We need to keep track of multiple new changes simultaneously

You can check out [this guide](https://www.atlassian.com/git/tutorials/what-is-version-control) by Atlassian (the company behind ```Sourcetree```) for some more information about why we might need source control in our lives.

## Using git

---

```git``` is a command line program, so we use ```Sourcetree``` as a pretty GUI instead of typing a bunch of commands. Many online sources will use the command line, but each command that you see should directly correspond to a button in ```Sourcetree```.

```git``` is [famously very tricky to learn](https://xkcd.com/1597/), but it is really useful once you understand what it is you are doing. The key is patience and practice. Check out some of these guides for using ```git```:

- [WPILib git Tutorial](https://docs.wpilib.org/en/stable/docs/software/basic-programming/git-getting-started.html) - great intro to ```git```, definitely read this first
- [Video by Colt Steele](https://youtu.be/USjZcfj8yxE) - 15 minute crash course on ```git```
- [Sourcetree documentation](https://confluence.atlassian.com/get-started-with-sourcetree) - understanding the ```Sourcetree``` interface
- [Interactive tutorial](https://learngitbranching.js.org/) - understanding the ```git``` branching model
- [Blog post by Marco Chiappetta](https://blog.hipolabs.com/how-to-work-in-a-team-version-control-and-git-923dfec2ac3b) - using source control with a team
- [Blog post by Chris Beams](https://chris.beams.io/posts/git-commit/) - guide to useful commit messages

## Next Steps

---

By now, you should know the basics of Java programming, and you should have an idea of how to use Git for version control. You should also have a GitHub account and Sourcetree set up on your computer.

Now for the fun part! Head over to the next section [Hello World](https://github.com/JHS-Viking-Robotics/Java-Hello-World/wiki/Hello-World) to build your own ```Hello-World``` project to drive a robot!
