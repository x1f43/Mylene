# NAME

Mylene3e8 - control a Mylene1f4 server

# SYNOPSIS

**Mylene3e8** \[*OPTIONS*\] *PATH*\|*PORT* *COMMAND* \[*ARGUMENT*\]...

# DESCRIPTION

**Mylene3e8** sends control commands to a running **Mylene1f4**(1)
server listening on a local UNIX domain socket (*PATH*) or a local
TCP/IP port (*PORT* on localhost).

# OPTIONS

**-i**, **--inet**  
Connect to the TCP/IP (INET) server at *PORT* on localhost.

**-l**, **--list**  
List available commands and exit.

**-u**, **--unix**  
Connect to the UNIX domain socket server at *PATH* (default).

**-x**, **--synchronize**  
Wait for command execution to complete before exiting.

**--help**  
Print a usage summary and exit with a status code indicating success.

**--version**  
Print version information to standard output and exit.

# COMMANDS

**abort**  
Abort execution of the currently running program.

**add** \[**album**\|**artist**\|**comment**\|**match**\|**title**\|**year** *STRING*\]... \[**current** *INTEGRAL*\]... *STRING*  
Add the file specified by the final *STRING* argument to the file list.
If the file list is tagged (as configured via **Mylene1f4**'s **-i** or
**--tagged-list** options), the file's artist, year, album, title, and
comment metadata may be supplied using the **artist** (or **match**),
**year**, **album**, **title**, and **comment** modifiers, respectively.
If the file list is not tagged, any supplied metadata modifiers are
ignored.

If no insertion position is specified (via **current**), the file is
appended to the end of the current file list.

If an insertion position is specified via **current** *INTEGRAL*, the
file is inserted before the item at index *INTEGRAL* (when
non-negative), or before the item at index (*list_length* + 1 +
*INTEGRAL*) when *INTEGRAL* is negative. If *INTEGRAL* is less than
-(*list_length* + 1) or greater than *list_length*, the command is
ignored.

The newly added file is assigned an initial list index equal to one plus
the initial index of the most recently added file.

**album** *STRING*  
Set the album filter criterion to match *STRING*.

