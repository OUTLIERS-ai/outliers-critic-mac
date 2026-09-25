**This is the Mac version.** On Windows, use [outliers-critic](https://github.com/OUTLIERS-ai/outliers-critic).

# A critic you can point at your own work

Two files. `the-critic.md` is the instructions given to an AI model. `design-critique-canon.md`
is the standard it judges by, where the seven ways of looking are actually written out.

Both are plain text. Read them. Most of what they are worth is in how they are written, not
in running them.

## What it does

It drives a running piece of software the way a person would, judges it through seven
separate ways of looking that are meant to disagree with each other, and produces one
ranked report with the evidence attached.

Each way of looking writes its own findings **before any of them are compared.** Three
findings that arrived separately are worth more than one opinion repeated three times, and
letting them confer first destroys the only signal you have.

## The order is the design

The first way of looking is not about design at all. It asks whether the object works and
whether it tells the truth about its own state, and anything it finds outranks every design
finding underneath it.

That order exists for a measured reason. People rate an attractive design as easier to use
than testing shows it to be, and more strongly than they rate the one that is actually
easier. **Good looks buy forgiveness for real faults**, so a judge that starts with
appearance gets fooled.

The heaviest of the seven arrives knowing nothing, tries to complete real jobs cold, and
counts: time before anything useful happens, wrong turns per task, separate moments of not
understanding, and the point a real person would give up.

## The four instructions that make it useful rather than polite

- **Know nothing.** Discard what you know about where anything lives. Find out by clicking.
- **Write down the hunt.** Every wrong turn, every place you expected to find something, how
  many clicks before you gave up. Getting lost is the finding.
- **Complete, not tidy.** Stop when you have opened everything, not when you have enough.
- **Never be satisfied.** Concluding that something looks fine is named in the file as the
  failure mode and forbidden as a stopping point.

Those four transfer to any critic you write, for any department.

## Building your own, in six steps

1. **Name the object it judges.** One kind of object, not a department. A sales conversation.
   A delivered report. A month of numbers. A judge pointed at everything judges nothing.
2. **Write the truth check first.** Whatever the equivalent is of the screen disagreeing with
   the system underneath. Does the record match what was actually said. Does the reported
   status match the work actually done. This runs first and outranks everything.
3. **Write the stranger.** Somebody with no context meeting the object cold, narrating
   confusion as it happens, with counts.
4. **Add three or four ways of looking that disagree.** One question each. If two of them
   would always agree, you have one.
5. **Force independence.** Each writes its findings before any are compared.
6. **Add the four instructions above.**

## Two ways a critic goes wrong

**It becomes agreeable.** It finds less over time and what it finds gets smaller. Count
findings per run and top severity findings per run. A falling line with no change in what you
are making is the judge going soft, not the work getting better.

**It becomes harsh about the wrong work.** Trained judges and ordinary customers agree about
work that is clearly bad and split about work that is borderline. A hard verdict on borderline
work is not automatically right, and interesting work is borderline by definition. Treat a
kill on borderline work as provisional.

## Installing it

Drop `the-critic.md` into your agents folder and keep `design-critique-canon.md` beside it.
The agent file points at the canon by name, so if you rename one, rename the reference.

Then read both before you run either.

## What it will not do

It judges. It never edits. A judge that fixes what it finds has started marking its own work,
and by the second round there is no independent reader left.

This repo is made automatically from outliers-critic@555c141. To report a problem or suggest a change, use that repo, not this one.
