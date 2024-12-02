# Expert Systems
BMIG 5003 Fall 2022

Horacio Gomez-Acevedo

The text is based on the book by Gary Riley *Adventures in rule-based programming: A CLIPS tutorial*.

## Declarative Programming

During this course you have been exposed to Python which is considered to be an **imperative programming** language. In this type of language, you specific the logic of what must be done and also the specific sequence of steps whtat will control the flow.

By contrast **declarative programming** specifies the logic of what must be accomplished, however the specific sequence of steps that control the flow are not specified.

**Rule-based programming** 

An example of declarative programming is the *rule-based programming*. A **rule** is a primary unit of code. A rule embodies a set of conditions and the actions to be taken when those conditons are met:

```bash
When
    Conditons
Then
    Actions
```

The use of *when/then* is used to differentiate from the *if/then* structure that the imperative programming handles. 

### Example
```bash
Measles rule
    When
        Patient has high fever and
        Patient has cough and
        Patient has runny nose and
        Patient has red, watery eyes and
        Patient has rash
    Then
        Diagnose illness as measles.
```

Rule-based systems have three components:
+ Data
+ Rules
+ Inference engine

The **data** component consists of a collection of global objects that represent the current state of the program and it is encoded internally with specific data structures.

The **rules** componenet is a collection of rules, each of which has a set of conditions and actions. When the conditions of a rule are satisfied, its actions can be applied. The conditions of a rule are referred to as the **antecedent** or **left hand side**, whereas the actions are referred to as the **consequent** or **right hand side**. 

The **inference engine** determines how the declarative logic is processed. First, it determines which rules have their conditions satisfied by the data. Then it determines which rule should be selected to have its actions applied. This process is then repeated until there are no rules remaining to execute. 

Inference engines can be further categorized as

+ Forward chaining systems. Start with the premises and work forward to the consudion supported by those premisses.
+ Backward chining. Start with a conclusion to be proven and wrok backward to the premises that would support the conclusion.

## CLIPS

