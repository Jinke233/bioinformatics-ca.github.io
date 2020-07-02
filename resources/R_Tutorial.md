---
layout: post2
permalink: /CBW_R_Tutorial/
title: CBW R Tutorial
header1: CBW R Tutorial
header2: An Introduction to R 
image: Bioinfo_Logo.jpg
---

<ul id="navmenu">
  <li><a id="back_to_top">Contents</a>
     <ul class="sub1">
      <li><a href="#environment">Environment</a>
         <ul class="sub2">  
           <li><a href="#installation">Installation</a></li>
           <li><a href="#interface">User Interface</a>
               <ul class="sub3">
                   <li><a href="#RStudio">A Note on RStudio</a></li>
                   <li><a href="#help">The Help System</a></li>
                   <li><a href="#work">Working Directory</a></li>
                   <li><a href="#rprofile">.Rprofile - Startup Command</a>
                   	<ul class="sub4">
                   		<li><a href="#unix">Unix Systems</a></li>
                   		<li><a href="#macs">Mac OS X Systems</a></li>
                   		<li><a href="#windows">Windows Systems</a></li>	
                   	</ul>	
                  </li>
                  <li><a href="#workspace">Workspace</a></li>
               </ul>
           </li>     
           <li><a href="#packages">Packages</a></li>
           <li><a href="#scripts">Scripts</a></li>
        </ul>
      </li>
       <li><a href="#commands">Simple Commands</a>
          <ul class="sub2">
             <li><a href="#operators">Operators</a></li>
             <li><a href="#functions">Functions</a></li>
             <li><a href="#variables">Variables</a></li>
           </ul>
       </li>
       <li><a href="#scalars">Scalar Data</a></li>
       <li><a href="#vectors">Vectors</a></li>
       <li><a href="#matrices">Matrices</a></li>
       <li><a href="#lists">Lists</a></li>
       <li><a href="#data_frames">Data Frames</a></li>
       <li><a href="#writing_fucntions">Writing Functions</a>
          <ul class="sub2">
             <li><a href="#style">Coding Style</a></li>
             <li><a href="#debugging">Debugging</a></li>
             <li><a href="#finishing">Finishing</a></li>
           </ul>
        </li>
        <li><a href="#reading_resources">Further Reading and Resources</a></li>
    </ul>
  </li>
</ul> 

<br>

