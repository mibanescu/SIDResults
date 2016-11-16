
			How to use SIDResults

   SIDResults is a program designed to assist in processing
the results from an orienteering meet. In particular, what
this program does is to take the raw data from a SI-card 
download box, along with some files which describe the 
control numbers for the courses run as well as the names
of the people associated with a particular SI-card number,
and produce a text output listing people's finish times, 
places on various courses, and a splits listing.

   The assumption behind this program is that the meet has been
run without a computer on site (whether this is because power 
is not readily available, people didn't want to lug the 
machines and batteries to the start/finish, noone wants to 
risk the computer to the vaguearies of the weather, or the club 
just doesn't have access to a suitable laptop for the task). 
Thus, the download box serves to hold all the necessary 
information from the SI-cards (namely start, punch, and finish
times associated with each card number), while paper registration
holds information related to names associated with SI-card 
numbers (which is entered after the event), and this program
puts it all together into something easy to read and publish on 
the web.

   Note that this program is written in the programming language
Tcl/Tk.  This means that the program will be relatively portable
onto different computer systems; all you have to do is find an 
implementation of Tcl/Tk on your machine (some help is provided
for this below), and then SIDResults.tk should run whether your 
machine operates under Mac, Linux, or Windows/PC. 

_________________________________________________________________________
Installing Tcl/Tk


   First of all, in order to run this program you need to 
install Tcl/Tk (this will give you the shell, language 
interpreter, and other bits necessary to run programs written 
in this particular programming language). One way you can
install this programming language is the following:

   Visit www.tck.tk on the web. 
   Click "Get Tcl/Tk Now".
   Click "Visit ActiveSite's ActiveTcl page".
   Click "Get ActiveTcl", and then "Download"
      You will be asked to input some personal information, 
      but you do not need to enter anything here. Just leave
      everything blank if you want to, then click "Continue".
      In the end, you should not have to give away any personal
      information, and you should not have to pay anything.
   Choose the package which suits your computer.
   The download will be about 15-18 MB. 

   Note that there are other possible ways obtain and install
the Tcl/Tk system, including compiling your own. However, the 
above suggestion will likely prove to be easiest for most people. 
Note that Tcl and Tk come preinstalled on many Unix systems 
(including Mac OSX), and so you may not have to go through 
any of this. 

   After you download the Tcl/Tk package, unarchive the package
and then install it.  After installing, look for the program
"wish" (on some systems it may be called "Wish", "wish.ext", 
or "wish.app"). This is the program which actually interprets
the tcl code; to run SIDResults.tk, simply open it using wish.

   Note that on at least some Unix systems (again, including OS X), 
it will also be possible to run the program from the command line 
(as though it were a script); make sure wish is in your path, 
and that execute permission is set for SIDResults.tk. If you know what 
the above line meant and you find this still not working on your system, 
please let me know.

_________________________________________________________________________
Organizing the Input Files


   Next, the program works on a whole set of data files
(the raw data download and other files describing the 
courses, and the names of people with rented sticks
or old sticks which do not store names). Although you
could put these files anywhere, things will be much
simpler if you use the following organization; if you 
do this, then when you run the program, you will only
have to select the "working directory", i.e. the directory
containing the details of the particular event you 
are working on.

      Main Directory (call it whatever you want)

           SINamesGlobal (or just call it SINames)

           DirForEvent1 (call the directories whatever you want)
                SINames
                Courses.txt
                RawData.csv
           DirForEvent2
                SINames
                Courses.txt
                RawData.csv
           DirForEvent3
                SINames
               	Courses.txt
                RawData.csv

In other words, you have a main directory which contains 
the global SINames file and additional directories for 
each event. Then, each event directory contains its own
copy of the SINames, courses, and RawData files.  The 
"working directory" above would be, for example, DirForEvent3. 

Here is how to set up the three files which describe the event:

   SINames
   -------

   The SINames files are the easy way to tell the program
the name of the person using a given SI card in an event.
You do not need to have these files; there is a way to 
type in names to the program once it is running, but it is 
much quicker and easier to do this way.  

   There are two different kinds of people that we need
this information for: 1) People renting finger-sticks at 
the event, and will thus assign a different name to this 
stick number for every event,  and 2) People with old 
finger-sticks which do not store names internally, and thus 
we can store the names ourselves and not type them in every 
time.

   SINamesGlobal is the file with the card number and names
of people who use the same stick every event. You will only
have to make this file once, and occasionally add names to 
it when additinal people join the club who happen to own
old fingersticks. The file will look something like the 
following three lines:

