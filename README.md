# D001E - Laboratory Exercise 1

The goal of this lab is for you to get acquainted with basic VHDL syntax and to start using Xilinx Vivado, which will be used in the course for the generation of RTL (register-transfer level) schematic and simulation of VHDL code.

You should be able to complete the lab during the first lab session. You can commit the whole Vivado project to facilitate reviewers in their tasks. The README file (this file) includes the table of the preparation and the answers to the questions in this document, including the text and the index of the question for it to be traced.

## Preparation (to be completed BEFORE the lab session):

Start by reviewing the introduction to VHDL (slides from a lecture available in Canvas).

Fork this repository, and clone it to your computer.

Study the source files `bcscheck.vhd` and `bcscheck2.vhd` with two implementations of a 4-bit BCD checker (BCD stands for binary-coded decimal, see http://en.wikipedia.org/wiki/Binary-coded_decimal).

Note that the inputs `x0`, `x1`, `x2`, and `x3` represent bits 0 (least significant) to 3 (most significant) of the 4-bit encoding.

### Question 1

Review `bcscheck1.vhd` and `bcdcheck2.vhd`. Based on your understanding of the code, what will the (Boolean) output of each signal be given a specific input? Fill in the table below.

| Input  |  max  |  min  | even  |  lo3  | noBCD |
| :----: | :---: | :---: | :---: | :---: | :---: |
|   0    |   0   |   1   |   1   |   1   |   0   |
|   1    |   0   |   0   |   0   |   1   |   0   |
|   2    |   0   |   0   |   1   |   1   |   0   |
|   3    |   0   |   0   |   0   |   0   |   0   |
|   4    |   0   |   0   |   1   |   0   |   0   |
|   5    |   0   |   0   |   0   |   0   |   0   |
|   6    |   0   |   0   |   1   |   0   |   0   |
|   7    |   0   |   0   |   0   |   0   |   0   |
|   8    |   0   |   0   |   1   |   0   |   0   |
|   9    |   1   |   0   |   0   |   0   |   0   |
| 10 (A) |   0   |   0   |   1   |   0   |   1   |
| 11 (B) |   0   |   0   |   0   |   0   |   1   |
| 12 (C) |   0   |   0   |   1   |   0   |   1   |
| 13 (D) |   0   |   0   |   0   |   0   |   1   |
| 14 (E) |   0   |   0   |   1   |   0   |   1   |
| 15 (F) |   0   |   0   |   0   |   0   |   1   |

In the README file include the Boolean algebra equations for each variable of the truth table, with the same name as the columns and as a function of the inputs `x0`, `x1`, `x2`, and `x3` (corresponding to  `bcdcheck1.vhd`).

*Your answers go here*


max =    x3 * (not x2) * (not x1) * x0  

min =   (not x3) * (not x2) * (not x1) * (not x0)  
even =  (not x0) 

lo3 =   ((not x3) * (not x2) * (not x1) * (not x0)) +
        ((not x3) * (not x2) * (not x1) * x0) + 
        ((not x3) * (not x2) *  x1 * (not x0))

noBCD = ((x3 * x2) + (x3 * (not x2) * x1))

Also study the source files `bcdcheck_tb.vhd` and `bcdcheck2_tb.vhd`. These will be used to verify the implementations of `bcdcheck`er in simulation.

---

## Part 1 - Starting up a Xilinx Vivado project

1) Start `Vivado 2023.2`.
  
2) `Quick Start`. From `Quick Start` choose `Create Project`. Press `Next`, enter `lab1` as your `Project name` and the path to your folder for this lab as your `Project location`. (In my case it is `/home/pln/courses/d001e/d001e_lab1`.) Uncheck the `Create project subdirectory`, this is important for the Continuous Integration scripts to work.

3) `Project Type`. Leave the default setting `RTL Project` and click `Next`.

4) `Add Sources`. Click `Add Files` and chose to add the source files from the `code` folder, then click `Next`.

5) `Add Constraints`. We don't have any constraints so skip this by pressing `Next`.

6) `Default Part`. Enter `xc7a35tcsg324-1` in the search field and select the found part, then click `Next`.

