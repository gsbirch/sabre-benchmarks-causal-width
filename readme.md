# Sabre Benchmark Tool - Causal Width Experiments

This is a fork of the [Sabre Benchmark Tool](https://github.com/sgware/sabre-benchmarks),
a tool for testing narrative planners on a repository of example Sabre problems.
Refer to the original repository for more information on this tool.

This repository contains a configuration of the tool for experimenting with
our definition of causal width. Causal width is a measure of how many unnecessary
actions are in a plan. You can learn more about this in our [paper](paper.pdf).

Our [paper](paper.pdf) reports results for two different experiments. The first
compares our Causal Width Search to a form of Breadth First Search. You may
find these results in [this folder](experiment/cwc/) which are also presented
in a human readable [html file](experiment/cwc/results.html). Our second experiment
involves pruning plans with high causal width. Similarly, you may find these results in
[this folder](experiment/cwp/) and in this human readable [html file](experiment/cwp/results.html).

## Causal Width in Search

Our search techniques take place in the space of fully ground, totally ordered
sequential plans, built from start to finish. Consider a plan in this space. If
leaving out an action would cause the plan to be impossible, or cause the utility 
of a character or the author to decrease, we call the action necessary. Intutively,
the action is necessary because the plan can't happen without it or it is important
for the author or a character to achieve their goal and raise their utility.

The causal width of a plan is the amount of action in that plan that are *not*
causally necessary. We hypothesize that plans with a lower causal width (thus having
mostly necessary actions) are more likely to lead to a solution.

## Usage

To clone this project (including Sabre as a submodule), compile the code, and
run it:

```
git clone --recurse-submodules [INSERT URL HERE]
cd sabre-benchmarks
javac -cp sabre/build/jar/sabre.jar -sourcepath src -d bin src/edu/uky/cs/nil/sabre/bench/Main.java
java -Xms60g -Xmx60g -cp bin;sabre/build/jar/sabre.jar edu.uky.cs.nil.sabre.bench.Main
```

The `-Xms60g` argument sets the Java Virtual Machine's minimum heap space to 60
gigabytes, and the `-Xmx60g` argument sets the JVM's maximum heap space to 60
gigabytes. You can adjust these numbers up or down depending on how much memory
is available and how many threads will run simultaneously.

All relevant settings can be found at the top of
[`Main.java`](src/edu/uky/cs/nil/sabre/bench/Main.java). You can change how many
threads run in parallel. You can set the maximum number of nodes visited, nodes
generated, and time spent by each search. You can change the number of times
each planner is run on each problem and whether the order of actions is shuffled
between runs. You can comment out benchmark problems or planner configurations
you don't want to test.

To add a new benchmark problem, you need to place the relevant Sabre problem
file in the [problems](problems) directory and add a new line to
[`Main.java`](src/edu/uky/cs/nil/sabre/bench/Main.java) that gives a unique name
for the benchmark problem, the name of the problem file, the goal utility that
must be achieved, and the author temporal limit, character temporal limit, and
epistemic limit on the search. Optionally, you can also add a known solution to
the [solutions](solutions) directory. The name of the file should match the name
of the benchmark problem (not the name of the problem file). Before the tests
begin, every planner will attempt to reproduce that solution using a special
heuristic that only allows the planner to use actions from that solution in its
search.