This tutorial was created by [Boris Steipe](http://steipe.biochemistry.utoronto.ca/abc/index.php/R_tutorial) and converted and updated by the CBW team.

###  The Environment  <a id="environment"></a>

In this section we discuss how to download and install the software, how to configure an **R** session and what work with the **R** environment includes.

[&uarr;](#back_to_top)

#### Installation <a id="installation"></a> 

1. Navigate to [CRAN (the Comprehensive R Archive Network)](http://cran.r-project.org/) and follow the link to **Download R** for your computer's operating system..

2. Download a precompiled binary (or "build") of the **R** "framework" to your computer and follow the instructions for installing it. You don't need tools, or GUI versions for now, but do make sure that the program is the correct one for your **version** of your operating system.

3. Launch **R**.

The program should open a window - the "R console" - and greet you with its *input prompt*, awaiting your input:

```r
 >
```

The sample code on this page sometimes copies input/output from the console, and sometimes shows the actual commands only. The <code>></code> character at the beginning of the line is always just **R**'s *input prompt*; it is shown here only to illustrate the interactive use of the program and you do not need to type it. If a line starts with <code>[1]</code> or similar, this is **R**'s *output* on the console. A <code>#</code>-character marks the following text as a comment which is not executed by **R**. In principle, commands can be copied by you and pasted into the console, or into a script - obviously, you don't need to copy the comments. In addition, I use syntax highlighting on **R**-code, to colour language keywords, numbers, strings, etc. different from other text. This improves readability but keep in mind that the colours you see on your computer will be different.

Note the following though: it is convenient to copy/paste code, but you don't learn a lot. Practice has shown that it is much better to actually type commands. This ensures you are reading and translating the code character by character, and in computer code, every single character matters. For example, I expect that by typing out commands you will be much less likely to confuse <code>=</code> with <code><-</code> or even <code>==</code>. Also, you will sometimes mistype things and create errors. That's actually good, because you quickly learn to spot errors, fix them, and resume. That way you build confidence.

One more thing about the console: use your keyboard's *up-arrow* keys to retrieve previous commands, then enter the line with *left-arrow* to edit it; hit *enter* to execute the modified line. Or, if you are on a Mac, click the striped icon in the console window to show/hide the command line history.

[&uarr;](#back_to_top)

#### User Interface <a id="interface"></a>

R comes with a GUI to lay out common tasks. For example, there are a number of menu items, many of which are similar to other programs you will have worked with ("File", "Edit", "Format", "Window", "Help"  ...). All of these tasks can also be accessed through the command line. In general, GUIs are useful when you are not  sure what you want to do or how to go about it; the command line is much more powerful when you have more experience and know your way around in principle. **R** gives you both options. 

In addition to the *Console*, there are a number of other windows that you can open (or that open automatically). They all can be brought to the foreground with the **Windows** menu and include help, plotting, package browser, and other windows.

Let's look at some functions of the **R** console and associated windows that refer to **how** you work, not **what** you do.

[&uarr;](#back_to_top)

##### A Note on R Studio <a id="RStudio"></a>

[**R Studio**](http://www.rstudio.com/) is a free IDE (Integrated Development Environment) for **R**... 

Most workshops will use **R Studio**, even though the Mac OS X GUI for **R** has almost all the same functionality, and thus there is little advantage in adding a third-party wrapper around the **R** program. If you are working in a Linux or Windows environment, **R Studio** does offer tangible advantages, notably syntax-aware code coloring. Navigate to the [**R Studio** Website](http://www.rstudio.com/) to download and install. 

Here is a small list of differences. If you can contribute pros and cons from your personal experience, please let me know.

###### Pros:
* Integrated version control.

* A consistent interface across all supported platforms; base **R** GUIs are not all the same.

* Syntax aware code coloring.

* Better handling of the **Stop Execution** button, which sometimes does not recover a stuck process on the Mac.

* Code autocompletion in the script editor. (Depending on your point of view this can be a plus or a minus.)

* The ability to set breakpoints in the script editor.

* Support for knitr, Sweave, RMarkdown...

* Better support for switching between work on concurrent projects.

###### Cons:

* The *tiled* interface uses more desktop space than the windows of the **R** GUI.

* There are sometimes (rarely) situations where **R** functions do not behave in exactly the same way in **R Studio**.

* The supported **R** version is not always immediately the most recent release. 

[&uarr;](#back_to_top)

##### The Help system <a id="help"></a>

Help is available for all commands and for the **R** command line syntax. As well, help is available to find the names of commands when you are not sure of them. 


*("help" is a function, arguments to a function are passed in parentheses "()")*

```r
> help(rnorm)
> 
```

*(shorthand for the same thing)*

```r
> ?rnorm
> 
```


*(what was the name of that again ... ?)*

```r
> ?binom     
No documentation for 'binom' in specified packages and libraries:
you could try '??binom'
> ??binom
> 
```


*(found "Binomial" in the list of keywords)*

```r
> ?Binomial
> 
```


If you need help on operators, place them in quotation marks. Try:

```r
> ?"+"
> ?"~"
> ?"["
> ?"%in%"
> 
```


That's all fine, but you will soon notice that **R**'s help documentation is not all that helpful for newcomers (who need the most help). Here's what you might look for.

* The **Description** section describes the function in general technical terms. 

* The **Usage** section tells you what arguments are required (these don't have defaults), what arguments have defaults, and what the defaults are, and whether additional arguments ("...") are allowed. Often a function comes in several variants, you will find them here.

* The **Arguments** section provides detailed information . You should read it, especially regarding whether the arguments are single values, vectors, or other objects, and what effect missing arguments will have.

* The **Details** section might provide common usage and context information. It might also not. Often functions have crucial information buried in an innocuous note here.

* You have to really understand the **Value** section. It explains the output. Importantly, it explains the type of object a function returns - it could be a list, a matrix or something else. The value could also be an object that has special methods defined e.g. for plotting it. In that case, the object is formally a "list", and its named "components" can be retrieved with the usual list syntax (see below).

If you look at the bottom of the help function, you will usually find examples of the function's usage; these often make matters more clear than the terse and principled help-text above. 

What you often won't find:

* Clear commented, examples that relate to the most frequent use cases.

* Explanations **why** a particular function is done in a particular way.

* Notes on common errors.

* An exhaustive list of alternatives.

Therefore, my first approach for **R** information is usually to Google for what interests me and this is often the quickest way to find working example code. **R** has a very large user base and it is becoming very rare that a reasonable question will not have a reasonable answer among the top three hits of a Google search. Also, as a result of a Google search it may turn out that something *can't* be done (easily) - and you won't find things that can't be done in the help system at all. You may want to include "r language" in your search terms, although Google is usually pretty good at figuring out what kind of "r" you are looking for, especially if your query includes a few terms vaguely related to statistics or programming.

* There is an active [**R-help mailing list**](https://stat.ethz.ch/mailman/listinfo/r-help) to which you can post - or at least search the archives: your question probably has been asked and answered before. A number of SIGs (Special Interest Groups) exist for more specific discussions - e.g. for mac OS, geography, ecology etc. They are [listed here](https://stat.ethz.ch/mailman/listinfo).

* Most of the good responses these days are on *stack overflow*, discussion seems to be shifting to there from the R mailing list. Information on statistics questions can often be found or obtained from the *CrossValidated* forum of stackexchange.

  * try this [sample search on *stackOverflow*](http://stackoverflow.com/search?q=R+sort+dataframe)...

  * try this [sample search on *CrossValidated*](http://stats.stackexchange.com/search?q=R+bootstrapping+jackknifing+cross-validation)...

* [**Rseek**](http://rseek.org ) is a specialized Google search on **R**-related sites. Try "time series analysis" for an example.

* The **bioconductor** project has its own [support site on the Web](https://support.bioconductor.org/).

[&uarr;](#back_to_top)

##### Working directory <a id="work"></a>

To locate a file in a computer, one has to specify the *filename* and the directory in which the file is stored; this is sometimes called the *path* of the file. The "working directory" for **R** is either the directory in which the **R**-program has been installed, or some other directory, as initialized by a startup script. You can execute the command <code>getwd()</code> to list what the "Working Directory" is currently set to:

```r
> getwd()
[1] "/Users/steipe/R"
```

It is convenient to put all your **R**-input and output files into a project specific directory and then define this to be the "Working Directory". Use the <code>setwd()</code> command for this. <code>setwd()</code> requires a parameter in its parentheses: a string with the directory path. Strings in **R** are delimited with <code>"</code> or <code>'</code> characters. If the directory does not exist, an Error will be reported. Make sure you have created the directory. On Mac and Unix systems, the usual shorthand notation for relative paths can be used: <code>~</code> for the home directory, <code>.</code> for the current directory, <code>..</code> for the parent of the current directory. 

On **Windows** systems, you need know that backslashes - "\" - have a special meaning for **R**, they work as *escape characters*. Thus **R** gets confused when you put them into string literals, such as Windows path names. **R** has a simple solution: simply replace all backslashes with forward slashes - "/" -, and **R** will translate them back when it talks to your operating system. Instead of <code>C:\documents\projectfiles</code> you write <code>C:/documents/projectfiles</code>.

###### task

* Create a directory for your sample files and use <code>setwd("<i>your-directory-name</i>")</code> to set the working directory. 

* Confirm that this has worked by typing <code>getwd()</code>. 


The *Working Directory* functions can also be accessed through the Menu, under **Misc**.

A nice shortcut on the Mac is that you can drag/drop a folder or file icon into the **R** console or a script window to get the full filename/path. *If you know of equivalent functionality in Linux or Windows, let me know.*

[&uarr;](#back_to_top)

##### .Rprofile - startup commands <a id="rprofile"></a>

Often, when working on a project, you would like to start off in your working directory right away when you start up **R**, instead of typing the <code>setwd()</code> command. This is easily done in a special **R**-script that is executed automatically on startup. The name of the script is <code>.Rprofile</code> and **R** expects to find it in the user's home directory. You can edit these files with a simple text editor like Textedit (Mac), Notepad (Windows) or Gedit (Linux) - or, of course, by opening it in **R** itself. 

Besides setting the working directory, other items that might go into such a file could be

* libraries that you often use

* constants that are not automatically defined

* functions that you would like to preload.

[&uarr;](#back_to_top)

###### ... Unix Systems <a id="unix"></a>

*Navigate to your home directory (<code>cd ~</code>).

*Open a textfile

*Type in: <code>setwd("/path/to/your/project")</code>

*Save the file with a filename of <code>.Rprofile</code>. (Note the dot prefix!)

[&uarr;](#back_to_top)

###### ... Mac OS X Systems <a id="macs"></a>

On Macs, filenames that begin with a dot are not normally shown in the Finder. Either you can open a terminal window and use <code>nano</code> to edit, instead of Textedit. Or, you can configure the Finder to show you such so-called "hidden files" by default. To do this:

* Open a terminal window;

* Type: <code>$defaults write com.apple.Finder AppleShowAllFiles YES</code>

* Restart the Finder by accessing **Force quit** (under the Apple menu), selecting the Finder and clicking **Relaunch**.

* If you ever want to revert this, just do the same thing but set the default to <code>NO</code> instead.

In any case: the procedure is the same as for Unix systems. A text editor you can use is <code>nano</code> in a Terminal window.

[&uarr;](#back_to_top)

###### Windows Systems <a id="windows"></a>

Navigate to your home directory (use cd to change directory)
- Open a textfile (Notepad)

- Type in: 
```setwd("/path/to/your/project")```

- Save the file with a filename of .Rprofile

[&uarr;](#back_to_top)

##### Workspace <a id="workspace"></a>

During an **R** session, you might define a large number of variables, datastructures, load packages and scripts etc. All of this information is stored in the so-called "Workspace". When you quit **R** you have the option to save the Workspace; it will then be reloaded in your next session. 

*We can use the output of <code>ls()</code> as input to <code>rm()</code> to remove everything and clear the Workspace. (cf. <code>?rm</code> for details)*

```r
rm(list= ls()) 
> ls() 
character(0)
>
```

The **R** GUI has a *Workspace Browser* as a menu item.


[&uarr;](#back_to_top)

#### Packages <a id="packages"></a>

**R** has many powerful functions built in, but one of it's greatest features is that it is easily extensible. Extensions have been written by legions of scientists for many years, most commonly in the **R** programming language itself, and made available through [**CRAN** - The Comprehensive R Archive Network](http://cran.r-project.org/) or through the [**Bioconductor project**](http://www.bioconductor.org). 

A package is a collection of code, documentation and (often) sample data. To use packages, you need to install them (once), and add them to your current session (for every new session). You can get an overview of installed and loaded packages by opening the **Package Manager** window from the **Packages & Data** Menu item. It gives a list of available packages you currently have *installed*, and identifies those that have been *loaded* at startup, or interactively.

###### task

* Navigate to http://cran.r-project.org/web/packages/ and read the page.

* Navigate to http://cran.r-project.org/web/views/ (the **curated** CRAN task-views).

* Follow the link to **Genetics** and read the synopsis of available packages. The library <code>sequinr</code> sounds useful, but check first whether it is already installed.

* In the **R packages available** window, confirm that  <code>seqinr</code> is not yet installed.

* Follow the link to  <code>seqinr</code> to see what standard information is available with a package. Then follow the link to **Reference manual** to access the documentation pdf. This is also sometimes referred to as a "vignette" and contains usage hints and sample code.

* The fact that these methods work, shows that the package has been downloaded, installed, the library has been loaded and its functions and data are now available in the current environment. Just like many other packages, <code>seqinr</code> comes with a number of data files. Try: 

```r
?data
data(package="seqinr")   # list the available data
data(aaindex)            # load ''aaindex''
?aaindex                 # what is this?
aaindex$FASG890101       # two of the indices ...
aaindex$PONJ960101

# plot amino acid codes by hydrophobicity and volume
plot(aaindex$FASG890101$I, aaindex$PONJ960101$I, xlab="hydrophobicity", ylab="volume", type="n")
text(aaindex$FASG890101$I, aaindex$PONJ960101$I, labels=a(names(aaindex$FASG890101$I)))
```


* Just for fun and demonstration, let's use these functions to download a sequence and calculate some statistics (however, not to digress too far, without further explanation at this point). Copy the code below and paste it into the **R**-console

```r
choosebank("swissprot")
query("mySeq", "N=MBP1_YEAST")
mbp1 <- getSequence(mySeq)
closebank()
x <- AAstat(mbp1[[1]])
barplot(sort(x$Compo))
```

The function <code>require()</code> is similar to <code>library()</code>, but it does not produce an error when it fails because the package has not been installed. It simply returns <code>TRUE</code> if successful or <code>FALSE</code> if not. If the library has already been loaded, it does nothing. Therefore I usually use the following code paradigm in my **R** scripts to avoid downloading the package every time I need to run a script:

```r
if (!require(seqinr)) {
    install.packages("seqinr")
    library(seqinr)
}
```

Note that <code>install.packages()</code> takes a (quoted) string as its argument, but <code>library()</code> takes a name (no quotes). New users usually get this wrong :-)

As is often the case, one of the challenges is to find the right package that contains a particular function you might be looking for. In **R** there is a package to help you do that. Try this:

```r
if (!require(sos)) {
    install.packages("sos")
    library(sos)
}

findFn("moving average")
```

Note that the **Bioconductor** project has its own installation system the <code>bioclite()</code> function. It is explained [**here**](http://www.bioconductor.org/install/).

[&uarr;](#back_to_top)

#### Scripts <a id="scripts"></a>

My preferred way of working with **R** is not to type commands into the console. **R** has an excellent script editor which I use by opening a new file - a script - and entering my **R** commands into the editor window. Then I execute the commands directly from the script. I may try things in the console, experiment, change parameters *etc.* - but ultimately everything I do goes into the file. This has four major advantages:

* The script is an accurate record of my procedure so I know exactly what I have done;

* I add numerous comments to record what I was thinking when I developed it;

* I can immediately reproduce the entire analysis from start to finish, simply by rerunning the script;

* I can reuse parts easily, thus making new analyses quick to develop.


###### task

* Use the *File* menu to open a *New Document* (on Mac) or *New Script* (on Windows).

* Enter the following code (copy from here and paste):

```r
# sample script:
# define a vector
a <- c(1, 1, 2, 3, 5, 8, 13)
# list its contents
a
# calculate the mean of its values
mean(a)
```

* save the file in your working directory (e.g. with the name <code>sample.R</code>).

Placing the cursor in a line and pressing <code>command-return</code> (on the Mac, <code>ctrl-r</code> on Windows) will execute that line and you see the result on the console. You can also select more than one line and execute the selected block with this shortcut. Alternatively, you can run the entire file. In the console type:

```r
source("sample.R")
```

However: this will not print output to the console. When you run a script, if you want to see text output you need to explicitly <code>print()</code> it.

*Change your script to the following, save it and <code>source()</code> it.

```r
# sample script:
# define a vector
a <- c(1, 1, 2, 3, 5, 8, 13)
# list its contents
print(a)
# calculate the mean of its values
print(mean(a))
```

* Confirm that the <code>print(a)</code> command also works when you execute the line directly from the script.



Nb. if you want to save your output to file, you can divert it to a file with the <code>sink()</code> command. You can read about the command by typing:

```r
?sink
```

Some useful shortcuts:

* Typing an opening parenthesis automatically types the closing parenthesis.

* Typing a newline character automatically indents the following line.

* Selecting some text and typing a quotation mark quotes the text. This also works with single quotation marks, parentheses, square brackets and curly braces.

[&uarr;](#back_to_top)

###  Simple Commands  <a id="commands"></a>

The **R** command line evaluates expressions. Expressions can contain constants, variables, operators and functions of the various datatypes **R** recognizes.

[&uarr;](#back_to_top)

#### Operators <a id="operators"></a>

The common arithmetic operators are recognized in the usual way. Try the following operators on numbers:

```r
5
5 + 3
5 + 1 / 2
3 * 2 + 1
3 * (2 + 1)
2^3 # Exponentiation
8 ^ (1/3) # Third root via exponentiation
7 %% 2  # Modulo operation (remainder of integer division)
7 %/% 2 # Integer division
```

[&uarr;](#back_to_top)

#### Functions <a id="functions"></a>

**R** is considered an (impure) [functional programming language](https://en.wikipedia.org/wiki/Functional_programming) and thus the focus of **R** programs is on functions. They key advantage is that this encourages programming without side-effects and this makes it easier to reason about the correctness of programs. Arguments are passed into functions as parameters, and results are returned. The return values can either be assigned to a variable, or used directly as the parameter in another function. 

Functions are either *built-in*, loaded in specific packages (see above), or they can be easily defined by you (see below). In general a function is invoked through a name, followed by one or more arguments (also *parameters*) in parentheses, separated by commas. Whenever I refer to a function, I write the parentheses to identify it as such and not a constant or other keyword eg. <code>log()</code>. Here are some examples for you to try and play with:

```r
cos(pi) #"pi" is a predefined constant.
sin(pi) # Note the rounding error. This number is not really different from zero.
sin(30 * pi/180) # Trigonometric functions use radians as their argument - this conversion calculates sin(30 degrees)
exp(1) # "e" is not predefined, but easy to calculate.
log(exp(1)) # functions can be arguments to functions - they are evaluated from the inside out.
log(10000) / log(10) # log() calculates natural logarithms; convert to any base by dividing by the log of the base. Here: log to base 10.
exp(complex(r=0, i=pi)) #Euler's identity
```

There are several ways to populate the argument list for a function and **R** makes a reasonable guess what you want to do. Consider the specification of a complex number in Euler's identity above. The function <code>complex()</code> can work with a number of arguments that are given in the documentation (see: <code>?complex</code>). These include <code>length.out</code>, <code>real</code>, <code>imaginary</code>, and some more. The <code>length.out</code> argument creates a vector with one or more complex numbers. If nothing else is specified, this will be a vector of complex zero(s). If there are two, or three arguments, they will be placed in the respective slots. However, since the arguments are **named**, we can also define which slot of the argument list they should populate. Consider the following to illustrate this:

```r
complex(1)
complex(4)
complex(1, 2) # imaginary part missing - defaults to zero
complex(1, 2, 3) # one complex number
complex(4, 2, 3) # four complex numbers
complex(real = 0, imaginary = pi) # defining via named parameters
complex(imaginary = pi, real = 0) # same thing - if names are used, order is not important
complex(re = 0, im = pi) # names can be abbreviated ...
complex(r = 0, i = pi) # ... to the shortest string that is unique among the named parameters. Use this with discretion to keep your code readable.
complex(i = pi, 1, 0) # Think: what have I done here? Why does this work?
exp(complex(i = pi, 1, 0)) # (The complex number above is the same one as in Euler's identity.)
```

[&uarr;](#back_to_top)

#### Variables <a id="variables"></a>

In order to store the results of evaluations, you can freely assign them to variables. Variables are created internally whenever you first use them (*i.e.* they are allocated and instantiated). Variable names are case sensitive. There are a small number of reserved strings, and a very small number of predefined constants, such as <code>pi</code>. However these constants can be overwritten - be careful. Read more at:

 ```r
?make.names
?reserved
```

To assign a value to a constant, use the assignment operator <code> &lt;- </code>. This is the default way of assigning values in **R**. You could also use the <code>=</code> sign, but there are subtle differences. (See: <code>?"&lt;-"</code>). There is a variant of the assignment operator <code> &lt;&lt;- </code> which is sometimes used inside functions. It assigns to a global context. This is possible, but not preferred since it generates a side effect of a function.

```r
a <- 5
a
a + 3
b <- 8
b
a + b
a == b # not assignment: equality test
a != b # not equal
a < b  # less than

```

Note that **all** of **R**'s data types (as well as functions and other objects) can be assigned to variables.

There are very few syntactic restrictions on variable names ([discussed eg. here](http://stackoverflow.com/questions/9195718/variable-name-restrictions-in-r)) but this does not mean esoteric names are good. For the sake of your sanity, use names that express the meaning of the variable, and that are unique. Many **R** developers use <code>dotted.variable.names</code>, my personal preference is to write <code>camelCaseNames</code>. And while the single letters <code>c f n s Q</code> are syntactically valid variable names, they coincide with commands for the debugger browser and will execute debugger commands, rather than displaying variable values when you are debugging. . Finally, try not to use variable names that coincide with parameter labels in functions. Alas, you see this often in code, but in my opinion this is intellectually lazy and leads to code that is hard to read.

```r
# I don't like...
col <- c("red", "grey")
hist(rnorm(200), col=col)

# I prefer...
rgStripes <- c("red", "grey")
barplot(1:10, col=rgStripes)

```

[&uarr;](#back_to_top)

### Scalar Data <a id="scalars"></a>

*Scalars* are single numbers, the "atomic" parts of more complex datatypes. We have covered many of the properties of scalars above, e.g. the use of constants and their assignment to variables. To round this off, here are some remarks on the types of scalars **R** uses and on *coercion*, or casting one datatype into another. The following scalar types are supported:

* Boolean constants: <code>TRUE</code> and <code>FALSE</code>. This type has the "mode" *logical*;
* Integers, floats (floating point numbers) and complex numbers.  These types have the mode *numeric*;
* Strings. These have the mode *character*.

Other modes exist, such as <code>list</code>, <code>function</code> and <code>expression</code>, all of which can be combined into complex objects.

The function <code>mode()</code> returns the mode of an object and <code>typeof()</code> returns its type. Consider:

```r
a <- 3 > 5; a; mode(a); typeof(a) # Note: a > 5 is a logical expression, its value is FALSE.
a <- 3 < 5; a; mode(a); typeof(a)

a <- 3.0;   a;  mode(a); typeof(a) # Double precision floating point number
a <- 3.0e0; a;  mode(a); typeof(a) # Same value, exponential notation

a <- 3;     a;  mode(a); typeof(a) # Note: numbers are double precision floats by default.
a <- as.integer(3);  a;  mode(a); typeof(a) # If we really want an integer, we must coerce to type integer.

a <- "3"; a;  mode(a); typeof(a) # Forcing the number to be interpreted as a character.

# More coercions. For each of these, first think what result you would expect:
as.numeric("3") # character as numeric
as.numeric("3.141592653") # string as numeric
as.numeric("pi") # another string as numeric
as.numeric(pi) # not a string, but a predefined constant

as.logical(0)
as.logical(1)
as.logical(-1)
as.logical(pi) # any non-zero number is TRUE ...
as.logical("pi") # ... but not non-numeric types. NA is "Not Available".
```

[&uarr;](#back_to_top)

### Vectors <a id="vectors"></a>

Since we (almost) never do statistics on scalar quantities, **R** obviously needs ways to handle collections of data items. In its the simplest form such a collection is a **vector**: an ordered list of items of the same type. Vectors are created from scratch with the <code>c()</code> function which **c**oncatenates individual items into a list, or with various sequencing functions. Vectors have properties, such as length; individual items  in vectors can be combined in useful ways. All elements of a vector must be of the same type. If they are not, they are coerced silently to the most general type (which is often <code>character</code>).

```r
#Create a vector and list its contents and length:
f <- c(1, 1, 3, 5, 8, 13, 21)
f
length(f)

# Various ways to retrieve values from the vector.
f[1] # By index: "1" is first element. 
f[length(f)] # length() is the index of the last element.
1:4 # This is the range operator
f[1:4] # using the range operator (it generates a sequence and returns it in a vector)
f[4:1] # same thing, backwards
seq(from=2, to=6, by=2) # The seq() function is a flexible, generic way to generate sequences
seq(2, 6, 2) # Same thing: arguments in default order
f[seq(2, 6, 2)]

# since a scalar is a vector of length 1, does this work?
5[1]


# ...using an index vector with positive indices
a <- c(1, 3, 4, 1) # the elements of index vectors must be valid indices of the target vector. The index vector can be of any length.
f[a] # Here, four elements are retrieved from f[]

# ...using an index vector with negative indices
a <- -(1:4) # If elements of index vectors are negative integers, the corresponding elements are excluded.
f[a] # Here, the first four elements are omitted from f[]
f[-((length(f)-3):length(f))] # Here, the last four elements are omitted from f[]

# ...using a logical vector
f>4 # A logical expression operating on the target vector returns a vector of logical elements. It has the same length as the target vector.
f[f>4]; # We can use this logical vector to extract only elements for which the logical expression evaluates as TRUE


# Example: extending the Fibonacci series for three steps. 
# Think: How does this work? What numbers are we adding here and why does the result end up in the vector?
f <- c(f, f[length(f)-1] + f[length(f)]); f 
f <- c(f, f[length(f)-1] + f[length(f)]); f 
f <- c(f, f[length(f)-1] + f[length(f)]); f 


# coercion
c(1, 2.0, "3", TRUE)
[1] "1"    "2"    "3"    "TRUE" 
```

Many operations on scalars can be simply extended to vectors and **R** computes them **very** efficiently by iterating over the elements in the vector.

```r
f
f+1
f*2

# computing with two vectors of same length
a <- f[-1]; a # like f[], but omitting the first element
b <- f[1:(length(f)-1)]; b # like f[], but shortened by the least element
c <- a / b # the "golden ratio", phi (~1.61803 or (1+sqrt(5))/2 ), an irrational number, is approximated by the ratio of two consecutive Fibonacci numbers.
c
abs(c - ((1+sqrt(5))/2)) # Calculating the error of the approximation, element by element
```


[&uarr;](#back_to_top)

### Matrices <a id="matrices"></a>

If we need to operate with several vectors, or multi-dimensional data, we make use of *matrices* or more generally *k*-dimensional *arrays* **R**. Matrix operations are very similar to vector operations, in fact a matrix actually is a vector for which the number of rows and columns have been defined.

The most basic form of such definition is the <code>dim()</code> function. Consider:

```r
a <- 1:12; a
dim(a) <- c(2,6); a
dim(a) <- c(2,2,3); a
```

<code>dim()</code> also allows you to retrieve the number of rows and columns. For example:

```r
dim(a)    # returns a vector
dim(a)[3]  # only the third value of the vector
```

If you have a two-dimensional matrix, the function <code>nrow()</code> and <code>ncol()</code> will also give you the number of rows and columns, respectively. Obviously, <code>dim(mat)[1]</code> is the same as <code>nrow(a)</code>.

As an alternative to <code>dim()</code>, matrices can be defined using the <code>matrix()</code> or <code>array()</code> functions (see there), or "glued" together from vectors by rows or columns, using the  <code>rbind()</code> or  <code>cbind()</code> functions respectively:

```r
a <- 1:4
b <- 5:8
c <- rbind(a, b); c
d <- cbind(a, b); d
e <- cbind(d, 9:12); e
```

Addressing (retrieving) individual elements or slices from matrices is simply done by specifying the appropriate indices, where a missing index indicates that the entire row or column is to be retrieved

```r
e[1,] # first row
e[,2] # second column
e[3,2] # element at index row=3, column = 2
e[3:4, 1:2] # submatrix
```

Note that **R** has numerous functions to compute with matrices, such as transposition, multiplication, inversion, calculating eigenvalues and eigenvectors and more.

[&uarr;](#back_to_top)

### Lists

While the elements of matrices and arrays all have to be of the same type, lists are more generally ordered collections of *components*. Lists are created with the <code>list()</code> function, which works similar to the <code>c()</code> function. Components are accessed through their index in double square brackets, or through their name, if the name has been defined. Here is an example:

```r
pUC19 <- list(size=2686, marker="ampicillin", ori="ColE1", accession="L01397", BanI=c(235, 408, 550, 1647) )
pUC19[[1]]
pUC19[[2]]
pUC19$ori
pUC19$BanI[2]

```


[&uarr;](#back_to_top)

### Data frames <a id="data_frames"></a>

Data frames combine features of lists and matrices, they are one of the most important data objects in **R**, because the result of reading an input file is usually a data frame. Lets create a little datafile and save it in the current working directory. You can use the "New document" command from the menu and save the following data as e.g. <code>vectors.tsv</code> (".tsv" for "tab separated values").

```r
 Name	Size	Marker	Ori	Sites
 pUC19	2686	Amp	ColE1	EcoRI, SacI, SmaI, BamHI, XbaI, PstI, HindIII
 pBR322	4361	Amp, Tet	ColE1	EcoRI, ClaI, HindIII
 pACYC184	4245	Tet, Cam	p15A	ClaI, HindIII
 ```

This data set uses tabs as column separators and it has a header line. Similar files can be exported from Excel or other spreadsheet programs. Read this as a data frame as follows:

```r
Vectors <- read.table("vectors.tsv", sep="\t", header=TRUE, stringsAsFactors = FALSE)
Vectors
```

Note the argument <code>stringsAsFactors = FALSE</code>. If this is <code>TRUE</code> instead, **R** will convert all strings in the input to factors and this may lead to problems. Make it a habit to turn this behaviour off, you can always turn a column of strings into factors when you actually mean to have factors.

You can edit the data through a spreadsheet-like interface with the <code>edit()</code> function.

```r
V2 <- edit(Vectors)
```


Here is a collection of examples of subsetting data from this frame:

```r
Vectors[1, ]
Vectors[2, ]
Vectors[ ,2 ]

Vectors$Name

Vectors$Size > 3000
Vectors$Name[Vectors$Size > 3000]
Vectors$Name[Vectors$Ori != "ColE1"]

Vectors[order(Vectors$Size), ]

grep("Tet", Vectors$Marker)
Vectors[grep("Tet", Vectors$Marker), ]
Vectors[grep("Tet", Vectors$Marker), "Ori"]
as.vector(Vectors[grep("Tet", Vectors$Marker), "Ori"])

```

[&uarr;](#back_to_top)

### Writing Functions <a id="writing_functions"></a> 

Writing your own functions in **R** is easy and gives you access to flexible, powerful and reusable solutions. Functions are assigned to function names and invoking the function returns some value, vector or other data object. Data gets **into** the function via the function's arguments, data is **returned** from a function in the <code>return()</code> statement. One and only one object is returned from a function. However the object can be a list, and thus contain values of arbitrary complexity. 

The **scope** of functions is local, all variables within a function are lost upon return, and global variables are not overwritten by a definition within a function. However variables that are defined outside the function are also available inside.

Here is a simple example: a function that takes a binomial species name as input and creates a five-letter code as output:

```r
biCode <- function(s) { 
	substr(s, 4, 6) <- substr(strsplit(s,"\\s+")[[1]][2], 1, 2)
	return (toupper(substr(s, 1, 5)))
}

biCode("Homo sapiens")              # HOMSA
biCode("saccharomyces cerevisiae")  # SACCE
```

We can use loops and control structures inside functions. For example the following creates a series of *n* Fibonacci numbers.

```r
fibSeq <- function(n) { 
   if (n < 1) { return( c(0) ) }
   else if (n ==  1) { return( c(1) ) }
   else if (n ==  2) { return( c(1, 1) ) }
   else {
      v <- c(1, 1)
      for ( i in 3:n ) {
         v <- c(v, v[length(v)-1] + v[length(v)])
      }
      return( v )
   }
}
```


##### Coding style <a id="style"></a>

Code is read much more often than it is written and it should always be your goal to write as clearly as possible. **R** has many complex idioms, and as a functional language that can generally insert functions anywhere into expressions it is possible to write very terse, expressive code. Don't do it. Pace yourself, and make sure your reader can follow your flow of thought. More often than not the poor soul who will be confused by a particularly witty use of the language will be you, yourself, half a year later. There is an astute observation by Brian Kernighan that applies completely:

> Debugging is twice as hard as writing the code in the first place. Therefore, if you write the code as cleverly as possible, you are, by definition, not smart enough to debug it.


A number of guides for **R** coding style are available but I recommend:
 
* Use informative **filenames** for code sources; give them the extension <code>.R</code>

* Give your **sources** headers stating purpose, author, date and version information, and note bugs and issues.

* Give your **functions** headers that describe purpose, arguments (including required datatypes), and return values. Callers should be able to work with the function without having to read the code.

* Use lots of **comments**. Never describe what the code does, but explain **why**. 

* Use **separators** (<code># --- SECTION -----------------</code>) to structure your code.

* **Indent** comment hashes to align with the expressions in a block.

* Use only <code><-</code> for assignment, not <code>=</code>

* ...but do use <code>=</code> when defining parameters of functions.

* Don't use <code><<-</code> (global assignment) except in very unusual cases.

* Use <code>camelCase</code> for variable names, never use the <code>confusing.dot.style</code>.

* Define parameters at the beginning of the code, use all caps variable names (<code>MAXWIDTH</code>). Never have "magic numbers" appear in your code.

* In mathematical expressions, always use **parentheses** to define priority. Never rely on convention. <code>(( 1 + 2 ) / 3 ) * 4</code>

* Always separate operators and arguments with spaces.

* Never separate function names and the brackets that enclose argument lists.

* Don't abbreviate argument names.

* Try to limit yourself to ~80 characters per line. 

* Always use braces <code>{</code>}, even if you write single line if statements and loops.

* Always use a **space** after a comma, and never before a comma.

* Always **explicitly return** values from functions, never rely on returning the last expression.

* Use spaces to **align** repeating parts of code, so errors become easier to spot.

* **Don't repeat** code. Use functions instead.

* Write out crucial arguments to functions, even if you think you know that this is redundant with the default.

* Never reassign reserved words.

* Don't use <code>c</code> as a variable name since <code>c()</code> is a function.

* Don't use semicolons, and don't write more than one command on a line.

* Don't use <code>attach()</code>.

* Use <code>for (i in seq(along=x)) {...</code>}   not <code>for (i in 1:length(x)) {...</code>} because of an unwanted loop if <code>x ###  NULL</code>

* If possible, do not grow data structures, but create the whole structure with <code>NULL</code> values, then modify them. This is **much** faster


###### Specific naming conventions I like:

<code>isValid</code>, <code>hasNeighbour</code>  ... Boolean variables  
<code>findRange()</code>, <code>getLimits()</code> ... simple function names  
<code>initializeTable()</code> ... not <code>initTab()</code>  
<code>node</code> ... one element; <code>nodeVector</code> ... more elements  
<code>nPoints</code> ... for number-of  
<code>setNo</code>  ... identify an individual  
<code>isError</code> ... not <code>isNotError</code>: avoid double negation  


Consider using the [**formatR**](http://cran.r-project.org/web/packages/formatR/index.html) package for consistent code.


And seriously - [XKCD: Code Quality](http://xkcd.com/1513/)

[&uarr;](#back_to_top)

#### Debugging <a id="debugging"></a>

Don't even *think* of sprinkling <code>print()</code> statements into your code to help you find out where something went wrong, when it goes wrong. From the beginning of your programming work, make yourself familiar with the debug functions. There are three simple concepts to remember: 

* Debugging is done by entering a "browser" mode that allows you to step through a function.

* Call <code>debug(*function*)</code> to invoke the mode when the function is next executed,  <code>undebug(*function*)</code> to clear the debugging mode.

* Insert <code>browser()</code> into your function code to enter the browser mode. This sets a *breakpoint* into your function; use  <code>if (condition) browser()</code> to insert a *conditional breakpoint* (or watchpoint).

Here is an example: let's write a rollDice-function, i.e. a function that creates a vector of *n* integers between 1 and MAX - the number of faces on your die.

```r
rollDice <- function(len=1, MIN=1, MAX=6) {
	v <- rep(0, len)
    for (i in 1:len) {
    	x <- runif(1, min=MIN, max=MAX)
    	x <- as.integer(x)
    	v[i] <- x
    }
	return(v)
}
```

Lets try running this...

```r
rollDice()
table(rollDice(1000))
```

Problem: we see only values from 1 to 5. Why? Lets flag the function for debugging...

```r
debug(rollDice)
rollDice(10)
debugging in: rollDice(10)
debug at #1: {
    v <- rep(0, len)
    for (i in 1:len) {
        x <- runif(1, min = MIN, max = MAX)
        x <- as.integer(x)
    	v[i] <- x
    }
    return(v)
}
Browse[2]> 
debug at #2: v <- rep(0, len)
Browse[2]> 
debug at #3: for (i in 1:len) {
    x <- runif(1, min = MIN, max = MAX)
    x <- as.integer(x)
    v[i] <- x
}
Browse[2]> 
debug at #4: x <- runif(1, min = MIN, max = MAX)
Browse[2]> 
debug at #5: x <- as.integer(x)
Browse[2]> x   # Here we examine the current value of x
[1] 4.506351
Browse[2]> 
debug at #6: v[i] <- x
Browse[2]> 
debug at #4: x <- runif(1, min = MIN, max = MAX)
Browse[2]> v
[1] 4      # Aha: as.integer() truncates, but doesn't round!
Browse[2]> Q
```


We need to change the range of the random input values...

```r
rollDice <- function(len=1, MIN=1, MAX=6) {
	v <- rep(0, len)
    for (i in 1:len) {
    	x <- runif(1, min=MIN, max=MAX+1)
    	x <- as.integer(x)
    	v[i] <- x
    }
	return(v)
}
table(rollDice(1000))
```


Now the output looks correct.

```r
# Disclaimer: this function would be better
# written as ...

rollDice <- function(len=1, MIN=1, MAX=6) {
	return(as.integer(runif(len, min=MIN, max=MAX+1)))
}

# Check:
table(rollDice(1000))
# ... since runif() can return a vector of deviates,
# but we would not be able to check the value of
# individual trials.


# Disclaimer 2: the function relies on a side-effect of as.integer(), which is
# to drop the digits after the comma when it converts. More explicit and
# therefore clearer would be to use the function floor() instead. Here, the
# truncation is not a side effect, but the desired behaviour. This is
# actually important: there is no guarantee how as.integer() constructs an
# integer from a float, it could e.g. round, instead of truncating. But rounding
# would give a wrong distribution! An error that may be hard to spot. (You
# can easily try using the round() function and think about how the result is wrong.)

# A better alternative is thus to write:

rollDice <- function(len=1, MIN=1, MAX=6) {
	return(floor(runif(len, min=MIN, max=MAX+1)))
}


# Disclaimer 3
# A base R function exists that already rolls dice in the required way: sample()

table(sample(1:6, 1000, replace=TRUE))
```



For visual debugging with **R Studio**, see [**here**](http://www.r-bloggers.com/visual-debugging-with-rstudio/).

For a deeper excursion into **R** debugging, see [this overview by Duncan Murdoch at UWO](http://www.stats.uwo.ca/faculty/murdoch/software/debuggingR/debug.shtml), and [Roger Peng's introduction to R debugging tools](http://www.biostat.jhsph.edu/~rpeng/docs/R-debug-tools.pdf).


[&uarr;](#back_to_top)

#### Finishing <a id="finishing"></a> 

This concludes our introduction to **R**. We haven't actually done anything significant with the language yet, but we have developed the basic skills to begin. The next steps should be:

* read data;
* organize it;
* explore it with descriptive statistics;
* explore patterns, structures and relationships within the data graphically;
* formulate hypotheses;
* test the hypotheses.



[&uarr;](#back_to_top)


### Further reading and resources <a id="reading_resources"></a>

[**R** on Wikipedia](https://en.wikipedia.org/wiki/R_(programming_language))

[Introduction to **R** at CRAN](http://cran.r-project.org/doc/manuals/R-intro.html)

[User-contributed documents about **R** at CRAN](http://cran.r-project.org/other-docs.html)  &ndash; including for example E. Paradis' _**R** for Beginners_ and J. Lemon's _Kickstarting **R**_.

[The "Task-views" section of CRAN](http://cran.r-project.org/web/views/): thematically organized collections of **R**-packages.

[The **"Views"** section of Bioconductor](http://www.bioconductor.org/packages/release/BiocViews.html), and [Bioconductor annotated **workflows**](http://www.bioconductor.org/help/workflows/).

[**Quick-R** how-to's](http://www.statmethods.net/index.html)

[**R** tagged questions on *stackoverflow*](http://stackoverflow.com/tags/r/info )

[**Cross Validated** statistics questions on *stackexchange*](http://stats.stackexchange.com/).


[&uarr;](#back_to_top)