7) If things went well, you should have a `New Project Summary` reflecting the above-made choices, click `Finish` to continue.

8) Vivado will automatically associate identify the two test-benches (`bcdcheck_tb.vhd` and `bcdcheck2_tb.vdh`) and associate those to their respective instances to test (declared in `bcdcheck.vdh` and `bcdcheck2.vhd` respectively).

9) Select `bcdcheck_tb` as the top module. (Right-click on the file under `Simulation Sources`, and select `Top Module`).

10) Click `Run Simulation` (found under `Flow Navigator`/`SIMULATION`). Select `Run Behavioral Simulation`. It will first elaborate the design (compile the VHDL code) then open a `SIMULATION` view/pane.

1) In the simulation view, click `Zoom Fit`, to view the signal waveforms. Familiarize yourself with the simulation tool, scrolling, zooming. Double-clicking a signal in the `Object` view will open the test-bench `bcdcheck_tb.vhd` and highlight the signal declaration.

12) Open the corresponding test bench, and place a breakpoint at the last `wait for PERIOD`. Restart the simulation (`Restart` button is found above the `SIMULATION` pane, or press `CTRL-SHIFT-F5`). Now run the simulation until the breakpoint is hit, (`Run All` button, or press `F3`). You can now toggle back to the waveform view to see the simulation up to this point. You can split the view so you can have both the test bench and waveform displayed simultaneously. Play around with breakpoints to make yourself familiar.

Note that “Run All” will run a simulation until paused if no breakpoint is set. Later when we are going to run simulations in the "cloud" there is no one there to press the "pause", so it would run forever (which is not what we typically want.) You may also run the simulation for a predefined time `Run For Xus` button (`SHIFT F2`), where `X` can be set in the accompanying field.

By default, only signals declared in the top-level entity (in our case, `bcdcheck_tb`) are shown in the wave window. However, you can add internal signals of components (e.g. `bcdcheck_instance`) by selecting a component in the `Scope` panel and dragging signals from the `Objects` panel to the wave window.

To avoid ambiguity between signal names, we use `_test` to indicate that a signal stems from the test bench.

---

## Part 2. Verifying the `bcdcheck`er

Use the table you prepared earlier and verify that for each tested input, all the output signals of ``bcdcheck`` are correct.

### Question 2

1) Make a screenshot of the wave window. Make sure that the simulation screenshot captures the whole simulation. Store the screenshot in the `images` folder.

   *Link to your image goes here*
   ![q2](images/image.png)

   https://gyazo.com/6dc82d7f02ff8a1b882c306f3272f4ca

    

2) Explain why these values of the output signals are correct (it is not sufficient to refer to the truth table alone, you need to refer to the VHDL code here). Hint, look at how the algebraic expressions are encoded in VHDL for each signal.

   

   According to the truth table with our simulation and the code provided, we can have more security according the results are the same, but are structred in different ways. 
   
   If we analyze each output based on the VHDL code

   **max**
   In `BCDcheck`, the output max is set to 1 if x3 AND x0 turns out to be high while the rest of the bits are low. Thn we areleft with the bcd 1001, which is 9 also known as our highest bcd number.
   


   **min**
   in `BCDcheck`, the output min is set to 1 if all inputs are low. By having all of them as not, we will be able to set min as 1 when all of them are NOT high(1111), which is when they all are low (0000). 
   
   **even**
   In both, even is set to 1 as long as x0, the last digit, is low, which is an even number.

   **lo3**
   In `BCDcheck`, the output lo3 is set to 1 if the input is less than 3. Each "()" of code represents 0,1,2 and that it is only True, 1 when all of these are True.
   
   **noBCD**
   In `BCDcheck`, the output noBCD is set to 1 if either two patterns is detected by using logical operators. By having or, only one of the patterns must be detected for it to be 1. (Patterns: 10x or 101x) Cause these patterns give us a value higher than 9.
  
---

## Part 3. Generating the RTL schematic

Leave the default elaboration settings in the `RTL analysis` and generate the schematic by clicking on “OpenElaboratedDesign”. Once finished, the schematic of ``BCDcheck`` design is displayed. It should look something like this.