From their website (https://www.clipsrules.net/) the software is defined as

A Tool for Building Expert Systems

Developed at NASA’s Johnson Space Center from 1985 to 1996, the C Language Integrated Production System (CLIPS) is a rule‑based programming language useful for creating expert systems and other programs where a heuristic solution is easier to implement and maintain than an algorithmic solution. Written in C for portability, CLIPS can be installed and used on a wide variety of platforms. Since 1996, CLIPS has been available as public domain software.

![Clips](../../Figures/fig_clips_ide.png)

The website has very good documentation about CLIPS and its use. See in particular Chapter 2 of the user guide.  

Once you entered CLIPS program you will interact with the *read-eval-print-loop* (REPL). This is different for what you have experienced in python. 

Other big difference between CLIPS and Python is the way rules are evaluated. For instance

```bash
(python) (3+4)<6
(clips) (< (+ 3 4 ) 6)
```

In this regard, CLIPS syntax is more closely related to LISP. 

To exit clips

```bash
(exit)
```

I will use the program *cholesterol.clp* to explain some of the basic components of CLIPS.

### Constructs

Constructs are the building blooks of a CLIPS program and they use parentheses as delimiters. Constructs persist until explicitly removed.

Some basic constructs are:

+ defrule
+ deftemplate
+ deffacts
+ deffunction

Let's check a couple of them

#### deftemplate construct

This construct is used to define the structure of facts sharing common attributes.
Each slote stores the value of an atribut for a fact.

```bash
(deftemplate attribute
   (slot name)
   (slot value))
```
#### defrule construct

This construct is used to create rules. The "=>" symbol separates the conditions of the rule from the actions of the rule. The **assert** command creates one or more facts. More specifically, this command returns a fact-address.

```bash
(defrule get-name
   =>
   (printout t "What is your name? ")
   (assert (attribute (name name) (value (read)))))
```

### The agenda

An **agenda** is the list of rule activations ready for execution. 

So, when a rule has its conditions satisfied, an *activation* for the rule is created and added to the *agenda*. Once an activation has been executed, it is removed from the agenda.




## Clipsy

The program *clipsy* (https://clipspy.readthedocs.io/en/latest/) is a set of bindings to CLIPS

This program is installed via pip in your environment. 

```bash
pip install clipsy
```

### Example

Let's start with a very simple program in CLIPS and load it into Python via clipsy. The file is named * cholesterol.clp* (https://github.com/garydriley/SmallCLIPSExamples)

```bash
;;; CLIPS Website
;;; clipsrules.net

;;; CLIPS on SourceForge
;;; sourceforge.net/projects/clipsrules/

;;; Adventures in Rule-Based Programming: A CLIPS Tutorial
;;; clipsrules.net/airbp

;;; Sample Run 
;;;
;;; CLIPS> (load cholesterol.clp)
;;; %*********
;;; TRUE
;;; CLIPS> (reset)
;;; CLIPS> (run)
;;; What is your name? Fred
;;; Fred, what is your gender? male
;;; Fred, what is your total cholesterol? 180
;;; Fred, what is your HDL? 50
;;; Fred, you have a moderate risk of heart disease.
;;; CLIPS> 

(deftemplate attribute
   (slot name)
   (slot value))

(defrule get-name
   =>
   (printout t "What is your name? ")
   (assert (attribute (name name) (value (read)))))
   
(defrule get-gender
   (attribute (name name) (value ?name))
   (not (attribute (name gender)))
   =>
   (printout t ?name ", what is your gender? ")
   (assert (attribute (name gender) (value (read)))))
   
(defrule get-total-cholestorol
   (attribute (name name) (value ?name))
   (not (attribute (name total-cholesterol)))
   =>
   (printout t ?name ", what is your total cholesterol? ")
   (assert (attribute (name total-cholesterol) (value (read)))))
   
(defrule get-HDL
   (attribute (name name) (value ?name))
   (not (attribute (name HDL)))
   =>
   (printout t ?name ", what is your HDL? ")
   (assert (attribute (name HDL) (value (read)))))
      
(defrule bad-answer
   (declare (salience 10))
   (or ?a <- (attribute (name gender) (value ~male&~female))
       ?a <- (attribute (name total-cholesterol | HDL)
                        (value ?value&:(or (not (numberp ?value))
                                           (<= ?value 0)))))
   =>
   (retract ?a))

(defrule compute-ratio
   (attribute (name total-cholesterol) (value ?total))
   (attribute (name HDL) (value ?hdl))
   =>
   (assert (attribute (name ratio) (value (/ ?total ?hdl)))))
   
(defrule low-ratio
   (attribute (name name) (value ?name))
   (or (and (attribute (name gender) (value male))
            (attribute (name ratio) (value ?ratio&:(< ?ratio 3.5))))
       (and (attribute (name gender) (value female))
            (attribute (name ratio) (value ?ratio&:(< ?ratio 3.0)))))
   =>
   (printout t ?name ", you have a low risk of heart disease." crlf))

(defrule moderate-ratio
   (attribute (name name) (value ?name))
   (or (and (attribute (name gender) (value male))
            (attribute (name ratio) (value ?ratio&:(>= ?ratio 3.5)&:(<= ?ratio 5.0))))
       (and (attribute (name gender) (value female))
            (attribute (name ratio) (value ?ratio&:(>= ?ratio 3.0)&:(<= ?ratio 4.4)))))
   =>
   (printout t ?name ", you have a moderate risk of heart disease." crlf))
   
(defrule high-ratio
   (attribute (name name) (value ?name))
   (or (and (attribute (name gender) (value male))
            (attribute (name ratio) (value ?ratio&:(> ?ratio 5.0))))
       (and (attribute (name gender) (value female))
            (attribute (name ratio) (value ?ratio&:(> ?ratio 4.4)))))
   =>
   (printout t ?name ", you have a high risk of heart disease." crlf))

   ```

If we want to run it in Python 

```python
import clips

env=clips.Environment()
env.load("cholesterol.clp")
env.run()

```

## Pyknow

PyKnow (<https://github.com/buguroo/pyknow>) is a pure Python library for building expert systems inspired by CLIPS.
However, it is programmatically a bit more challenging since it uses objects extensively.


This program follows the old installation format for python. 

1. Clone the repository
2. Move to the root of the repository
3. Activate your environment (conda or mamba)

```bash
pip install .
```



Experta (<https://github.com/nilp0inter/experta>) is a fork of PyKnow

```bash
pip install experta
```

> Keep in mind that PyKnow has not been maintained in a long time (5 years) and experta has not been updated in 3 years. Also, it requires python 3.9 or below.


