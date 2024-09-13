This repository shows off some of my programs written in WHILE below you can find the manual for this language.

hwhile Manual
hwhile is a command line While interpreter developed by Alex Jeffery (a former Limits student, now Sussex PhD student) under my supervision specifically for the Limits module.

Supported Language
It supports the WHILE language including extensions with the following few exceptions:

there is only one expression allowed in a case clause of a switch statement
the special atoms for the self-interpreter are available, but need to be preceded by @, e.g. write @while (see also Session "Programs as data objects"). Note that no @atoms other than those needed for the original self-interpreter. So @loveLimits will not parse!
the file passed to hwhile has to contain exactly one program and both, file and program, need to have the same name, but the file needs .while extension.
Examples of running hwhile follow further down.
Multiline comments of the form
(* this is a comment *)

can only be used before the start of the program, even if they are just one line long. Within programs you can only use the single line comments:
// this is a comment

On the other hand, hwhile supports tree literals. So, this means your programs can use constant trees using the <_._> constructor. So the tree literal <nil.<nil.nil>> can be written exactly in that way in programs for hwhile.

 
Running hwhile on your own machine
You need to run hwhile in your shell. Windows users need to use the command prompt window cmd. The command line usage of hwhile is as follows:

hwhile -FLAGS program.while INPUT
where FLAGS is a combination of characters that determine how the result is printed (unparsed).

Note that if you run a shell in a directory and have not set the PATH environment variable accordingly you might actually need to write

./hwhile -FLAGS program.while INPUT
Which flags are available can be checked with the command

hwhile -h
Moreover, INPUT is the input you want to pass to the program in question. This can be in form of a tree using the <_._> syntax,using double quotes, or much more conveniently, using list and number encodings as:

[[3,4],[5,6]]
or even programs as data encodings, where the atoms used as tags need @ as explained above. A few example calls are listed further down. If your input is long and stretches several lines (for instance, if you pass a program as input) then you must enclose your input in " ". I realised that using zsh on Macs with BigSur and later, also list arguments need to be enclosed in double quotes, so you have to use 

"[[3,4],[5,6]]"
Please note that hwhile does not run properly when you call it from cloud storage. It must always be installed locally.

 
Running hwhile in the Labs
hwhile is installed in Labs 1 and 2 (not the other Labs though).  