![Part3](images/bcdcheck_instance.png "bcdcheck_instance.png")

### Question 3

Make a screenshot of your circuit elaboration:
![Alt text](images/q3.png)
https://gyazo.com/f40dfacce172ed3b31ec64b5758f0f73

---

## Part 4. `bcdcheck`2.vhd`

Repeat Part2 and part3 to verify and implement `bcdcheck2`. This time, set ``bcdcheck2_tb` should be set as the top module for both design and simulation. (You need to re-run simulation and `Reload` elaboration.)

### Question 4

1) Explain in your own words the main difference between `bcdcheck` and `bcdcheck2`.

   The main difference between the two is how they check the input values to determine the output. `bcdcheck` uses 4 variables (x3,x2,x1,x0) while `bcdcheck2` only uses 1 (x)
   

2) Now review ``bcdcheck2.vhd`. Based on your understanding of the code, what will the boolean equation be for each and every output?

   *Your answers go here*

   max =  x3 * (not x2) * (not x1) * x0  

   min  =  (not x3) * (not x2) * (not x1) * (not x0)  
   even =  (not x0) 

   lo3 = ((not x3) * (not x2) * (not x1) * (not x0)) +
         ((not x3) * (not x2) * (not x1) * x0) + 
         ((not x3) * (not x2) *  x1 * (not x0))

   noBCD = ((x3 * x2) + (x3 * (not x2) * x1))


3) Make a screenshot of the wave window. Make sure that the simulation screenshot captures the whole simulation. Store the screenshot in the `images` folder.

![question4](images/q4.png)

https://gyazo.com/361786b94a0cb7258c16f15a188af382



4) Explain why these values of the output signals are correct (it is not sufficient to refer to the truth table alone, you need to refer to the VHDL code here). Hint, look at how the algebraic expressions are encoded in VHDL for each signal.


   **max**
   `BCDcheck2`, max is set to 1 if the input x is "9" (bcd: 1001), which is the highest binary number we have.

   **min**
   `BCDcheck2`, min is set to 1 if the input x is "0".

   **even**
   In both, even is set to 1 as long as x0, the last digit, is low, which is an even number.

   **lo3**
   `BCDcheck2`, the output lo3 is set 1 if x is greater than or equal to "0" AND less than "3". 

   **noBCD**
   `BCDcheck2`, the output noBCD is set to 1 if the input x is greater than "9" and less than or equal to "F"


5) Make a screenshot of your circuit elaboration:

![q4.5](images/q4.5.png)

   https://gyazo.com/2d1bd077c5ab932bff1eaa934552b6e3

You should find that the design is now captured by high-level blocks instead of primitive gates.

---

### Part 5. Test benches

Waveforms are great to get a graphical view of the signals to verify that the design is correct. However, think of systems with thousands (or even millions) of signals (like in a full-scale Processor). Obviously, it will be very hard to validate by looking at the waveform. Instead, we automate the process. Look at the `bcdcheck2_tb`:

``` vhdl
    x <= "0000";
    wait for PERIOD;
    assert max = '0' and min = '1' and even = '1' and lo3 = '1' and noBCD = '1' report "Error: Input value 0000" severity error;
```

The assertion is checked after we set `x <= "0000"` and the `wait for PERIOD` (allowing the signals to propagate).

By running the simulation again inside Vivado you should see the error in the TCL console by looking for the line: 

``` shell
Error: Error: Input value 0000
```

So the assertion failed. Why? Let's have a look at the `assertion` and compare that to the first row of your truth table (Question 1). You should see that I (intentionally) made a mistake. Fix the bug in the assertion statement and re-run the script.

Now there should no longer be an error in the TCL console and it should only display the 'normal' output.