**album/** *STRING*  
Set the album filter criterion to match *STRING* and refilter the
currently playing file (equivalent to **album** *STRING* followed by
**join**).

**album@**  
Set the album filter criterion to match the album property of the
currently playing file.

**album^**  
Reset the album filter criterion to **Mylene1f4**'s configured default.

**album^/**  
Reset the album filter criterion to **Mylene1f4**'s configured default
and refilter the currently playing file (equivalent to **album^**
followed by **join**).

**artist** *STRING*  
Set the artist filter criterion to match *STRING*.

**artist/** *STRING*  
Set the artist filter criterion to match *STRING* and refilter the
currently playing file (equivalent to **artist** *STRING* followed by
**join**).

**artist@**  
Set the artist filter criterion to match the artist property of the
currently playing file.

**artist^**  
Reset the artist filter criterion to **Mylene1f4**'s configured default.

**artist^/**  
Reset the artist filter criterion to **Mylene1f4**'s configured default
and refilter the currently playing file (equivalent to **artist^**
followed by **join**).

**attach**  
Instruct **Mylene1f4** to re-attach the system audio device.

**back**  
Equivalent to **back** **1**.

**back** *INTEGRAL*  
If **Mylene1f4**'s current load order is *random*, behaves the same as
**rand**; otherwise, skips backward by *INTEGRAL* files (including the
current file). If the target file does not match the active filter
criteria, the closest preceding matching file is selected instead. Any
active file repeat count is reset (as with **cycle** **off**).
Equivalent to **skip** -*INTEGRAL*.

**back\***  
Equivalent to **back\*** **0**.

**back\*** *INTEGRAL*  
Skip to the file at index *INTEGRAL*, counting from 0 at the end (tail)
of the file list toward the beginning (head). If *INTEGRAL* is negative,
the target index is computed as (*list_length* + *INTEGRAL*). If the
target file does not match the active filter criteria, the closest
following matching file is selected instead. Any active file repeat
count is reset (as with **cycle** **off**). Equivalent to **skip\***
-*INTEGRAL*.

**basename** *STRING*  
Set the basename filter criterion to match *STRING*.

**basename/** *STRING*  
Set the basename filter criterion to match *STRING* and refilter the
currently playing file (equivalent to **basename** *STRING* followed by
**join**).

**basename^**  
Reset the basename filter criterion to **Mylene1f4**'s configured
default.

**basename^/**  
Reset the basename filter criterion to **Mylene1f4**'s configured
default and refilter the currently playing file (equivalent to
**basename^** followed by **join**).

**begin** \[\[*HOURS*:\]*MINUTES*:\]*SECONDS*\[,*FRAMES*\]  
Set the playback start stream time to the specified time offset.

**begin^**  
Reset the playback start position to **Mylene1f4**'s configured default
start time (**-e**/**--start-time**) or start frame
(**-f**/**--start-frame**). Equivalent to **start^**.

**comment** *STRING*  
Set the comment filter criterion to match *STRING*.

**comment/** *STRING*  
Set the comment filter criterion to match *STRING* and refilter the
currently playing file (equivalent to **comment** *STRING* followed by
**join**).

**comment@**  
Set the comment filter criterion to match the comment property of the
currently playing file.

**comment^**  
Reset the comment filter criterion to **Mylene1f4**'s configured
default.

**comment^/**  
Reset the comment filter criterion to **Mylene1f4**'s configured default
and refilter the currently playing file (equivalent to **comment^**
followed by **join**).

**cycle**  
Toggle indefinite repeating of the currently playing file.

**cycle** *CARDINAL*  
Repeat the currently playing file *CARDINAL* times.

**cycle** **off**  
Disable repeating and advance to the next file after the current file
finishes.

**cycle** **on**  
Repeat the currently playing file indefinitely.

**cycle+** *INTEGRAL*  
Increase the repeat count of the currently playing file by *INTEGRAL*:
if repeating indefinitely, negative values act like **cycle** **off**
and non-negative values act like **cycle** *INTEGRAL*; if not currently
set to repeat, negative values have no effect and non-negative values
act like **cycle** *INTEGRAL*; if set to a finite repeat count,
*INTEGRAL* is added to that count (with a negative sum acting like
**cycle** **off**). Equivalent to **cycle-** -*INTEGRAL*.

**cycle-** *INTEGRAL*  
Decrease the repeat count of the currently playing file by *INTEGRAL*:
if repeating indefinitely, positive values act like **cycle** **off**
and non-positive values act like **cycle** -*INTEGRAL*; if not currently
set to repeat, positive values have no effect and non-positive values
act like **cycle** -*INTEGRAL*; if set to a finite repeat count,
*INTEGRAL* is subtracted from that count (with a negative result acting
like **cycle** **off**). Equivalent to **cycle+** -*INTEGRAL*.

**delete** *INTEGRAL*  
Remove the program at index *INTEGRAL* from the execution queue. If
*INTEGRAL* is negative, the target index is computed as
(*queue_length* + *INTEGRAL*). If the resolved index is out of range
(less than 0 or greater than or equal to *queue_length*), the command is
ignored.

**delete** **play** **class**\|**start** *INTEGRAL*  
Remove the program at index *INTEGRAL* from the **play class** (playback
finish) or **play start** (playback start) execution list. If *INTEGRAL*
is negative, the target index is computed as (*list_length* +
*INTEGRAL*). If the resolved index is out of range, the command is
ignored.

**deque**  
Remove the last program from the execution queue.

**deque** *CARDINAL*  
Remove the last *CARDINAL* programs from the execution queue.

**deque** **all**  
Remove all programs from the execution queue.

**detach**  
Instruct **Mylene1f4** to temporarily detach the system audio device.

**exit**  
Terminate **Mylene1f4**. On some systems, terminating an INET-mode
**Mylene1f4** server via a **Mylene3e8** command leaves its TCP port
blocked in **TIME_WAIT** for some time (unless **--reuse-address** was
enabled).

**filter** *STRING*  
Set the complex filter criterion to *STRING*, using the same syntax as
**Mylene1f4**'s **-F** / **--filter** options.

**filter/** *STRING*  
Set the complex filter criterion to *STRING* and refilter the currently
playing file (equivalent to **filter** *STRING* followed by **join**).

**filter^**  
Reset the complex filter criterion to **Mylene1f4**'s configured
default.

**filter^/**  
Reset the complex filter criterion to **Mylene1f4**'s configured default
and refilter the currently playing file (equivalent to **filter^**
followed by **join**).

**flush**  
Discard cached PCM audio data and restart decoding from the current
stream time offset. Any pending decoder setting changesâsuch as MPEG
Layer III amplification and equalizer settings modified via
**l3amplification**, **l3frequency**, and **l3level**âtake effect
immediately.

**format** *STRING*  
Query **Mylene1f4** for status information formatted according to
*STRING*, using standard **Mylene1f4** format string syntax. Syntax
errors in *STRING* are not caught gracefully and will trigger a protocol
error, closing the connection.

**insert** *INTEGRAL* *PROGRAM*  
Insert *PROGRAM* into the execution queue before the program at index
*INTEGRAL*. If *INTEGRAL* is negative, *PROGRAM* is inserted before the
element at index (*queue_length* + 1 + *INTEGRAL*). If *INTEGRAL* is
less than -(*queue_length* + 1) or greater than *queue_length*, the
command is ignored.

**insert** *INTEGRAL* *STRING* *PROGRAM*  
Insert *PROGRAM* with title *STRING* into the execution queue before the
program at index *INTEGRAL*. If *INTEGRAL* is negative, *PROGRAM* is
inserted before the element at index (*queue_length* + 1 + *INTEGRAL*).
If *INTEGRAL* is less than -(*queue_length* + 1) or greater than
*queue_length*, the command is ignored.

**insert** **play** **class**\|**start** *INTEGRAL* *PROGRAM*  
Insert *PROGRAM* into the **play class** (playback finish) or **play
start** (playback start) execution list before the program at index
*INTEGRAL*. If *INTEGRAL* is negative, *PROGRAM* is inserted before the
element at index (*list_length* + 1 + *INTEGRAL*). If *INTEGRAL* is less
than -(*list_length* + 1) or greater than *list_length*, the command is
ignored.

**insert** **play** **class**\|**start** *INTEGRAL* *STRING* *PROGRAM*  
Insert *PROGRAM* with title *STRING* into the **play class** (playback
finish) or **play start** (playback start) execution list before the
program at index *INTEGRAL*. If *INTEGRAL* is negative, *PROGRAM* is
inserted before the element at index (*list_length* + 1 + *INTEGRAL*).
If *INTEGRAL* is less than -(*list_length* + 1) or greater than
*list_length*, the command is ignored.

**instant** *CARDINAL* *CARDINAL* *PROGRAM*  
Execute *PROGRAM* immediately within the execution context of the host
program identified by the first *CARDINAL* (major ID) and second
*CARDINAL* (minor ID). The host program must be an active
execution-queue or context-list program that has already started
execution.

*PROGRAM* runs without delay and shares the host program's memory
allocator and global variables (such as **context**), allowing it to
inspect, modify, or inject data structures and functions into the host
program.

**join**  
Refilter the currently playing file. If it matches the active filter
criteria, playback continues uninterrupted; otherwise, the next matching
file in the current load order is loaded and any active file repeat
count is cleared (as with **cycle** **off**).

**l3amplification** *REAL*  
Set MPEG Layer III audio stream amplification to *REAL* (matching
**Mylene1f4**'s **--layer3-amplification** option). The change does not
affect the currently playing stream until **flush** is issued.

**l3frequency** *CARDINAL* *REAL*  
Set the frequency of the *CARDINAL*-th MPEG Layer III equalizer band to
*REAL* (matching **Mylene1f4**'s **--layer3-frequencies** option). The
change does not affect the currently playing stream until **flush** is
issued.

**l3level** *CARDINAL* *REAL*  
Set the level of the *CARDINAL*-th MPEG Layer III equalizer band to
*REAL* (matching **Mylene1f4**'s **--layer3-levels** option). The change
does not affect the currently playing stream until **flush** is issued.

**mail** *CARDINAL* *CARDINAL* *STRING*  
Send *STRING* as a message to the program identified by the first
*CARDINAL* (major ID) and second *CARDINAL* (minor ID).

**match** *STRING*  
Equivalent to **artist** *STRING*.

**match/** *STRING*  
Equivalent to **artist/** *STRING*.

**match@**  
Equivalent to **artist@**.

**match^**  
Equivalent to **artist^**.

**match^/**  
Equivalent to **artist^/**.

**move** *INTEGRAL* *INTEGRAL*  
Remove the program at the first *INTEGRAL* index from the execution
queue and reinsert it before the program at the second *INTEGRAL* index.
Negative values for the first index are resolved as (*queue_length* +
*INTEGRAL*) (as in **delete**), and negative values for the second index
are resolved as (*queue_length* + 1 + *INTEGRAL*) (as in **insert**).

**order** *back*\|*last*\|*next*\|*previous*\|*random*  
Set the file load order (matching **Mylene1f4**'s **-L** and
**--load-order** options).

**order^**  
Reset the file load order to **Mylene1f4**'s configured default
(**-L**/**--load-order**).

