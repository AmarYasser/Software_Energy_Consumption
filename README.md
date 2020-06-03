# About

PowerAPI is a middleware toolkit for building software-defined power meters. Software-defined power meters are configurable software libraries that can estimate the power consumption of software in real-time. PowerAPI supports the acquisition of raw metrics from a wide diversity of sensors (eg., physical meters, processor interfaces, hardware counters, OS counters) and the delivery of power consumptions via different channels (including file system, network, web, graphical). As a middleware toolkit, PowerAPI offers the capability of assembling power meters «à la carte» to accommodate user requirements.

PowerAPI is an open-source project developed by the [Spirals research group](https://team.inria.fr/spirals) (University of Lille 1 and Inria) and fully managed with setuptools.

The documentation is available [here](https://powerapi.org/).

### Used package
There are different packages available for the tool

In this tutorial, we will use this [package](https://github.com/powerapi-ng/powerapi-scala). 

## Installation

PowerAPI is completely written with  [Scala](http://www.scala-lang.org/) (v2.11) 
and fully managed by [sbt](http://www.scala-sbt.org/).

We  have to install scala, sbt and Java
```bash
sudo apt-get install scala
sudo apt-get install sbt
```
For Java
```bash
sudo apt-get update
sudo apt-get install default-jre
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java8-installer
```

then, clone and  build the package
```bash
git clone https://github.com/powerapi-ng/powerapi-scala.git
sudo sbt publishLocal
```
then add the following line to `/build.sbt` file
```bash
libraryDependencies += "org.powerapi" % "powerapi-core_2.11" % "4.0"
```
### Configuration
before using the package, we should adjust some [configuration](https://github.com/powerapi-ng/powerapi-scala/wiki/Modules) files

you can find it in `powerapi-scala/powerapi-cli/target/universal/stage/conf
`
we now will adjust `./powerapi.conf` file 

regarding to the formula used for this module the [TDP](https://en.wikipedia.org/wiki/Thermal_design_power), TDP factor and Frequencies/Voltage  values should be identified 

theses value should be different for processors and could be found from the processors' Manufactures.

```
powerapi.cpu.tdp = 35
powerapi.cpu.tdp-factor = 0.7
powerapi.cpu.frequencies = [
	{ value = 1800002, voltage = 1.31 }
	{ value = 2100002, voltage = 1.41 }
	{ value = 2400003, voltage = 1.5 }
]

```
## building 
then build the required module using sbt interactive tool
```bash
sudo sbt
projects
```
now you can see one of `cli` project.
then we should stage it.
```
project cli
stage
exit
```



## Test
Now we can test the Tool using any running software on the system

included, a code source of the Tower of Hanoi problem. so, our test will be as following.

### 1. Launch the software
 
`./Tower_of_hanoi`
 
### 2. Find its PID / Name from the system 

Using [Top](https://linux.die.net/man/1/top) or [htop](https://hisham.hm/htop/) tools 

### 3. Use PowerAPI tool 

```bash
cd powerapi-cli/target/universal/stage

./bin/powerapi \
    modules procfs-cpu-simple \
    monitor \
      --frequency 500 \
      --pids [Pid of program] \
      --chart
```
or 
```bash
./bin/powerapi \
    modules procfs-cpu-simple \
    monitor \
      --frequency 500 \
      --apps Tower_of_hanoi \
      --chart
```

![alt text][logo]

[logo]: https://gite.lirmm.fr/aelmeligy/software_energy_estimation/-/blob/master/Screenshot_from_2020-06-03_13-07-44.png "Screen Shot"