#### Ci scripts (linux only)
In this course, we will use [Continuous integration](https://en.wikipedia.org/wiki/Continuous_integration) or CI for short to see if the code runs properly. 
This is achieved by having some script (defined in the `ci_scripts` directory) run the teach bench each time code is pushed to a given git repo.

If you are interested in how this functions and how to run it locally, a step by step will be shown below. This will assume that you are on a Linux based system using bash. ***If you are on a windows based system or you simply don't find these things interesting, you can skip this step completely and just rely on looking at the TCL console for the error output when answering question 5.*** 

Getting the ci scripts to run locally is achieved by:
1. Putting the Vivado path into the PATH environmental variable
2. Reloading the shell
3. Running the script

***This is written for bash, if you are using something else it should not be very difficult to extrapolate the instructions to your favourite shell.***

Hence, 
1. Find the Vivado path by searching for it on your machine, right-click and chose "Edit application". The path is defined by command, where you remove the last part as we want the path to the program, not the program itself. For me, this was `/software/Vivado/2022.2/bin`. Save this path.
2. Open the file `~/.bashrc` in your favourite editor, if it doesn't exist, create the file and open it.
3. Add the line `export PATH="VIVADO_PATH:$PATH`, where `VIVADO_PATH` is defined by the path we saved earlier. For me, the line I would add is `export PATH="/software/vivado/2022.2/bin:$PATH"`.
4. Save and close the file
5. Restart the shell and Vivado.

Once this is done you are now ready to run the `ci_scripts` from the terminal.

``` shell
> cd <to_the_local_folder_you_have_cloned_your_repository>
> ./ci_scripts/run.sh
```

If you don't get the script to run natively, you may instead look in the `tcl` window (it shows the text output of the simulation run).

The `cd <to_the_local_folder_you_have_cloned_your_repository>`, sets your current working directory (in my case it is `/home/pln/courses/d001e/d001e_lab1`, but depending where you "cloned" the lab it may vary of course).

The `./ci_scripts/run.sh` launches the run script (found in the `ci_scripts` folder). If this fails (by saying something like `no permission`) you may need to set the file type to executable `chmod +x ci_scripts/*`.

It takes a while for the script to run (30 seconds on my machine), the result should look like this if it fails.

``` shell
Error: Error: Input value 0000
Time: 40 ns  Iteration: 0  Process: /`BCDcheck`2_tb/line__25  File: /home/pln/courses/d001e/d001e_lab1/sim/xsim/srcs/`BCDcheck`2_tb.vhd
## quit
INFO: [Common 17-206] Exiting xsim at Mon Mar 29 1:57:33 2021...
====================================
==========  OH NO !!!!  ============
====================================
```

Or if it succeeds you should get:

``` shell
## run -all
## quit
INFO: [Common 17-206] Exiting xsim at Mon Mar 29 12:07:29 2021...
====================================
=============  OK !!  ==============
====================================
```

### Question 5

Now extend the test bench with one assertion per test (assignment of `x`). So there should be 16 assertions according to your truth table in Question 1.

Re-run the script locally from terminal (or if under Windows simulate och check TCL terminal). If you find that some tests fail, you most likely have an error in the assertion. Make sure all tests pass, and that the script produces an `OK !!` message (or that the TCL after simulation does not show any errors).

Think of possible advantages of automated tests, and explain in your own words why test automation is a must.

Mainly to improve effecinecy and time. Instead of manually testing repteaed codes, automated test can excecute these test faster and repeatedly once created without having to be manually fixed for each test. This in the end can also reduce potential costs. 
Since it is more time efficent, does automated test ensure in higher consistency in software which can cover more and do larges pices of software testing in less time. 

## Final remarks
Now you have learned to use vivado and taken the first steps towards design validation by writing your own tests/assertions.

Now add all changed files to your repository.

``` shell
> git status
...
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md
        modified:   code/`BCDcheck`2_tb.vhd
...
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .Xil/
...

```

You will find some files marked as modified, others as un-tracked.

``` shell
> git add <file>
```

Adds the file (and tracks it if not already tracked). Use this to add the `README.md`, `code/`BCDcheck`2_tb.vhd`, `lab1.xpr`, and the folder `images`.

``` shell
> git commit -m "my awesome lab1"
> git push
```

First, you commit the files (locally) and then push the commit to the upstream repository (on the vesuvio gitlab server).