221779 Josef Trzicky
239210 Terese Camp
239300 Ken. Hanson

   This is a simple text file; create it using NotePad, TextEdit,
or your own favorite text editor (do not use a word processor unless
you can save as a plain text file).  Note that the very first thing 
on the line is the SI card number (no leading spaces, tabs, etc), 
then there is one space, then the person's name.  The name can be 
as complex as needed (there is no need to have a first-name last-name 
form; anything after that first space will be what shows up as the name 
in the final results). 

   You will make a second SINames file to be placed in the 
event directory.  This second file is for the people renting sticks
(or otherwise having a name assigned to a stick number for that 
event only).  The format of the file will be the same as the other
one. 

   I find it easiest to take the stack of registration cards,
and divide it into two piles: people who own their stick, and
people who rented a stick.  Then, I open a new copy of SINames
in the event directory and start typing in the number and name
from each registration card in the "rented" stack.  (Because 
most of our club's rental sticks start with "20050" or "20060",
a couple of macros or appropriate copy-and-paste preparations
can make typing in the numbers even faster.)

   Note that if multiple entries exist for a given SI-card number,
the last entered name will be used. For example, the day-of-event
file will be used over the global file, and names entered later
in the file will be used over those entered earlier in the file.
This causes a probelm in the case of multiple people using the 
same SI-card on the same day; this can happen easily if sticks
are rented and an early runner returns leaving their stick to be 
rented out a second time that day. At the moment, the solution is
to look at the results and see whether the same name turns up 
suspiciously twice (or more), and then after checking registration 
records use the edit controls to change any names which are wrong.

   By request from people on systems with draconian filenaming rules
regarding filename extensions, the SINames files can now be named 
SINames.txt, SINamesGlobal.txt, etc. 

   Courses.txt
   -----------

   This file describes the controls, in order, for the various
courses of that day's event. 

   Here is an example file: 

White=65,66,67,58,68,61,62,69,70=2.1
Yellow=55,56,57,58,59,60,61,62,63,64=2.5
Orange=50,69,59,51,77,52,53,71=3.6
Brown=71,72,73,46,47,48=3.8
Green=71,72,74,40,43,46,47,48=5.8
Red=71,72,74,40,41,42,43,44,45,46,47,48=7.4

   Again, this is a plain text file.  The format is one course per
line, order the courses the same as you want them ordered in the 
final results.  For each line, first is the course name, followed
by an equal sign, followed by the control numbers in order for that
course separated by commas, followed by a second equal sign, and
finished with the length of the course in kilometers.  Don't include
more than two equal signs on the line (i.e. there is no way to 
have "=" in the name of a course). 

   Note that the names of the courses can be anything.  Here is 
the courses.txt file for a recent sprint event: 

White=41,60,42,63,40,59,71,53,72,52,73,51=2.0
Sprint East=62,63,64,65,66,67,68,69,70,40,41=2.2
Sprint West=51,52,53,54,55,56,57,58,59,60,61=2.4
 
   The program is not currently written to allow for a score-o
format. (It is on the to-do list.)


   RawData.csv
   -----------

   This is the raw data file which comes out of the download box
used at the event.  For our club at the present, Joseph Huberman 
takes the download box home, reads this raw datafile into his computer, 
and then emails it to whoever will be processing results.

   You can open this file in a text editor to examine the contents.
In fact, this is a good idea, just to make sure that the file contents
look reasonable. It is also possible to edit the values in the text
editor, for example if there is some problem which the SIDResults program 
cannot handle. 

   The first line of the file is a simple header describing the 
values in each field.  It is easy to spot the SI-Card (card number)
and name (stored as first and last; SIDResults will just join these together
into a single name field).  For punch times, CN stands for Control Number
and DOW is day-of-week. 

   Note that not all SI cards store the same values.  For example, 
older cards do not store clear or check times, nor do they store
the day-of-the-week for time values.  This is generally not a problem.
Old cards will store times in a 12 hour format, while new cards use
a 24 hour time; SIDResults converts times as needed.   It would be a 
good idea if all time values looked like hh:mm:ss. 


_________________________________________________________________________
Running the program


   Once the SINames, courses.txt and RawData.csv files are created and
in place, it is time to run the SIDResults program. 

(Minimal instructions included here. Needs to be expanded.)
Open using the program that came with Tcl/Tk called "wish"
(it may be something like "Wish" or "wish.exe")

   Once the program is running, here is how to process the results:

   First, you need to tell it where to find the files which were set up