Go to the Software Hub. There find a box called "hwhile interpreter", just double click it and a shell window pops up. You need to run your interpreter in this shell window. (We don't have a GUI for this. But if you are interested in programming one, do get in touch with me.)
Make sure that when you call hwhile you qualify the while program files with the correct paths. The best way to deal with this is to first go into the directory in which you store your .while programs:. If you store them directly in your Documents folder, e.g.
cd N:\Documents
for instance.

After the prompt in the window you just write then
hwhile <flag> <file> <input>
as explained above. Your files will be on the N: drive, so for example to run file prog.while in your Documents folder with input [1,2,3] you would use
hwhile  prog.while [1,2,3]
The output (and potentially debugging info) will be displayed in the shell window.
Never move your files in the HWhile-Master directory on the C: drive though from which the interpreter is launched in the shell window.

You can edit your .while programs in any editor that produces ASCII output, so an editor from an IDE should be preferred, Notepad with ANSII output should work too. Don't write the programs in Word, of course :-), as this will contain formatting info .

 

 
Programs to Data
hwhile can also translate your While-program into data (object) format. This is useful when you want to test the self-interpreter. For this to work, simply use the -u flag:

hwhile -u add.while
will return the following

[ 0 , [
       [@:=, 1, [@hd, [@var, 0]]] ,
       [@:=, 2, [@hd, [@tl, [@var, 0]]]] ,
       [ @while, [@var, 1] , [ [@:=, 3, [@var, 2]] , [@:=, 3, [@cons, [@quote, nil], [@var, 3]]] , [@:=, 2, [@var, 3]] , [@:=, 4, [@var, 1]] , [@:=, 4, [@tl, [@var, 4]]] , [@:=, 1, [@var, 4]] ] ]
     ] 
, 2 ]
which is a pretty-printed encoding of

add.while
as data.

 
Sample calls
Assume you copied the hwhile binary into a directory together with your sample program add.while that contains the add program in to the same directory.

hwhile -i add.while [3,7]
will run the add program in the file add.while with the obvious input. Because of the -i flag the output will be printed as a number, in this case 10.

You can even drop the .while suffix and simply write:

hwhile -i add  [3,7]
Similarly,

hwhile -i add.while "<<nil.nil>.<nil.nil>>"
returns 1 as the input is equivalent to [1,0]. Note that the double quotes are important here to alert the  parser that the input is given as a tree and not as a list.

The hwhile interpreter can be run via a variety of flags that determine the way the result is printed, given that the language is untyped:

hwhile -dli reverse [1,2,3,4]
will run the reverse program in the file reverse.while with the obvious input. Because of the -li flag the output will be printed as a list of numbers, in this case the output is:

(reverse) Y := []
(reverse) Y := [1]
(reverse) X := [2, 3, 4]
(reverse) Y := [2, 1]
(reverse) X := [3, 4]
(reverse) Y := [3, 2, 1]
(reverse) X := [4]
(reverse) Y := [4, 3, 2, 1]
(reverse) X := []
[4, 3, 2, 1]
Because of the -d (for debug) flag, a trace of all assignments executed is printed too. This information is helpful when you debug your code, as WHILE has no print statement. The text in brackets, here always (reverse), is the name of the macro that has executed the assignment in question.
The trace mode outputs all trees (evaluated expressions) in the output mode. In the above case as lists of integers. In this case this makes sense, as all variables (in our view) have this "type".

For more complex programs this will not be the case. It is recommended that you use the debugger (see next section). You could run the interpreter in debug mode several times with different output flags, each time looking at a different variable for which the flag is appropriate. But this is tedious and you should use the debugger.
Note also that you may need to enclose your input expression in double quotes (" ") if it contains blanks (spaces) or angle brackets (!), or if it stretches several lines, as otherwise the interpreter will think you have supplied too many arguments. This is particularly important when oyu pass programs-as-data to the self-interpreter u.while. Here is an example:

hwhile -i add.while "<nil.<nil.nil>>"
or

hwhile -i add "[ 4,  2 ]"
Note that if you don't use blanks and no <.> then you don't need the inverted commas, e.g.;

hwhile -i add [4,2]

For Windows: In Windows the " " (inverted double quotes) only work in the PowerShell. In other shells you may need to put ^ at the end of each line. However, windows users can use CTRL+SHIFT+C when copying the output from "hwhile -u <program>", in which case they don't have to use a ^ at the end of each line when pasting this into another hwhile call.


Calling the self-interpreter u
The self-interpeter u can be called like this:

hwhile -li u "[[ 0
 , 
    [ [@:=, 1, [@quote, nil]]
    , 
        [ @while, [@var, 0]
        , 
            [ [@:=, 1, [@cons, [@hd, [@var, 0]], [@var, 1]]]
            , [@:=, 0, [@tl, [@var, 0]]]
             ]
        ]
    ]
, 1
]
,[1,2,3]]"
where on needs to be careful with multi-line input as explained above. In this case, it means the reverse program with input [1,2,3] is fed to the self-interpreter and we will get as result:

[3, 2, 1]
 

hwhile Debugger
The latest version of hwhile provides an interactive mode for debugging that is called with flag -r

hwhile -r
This gets you into interactive mode (a bit like hugs in Haskell). Yo do not need a program argument at this stage (done interactively later). You get a reply:

Welcome to HWhile interactive mode. Type ':help' for more information.
If you now type :help you'll see the options of a debugger. Note that the : prefix is essential and you need to type it every time. If you forget the : the debugger will misinterpret the command as file name and simply answer nil (the value of this variable) .

hWhile interactive mode.
Possible options for debug commands are:

<EXPR> - Evaluate a While expression.

<CMD>  - Execute a While command.

:help - Print this message.

:load p <EXPR> - Load a while program 'p' (i.e. from the file 'p.while', so you must drop the file extension .while here!) for execution with argument <EXPR>. Note that this clears the current store contents.

:run - Run the loaded program up until the next breakpoint.

:step - Step through a single line of the loaded program.

:store - Print the current store contents.

:printmode m - Set the print mode to mode 'm'. Valid modes are i, iv, l, li, liv, L, and La. Quit interactive mode and then run 'hwhile -h' for more info on print modes.

:cd dir - Change the current file search path to 'dir'.

:break n - Add a breakpoint to line 'n' of the loaded program.

:break n p - Add a breakpoint to line 'n' of program 'p'.

:break - Print all breakpoints.

:delbreak n - Delete the breakpoint on line 'n' of the loaded program.

:delbreak n p - Delete the breakpoint on line 'n' of program 'p'.

(Ctrl+D) - Quit interactive mode.
