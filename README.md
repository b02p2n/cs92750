java cChapter 5
Week 4: Creating a small shell interface
You must submit your work to the appropriate submission point in Gradescope, which will
be automatically marked. You should submit a single file called my_shell.c. Any other
files you submit will not be marked. Although you do not need to include any additional
supporting documentation or report, we do expect that your code is well written, tested and
commented.
Deadline: Week 6 of teaching. Thursday. 7th of Novem ber, 2024. 14:00. Extensions of up to 7 days are available.
Weighting: 40% of the final module mark.
In this coursework you will demonstrate:
• An understanding of how processes are created by the operating system.
• An understanding of file descriptors and their relationship to pipes and redirection.
• The ability to program components of an operating system.
Exercise
In this coursework you will implement a simple shell for the xv6 operating system. This
new shell will be implemented as a user space program. Before you attempt this coursework,
make sure you have gone through most of the formative assessment exercises in the preceding
weeks and convinced yourself that you know how various parts work. Where you have doubts,
read relevant parts again and redo the coursework, which will make you spot new things and
gain a deeper understanding of the material. You should provide your implementation in a
new file called my_shell.c. You may use any helper functions provided by the xv6 kernel or
user libraries. For each of the following items implement the feature into your shell, as you
progress the features to implement become harder. This exercise should not require you to
modify any file other than my_shell.c and the Makefile.
To start with clone the repository containing the starting code and copy my_shell.c
from it into your xv6 user/ directory:
29
$ git clone https://github.com/mmikaitis/COMP2211-shell-template.git
Modify the Makefile accordingly and rebuild xv6. It will not compile because my_shell.c is
not finalised yet. However, it also contains some comments that should help in finishing the
intended structure. Your task is to finish writing methods getcmd, run_command, and main,
by inserting code in the indicated locations. No other methods should be developed.
You are allowed to look at a default xv6 shell source code as well as
learn about implementing shells using external resources. However,
you are required to follow the unique structure outlined in the tem plate and are not allowed to supply any code which was not developed
solely by yourself, starting from design stage. If you depend highly
on some online tutorials then you need to declare the sources in the
comments, which includes large language models. If you discuss early
ideas with someone in the lab you should make sure that you don’t
end up with similar code structure; you should not code together.
Gradescope will run a similarity check of your submission and if the
logic of the new code is reported to be similar to someone else’s,
the submission will be carefully checked manually and reported as
academic integrity violation if required. See this website for some
detail. The similarity check is resilient to changing variables names
or adding comments and new lines.
Going through academic integrity interviews is a daunting process and
may result in severe delays to your degree progression. It is better
to submit nothing than submit the code that was partially developed
by others. If you are behind, speak to the lab demonstrators and the
module lead for guidance on best ways forward.
Part 1: Execute simple commands (5 Marks)
Implement the execution of simple commands. Your shell should be able to:
• Prompt the user for a command by printing “>>>” as a command prompt.
• Execute a command inputted to the command prompt.
• Loop indefinitely until the shell is exited.
• Handle the “cd” command—you will notice that this command will need to be treated
as a special case.
Do not f代 写program、C/C++
代做程序编程语言orget to stress-test your simple shell before moving on to advanced features. The
automatic marking will be testing it on various cases and marks will be deducted if it does
not work when the same command is provided in a different format, such as with extra
30
spacing. For example, consider (note the amount and location of space characters which
may impact the shell if they are not detected):
$ echo hello world
$ echo hello world
Once you are comfortable that you have tested your shell with any possible command that
could reveal bugs, move on to implement the following advanced features.
Part 2: Input/Output redirection (6 Marks)
Implement Input/Output redirection. Your shell should be able to handle two element
redirections. For example,
$ echo "Hello world" > temp
$ cat  myoutput
3. Implement the “;” operator that allows a list of shell commands to be given and
executed sequentially.
$ ls | grep test | cat > myoutput; cat myoutput
Marking
Gradescope will run 26 test commands and award a mark out of 25. The commands that will
be run are not disclosed and you are required to use creativity to think of various scenarios
which may break your shell and test it thoroughly before submitting. 3 out of 25 marks
31
will be awarded to those who spot three especially tricky cases of specifying commands and
implement their shells to get around them.
There are many ways to type commands, some straightforward as shown above, and
some not, such as when people type commands without using any spaces or with arbitrary
number of spaces in various places. Your shell should be resilient to this ambiguity in
specifying commands. Those students who spent more time in thinking about various test
cases and check them will get more marks than those who only try a few straightforward
commands listed above.
Here are a few example commands running in the new completed shell to get you started:
xv6 kernel is booting
hart 2 starting
hart 1 starting
init: starting sh
$ my_shell
>>> mkdir tempdir
>>> ls
. 1 1 1024
.. 1 1 1024
README 2 2 2292
cat 2 3 35080
echo 2 4 33960
forktest 2 5 16080
grep 2 6 38512
init 2 7 34424
kill 2 8 33888
ln 2 9 33712
ls 2 10 37016
mkdir 2 11 33952
rm 2 12 33936
sh 2 13 56504
stressfs 2 14 34816
usertests 2 15 179160
grind 2 16 49736
wc 2 17 36024
zombie 2 18 33288
my_shell 2 19 40032
console 3 20 0
tempdir 1 21 32
>>> cd tempdir
>>> ../ls
. 1 21 32
32
.. 1 1 1024
>>> cd ..
>>> ls
. 1 1 1024
.. 1 1 1024
README 2 2 2292
cat 2 3 35080
echo 2 4 33960
forktest 2 5 16080
grep 2 6 38512
init 2 7 34424
kill 2 8 33888
ln 2 9 33712
ls 2 10 37016
mkdir 2 11 33952
rm 2 12 33936
sh 2 13 56504
stressfs 2 14 34816
usertests 2 15 179160
grind 2 16 49736
wc 2 17 36024
zombie 2 18 33288
my_shell 2 19 40032
console 3 20 0
tempdir 1 21 32
>>> echo hello
hello
>>> echo hello
hello
>>> cat README | grep xv6
xv6 is a re-implementation of Dennis Ritchie’s and Ken Thompson’s Unix
Version 6 (v6). xv6 loosely follows the structure and style of v6,
xv6 is inspired by John Lions’s Commentary on UNIX 6th Edition (Peer
(kaashoek,rtm@mit.edu). The main purpose of xv6 is as a teaching
>>> cat README| grep xv6
xv6 is a re-implementation of Dennis Ritchie’s and Ken Thompson’s Unix
Version 6 (v6). xv6 loosely follows the structure and style of v6,
xv6 is inspired by John Lions’s Commentary on UNIX 6th Edition (Peer
(kaashoek,rtm@mit.edu). The main purpose of xv6 is as a teaching
>>>
It is worth to note that the default xv6 does not pass all of our expected tests. For
example:
33
xv6 kernel is booting
hart 2 starting
hart 1 starting
init: starting sh
$ cd .
$ cd .
cannot cd .
$ mkdir temp
$ cd temp
$ cd ..
$ cd temp
cannot cd temp
$
Submission
You are required to submit only my_shell.c. See Minerva.
34

         
加QQ：99515681  WX：codinghelp  Email: 99515681@qq.com