above (courses.txt, etc).  The easiest way is to click the <Set Work Dir>
button, then use the file requestor to select the event directory
you made for this event.  If you do that, and all the files are named 
as I suggested, the program will automatically find everything.
(The program will tell you what it selected as the defaults for the 
raw file and course description file). 

   If you want to, you could instead point to a raw datafile or course
file. In that case, once you did that, SIDResults will try to use that
file's directory as a working directory (if you haven't already selected
one), and will try to find all the other files from there.

   (The program does not currently allow a different name for the SINames
files. That is on the to-do list.  For the moment, just name it as suggested.
Note that I have not tested the program in a Windoze environment, and 
do not know how it will react to a file without a type extension. There 
may be problems yet with SINames on such systems; please let me know if 
this is the case.  If you created SINames files, but still see lots of
*** BOK RENTAL *** names, this is where the problem likely lies.)

   Once all of the needed files have been located, the program will 
start processing results. Any warnings will be displayed in the small window
below the "Course Desc File" line. These warnings will mostly have to 
do with removal of duplicate downloads, with occasional mention of 
other difficulties encountered.

   Below the warnings window is a section for editing entries; I will
describe that later.

   Finally is the large section containing the results.

   NOTE: THERE IS A SMALL PROGRAM BUG WHICH MUST BE ACCOMIDATED:
Once results are processed, there will likely be some scroll-bars
attached to the main results window. For a little while, this window
will "wriggle"; it appears to be a resizeing issue associated with
either the scroll bars or an interaction with the warnings window. 
WAIT FOR THE WRIGGLES TO STOP BEFORE DOING ANYTHING. If you try
to scroll while the window is still moving about, the program will
crash.  The wriggles will likely start again upon resizing the main 
program window. Again, just wait for the shimmering to stop before 
doing anything.  Fixing this bug is definately on the to-do list.

   The first section of the results will contain a simple output line 
for every line read in from the raw data file; this is the so-called
"raw results section".  This section is mostly used while editing the 
results; you will want to edit this out of the final published results. 
However, the lines can be useful now for checking whether a "duplicate" 
line really was treated properly, or for checking for reasonableness in 
the pretty output.

   Followed by the raw output section is the actual publishable results.

   Clicking on any line in the results window will select all lines
associated with that entry.  For example, clicking on a name in the 
primary results section will also select that entry in the splits section
and in the raw results section. 

   Once an entry is selected, the edit buttons (in the middle section
I said I would talk about later) will be usable on that entry.  At the 
moment, there are three editable items: the name, the course, and 
toggling whether or not to include this entry.  If you enter a new 
name, make certain to hit <Return> to commit the name change.

   Following any edit, all results will be recalculated, and a note 
should be logged into the warnings section indicating the edit.

   Note that making changes using these editing fields will not make 
any change to the raw results section. This is useful, for example,
when an entry is deleted; its line in the raw results section will 
remain, and this is where you would be able to go to undelete that entry.

   Finally, when the results look correct, click the <Save Results>
button at the top right. This will output the contents of the results
window (including the raw results section) to a text file.  You can
then edit this file as you want (for example, making additional changes
to names, removing the raw results section,  or adding course director 
comments to the top of the results).

   The results will save as a plain text file at this point in time.
If you want the output to be in html form for web publishing, click
the <Turn HTML On> button. This button will toggle html code on or off.
With html toggled on, you will see the raw html commands (little 
tags like <html> or <title>, etc) scattered through the results section.
When you save the results in this form, you can read the resulting 
Results.html file into your web browser to see what it looks like. 

   At the moment, there is no way to save anything other than the final
results text file. In particular, there is no way to read in that saved 
file and continue with additional editing.  This is not a problem the
way the program is used currently (most data entry is to text files
outside the program), but should be kept in mind.

   The <Quit> button will exit the program with no additional warning.

   This program is currently in a "minimally usable" state of development.  
There likely exist a large number of easy ways to crash the program.  
It would be a good idea to be careful in its use.

   If you need to contact me, you can do so at SIDResults@mirrorlab.org.
(Please start your subject line with the keyword "SIDResults".  I might
see your message anyway even if you don't do this, but I would hate for 
your thoughtful email message to get lost among the spam.)

   I wrote this program in my spare time so that our local club 
(Backwoods Orienteering Klub, in North Carolina) could do something 
interesting, namely to run events using electronic punching without
the need of a computer on-site at events.  Neither I, nor the club,
have been paid anything to produce this software.  Any additional work on 
this program, including bug fixes and feature implementation will occur
as future spare time allows. That said, I welcome suggestions regarding
the program. In particular, if your orienteering club uses this software
I would like to hear from you.



					Ken. Hanson
					2008/07/31
					Last revision: 2008/09/16

