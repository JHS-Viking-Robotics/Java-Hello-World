---
title: "Repl.it Online IDE"
parent: "Install Software"
nav_order: 4
---
# Repl.it Online IDE

No computer? No problem! You can use Repl.it, which is an online IDE that you use in a web browser like Google Chrome. This setup is limited to editing code only, and is only intended to enable development from a Chromebook, tablet, etc.

1. Create a free [GitHub account](https://github.com/)
2. Go to [repl.it](https://repl.it) and sign up with your GitHub account
3. To edit an existing GitHub project, click ```New Repl```, ```Import from GitHub```, and choose the repository you want to edit

If you are creating a new project for Repl.it, create the following ```.replit``` file in the root of the repository to configure the repl.it environment:

```plaintext
language = "java11"
onBoot = "cd ~/ && wget https://github.com/wpilibsuite/roborio-toolchain/releases/download/v2022-1/FRC-2022-Linux-Toolchain-7.3.0.tar.gz | bash -c 'cd ~/ && tar xzf - '"
run = ["./gradlew", "build"]
entrypoint = "README.md"
```

This tells the container to fetch and unpack the build tools from GitHub, and configures the ```run``` button in repl.it to launch the ```gradel build``` process. Now when you click ```run```, it should be able to compile the code!

Head over to the next section [Learn Java](/Learn-Java) to start learning how to use these tools.
