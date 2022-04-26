# Installing GraalVM Enterprise Edition and Setup

This guide will walk you through the process of installing GraalVM (in this example, GraalVM Enterprise Edition 22). 

For an optimal lab experience, please install the following tools: `git`, `curl`, `unzip`, Docker or Podman, Apache Maven and your favorite IDE (optional).

## Downloading GraalVM Enterprise Edition 22

It is important to note there are two versions of `GraalVM`, the Enterprise Edition (supported and with better performance) and the Community Edition (free and Open Source).

Both of these can be downloaded from the GraalVM website, but these instructions will detail download and installation of the Enterprise Edition.

The Enterprise Edition download can be found at: [https://www.oracle.com/downloads/graalvm-downloads.html?selected_tab=22](https://www.oracle.com/downloads/graalvm-downloads.html?selected_tab=22)

You should see the following when you access the Download Page:

![GraalVM Download Page](./images/graalvm-download.png)

Click on the `Current Release` tab on the page to display the download links for the current version. Choose the Release Version, Java Version, OS and Architecture then click on the "Download" icon(s).

You will be asked to accept the Oracle License Agreement. 

![GraalVM Download Page - Select Current Version, JDK and OS](./images/graalvm-ee-3.png)

You will be prompted to login to your Oracle account. If you have an Oracle OTN account then use this identity to download GraalVM. If not, create one by hitting the "Create Account" button at the bottom of the page:

![GraalVM Download Page - Login](./images/oracle-acct.png)

After downloading the Core (highlighted in the steps above), you can download any of the following optional modules/languages for GraalVM. All of these are available from the download site:

Please download the following optional module:

* Oracle GraalVM Enterprise Edition Native Image module

Optional modules for GraalVM: 

* GraalVM LLVM Toolchain module
* Ideal Graph Visualizer
* Oracle GraalVM Enterprise Edition Ruby Language module
* Oracle GraalVM Enterprise Edition Python Language module
* Oracle GraalVM Enterprise Edition WebAssembly Language module
* Oracle GraalVM Enterprise Edition Java on Truffle
* GraalVM R Language module* (GitHub download)

## Installing GraalVM

The full instructions on setup can be found [here](https://docs.oracle.com/en/graalvm/enterprise/22/docs/getting-started/#install-graalvm-enterprise).

A quick summary of the steps outlined in the link above are:

1. Download the latest version of GraalVM EE for your OS from the [Download page](https://www.oracle.com/downloads/graalvm-downloads.html)
    - See earlier section, Downloading GraalVM
2. You will need to download the following modules:
    - Oracle GraalVM Enterprise Edition for JDK17 (Version 22)
    - Oracle GraalVM Enterprise Edition Native Image based on JDK17 (Version 22)
    
3. The following modules are optional, based on your use case:
    - GraalVM LLVM Toolchain module (Version 22)
    - Oracle GraalVM Enterprise Edition Python for JDK17 (Version 22)
    - Oracle GraalVM Enterprise Edition Ruby for JDK17 (Version 22)
4. Install the downloaded GraalVM EE core. This is a `tar.gz` file. You will need to extract it to a location that works for you and that will then become the root of your install. For example: `/opt/graalvm-ee-java17-22.1.0`
4. Update your environment in order to add a `GRAALVM_HOME` and add this to the path. You can do this by updating your shell startup script in `~/.bash_rc` or `~/.zshrc`:

    ~~~ {.bash}
    # Using the JDK17 version.
    export GRAALVM_HOME=/opt/graalvm-ee-java17-22.1.0
    export JAVA_HOME="${GRAALVM_HOME}"
    # Add the bin dir of GraalVM to your path
    export PATH="${GRAALVM_HOME}/bin:$PATH"
    ~~~


#### Installing on Mac

If you install on MacOS, a number of details vary from the general Linux approach.  See full instructions [here](https://docs.oracle.com/en/graalvm/enterprise/22/docs/getting-started/installation-macos/).

First, if you direct `GRAALVM_HOME` to `/Library/Java/JavaVirtualMachines/`, MacOS picks the JDK as default-jdk (as long it’s the newest JDK in the directory). You may not want this outcome. 

Secondly, the path that the core GraalVM extracts differs from that of Linux. For Mac, it adds a few sub-directories and the directory that will be used as your `GRAALVM_HOME` will be relative to the directory where you extracted:

    <install-dir>/graalvm-ee-java17-22.1.0/Contents/Home

<!--The third major difference is that on some versions of OSX, the `GateKeeper` service on OSX will block you from running `GraalVM` as it is not a signed application / binary. We can work around this challenge in a number of ways:

* Disabling `GateKeeper`, running and then reenabling
* On latter versions of OSX, you will be prompted that GraalVM is blocked and `GateKeeper` will prompt you to add an exception
* You can allow access by using the `spctl` tool:

    ~~~ {.bash}
    # Run from the terminal
    # This assumes that you install /Library/Java/...
    # Please update if you choose to install in another location
    $ sudo spctl add $GRAALVM/bin/java
    ~~~
-->    

Also, if you are using MacOS Catalina and later you may need to remove the quarantine attribute from the bits before you can use them. To do this, run the following:

```
$ sudo xattr -r -d com.apple.quarantine /path/to/GRAALVM_HOME
```

On MacOS, when you extract the downloaded GraalVM (the core download, not the language modules), the path is slightly different to Linux. For example:

~~~ {.bash}
# Using the JDK17 version.
export GRAALVM_HOME=~/bin/graal/graalvm-ee-java17-22.1.0/Contents/Home
export JAVA_HOME=${GRAALVM_HOME}
# Add the bin dir of GraalVM to your path, so you will be able to reference the exes
export PATH=${GRAALVM_HOME}/bin:$PATH
~~~

### Installing on Windows

Please review the [this article](https://medium.com/graalvm/using-graalvm-and-native-image-on-windows-10-9954dc071311) for the most straightforward instructions on Windows installation. 

### Testing Your Installation

Create a new shell (or source your shell script) and type the following:

~~~ {.bash}
$ java -version
java version "17.0.2" 2022-01-18 LTS
Java(TM) SE Runtime Environment GraalVM EE 22.1.0 (build 17.0.2+8-LTS-jvmci-22.0-b05)
Java HotSpot(TM) 64-Bit Server VM GraalVM EE 22.1.0 (build 17.0.2+8-LTS-jvmci-22.0-b05, mixed mode, sharing)
~~~

> #### Mac - GateKeeper
> On OSX you may see a pop-up warning you that the `java` is not a signed application and can not be run. This is not something to worry about, it is just OSX being over protective :)
> See the earlier section of this page on the Mac specific steps on installing GraalVM (you need to let the `GateKeeper` service know that GraalVm is safe to be run).

Did you see the same output as above? If so, then it worked!

Test the `gu` utility:

~~~ {.bash}
$ gu --help
~~~

Did that work? If so, you now have access to the utility that allows you to install various additional modules available with GraalVM.

### Installing Language Modules

We use the `gu` tool to install the additional modules that you can use with GraalVM, such as the various language runtimes. In this section, we will step through the installation process.

All language / component installations are executed using the `gu` tool distributed with GraalVM. The latest instructions on using this tool can be found at:

[gu Tool - Enterprise Edition](https://docs.oracle.com/en/graalvm/enterprise/22/docs/reference-manual/graalvm-updater/)

#### Installing Native Image

Full instructions can be found [here](https://docs.oracle.com/en/graalvm/enterprise/22/docs/getting-started/#native-images).

Ensure you have the prerequisite libs available on your system: `glibc-devel, zlib-devel`
    - On linux these can be installed using your package manager
    - On OSX you will need to ensure that `xcode` is installed: `xcode-select --install`

There are two methods for installing the modules:

1. Install directly using `gu install`
2. Install using `gu install` with a downloaded module   

**Method 1: Direct Installation**

You may need root permissions to install the modules.

```
$ gu install native-image
Error: Insufficient privileges for administration of the GraalVM installation. You need to become "root" user in order to perform administrative tasks on GraalVM.
NOTE: depending on your operating system, you may need to use OS tools to install or uninstall GraalVM components.
```
Determine the path to your GraalVM installation because you'll need to provide the full path to the `gu` utility:

```
$ which java
~/.sdkman/candidates/java/22.1.0-r17-ee/bin/java
```

Install the `native-image` module as `root` using the full path to the `gu` utility.  In this example, we're using SDKMAN:

```
$ sudo ~/.s~/.sdkman/candidates/java/22.1.0-r17-ee/bin/gu install native-image
[sudo] password for user:
Downloading: Artifacts catalog from gds.oracle.com
Downloading: Release index file from oca.opensource.oracle.com
Skipping ULN EE channels, no username provided.
Downloading: Component catalog from www.graalvm.org
Processing Component: Native Image
Enter your download token and press ENTER, or press ENTER to generate a new download token.
Enter a valid download token:
Please provide an email address. Please review Oracles Privacy Policy (https://www.oracle.com/legal/privacy/privacy-policy.html).
Enter a valid email address: <your-email-address>
Obtained download token "REQ4MTMyMzc3MUE0ODI2MUUwNTM2RTE2MDAwQUE2Mjk6YjIyxxxxx".

The license has been sent to <your-email-address>. Accept it and then press ENTER to continue.

Downloading: Component native-image: Native Image from gds.oracle.com
Installing new component: Native Image (org.graalvm.native-image, version 22.1.0)
```

You will be sent and email asking to verify your email:

![](images/oracle-download-email-0.png)

Scroll to the bottom and click on the **Verify email** button:

![](images/oracle-download-email.png)

After you verify your email, you'll receive confirmation.  You may now continue to download the `native-image` module:

![](//wsl$/OracleLinux_8_5/home/sseighma/code/Installing-GraalVM-Enterprise-Edition/images/oracle-download-token.png)



Confirm the module installed properly:

```
$ native-image --version
GraalVM 22.1.0 Java 17 EE (Java Version 17.0.2+8-LTS-jvmci-22.0-b05)
```
You can also view which modules have been installed by executing `gu list`:

```
$ gu list
ComponentId              Version             Component name                Stability                     Origin
---------------------------------------------------------------------------------------------------------------------------------
graalvm                  22.1.0            GraalVM Core                  Supported
js                       22.1.0            Graal.js                      Experimental
native-image             22.1.0            Native Image                  Experimental
```

In addition, you can view which modules are available to install executing `gu available`:

```
$ gu available
Downloading: Release index file from oca.opensource.oracle.com
Downloading: Component catalog for GraalVM Enterprise Edition 22.0.0 on jdk17 from oca.opensource.oracle.com
Downloading: Component catalog for GraalVM Enterprise Edition 22.1.0 on jdk17 from oca.opensource.oracle.com
Skipping ULN EE channels, no username provided.
Downloading: Component catalog from www.graalvm.org
ComponentId              Version             Component name                Stability                     Origin
---------------------------------------------------------------------------------------------------------------------------------
espresso                 22.1.0            Java on Truffle               Experimental                  oca.opensource.oracle.com
espresso-llvm            22.1.0            Java on Truffle LLVM Java librExperimental                  oca.opensource.oracle.com
llvm-toolchain           22.1.0            LLVM.org toolchain            Experimental                  github.com
native-image             22.1.0            Native Image                  Experimental                  oca.opensource.oracle.com
nodejs                   22.1.0            Graal.nodejs                  Experimental                  oca.opensource.oracle.com
python                   22.1.0            Graal.Python                  Experimental                  oca.opensource.oracle.com
R                        22.1.0            FastR                         Experimental                  github.com
ruby                     22.1.0            TruffleRuby                   Experimental                  oca.opensource.oracle.com
wasm                     22.1.0            GraalWasm                     Experimental                  oca.opensource.oracle.com
```

**Method 2: Install Using a Downloaded Module**

As an option, modules can also be downloaded and installed using `gu`.  Modules are available from the [Download page](https://www.oracle.com/downloads/graalvm-downloads.html). Download the desired modules.

Once again, installation requires `root` permissions and you'll need to provide the full path to the `gu` utility (see instructions above).

```
$  ~/.sdkman/candidates/java/22.1.0-r17-ee/bin/gu install -L <DOWNLOAD-DIR>/native-image-installable-svm-svmee-java17-darwin-amd64-22.1.0.jar
```

Validate the installation:

```
$ native-image --version
GraalVM 22.1.0 Java 17 EE (Java Version 17.0.2+8-LTS-jvmci-22.0-b05)
```

These two methods are applicable for any additional module you wish to install.


#### Installing JS / Node

Nothing to do here, these are available by default.

You can test the `node` and `JS` tooling as follows:

```
$ node --help
$ js --show-version
GraalVM JavaScript (GraalVM EE Native 21.2.0.1)
```

Both of the above commands will make reference to GraalVM and the version. If you have node already installed, you may need to change your path or explicitly specify the path to the GraalVM version of node

#### Installing the llvm-toolchain

A number of the modules require the `llvm-toolchain` in order to work. Regardless of whether you are using the Enterprise or Community Edition, you will need to install the same version.

This is installed simply using the `gu` command:

```
$ gu install llvm-toolchain
```

or

```
$ gu install -L <DOWNLOAD-DIR>/llvm-toolchain-installable-java11-darwin-amd64-21.2.0.jar
```

#### Installing Ruby

Full instructions can be found [here](https://docs.oracle.com/en/graalvm/enterprise/21/docs/getting-started/#ruby).

But the basic steps are:

```
$ gu install ruby
```

or

```
$gu install -L <DOWNLOAD-DIR>/ruby-installable-svm-svmee-java11-linux-amd64-21.2.0.jar
```

Test that your installation works by running ruby:

```
$ which ruby
$ ruby --version
truffleruby 21.2.0.1, like ruby 2.7.3, GraalVM EE Native [x86_64-linux]
```

If you already have Ruby installed, then you will need to adjust your path or use the full path to the ruby binary.

#### Installing Python

Full instructions can be found [here](https://docs.oracle.com/en/graalvm/enterprise/21/docs/getting-started/#python).

But the basic steps are:

```
$ gu install python
```

or

```
$ gu -L install <DOWNLOAD-DIR>/python-installable-svm-svmee-java17-linux-amd64-22.1.0.jar
```

Test that Python is now installed:

```
$ graalpython
Python 3.8.5 (Fri Jul 30 17:40:39 UTC 2021)
[Graal, GraalVM EE, Java 11.0.12] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

#### Installing R

Full instructions can be found [here](https://docs.oracle.com/en/graalvm/enterprise/21/docs/getting-started/#r).

When installing the R language module, it is best to consult the installation page, above, as there are a number of prerequisites that need to be installed.

### Install Visual Studio Code

A number of modules have been developed for Microsoft VSCode that allow for better integration with the GraalVM eco-system.

Of course, you are welcome to use another IDE. When it comes to the polyglot debugging, please make sure that you have the Google Chrome browser installed.

### Install VS Code modules

Please install the following modules:

1. GraalVM Extension Pack [download](https://marketplace.visualstudio.com/items?itemName=oracle-labs-graalvm.graalvm)

This will install all of the individual extensions.

## Docker - Pre-built Docker Images

GraalVM is available on a number of pre-built container images that can be downloaded from GitHub and the Oracle Container Registry.

You can find the Community Edition images here: [here](https://github.com/orgs/graalvm/packages/container/package/graalvm-ce).

GraalVM Enterprise Edition images can be found [here](http://container-registry.oracle.com/) (login using your Oracle ID).


## Upgrading GraalVM

GraalVM includes a command-line utility called GraalVM Updater (`gu`) to install and manage optional GraalVM language runtimes and utilities. As of version 21.0.0, GraalVM Updater has become more efficient, and can be used to update your local GraalVM installation to a newer version or upgrade from a Community to Enterprise Edition.

GraalVM Updater will attempt to download the latest version of either GraalVM Enterprise or GraalVM Community Edition, if available. It will not rewrite the existing installation, but unpack it into a new directory and print out the location path. It will also verify if you have any optional components installed in the current GraalVM installation and update those as well. If the “parent” installation contains a symlink to the current GraalVM installation, that symlink will be updated. If your setup involves some environment variables (e.g., PATH) pointing to a selected GraalVM installation, those variables should be updated manually. *(See note below regarding SDKMAN)*

In the following example, the current GraalVM version is 21.3.0, we'll be upgrading to the latest version (22.1.0):

```
$ sudo gu upgrade
Password:
Downloading: Release index file from oca.opensource.oracle.com
Downloading: Component catalog for GraalVM Enterprise Edition 22.1.0 on jdk17 from oca.opensource.oracle.com
Downloading: Component catalog for GraalVM Enterprise Edition 22.1.0 on jdk17 from oca.opensource.oracle.com
Downloading: Component catalog for GraalVM Enterprise Edition 22.1.0 on jdk17 from oca.opensource.oracle.com
Downloading: Component catalog for GraalVM Enterprise Edition 22.1.0 on jdk17 from oca.opensource.oracle.com
Downloading: Component catalog for GraalVM Enterprise Edition 22.1.0 on jdk17 from oca.opensource.oracle.com
Skipping ULN EE channels, no username provided.
Downloading: Component catalog from www.graalvm.org

=========================================================================
	Preparing to install GraalVM Core version 22.1.0.
	Destination directory: /Library/Java/JavaVirtualMachines/graalvm-ee-java17-22.1.0
=========================================================================
The component(s) GraalVM Core requires to accept the following license: GraalVM Enterprise Edition License
Enter "Y" to confirm and accept all the license(s). Enter "R" to the see license text.
Any other input will abort installation:  **Y**
Downloading: Contents of "GraalVM Enterprise Edition License" from oca.opensource.oracle.com
Please provide an e-mail address to not show the license check again (optional). You may want to check Oracle Privacy Policy (https://www.oracle.com/legal/privacy/privacy-policy.html).
Enter a valid e-mail address: **<your-email-address>**
Downloading: Component core: GraalVM Core  from oca.opensource.oracle.com          ]
Downloading: Component org.graalvm: GraalVM Core  from oca.opensource.oracle.com
Installing GraalVM Core version 22.1.0 to /Library/Java/JavaVirtualMachines/graalvm-ee-java11-22.1.0...
Copying license info from 21.0.0 to 22.1.0.
Warning: Could not migrate GDS settings
Processing Component: Native Image
Processing Component: Graal.Python
Downloading: Release index file from oca.opensource.oracle.com
Downloading: Component catalog for GraalVM Enterprise Edition 22.1.0 on jdk17 from oca.opensource.oracle.com
Downloading: Component catalog for GraalVM Enterprise Edition 22.1.0 on jdk17 from oca.opensource.oracle.com
Processing Component: GraalWasm
Processing Component: FastR
Processing Component: TruffleRuby
Processing Component: Java on Truffle
Processing Component: LLVM.org toolchain
Additional Components are required:
    LLVM.org toolchain (org.graalvm.llvm-toolchain, version 22.1.0), required by: Graal.Python (org.graalvm.python)
The component(s) Native Image requires to accept the following license: Oracle GraalVM Enterprise Edition Native Image License
Enter "Y" to confirm and accept all the license(s). Enter "R" to the see license text.
Any other input will abort installation:  **Y**
Downloading: Contents of "Oracle GraalVM Enterprise Edition Native Image License" from oca.opensource.oracle.com
Downloading: Component org.graalvm.native-image: Native Image  from oca.opensource.oracle.com
Downloading: Component org.graalvm.python: Graal.Python  from oca.opensource.oracle.com
Downloading: Component org.graalvm.wasm: GraalWasm  from oca.opensource.oracle.com
Downloading: Component org.graalvm.R: FastR  from github.com
Downloading: Component org.graalvm.ruby: TruffleRuby  from oca.opensource.oracle.com
Downloading: Component org.graalvm.espresso: Java on Truffle  from oca.opensource.oracle.com
Please provide an e-mail address to not show the license check again (optional). You may want to check Oracle Privacy Policy (https://www.oracle.com/legal/privacy/privacy-policy.html).
Enter a valid e-mail address: **<your-email-address>**
Downloading: Component org.graalvm.llvm-toolchain: LLVM.org toolchain  from oca.opensource.oracle.com
Installing new component: LLVM.org toolchain (org.graalvm.llvm-toolchain, version 22.1.0)
Installing new component: Native Image (org.graalvm.native-image, version 22.1.0)
Installing new component: Graal.Python (org.graalvm.python, version 22.1.0)
Installing new component: GraalWasm (org.graalvm.wasm, version 22.1.0)
Installing new component: FastR (org.graalvm.R, version 22.1.0)
Installing new component: TruffleRuby (org.graalvm.ruby, version 22.1.0)
Installing new component: Java on Truffle (org.graalvm.espresso, version 22.1.0)
NOTES:
---------------
FastR should work out of the box on most Linux distributions and recent MacOS versions. Run the following script to check FastR requirements and create a personal R packages library directory:
    /Library/Java/JavaVirtualMachines/graalvm-ee-java17-22.1.0/Contents/Home/languages/R/bin/configure_fastr

The R component comes without native image by default. If you wish to build the native image, which provides faster startup, but slightly slower peak performance, then run the following:
    gu rebuild-images R

The native image is then used by default. Pass '--jvm' flag to the R or Rscript launcher to use JVM instead of the native image. Note that the native image is not stable yet and is intended for evaluation and experiments for curious users. Some features may not work in the native image mode. Most notably, the --polyglot switch works only in JVM mode (when --jvm is used).

See http://www.graalvm.org/docs/reference-manual/languages/r for more.


IMPORTANT NOTE:
---------------
The Ruby openssl C extension needs to be recompiled on your system to work with the installed libssl.
First, make sure TruffleRuby's dependencies are installed, which are described at:
  https://github.com/oracle/truffleruby/blob/master/README.md#dependencies
Then run the following command:
        /Library/Java/JavaVirtualMachines/graalvm-ee-java17-22.1.0/Contents/Home/languages/ruby/lib/truffle/post_install_hook.sh


This version of Java on Truffle is experimental. We do not recommended it for production use.

Usage: java -truffle [-options] class [args...]
           (to execute a class)
    or java -truffle [-options] -jar jarfile [args...]
           (to execute a jar file)

To rebuild the polyglot library:
    gu rebuild-images libpolyglot -cp ${GRAALVM_HOME}/lib/graalvm/lib-espresso.jar


IMPORTANT NOTE:
---------------
Set of GraalVM components that provide language implementations have changed. The Polyglot native image and polyglot native C library may be out of sync:
- new languages may not be accessible
- removed languages may cause the native binary to fail on missing resources or libraries.
To rebuild and refresh the native binaries, use the following command:
        /Library/Java/JavaVirtualMachines/graalvm-ee-java17-22.1.0/Contents/Home/bin/gu rebuild-images
```

**Note:** If you're using SDKMAN, you will need to install a new Java version linking the newly upgraded GraalVM.

For example:

```
$ sdk install java 22.1.0-r17-ee /Library/Java/JavaVirtualMachines/graalvm-ee-java17-22.1.0/Contents/Home
```

Confirm both GraalVM and all of the modules were upgraded:

```
$ java -version
java version "17.0.2" 2022-01-18 LTS
Java(TM) SE Runtime Environment GraalVM EE 22.1.0 (build 17.0.2+8-LTS-jvmci-22.0-b03)
Java HotSpot(TM) 64-Bit Server VM GraalVM EE 22.1.0 (build 17.0.2+8-LTS-jvmci-22.0-b03, mixed mode, sharing)
```

```
$ gu list
ComponentId              Version             Component name                Stability                     Origin
---------------------------------------------------------------------------------------------------------------------------------
graalvm                  22.1.0            GraalVM Core                  Supported
js                       22.1.0            Graal.js                      Experimental
native-image             22.1.0            Native Image                  Experimental                  oca.opensource.oracle.com
```

