# COMP 310 — Operating Systems
## Week 4 — Report

<!-- Replace the italic prompt under each heading with your own words. Keep the headings: the specification names them, and `make test` checks that they are here. -->

## Answers

<!-- One per line. Fill in the right-hand side. The layout is just so the
     answers are easy to find - it is not a mark in itself. -->

```
sigchld_handler_calls: waitpid and write_pid_line
sigint_handler_calls: write
errno_saved: yes
```

## What

*What you built, and how. Under this heading, argue that every function your two handlers call is async-signal-safe.*

I built a command runner that can run jobs in the foreground and background. I used signals to know when a background job is done and to handle Ctrl+C

The SIGCHLD handler uses `waitpid()` and `write()`, and the SIGINT handler uses `write()`. These functions are async-signal-safe, so they are safe to use inside the signal handlers

## Results

*What you found. Include the `make test` output and a transcript of a background job, its `[done]` line, and Ctrl-C ending a foreground command.*

== the runner still runs commands ==
  PASS: runs an external command
  PASS: an unknown command reports why
  PASS: a foreground command is waited for
  PASS: a foreground command is not reported as '[done]'

== background jobs: cmd & ==
  PASS: 'sleep 4 &' returns at once
  PASS: prints '[bg] <pid>' for a background job

== reaping: the SIGCHLD handler ==
  PASS: a finished background job is reported as '[done] <pid>'
  PASS: a finished background job leaves no zombie
  PASS: four jobs finishing together are all reaped

== Ctrl-C: SIGINT to the foreground process group ==
  PASS: the shell survives Ctrl-C
  PASS: Ctrl-C ends the foreground command
  PASS: a background job survives Ctrl-C

== 12 passed, 0 failed ==
== the files the assignment asks for ==
  [ok]   src is there
  [ok]   tests is there
  [ok]   Makefile is there
  [ok]   README.md is there
  [ok]   REPORT.md is there

== the headings your report and readme need ==
  [ok]   README.md has Build
  [ok]   README.md has Run
  [ok]   README.md has File map
  [ok]   README.md has Notes
  [ok]   REPORT.md has What
  [ok]   REPORT.md has Results
  [ok]   REPORT.md has Citations

== submission.json ==
  [ok]   submission.json declares label
  [ok]   submission.json declares student
  [ok]   submission.json declares entrypoint
  [ok]   submission.json declares what_i_built
  [ok]   submission.json has your own values

  17 passed, 0 failed

  csh> sleep 3 &
[bg] 22542
csh> [done] 22542

I tested Ctrl-C with a foreground command like sleep 30. Ctrl-C stopped the foreground command, but the shell stayed running

## Citations

- Course Material

- Class notes

- https://www.w3schools.com/c/c_intro.php

- https://www.geeksforgeeks.org/c/c-programming-language/

