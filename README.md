# postwrite-toolbox
Community tools for Figura 1.0 development.

## Testing (not developing) Figura 1.0
Before starting, you'll need:
- A JDK 21 installation somewhere
  - You'll be prompted to enter the location of the JDK (the value that `JAVA_HOME` would have); this is _not_ the location of `java`, but the folder that contains `bin`, `conf`, `include` etc
  - Hint: if you have a system OpenJDK installation on Linux, do `realpath "$(which javac)"` to get an idea of where to look
- `git`, the version control software

### on Linux and maybe MacOS
A setup script exists to guide you through the installation process. Download and run the `setup` file from this repository.
- If necessary, give yourself execute permissions on the script (`chmod +x setup`), then run it (`./setup`)
- When asked to clone a fork, choose no (`n`)
  - **These instructions are just for users who want to _test_ Figura 1.0.** If you want to contribute, see the section below.
- Enter a path to clone to. (the default is the current directory)
  - Wait a bit for all the repositories to clone.
- Enter the path to the JDK you found earlier.
  - The script will output the version of `javac` it found. Make sure this is correct.
- Wait a bit longer for all the repositories to be set up and compiled for the first time.
- Enter `figura-client` and do `./gradlew runClient` to start the game!

### on other operating systems (aka Windows)
You'll have to set up the repositories yourself.

Run these commands in the directory you want to work in:
```sh
git clone https://github.com/FiguraMC/figura-client.git
git clone https://github.com/FiguraMC/figura-core.git
git clone https://github.com/FiguraMC/figura-translations.git
git clone https://github.com/FiguraMC/figura-cobalt.git
git clone https://github.com/FiguraMC/figura-molang.git
git clone https://github.com/FiguraMC/memory-tracker.git
```

Set the `JAVA_HOME` environment variable to the location of your JDK 21 installation.
Here's how to do it in PowerShell (if you're on windows and not using powershell you should switch to powershell tbh)
```ps1
$env:JAVA_HOME="A:\path\to\jdk\21"
```

Then, you'll have to do the setup process for each project manually.
This involves entering each project (with `cd` or similar) and running `./gradlew pubTML` (short for `./gradlew publishToMavenLocal`).
Set up the projects in this order:
1. `memory-tracker`
2. `figura-translations`
3. `figura-molang`
4. Cobalt has some interesting interdependency issues that mean we have to bootstrap its build plugin by itself first:
    1. enter `figura-cobalt/cobalt-build-tools`
    2. use the `gradlew` from the parent directory to publish just the plugin by itself: `../gradlew pubTML`
    3. return to the parent directory (`cd ..`, you should now be in `figura-cobalt`)
    4. `./gradlew pubTML` the whole `figura-cobalt` project
5. `figura-core` 

Then, to check that everything worked without running the game, get Gradle to configure the `figura-client` project with `./gradlew help`.

After following those steps, you should be good to go! Run `./gradlew runClient` in `figura-client` to start the game.