**path** *STRING*  
Set the path filter criterion to match *STRING*.

**path/** *STRING*  
Set the path filter criterion to match *STRING* and refilter the
currently playing file (equivalent to **path** *STRING* followed by
**join**).

**path^**  
Reset the path filter criterion to **Mylene1f4**'s configured default.

**path^/**  
Reset the path filter criterion to **Mylene1f4**'s configured default
and refilter the currently playing file (equivalent to **path^**
followed by **join**).

**pause**  
Pause playback (equivalent to **space** **on**).

**pick** *INTEGRAL*  
Seek to frame index *INTEGRAL* if non-negative, or to (*frame_count* +
*INTEGRAL*) if *INTEGRAL* is negative and the total frame count is
known; otherwise, the command is ignored. Ignored if the currently
playing file is a 1f42 map covering more than one audio stream.

**pick+** *INTEGRAL*  
Seek forward by *INTEGRAL* frames. Ignored if the currently playing file
is a 1f42 map covering more than one audio stream.

**pick-** *INTEGRAL*  
Seek backward by *INTEGRAL* frames. Ignored if the currently playing
file is a 1f42 map covering more than one audio stream.

**play**  
Resume playback (equivalent to **space** **off**).

**progress1** *STRING*  
Set the stream header progress format string to *STRING*, using standard
**Mylene1f4** format string syntax. Syntax errors in *STRING* are not
caught gracefully and will trigger a protocol error, closing the
connection.

**progress1^**  
Reset the stream header progress format string to **Mylene1f4**'s
configured default.

**progress2** *STRING*  
Set the frame progress format string to *STRING*, using standard
**Mylene1f4** format string syntax. Syntax errors in *STRING* are not
caught gracefully and will trigger a protocol error, closing the
connection.

**progress2^**  
Reset the frame progress format string to **Mylene1f4**'s configured
default.

**progress3** *STRING*  
Set the stream trailer progress format string to *STRING*, using
standard **Mylene1f4** format string syntax. Syntax errors in *STRING*
are not caught gracefully and will trigger a protocol error, closing the
connection.

**progress3^**  
Reset the stream trailer progress format string to **Mylene1f4**'s
configured default.

**queue** *PROGRAM*  
Append *PROGRAM* to the execution queue (equivalent to **insert** **-1**
*PROGRAM* or **insert** *queue_length* *PROGRAM*).

**queue** *STRING* *PROGRAM*  
Append *PROGRAM* with title *STRING* to the execution queue (equivalent
to **insert** **-1** *STRING* *PROGRAM* or **insert** *queue_length*
*STRING* *PROGRAM*).

**quit**  
Equivalent to **exit**.

**rand**  
Jump to a randomly selected file and reset any active file repeat count
(as with **cycle** **off**).

**remove** *CARDINAL*  
Remove the last *CARDINAL* files from the file list. If the currently
playing file is among them, it is removed only after **Mylene1f4**
advances to another file. If the currently playing file was already
scheduled for deletion by an earlier **remove** command, the last
*CARDINAL* remaining files preceding it are removed instead. If the file
list becomes empty, **Mylene1f4** exits.

**remove** **current**\|**initial** *CARDINAL*  
Remove the file whose index in the current file list (**current**) or
initial file list (**initial**) matches *CARDINAL*. If the currently
playing file is matched, it is removed only after **Mylene1f4** advances
to another file. If the file list becomes empty, **Mylene1f4** exits.

**restore**  
Restore the original file list order.

**scrap**  
Schedule the currently playing file for deletion; it is removed from the
file list once **Mylene1f4** advances to another file. If the file list
becomes empty, **Mylene1f4** exits.

**seek** \[\[*HOURS*:\]*MINUTES*:\]*SECONDS*\[,*FRAMES*\]  
Seek to the specified stream time offset. Ignored if the currently
playing file is a 1f42 map covering more than one audio stream.

**seek+** \[\[*HOURS*:\]*MINUTES*:\]*SECONDS*\[,*FRAMES*\]  
Seek forward by the specified duration. Ignored if the currently playing
file is a 1f42 map covering more than one audio stream.

**seek-** \[\[*HOURS*:\]*MINUTES*:\]*SECONDS*\[,*FRAMES*\]  
Seek backward by the specified duration. Ignored if the currently
playing file is a 1f42 map covering more than one audio stream.

**select^**  
Reset all file selection filter criteria to **Mylene1f4**'s configured
defaults (equivalent to running **album^**, **artist^**, **basename^**,
**comment^**, **filter^**, **path^**, and **title^**).

**select^/**  
Reset all file selection filter criteria to **Mylene1f4**'s configured
defaults and refilter the currently playing file (equivalent to
**select^** followed by **join**).

**shuffle**  
Shuffle (or reshuffle) the file list.

**skip**  
Equivalent to **skip** **1**.

**skip** *INTEGRAL*  
If **Mylene1f4**'s current load order is *random*, behaves the same as
**rand**; otherwise, skips forward by *INTEGRAL* files (including the
current file). If the target file does not match the active filter
criteria, the closest following matching file is selected instead. Any
active file repeat count is reset (as with **cycle** **off**).
Equivalent to **back** -*INTEGRAL*.

**skip\***  
Equivalent to **skip\*** **0**.

**skip\*** *INTEGRAL*  
Skip to the file at index *INTEGRAL*, counting from 0 at the beginning
(head) of the file list toward the end (tail). If *INTEGRAL* is
negative, the target index is computed as (*list_length* + *INTEGRAL*).
If the target file does not match the active filter criteria, the
closest following matching file is selected instead. Any active file
repeat count is reset (as with **cycle** **off**). Equivalent to
**back\*** -*INTEGRAL*.

**space**  
Toggle playback pause state.

**space** **off**  
Resume playback.

**space** **on**  
Pause playback.

**start** *CARDINAL*  
Set the number of audio frames skipped at the start of each played file
to *CARDINAL*.

**start^**  
Reset the playback start position to **Mylene1f4**'s configured default
start time (**-e**/**--start-time**) or start frame
(**-f**/**--start-frame**). Equivalent to **begin^**.

**suspend**  
Suspend the currently running program and start the next program in the
execution queue. The suspended program is reinserted at index 1 in the
queue (right behind the newly started program) and resumes once that
program completes. Equivalent to **suspend** **1**.

**suspend** *INTEGRAL*  
Suspend the currently running program and reinsert it into the execution
queue before the program at index *INTEGRAL*. If *INTEGRAL* is negative,
it is reinserted before the element at index (*queue_length* + 1 +
*INTEGRAL*) (as in **insert**).

**take** *CARDINAL*  
Jump to the file whose index in the initial file list matches
*CARDINAL*, if such a file exists.

**title** *STRING*  
Set the title filter criterion to match *STRING*.

**title/** *STRING*  
Set the title filter criterion to match *STRING* and refilter the
currently playing file (equivalent to **title** *STRING* followed by
**join**).

**title@**  
Set the title filter criterion to match the title property of the
currently playing file.

**title^**  
Reset the title filter criterion to **Mylene1f4**'s configured default.

**title^/**  
Reset the title filter criterion to **Mylene1f4**'s configured default
and refilter the currently playing file (equivalent to **title^**
followed by **join**).

**wind**  
Equivalent to **wind** **1**.

**wind** *INTEGRAL*  
Advance relative to the active load order: behaves like **rand** when
the load order is *random*, like **back** *INTEGRAL* when the load order
is *previous*, and like **skip** *INTEGRAL* when the load order is
*next*.

Commands and keyword arguments may be abbreviated as long as the
abbreviation is unambiguous.

Time offsets in \[\[*HOURS*:\]*MINUTES*:\]*SECONDS*\[,*FRAMES*\] format
(used by **begin**, **seek**, **seek+**, and **seek-**) are evaluated as
*HOURS* \* 3600 + *MINUTES* \* 60 + *SECONDS* + *FRAMES* / 60 seconds.
Fields are decimal numbers and are not restricted to the 0â59 range; no
whitespace is allowed before or after **:** or **,**.

Unless otherwise noted, cardinal and integer arguments follow C integer
constant rules: a **0x** prefix denotes hexadecimal (base 16), a leading
**0** denotes octal (base 8), and all other numbers are decimal (base
10).

# EXAMPLES

Instruct **Mylene1f4** (listening on TCP/IP port 32767) to immediately
switch to playing *Crier La Vie*:

> **Mylene3e8 -i 32767 title/ "Crier La Vie"**

Repeat the current track indefinitely:

> **Mylene3e8 -i 32767 cycle**

Query the title of the currently playing track:

> **Mylene3e8 -i 32767 format "%G(%t)\n"**

which outputs:

> **Crier La Vie**

Instruct **Mylene1f4** (assumed to be running with **-r** or
**--repeat** enabled) to play every file whose title is *Pas De Doute*
twice, disregarding active filter criteria:

> **Mylene3e8 -i 32767 queue "$(cat program)"**

where *program* contains:

    int i;

    i = 0;
    while (i < advance()) {
        i = i + 1;
        E_load();
        if (!match("Pas De Doute", title())) {
            E_down(2);
        }
    }

# SEE ALSO

**Mylene1f4**(1)

# AUTHORS

Ciprian Niculescu
