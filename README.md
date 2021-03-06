# Multicore Communication

Collection of different communication methods for chip mulitprocessors.

This repo shall include all the multicore communication we have done in
[T-CREST](https://github.com/t-crest) as a standalone repo to make the
work more useful.

Currently we plan to use a simple rd/wr/address/data/rdy interface, which maps
dirctly to the Patmos OCPcore interface (A command is acked in the next
clock cycle or later, IO devices need to be ready to accept a command
during the ack cycle).

The S4NOC has currently a slightly different interface (no rdy needed).

We may consider moving to AXI.

## Usage

This project is published iwth Maven Central. Add following line to your ```build.sbt```

```
libraryDependencies += "io.github.t-crest" % "soc-comm" % "0.1.1"
```
## Dependency

This project depends on [ip-contributions](https://github.com/freechipsproject/ip-contributions),
which is resolved in ```build.sbt``` 

## Setup

The hardware is described in [Chisel](https://chisel.eecs.berkeley.edu/)
and needs just a Java JDK (version 8) and `sbt` installed. All other needed packages
will be automatically downloaded by `sbt`.


On a Mac with OS X `sbt` can be installed, assuming using [homebrew](http://brew.sh/)
as your package management tool, with:
```
brew install sbt
```

On a Debian based Linux machine, such as Ubuntu, you can install `sbt` with:
```
echo "deb https://dl.bintray.com/sbt/debian /" | sudo tee -a /etc/apt/sources.list.d/sbt.list
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 \
  --recv 2EE0EA64E40A89B84B2DF73499E82A75642AC823
sudo apt-get update
sudo apt-get install sbt
```

For the Chisel based tests compiler with gcc like interface is needed.

## Projects

### S4NOC

The network interface and the S4NOC are written in Chisel and the
source can be found in [s4noc](src/main/scala/s4noc)

The tests can run from the current folder with a plain

```
sbt test
```


or from your favorite Scala IDE (e.g., InelliJ or Eclipse).

To generate the Verilog code with a traffic generator execute

```
sbt "runMain s4noc.S4nocTrafficGen n"
```

where `n` is the number of cores.

The generated Verilog file can be found in ```generated/S4nocTrafficGen.v```
and can be synthesized to provide resource numbers and maximum
clocking frequency.  An Intel Qartus project is available in [quartus](quartus).

## TODO

 * Do a simple multicore device (no communication,
   just a register and core number) and integrate it with T-CREST
   * A multicore "Hello World"
 * drop iotester dependency
 * s4noc: Have the core # to slot # mapping in HW (it should be cheap)
 * s4noc: Play with FIFO buffer variations
 * Do traffic generators like in the Dally book
 * Have a better defined CPU interface, with one cycle latency here not in OCP wrapper
 * Provide blocking in the CPU interface if not data available or TX FIFO full
 * Get the Chisel 2 NoC running

### Next Paper

 * Build a standard NoC router for best effort
