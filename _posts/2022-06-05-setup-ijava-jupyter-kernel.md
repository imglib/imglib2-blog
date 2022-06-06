# Setup the IJava jupyter kernel
> Follow these instructions to setup the [IJava juyter kernel](https://github.com/SpencerPark/IJava) by [Spencer Park](https://github.com/SpencerPark)

- toc:true- branch: master
- badges: true
- comments: true
- author: Stephan Saalfeld
- categories: [jupyter, ijava, jshell, java, kernel]

## Introduction

In this blog, we will show code snippets and examples to make the best use of [ImgLib2](https://github.com/imglib/imglib2), [BigDataViewer](https://github.com/bigdataviewer/bigdataviewer-core), and friends.  ImgLib2 is written to be fast and we will run code that needs to be compiled, so we cannot use any of the various interpreted scripting languages like Python, Groovy, or Javascript.  Instead, we will use the [JShell tool](https://docs.oracle.com/javase/9/jshell/introduction-jshell.htm#JSHEL-GUID-630F27C8-1195-4989-9F6B-2C51D46F52C8) that you can use directly in a terminal or through [Spencer Park's IJava jupyter kernel](https://github.com/SpencerPark/IJava).  You can also follow these tutorials in your own Java project and use your preferred IDE, but Jupyter notebooks are a great teaching tool.  Since jupyter is written in Python and most popular with the Python community, let's follow their ways and first thing create a virtual environment with conda.  The lack of version controlled dependency management for Python projects makes it necessary that practically every project must run in a container or virtual environment because the dependencies of different projects almost inevitably collide.  Conda is the most popular of several attempts to address this situation.  Conda cannot currently be installed from the default Ubuntu repositories, so much about that, but the instructions at [https://docs.conda.io/projects/conda/en/latest/user-guide/install/rpm-debian.html] are tolerable, there is a PPA.  Now let's create an environment for jupyter:

```
conda create -n jshell-jupyter python=3
conda init bash
conda activate jshell-jupyter
conda install jupyter
conda install openjdk
conda install -c conda-forge maven
```

This may take a day of solving environments, so you may not want to wait for this and use your global Java and Maven installations instead.  The IJava kernel currently does not build with Java 17 or 18, so the easiest at this time is to make it with OpenJDK-11.  If you don't have it yet, install it:

```
sudo apt install openjdk-11-jdk maven
```

If you have other versions installed, you can switch between them with the `alternatives` tool:

```
sudo update-alternatives --config java
sudo update-alternatives --config javac
```

Now check out IJava and build and install the kernel IJava Jupyter kernel following [the installation instructions](https://github.com/SpencerPark/IJava#install-from-source) or:

```
git clone https://github.com/SpencerPark/IJava.git
cd IJava/
./gradlew installKernel
jupyter kernelspec list
```

This should print something like this

```
Available kernels:
  java       /home/saalfeld/.local/share/jupyter/kernels/java
  python3    /home/saalfeld/anaconda3/envs/jshell-jupyter/share/jupyter/kernels/python3
```

You can now start the jupyter notebook server

```
jupyter notebook --kernel=java
```

And experiment with the examples.  [Spencer Park's IJava jupyter kernel](https://github.com/SpencerPark/IJava) makes it very easy to include dependencies.  You can include the relevant snippets from a Maven POM into a tagged code block, e.g.

```xml
%%loadFromPOM
<repository>
    <id>scijava.public</id>
    <url>https://maven.scijava.org/content/groups/public</url>
</repository>
<dependency>
    <groupId>sc.fiji</groupId>
    <artifactId>bigdataviewer-vistools</artifactId>
    <version>1.0.0-beta-29</version>
</dependency>
```

If you prefer to run [JShell](https://docs.oracle.com/javase/9/jshell/introduction-jshell.htm#JSHEL-GUID-630F27C8-1195-4989-9F6B-2C51D46F52C8) directly, you can pull in the dependencies from a complete Maven POM with John Pooth's Maven Jshell plugin

```
mvn compile com.github.johnpoth:jshell-maven-plugin:1.3:run
```

Happy JShelling!
