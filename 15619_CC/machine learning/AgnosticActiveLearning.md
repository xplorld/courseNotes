# Agnostic Active Learning

[pdf](http://www.cs.cmu.edu/~ninamf/papers/a2.pdf)

## active learning

As opposed to passive learning: given data and label, train a model and predict.

Active learning: given few (or no) examples, the algorithm asks for labels for certain data, and an Oracle replies. loop until (kind of) converge.

advantage: need less (logarithmic) labeled data than passive.

## Input

A stream of i.i.d. unlabeled examples.

