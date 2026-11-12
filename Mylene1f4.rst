NAME
====

Mylene1f4 - play MPEG audio streams

SYNOPSIS
========

**Mylene1f4** [*OPTIONS*] *FILE*...

DESCRIPTION
===========

**Mylene1f4** plays MPEG audio streams from *FILE*\ (s).

**Mylene1f4** operates in one of two primary modes:

**Non-interactive mode (default)**
   Plays the specified *FILE*\ (s) sequentially from start to finish and
   exits.

**Interactive (server) mode**
   Enabled via **--inet-server** or **--unix-server**. **Mylene1f4**
   provides no built-in terminal input interface; instead, it listens on
   a socket for playback control commands and embedded *aime* programs
   sent by a client such as **Mylene3e8**\ (1).

A third running mode allows the player to run slaved to a more
user-oriented program (see **-!**).

**Mylene1f4** readily plays standard MPEG audio files (such as MP3s) and
system-embedded MPEG audio streams (such as MPEG A/V containers) out of
the box. **Mylene1f4** can also play 1f42 files, which are stream maps
designed to address the shortcomings of plain MPEG streams‚where stream
duration cannot be known in advance and time-based seeks on large files
can be slow. Created with **mapmaudio**\ (1), 1f42 files supply
frame-level index metadata and can either embed the audio stream
directly or exist as small standalone metadata files separate from the
audio files they map.

EXAMPLES
========

Play a single file in non-interactive mode:

   **Mylene1f4 desenchantee.mp3**

Play an album with detailed stream coding information and real-time
playback position updates (**-px1f4**):

   **Mylene1f4 -px1f4 Anamorphosee/\*.mp3**

Start **Mylene1f4** in interactive server mode listening on a UNIX
domain socket:

   **Mylene1f4 Interstellaires/\*.mp3 Desobeissance/\*.mp3 --unix-server
   /tmp/.Mylene1f4**

Seek to 2 minutes into the currently playing track or skip to the next
track using **Mylene3e8**\ (1):

   **Mylene3e8 /tmp/.Mylene1f4 seek 2:0**

..

   **Mylene3e8 /tmp/.Mylene1f4 skip**

OPTIONS
=======

**-!**
   Run slaved to a master application. **Mylene1f4** will accept
   requests on standard input and send replies on standard output using
   the *lanes* media file playback protocol.

**-1**, **--progress1** *FORMAT*
   Set the format string for stream header progress information (printed
   each time audio stream decoding starts) to *FORMAT*. See **TEXT
   FORMATTING** for format string syntax.

**-2**, **--progress2** *FORMAT*
   Set the format string for frame progress information (printed each
   time an audio frame is about to be decoded, among other events) to
   *FORMAT*. See **TEXT FORMATTING** for format string syntax.

**-3**, **--progress3** *FORMAT*
   Set the format string for stream trailer progress information
   (printed each time audio stream decoding completes) to *FORMAT*. See
   **TEXT FORMATTING** for format string syntax.

**-A**, **--album** *STRING*
   Set the album file selection filter criterion to *STRING*. Only files
   whose album property matches the wildcard pattern *STRING* will be
   loaded.

**-B**, **--basename** *STRING*
   Set the basename file selection filter criterion to *STRING*. Only
   files whose basename matches the wildcard pattern *STRING* will be
   loaded.

**-C**, **--device-name** *STRING*
   Set the first argument (**argv[0]**) passed to the output device
   program to *STRING*. See **--device** for details on the output
   device program.

**-D**, **--device-args** *STRING*
   Pass extra arguments specified by *STRING* to the output device
   program. See **--device** for details on the output device program.

**-F**, **--filter** *FILTER*
   Set the complex file selection filter criterion to *FILTER*. This
   provides more flexible filtering than the simple wildcard criteria
   (**-A**/**--album**, **-B**/**--basename**, **-P**/**--path**,
   **-a**/**--artist**, **-c**/**--comment**, and **-t**/**--title**).
   See **COMPLEX FILTER SYNTAX** for syntax details.

**-L**, **--load-order** *STRING*
   Set the file load order. Valid values for *STRING* are *back*,
   *last*, *next*, *previous*, and *random* (*back* and *last* are
   aliases for *previous*). If *next* is specified, **Mylene1f4** loads
   files in their current order, advancing from first to last. If
   *back*, *last*, or *previous* is specified, **Mylene1f4** loads files
   in reverse order, advancing from last to first. If *random* is
   specified while output is set to *device* (via **-o** or
   **--output**) and a controlling server is enabled (via
   **--inet-server** or **--unix-server**), **Mylene1f4** loads files in
   random order; otherwise, it behaves the same as *next*.

**-P**, **--path** *STRING*
   Set the path file selection filter criterion to *STRING*. Only files
   whose path matches the wildcard pattern *STRING* will be loaded.

**-S**, **--shuffle**
   Shuffle the file list.

**-a**, **--artist** *STRING*
   Set the artist file selection filter criterion to *STRING*. Only
   files whose artist property matches the wildcard pattern *STRING*
   will be loaded.

**-b**, **--buffer** *CARDINAL*
   Set the output cache size to *CARDINAL* \* 36864 bytes. The default
   value is 81 (2985984 bytes, or 2916 kilobytes). Some systems do not
   support shared memory segments of that size; if so, a smaller value
   (such as 56 or 28) may be required.

**-c**, **--comment** *STRING*
   Set the comment file selection filter criterion to *STRING*. Only
   files whose comment property matches the wildcard pattern *STRING*
   will be loaded.

**-d**, **--device** *PATH*
   Set the output device program to *PATH*. **Mylene1f4** executes this
   program and redirects decoded PCM data to it (see **--output** for
   when this option takes effect). The default value is
   **Mylene0fa**\ (1). Note that it is possible for the executed program
   to run out of data, especially at playback start. Instructing the
   device program to delay playback until enough data is cached can
   prevent this. For example, **Mylene0fa**\ (1) accepts a minimum
   amount of cached data in microseconds before starting playback (see
   **Mylene0fa**\ (1) and **--device-args** for details).

   Example (assuming **Mylene0fa**\ (1) is the output device program):

      **Mylene1f4** **-D** **"-l 1000000"** ...

   instructs **Mylene0fa**\ (1) to wait for one second of cached audio
   before starting playback.

**-e**, **--start-time** [[*HOURS*:]\ *MINUTES*:]\ *SECONDS*\ [,\ *FRAMES*]
   Set the playback start stream time to *HOURS* \* 3600 + *MINUTES* \*
   60 + *SECONDS* + *FRAMES* / 60 seconds. No whitespace is allowed
   before or after the **:** and **,** separators. Fields are not
   restricted to the 0‚59 range.

**-f**, **--start-frame** *CARDINAL*
   Skip the specified number of audio frames at the beginning of each
   file.

**-i**, **--tagged-list** *PATH*
   Read a tagged file list from *PATH*, ignoring **-l** and **--list**
   options. Consecutive file records in *PATH* are separated by two or
   more null (zero) bytes. Each file record consists of one or more
   fields separated by a single null (zero) byte. Each field begins with
   a single character identifying its type:

   A. Album.

   B. File name (path).

   a. Artist.

   b. Comment.

   c. Genre.

   d. Title.

   e. Year.

   The characters immediately following the type identifier form the
   field value. If multiple fields of the same type appear in a single
   record, all but the last are ignored. The **N** (file name) field is
   mandatory.

   When a tagged file list is supplied, **Mylene1f4** uses the tags from
   *PATH* instead of reading ID3 tags from the audio files. As a result,
   file selection via **-A**/**--album**, **-a**/**--artist**,
   **-c**/**--comment**, **-t**/**--title**, and **-F**/**--filter** can
   be significantly faster.

   Tagged file lists can be generated using **Mylene1f4** with commands
   such as:

      **Mylene1f4** **--output** *skip* **--progress1**
      **%G(A%A\\0a%a\\0c%c\\0g%g\\0t%t\\0y%y\\0)N%N\\0\\0** [*FILE*...]

   or

      **Mylene1f4** **--output** *skip* **--progress1** *PROGRESS*
      [*FILE*...]

   ..

      where *PROGRESS* concatenates:

         **%G(%z(%A(A%:2:A\\0)%a(a%:2:a\\0)%c(c%:2:c\\0)))**

      ..

         **%G(%z(%g(g%:2:g\\0)%t(t%:2:t\\0)%y(y%:2:y\\0)))**

      and

         **N%N\\0\\0**

   or the equivalent shorthand:

      **Mylene1f4** **--output** *skip* **--progress-type** *tags*
      [*FILE*...]

   Note that the **--progress1** argument may require shell quoting.

**-l**, **--list** *PATH*
   Read a file list from *PATH*, ignoring **-i** and **--tagged-list**
   options. Consecutive file names (paths) in *PATH* are separated by
   one or more null (zero) bytes. File arguments on the command line are
   ignored.

**-o**, **--output** *STRING*
   Set the output type. Valid values for *STRING* are *device*, *null*,
   and *skip*. If *device* is specified, **Mylene1f4** redirects PCM
   output to the program specified via **-d** or **--device**. If *null*
   is specified, **Mylene1f4** decodes the stream and discards the PCM
   data. If *skip* is specified, **Mylene1f4** skips PCM decoding
   altogether. The default value is *device*.

**-p**, **--progress-type** *STRING*
   Set the progress output format strings to one of the built-in
   presets. Valid values for *STRING* are *null*, *path*, *tags*, and
   *x1f4*. Note that the *tags* preset is intended for generating tagged
   file lists rather than console output. Use **--progress-print** in
   conjunction with **--progress-type** to inspect the built-in presets.

**-q**, **--quiet**
   Suppress error messages altogether.

**-r**, **--cycle**, **--repeat**
   Restart playback of *FILE*\ (s) after playing the last file.

**-s**, **--start** *INTEGRAL*
   Specify the index in the file list (after shuffling, if requested) of
   the file to play first. If negative, it is offset from the end of the
   file list (incremented by the number of files).

**-t**, **--title** *STRING*
   Set the title file selection filter criterion to *STRING*. Only files
   whose title property matches the wildcard pattern *STRING* will be
   loaded.

**-v**, **--verbose**
   Print file and audio stream information to standard error. May be
   specified multiple times to increase verbosity.

**-x**, **--synchronize**
   Recover from initial audio stream mis-synchronization (default).
   **Mylene1f4** tolerates leading non-audio data or errors at the start
   of a stream and searches for a valid audio frame. Note that
   subsequent decoding errors are not recovered.

**--attached**
   Start with the system audio device attached (default). **Mylene1f4**
   instructs the output device program to open the system audio device
   upon startup. Ignored if output is not set to *device* (via **-o** or
   **--output**) or if no controlling server was requested (via
   **--inet-server** or **--unix-server**).

**--backward-seek-complaints**
   Allow backward seek warnings from the MPEG audio decoder when a frame
   being decoded references nonexistent preceding frames (default).

**--clear-title** *STRING*
   Clear the title assigned to the next program to be queued (via
   **--queue**).

**--context-interrupt** *CARDINAL*
   Set the maximum number of instructions that programs in an execution
   context may execute continuously. If set to 0, context program
   execution is never interrupted for failing to yield control (see also
   **--program-interrupt**). Otherwise, if the number of executed
   instructions exceeds this limit, all programs in that execution
   context are discarded. The default is 65536 (see **PROGRAM
   EXECUTION**). Note that there is no direct one-to-one mapping between
   source statements and executed instructions, though the latter does
   not usually exceed twice the former.

**--context-list-length** *CARDINAL*
   Set the maximum context execution list length to *CARDINAL*. This
   limit applies only to programs received via a controlling server. The
   default is 1024 (see **PROGRAM EXECUTION**).

**--deque** *CARDINAL*
   Remove the last *CARDINAL* programs (queued via **--queue**) from the
   execution queue.

**--deque-all**
   Remove all programs (queued via **--queue**) from the execution
   queue.

**--deque-last**
   Remove the last program (queued via **--queue**) from the execution
   queue.

**--detached**
   Start with the system audio device detached. **Mylene1f4** instructs
   the output device program not to open the system audio device upon
   startup. Ignored if output is not set to *device* (via **-o** or
   **--output**) or if no controlling server was requested (via
   **--inet-server** or **--unix-server**).

**--disable-expression-optimizations**
   Disable expression optimizations for complex file selection filters.

**--disable-id3v2-read**
   Disable ID3v2 tag reading.

**--dumb**
   Do not recover from initial audio stream mis-synchronization. See
   **-x**, **--synchronize**.

**--enable-expression-optimizations**
   Enable expression optimizations for complex file selection filters
   (default).

**--enable-id3v2-read**
   Enable ID3v2 tag reading (default).

**--error** *STRING*
   Set the load error handling behavior. Valid values for *STRING* are
   *abort* and *ignore*. If *abort* is specified, **Mylene1f4** aborts
   on load errors. If *ignore* is specified, **Mylene1f4** ignores load
   errors. The default value is *abort*.

**--head-b-operators**
   List available binary operators for complex filters and exit.

**--head-p-b-operators**
   List available binary operators for programs and exit.

**--id3v2-complaints**
   Allow warnings from the ID3v2 tag parser.

**--id3v2-parser** *PATH*
   Set the path to the ID3v2 tag parser executable to *PATH*.
   **Mylene1f4** executes this program to parse ID3v2 tags from audio
   files. If *PATH* is empty, the default is used. The default value is
   *prefix*\ **/lib/Mylene1f4/id3v2-id3tag**, where *prefix* is set at
   compile time and defaults to **/usr/local**.

**--id3v2-timeout** *CARDINAL*
   Set the ID3v2 parser timeout to *CARDINAL* microseconds (the maximum
   time **Mylene1f4** waits for the ID3v2 parser to complete). If 0 is
   specified, the default is used. *CARDINAL* is not restricted to the
   1‚1000000 range.

**--id3v2-transfer** *CARDINAL*
   Set the maximum ID3v2 parser transfer size to *CARDINAL* bytes (the
   maximum number of bytes **Mylene1f4** reads from the ID3v2 parser).
   If 0 is specified, the default is used. This option is currently
   unimplemented; the default value is always used.

**--inet-server** *PORT*
   Establish a controlling TCP/IP server on *PORT*, ignoring any local
   UNIX server requested via **--unix-server**. **Mylene1f4** executes
   commands received over this server. **Mylene3e8**\ (1) is a typical
   client that relays user commands to the server. Note that there are
   no security mechanisms to restrict access: any host able to reach the
   server over the network can transmit commands for execution (read:
   mess up your sorry existence).

   Currently, this option is ignored unless output is set to *device*
   (via **-o** or **--output**).

   See **SERVER MODE** for more information.

**--layer3-amplification** *REAL*
   Set MPEG Layer III audio stream amplification to *REAL*. *REAL* is
   expected to be in the -20‚20 range.

**--layer3-frequencies** *LIST*
   Set MPEG Layer III equalizer band frequencies to *LIST*, a
   comma-separated list of frequency values in the 20‚20000 range. The
   default frequencies are 32, 64, 128, 256, 512, 1024, 2048, 4096,
   8192, and 16384.

**--layer3-levels** *LIST*
   Set MPEG Layer III equalizer band levels to *LIST*, a comma-separated
   list of level values in the -20‚20 range. The number of band levels
   must match the number of equalizer band frequencies.

**--list-b-operators**
   List available binary operators for complex filters and exit.

**--list-directives**
   Display text formatting directives in a list view and exit.

**--list-fs-operators**
   List available file-selection unary operators for complex filters and
   exit.

**--list-functions**
   List available functions for complex filters and exit.

**--list-p-b-operators**
   List available binary operators for programs and exit.

**--list-p-fs-operators**
   List available file-selection unary operators for programs and exit.

**--list-p-functions**
   List available functions for programs and exit (same as
   **--list-q-functions**).

**--list-p-u-operators**
   List available unary operators for programs and exit.

**--list-q-functions**
   List available functions for programs and exit (same as
   **--list-p-functions**).

**--list-u-operators**
   List available unary operators for complex filters and exit.

**--mpeg-synchronization** *STRING*
   Set the MPEG audio synchronization method used when initial stream
   mis-synchronization recovery is enabled (via **-x** or
   **--synchronize**). Valid values for *STRING* are *double*, *select*,
   and *single*. With *single*, the first byte sequence resembling a
   frame header is accepted. With *double*, the first candidate frame
   header from which the next frame header position can be accurately
   predicted is accepted. With *select*, the first coherent sequence of
   consecutive frame headers (where each header position is predicted
   from the previous one) is accepted; the sequence length is set via
   **--mpeg-synchronization-sequence** and defaults to 4. The default
   value is *select*.

**--mpeg-synchronization-seek** *CARDINAL*
   Examine at most *CARDINAL* bytes when searching for a valid MPEG
   audio stream. **Mylene1f4** enforces this limit only loosely and may
   examine more bytes (if the limit is observed at all). The default
   value is 4194304 (4 megabytes).

**--mpeg-synchronization-sequence** *CARDINAL*
   Set the required sequence length of coherent frame headers for the
   *select* synchronization method. The default value is 4.

**--no-backward-seek-complaints**
   Suppress backward seek warnings from the MPEG audio decoder when a
   frame being decoded references nonexistent preceding frames.

**--no-cycle**, **--no-repeat**
   Do not restart playback of *FILE*\ (s) after playing the last file
   (default).

**--no-id3v2-complaints**
   Suppress ID3v2 parser warnings, including execution errors (default).

**--no-reuse-address**
   Do not reuse the local address when binding the INET controlling
   server (default). Without address reuse, binding the INET server may
   fail if the OS kernel still considers the address in use even after
   the previous socket has closed.

**--no-server-filter-complaints**
   Suppress parsing warnings for filter-setting requests received via
   the server.

**--no-server-format-complaints**
   Suppress parsing warnings for text-formatting requests received via
   the server.

**--no-server-progress-complaints**
   Suppress parsing warnings for progress-formatting requests received
   via the server.

**--no-shuffle**
   Do not shuffle the file list (default).

**--no-synchronization**
   Do not recover from initial audio stream mis-synchronization.
   **Mylene1f4** tolerates no errors in decoded streams, including
   leading non-audio data.

**--null-process** *STRING*
   Set the MPEG decoding mode used when output is set to *null* (via
   **-o** or **--output**). Valid values for *STRING* are *4294967291*,
   *futile*, *null*, *pcm1*, *real*, *skip*, and *utterly-futile*. If
   *4294967291*, *pcm1*, or *real* is specified, MPEG audio streams are
   fully decoded. If *futile* or *null* is specified, streams are parsed
   but not fully decoded (note that this mode is not supported for Layer
   III). If *skip* or *utterly-futile* is specified, only MPEG frame
   headers are examined. The default value is *4294967291* (which also
   happens to be the largest 32-bit prime number; the second largest is
   *4294967279*, a number that has no relevance yet for **Mylene1f4**).

**--pack-directives**
   Display text formatting directives in a compacted tree view and exit.

**--paused**
   Start playback paused. Ignored if no controlling server was requested
   (via **--inet-server** or **--unix-server**).

**--play-list-length** *CARDINAL*
   Set the maximum playlist length to *CARDINAL*. This limit applies
   only to playlist items received via a controlling server. The default
   is 65536.

**--playing**
   Start playing immediately (default). Ignored if no controlling server
   was requested (via **--inet-server** or **--unix-server**).

**--program-allocation** *CARDINAL*
   Set the maximum memory (in bytes) that a program may allocate to
   *CARDINAL*. This limit tracks explicit memory allocation requests and
   excludes standard allocator overhead, though it does include the
   memory needed to store the program's executable representation. The
   default is 2097152 (2 megabytes) (see **PROGRAM EXECUTION**).

**--program-interrupt** *CARDINAL*
   Set the maximum number of instructions a program may execute between
   two successive effector function calls to *CARDINAL*. If set to 0,
   program execution is never interrupted for running too long. The
   default is 65536 (see **PROGRAM EXECUTION**). Note that there is no
   direct one-to-one mapping between source statements and executed
   instructions, though the latter does not usually exceed twice the
   former.

**--progress-print**
   Display the resolved progress format strings (after merging
   **--progress-type**, **--progress1**, **--progress2**, and
   **--progress3**) and exit. The displayed output is the exact
   formatting that would be used without **--progress-print**.

**--queue** *PROGRAM*
   Queue *PROGRAM* for execution. May be specified multiple times (see
   **PROGRAM EXECUTION**).

**--queue-length** *CARDINAL*
   Set the maximum execution queue length to *CARDINAL*. This limit
   applies only to programs received via a controlling server. The
   default is 4096 (see **PROGRAM EXECUTION**).

**--request-transfer** *CARDINAL*
   Set the maximum transfer size for server-received requests to
   *CARDINAL* bytes. This applies to requests received on the UNIX or
   INET controlling server; connections sending requests larger than
   this threshold are closed. The default is 4096.

**--reuse-address**
   Reuse the local address when binding the INET controlling server.
   Enabling this option consistently can reduce bind failures when
   restarting the INET server.

**--server-filter-complaints**
   Allow parsing warnings for filter-setting requests received via the
   server (default).

**--server-format-complaints**
   Allow parsing warnings for text-formatting requests received via the
   server (default).

**--server-progress-complaints**
   Allow parsing warnings for progress-formatting requests received via
   the server (default).

**--store-title** *STRING*
   Set the title of the next program to be queued (via **--queue**) to
   *STRING*.

**--unix-server** *PATH*
   Establish a controlling local UNIX socket server at *PATH*, ignoring
   any TCP/IP server requested via **--inet-server**. **Mylene1f4**
   executes commands received over this server. **Mylene3e8**\ (1) is a
   typical client that relays user commands to the server.

   Currently, this option is ignored unless output is set to *device*
   (via **-o** or **--output**).

   See **SERVER MODE** for more information.

**--unix-server-mode** *MODE*
   Set the file permissions for the local UNIX socket server to octal
   *MODE*. *MODE* should include at least owner read and write
   permissions (00600). The default is 00600.

**--tree-directives**
   Display text formatting directives in a tree view and exit.

**--help**
   Print a usage summary and exit with a status code indicating success.

**--version**
   Print version information to standard output and exit.

All file selection filter criteria‚both the simple filters
(**-A**/**--album**, **-B**/**--basename**, **-P**/**--path**,
**-a**/**--artist**, **-c**/**--comment**, and **-t**/**--title**) and
the complex filter (**-F**/**--filter**)‚are enforced when loading
files. A file must satisfy every active filter criterion to be loaded.

Simple filter criteria perform case-sensitive wildcard matching using
shell patterns (**\*** and **?**). A **\*** matches any sequence of
characters, whereas an empty pattern (‚‚) matches only files where the
corresponding property exists and is empty. Note that file properties
are trimmed of leading and trailing whitespace before matching.

Option arguments that accept a fixed set of keywords may be abbreviated
as long as the abbreviation is unambiguous.

COMPLEX FILTER SYNTAX
=====================

The complex filter syntax resembles that of C expressions. It supports
prefix unary and infix binary operators, built-in functions, and
built-in variables (all defined by **Mylene1f4**; user-defined variables
are not supported). Four data types are supported: *cardinal* (unsigned
integer), *integer* (signed integer), *real* (floating-point), and
*text* (string).

Available functions include general mathematical and string utilities as
well as file-selection functions. Among the latter:

*integer* **advance**\ (*void*);
   is the file list length.

*text* **album**\ (*void*);
   is the album property of the current file.

*text* **artist**\ (*void*);
   is the artist property of the current file.

*integer* **clip**\ (*void*);
   is the clip index (incremented each time a file is examined).

*text* **comment**\ (*void*);
   is the comment property of the current file.

*integer* **current**\ (*void*);
   is the index of the file in the current file list.

*text* **file**\ (*void*);
   is the basename of the current file.

*integer* **original**\ (*void*);
   is the index of the file in the original (initial) file list.

*text* **path**\ (*void*);
   is the path of the current file.

*integer* **play**\ (*void*);
   is the play index (incremented each time a file is played).

*text* **title**\ (*void*);
   is the title property of the current file.

Among the general string utilities:

*text* **bfxa**\ (*integer* *b*, *integer* *f*, *integer* **x**, *integer* **m**);
   is the base-*x* representation of *m*, zero-padded to at least *f*
   digits and grouped into comma-separated blocks of width *b*.

*text* **cat**\ (*text* *s1*, *text* *s2*);
   is the concatenation of *s1* and *s2*.

*text* **cat2**\ (*text* *s1*, *text* *s2*);
   Equivalent to **cat**\ (*s1*, *s2*).

*text* **cat3**\ (*text* *s1*, *text* *s2*, *text* *s3*);
   is the concatenation of *s1*, *s2*, and *s3*.

*text* **cat4**\ (*text* *s1*, *text* *s2*, *text* *s3*, *text* *s4*);
   is the concatenation of *s1*, *s2*, *s3*, and *s4*.

*integer* **compare**\ (*text* *major*, *text* *minor*);
   is 0 if *minor* is identical to *major* (case-sensitive), or a
   non-zero value otherwise.

*text* **cut**\ (*text* *string*, *integer* *offset*, *integer* *length*);
   is the substring of *string* of length *length* starting at *offset*.

*text* **delete**\ (*text* *s*, *integer* *p*);
   is a copy of *s* with the character at index *p* removed (negative
   indices count backward from the end of the string).

*text* **insert**\ (*text* *s*, *integer* *p*, *integer* *c*);
   is a copy of *s* with character *c* inserted before index *p*
   (negative indices count backward from the end of the string).

*text* **itoa**\ (*integer* **m**);
   is the decimal string representation of *m* (equivalent to
   **bfxa**\ (0, 0, 10, *m*)).

*integer* **match**\ (*text* *major*, *text* *minor*);
   is 0 if *minor* matches the case-sensitive wildcard pattern *major*
   (where **\*** and **?** act as shell wildcards), or a non-zero value
   otherwise.

See **express17**\ (1) for additional mathematical and string functions.

Use **--list-functions** to obtain the complete list of functions
available in complex filters.

Most C prefix unary and infix binary operators are supported‚including
unary **!**, **+**, **-**, and **~**, and binary **\***, **/**, **%**,
**+**, **-**, **<<**, **>>**, **<**, **>**, **!=**, **==**, **&**,
**^**, **\|**, **&&**, and **\|\|**‚with semantics and precedence
matching or closely following their C counterparts.

Among the additional binary operators:

*text* *prefix-text-expression* **+** *suffix-text-expression*
   Concatenates *prefix-text-expression* and *suffix-text-expression*
   (equivalent to **cat**\ (*prefix-text-expression*,
   *suffix-text-expression*)).

Shorthand file-selection unary operators are also provided:

*integer* **~**\ *text-expression*
   Shorthand for **!match**\ (*text-expression*, **file**\ ())

*integer* **@**\ *text-expression*
   Shorthand for **!match**\ (*text-expression*, **path**\ ())

*integer* **#**\ *integer-expression*
   Shorthand for *integer-expression* **==** **current**\ ()

*integer* **$**\ *integer-expression*
   Shorthand for *integer-expression* **==** **original**\ ()

*integer* **%**\ *text-expression*
   Shorthand for **!match**\ (*text-expression*, **album**\ ())

*integer* **^**\ *text-expression*
   Shorthand for **!match**\ (*text-expression*, **artist**\ ())

*integer* **&**\ *text-expression*
   Shorthand for **!match**\ (*text-expression*, **title**\ ())

*integer* **\|**\ *text-expression*
   Shorthand for **!match**\ (*text-expression*, **comment**\ ())

Use **--list-fs-operators** to list all file-selection unary operators,
**--list-u-operators** to list all unary operators, and
**--head-b-operators** or **--list-b-operators** to list all binary
operators.

The filter expression must evaluate to an *integer*; a file is selected
when the expression evaluates to a non-zero value.

Examples:

   **&"pas de doute" \| &"et tournoie..."**

selects files whose title is either *pas de doute* or *et tournoie...*.

   **%"mylenium tour" && &"desenchantee"**

selects files whose album is *mylenium tour* and whose title is
*desenchantee*.

PROGRAM EXECUTION
=================

When running in server mode (**--inet-server** or **--unix-server**) and
directing PCM output to a device (**-o** or **--output** *device*),
**Mylene1f4** can accept programs for execution.

Programs are written in the *aime* programming language, a
general-purpose application extension language augmented with functions
and operators specific to **Mylene1f4**.

As with complex filter syntax, the accepted syntax closely resembles C.
It supports prefix unary and infix binary operators, built-in functions
(defined by **Mylene1f4**), and both built-in and user-defined
variables. Basic data types include *cardinal* (unsigned integer),
*integer* (signed integer), *real* (floating-point), and *text*
(string), while *void* is allowed as a function return type. Composite
types include *data* (byte array), *list* (sequence), and *record*
(associative array). *object* is a special generic type allowed for
function parameters and return values, enabling data of any proper type
to be passed. **Mylene1f4**-specific data types include *Deck* (an
execution queue program).

The C **if**, **else**, **while**, **do** **while**, and **break**
statements are supported with semantics matching C, along with an
additional **elif** keyword (borrowed from the C preprocessor, using
C-style syntax). Opening and closing block braces are mandatory.
Variable declarations must precede statements within a block, and
variable initialization at declaration is disallowed.

The formal syntax for block-level programs is:

program: block

block: declarations_section statements_section

declarations_section:

..

   \| *declarations_line* *declarations_section*

declarations_line: type name subsequent_name ;

subsequent_name:

..

   \| **,** *name* *subsequent_name*

statements_section:

..

   \| *statement_line* *statements_section*

statement_line: ;

..

   \| *expression* **;**

   \| **do** **{** *break_block* **}** **while** **(** *expression*
   **)** **;**

..

   \| **if** **(** *expression* **)** **{** *block* **}**
   *elif_block_list* *else_block*

   \| **while** **(** *expression* **)** **{** *break_block* **}**

elif_block_list:

..

   \| *elif_block* *elif_block_list*

elif_block: elif ( expression ) { block }

else_block:

..

   \| **else** **{** *block* **}**

break_block: declarations_section break_statements_section

break_statements_section:

..

   \| *break_statement_line* *break_statements_section*

break_statement_line: break ;

..

   \| *statement_line*

Functions and variables do not share the same namespace, so using a
function name as a variable name is permitted. However, the **break**,
**do**, **elif**, **if**, **else**, and **while** keywords, as well as
the *cardinal*, *integer*, *real*, and *text* type names, are reserved
and cannot be used as variable names.

A secondary program syntax allows defining functions using C-like
syntax. When using function definitions, a **main** function returning
*integer* and taking a *void* argument list is required as the entry
point.

The formal syntax for function-based programs is:

program: function subsequent_function

function: definition

..

   \| *declaration*

definition: void name ( arguments_list ) { block }

..

   \| *type_other_than_void* *name* **(** *arguments_list* **)** **{**
   *block_return* **}**

block_return: block return expression ;

declaration: type name ( arguments_bill ) ;

arguments_list: void

..

   \| *variables_list*

variables_list: variable subsequent_variable

variable: type reference_class name

arguments_bill: void

..

   \| *positions_bill*

positions_bill: position subsequent_position

position: type reference_class

..

   \| *type* *reference_class* *name*

reference_class:

..

   \| **&**

subsequent_variable:

..

   \| **,** *variable* *subsequent_variable*

subsequent_position:

..

   \| **,** *position* *subsequent_position*

subsequent_function:

..

   \| *function* *subsequent_function*

User-defined functions may only return *void*, an integer type, or a
real type (specifically, *cardinal*, *integer*, *real*, or *void*).

Comments begin with **#** and extend to the end of the line.

Available functions include all complex filter functions as well as
additional state-retrieving functions, including:

*text* **L_album**\ (*void*);
   is the active album file selection filter criterion.

*text* **L_artist**\ (*void*);
   is the active artist file selection filter criterion.

*integer* **L_back**\ (*void*);
   is a non-zero value if the load order is set to *previous*, or 0
   otherwise.

*text* **L_basename**\ (*void*);
   is the active basename file selection filter criterion.

*text* **L_class**\ (*void*);
   is the active complex file selection filter criterion.

*text* **L_comment**\ (*void*);
   is the active comment file selection filter criterion.

*text* **L_filter**\ (*void*);
   Equivalent to **L_class**\ ().

*integer* **L_integer**\ (*text* *name*, *integer* **&**\ *value*);
   is a non-zero value if an *integer* play state named *name* is
   defined (storing its value into *value*), or 0 otherwise. See **PLAY
   STATES** for details.

*real* **L_l3amplification**\ (*void*);
   is the MPEG Layer III audio stream amplification level.

*real* **L_l3frequency**\ (*integer* *index*);
   is the frequency of the *index*-th MPEG Layer III equalizer band.

*real* **L_l3level**\ (*integer* *index*);
   is the level of the *index*-th MPEG Layer III equalizer band.

*integer* **L_last**\ (*void*);
   Equivalent to **L_back**\ ().

*text* **L_lead**\ (*void*);
   is the first message in the message queue, or an empty string if the
   queue is empty.

*integer* **L_long**\ (*void*);
   is 0 if the message queue is empty, or a non-zero value otherwise.

*text* **L_match**\ (*void*);
   Equivalent to **L_artist**\ ().

*integer* **L_next**\ (*void*);
   is a non-zero value if the load order is set to *next*, or 0
   otherwise.

*integer* **L_original**\ (*void*);
   is a non-zero value if the playlist is not shuffled, or 0 otherwise.

*text* **L_path**\ (*void*);
   is the active path file selection filter criterion.

*integer* **L_previous**\ (*void*);
   Equivalent to **L_back**\ ().

*integer* **L_random**\ (*void*);
   is a non-zero value if the load order is set to *random*, or 0
   otherwise.

*integer* **L_repeat**\ (*void*);
   is the repeat count (the number of times the currently playing file
   has been continuously repeated as a result of repeat requests).

*integer* **L_shuffled**\ (*void*);
   is a non-zero value if the playlist is shuffled, or 0 otherwise.

*integer* **L_text**\ (*text* *name*, *text* **&**\ *value*);
   is a non-zero value if a *text* play state named *name* is defined
   (storing its value into *value*), or 0 otherwise. See **PLAY STATES**
   for details.

*text* **L_title**\ (*void*);
   is the active title file selection filter criterion.

State-setting functions include:

*void* **S_album**\ (*text* *album*);
   Sets the album file selection filter criterion to *album*.

*void* **S_artist**\ (*text* *artist*);
   Sets the artist file selection filter criterion to *artist*.

*void* **S_back**\ (*void*);
   Sets the load order to *previous*.

*void* **S_basename**\ (*text* *basename*);
   Sets the basename file selection filter criterion to *basename*.

*void* **S_class**\ (*text* *class*);
   Sets the complex file selection filter criterion to *class*.

*void* **S_comment**\ (*text* *comment*);
   Sets the comment file selection filter criterion to *comment*.

*void* **S_filter**\ (*text* *filter*);
   Equivalent to **S_class**\ (*filter*).

*void* **S_integer**\ (*text* *name*, *integer* *value*);
   Sets an *integer* file play state named *name* to *value*,
   overwriting any existing state with the same name. See **PLAY
   STATES** for details.

*void* **S_l3amplification**\ (*real* *amplification*);
   Sets the MPEG Layer III audio stream amplification level to
   *amplification*.

*void* **S_l3frequency**\ (*integer* *index*, *real* *frequency*);
   Sets the frequency of the *index*-th MPEG Layer III equalizer band to
   *frequency*.

*void* **S_l3level**\ (*integer* *index*, *real* *level*);
   Sets the level of the *index*-th MPEG Layer III equalizer band to
   *level*.

*void* **S_last**\ (*void*);
   Equivalent to **S_back**\ ().

*void* **S_match**\ (*text* *match*);
   Equivalent to **S_artist**\ (*match*).

*void* **S_next**\ (*void*);
   Sets the load order to *next*.

*void* **S_path**\ (*text* *path*);
   Sets the path file selection filter criterion to *path*.

*void* **S_previous**\ (*void*);
   Equivalent to **S_back**\ ().

*void* **S_random**\ (*void*);
   Sets the load order to *random*.

*void* **S_text**\ (*text* *name*, *text* *value*);
   Sets a *text* file play state named *name* to *value*, overwriting
   any existing state with the same name. See **PLAY STATES** for
   details.

*void* **S_title**\ (*text* *title*);
   Sets the title file selection filter criterion to *title*.

State-resetting functions include:

*void* **R_album**\ (*void*);
   Resets the album file selection filter criterion to its initial
   default value (as configured via **-A** or **--album**).

*void* **R_artist**\ (*void*);
   Resets the artist file selection filter criterion to its initial
   default value (as configured via **-a** or **--artist**).

*void* **R_basename**\ (*void*);
   Resets the basename file selection filter criterion to its initial
   default value (as configured via **-B** or **--basename**).

*void* **R_class**\ (*void*);
   Resets the complex file selection filter criterion to its initial
   default value (as configured via **-F** or **--filter**).

*void* **R_comment**\ (*void*);
   Resets the comment file selection filter criterion to its initial
   default value (as configured via **-c** or **--comment**).

*void* **R_filter**\ (*void*);
   Equivalent to **R_class**\ ().

*void* **R_match**\ (*void*);
   Equivalent to **R_artist**\ ().

*void* **R_order**\ (*void*);
   Resets the load order to its initial default value (as configured via
   **-L** or **--load-order**).

*void* **R_path**\ (*void*);
   Resets the path file selection filter criterion to its initial
   default value (as configured via **-P** or **--path**).

*void* **R_title**\ (*void*);
   Resets the title file selection filter criterion to its initial
   default value (as configured via **-t** or **--title**).

Pseudo-effector functions include:

*void* **F_abort**\ (*void*);
   Aborts program execution (also available in instant programs).

*void* **F_detach**\ (*void*);
   Instructs the output device program to temporarily detach the system
   audio device (the device can only be re-attached via a server
   command).

*void* **F_interrupt**\ (*void*);
   Suspends program execution (available in context execution lists
   only).

*void* **F_pause**\ (*void*);
   Instructs the output device program (typically **Mylene0fa**\ (1)) to
   pause playback (playback can only be resumed via a server command).

*void* **F_send**\ (*integer* *major*, *integer* *minor*, *text* *message*);
   Delivers *message* to the program identified by major ID *major* and
   minor ID *minor*.

*void* **F_scrap**\ (*void*);
   Deletes the first message in the message queue, if any.

*void* **F_skip**\ (*void*);
   Cancels playback of the current file selection. This function is
   available only in playback-start execution list programs. When
   called, any remaining programs in the playback-start execution list
   as well as all programs in the playback-finish execution list are
   skipped.

*void* **F_suspend**\ (*integer* *index*);
   Suspends execution of the running execution-queue program and
   reinserts it into the execution queue before the program at *index*.
   If *index* is negative, the program is reinserted before the element
   at index (*queue_length* + 1 + *index*)‚so *0* places it first, *1*
   places it second, *-1* places it last, *-2* places it second to last,
   and so on. If *index* is less than -(*queue_length* + 1) or greater
   than *queue_length*, this function has no effect.

   When called from a file-selection program (i.e., an execution queue
   program), the suspended program is the calling program itself.

Unless otherwise noted, pseudo-effector functions are not available in
instant programs.

Effector functions (available in the execution queue only) include:

*void* **E_down**\ (*integer* *count*);
   Sets the repeat count to *count* - 1 (if *count* is greater than 1)
   and plays the next file matching the current load order and filter
   criteria.

*void* **E_exit**\ (*void*);
   Exits **Mylene1f4**.

*void* **E_load**\ (*void*);
   Loads the next file according to the current load order.

*void* **E_loop**\ (*void*);
   Repeats the next file matching the current load order and filter
   criteria indefinitely (or until **Mylene1f4** is instructed
   otherwise).

*void* **E_play**\ (*void*);
   Plays the next file matching the current load order and filter
   criteria.

Use **--list-p-functions** to obtain the complete list of functions
available to programs.

All complex filter operators are available in programs, along with
string assignment:

*text* *text-variable* **=** *text-expression*
   Assigns *text-expression* to *text-variable* and evaluates to the
   assigned string.

Use **--list-p-fs-operators** to list all file-selection unary
operators, **--list-p-u-operators** to list all unary operators, and
**--head-p-b-operators** or **--list-p-b-operators** to list all binary
operators available to programs.

Programs cannot declare global variables. Predefined global variables
provided by **Mylene1f4** include:

*record* **context**
   A general-purpose associative array that is neither read nor modified
   by **Mylene1f4**. It can be used to share data across routines or
   between hosted programs and their hosts (see **HOSTED PROGRAMS**).

Executed programs may or may not perform file selection.

For file-selection programs, **Mylene1f4** maintains an execution queue
(see **PROGRAM EXECUTION QUEUE**).

Non-file-selection programs are executed at specific lifecycle events,
such as file playback start or finish (see **PROGRAM CONTEXT EXECUTION
LISTS**).

Each program is assigned a unique ID consisting of a pair of unsigned
integers; IDs are never reused.

Programs can send messages to one another using their respective IDs.

PROGRAM EXECUTION QUEUE
=======================

Programs in the execution queue run sequentially to decide which file to
play next.

The active program executes uninterrupted until it either selects a
track to play (via an effector function such as **E_play**\ ()) or
yields/completes. While an execution-queue program is active and driving
playback, subsequent programs in the queue wait their turn; a new
program starts only after the current one becomes inactive.

A program may select and play multiple files, or even run an endless
loop, without hogging the server. Its execution is active only during
the track-selection phase.

Program execution starts or resumes after file playback completes, but
before the file is unloaded. Consequently, file properties retrieved via
functions such as **artist**\ () and **title**\ () refer to the file
that just finished playing.

Instructions execute uninterrupted until an effector function is called.
If the number of uninterrupted instructions exceeds the configured limit
(set via **--program-interrupt**), program execution is aborted.

If **E_exit**\ () is called, **Mylene1f4** exits immediately.

If **E_down**\ (), **E_loop**\ (), or **E_play**\ () is called, the next
file matching the current load order and filter criteria is played as
requested, and program execution resumes after playback completes (again
before the file is unloaded).

If **E_load**\ () is called, the next file according to the current load
order is loaded (without filtering), and program execution resumes
before playback starts. At this point, functions such as **artist**\ ()
and **title**\ () refer to the newly loaded file. If the next effector
call is **E_down**\ (), **E_loop**\ (), or **E_play**\ (), the loaded
file is played immediately‚as if the effector had been called before the
previous file was unloaded and the loaded file satisfied the filter
criteria. Other effector calls behave normally (for instance, a
subsequent **E_load**\ () skips the loaded file), and if no further
effector function is called before the program ends, the loaded file is
skipped.

Commands received by the controlling UNIX or INET server continue to be
processed during program execution.

PROGRAM CONTEXT EXECUTION LISTS
===============================

For each non-file-selection execution context (such as playback start or
playback finish), **Mylene1f4** maintains a list of programs to run.
When a context is reached, all programs in its list execute sequentially
in list order.

Context lists begin execution as soon as their respective context is
reached.

Instructions execute uninterrupted; if the instruction count exceeds the
limit set via **--program-interrupt**, the program is aborted.

Suspended programs resume execution the next time their respective
context is reached.

Playback start and finish contexts are triggered on every playback pass
(including each repetition when a file is repeated).

The playback-start context is reached before stream header progress
information is formatted (as configured via **-1** or **--progress1**).

The playback-finish context is reached after stream trailer progress
information is formatted (as configured via **-3** or **--progress3**),
but before the play count (retrievable via **play**\ ()) is incremented.

HOSTED PROGRAMS
===============

**Mylene1f4** can accept programs for immediate ("instant") execution
within the execution context of an existing regular program. This shared
context includes the memory allocator and the list of
**Mylene1f4**-defined global variables (such as **context**), allowing
instant programs to inspect and modify the state of their host program.

Unlike host programs (which run in execution queues and context lists
between playback events), instant programs always execute during
playback.

A host program must have already started execution before an instant
program can be run within its context.

PLAY STATES
===========

Executed programs can store key-value data associated with the current
playback context. Values are stored as *integer* or *text* data via
**S_integer**\ () and **S_text**\ (), and can be retrieved by any
program via **L_integer**\ () and **L_text**\ ().

All stored play states are cleared before execution of the
file-selection queue resumes.

At most 256 play states may be stored at once, with a combined size
limit of 65536 bytes.

TEXT FORMATTING
===============

A format string consists of zero or more directives: ordinary characters
(other than **%**), which are copied unchanged to standard output, and
special directives introduced by **%**, which are expanded before
output.

In addition to standard **find**\ (1)-like format directives, scoping
directives ("subformat directives") are supported to group and output
related information. For instance, **%S(%u %g)** prints the user and
group IDs of the currently playing file. A subformat directive and its
subdirectives may not always apply (for example, MPEG directives apply
only to MPEG streams). A subformat directive consists of a leading
**%**, an identifying character, and a formatting substring enclosed in
parentheses. Unless otherwise specified, an optional second
parenthesized formatting substring may follow. The directive itself
produces no direct output; instead, it provides conditional branching
and scoping. If the condition associated with the directive is met, the
first formatting substring is evaluated; otherwise, the second substring
(if present) is evaluated. Within the first substring, the directive
replaces the current set of interpreted directives with its own
subdirective set.

A special case is the spacing subformat directive, which consists of a
leading **%** followed immediately by a parenthesized formatting
substring (with no identifying character). This directive is always
evaluated and does not alter the active set of interpreted directives.
For example, **%40(%d:%b)\\n** outputs the directory name and basename
of the currently playing file separated by a colon, padded to a minimum
width of 40 bytes, followed by a newline.

Another subclass of subformat directives operates on object collections
(such as files in the playlist). These directives iterate over all or a
selected subset of objects in the collection, evaluating the formatting
substring for each object in turn. When applied to the entire
collection, a collection directive takes the same form as a standard
subformat directive. To select a subset of objects, place a
comma-separated list of ranges enclosed in curly braces between the
leading **%** and the directive's identifying character. Each range
specifies a decimal start and end index separated by a hyphen (**-**),
or a single decimal index for a single item. Range specifications are
evaluated sequentially. They may overlap. They may be reversed (where
the start index is greater than the end index), in which case the
backward traversal order is observed.

Note that literal parentheses inside formatting substrings must be
escaped.

The interpreted \`\\' escapes are:

\\OOO
   The character whose ASCII code is octal OOO.

\\a
   Alarm bell.

\\b
   Backspace.

\\f
   Form feed.

\\n
   Newline.

\\r
   Carriage return.

\\t
   Horizontal tab.

\\v
   Vertical tab.

\\\\
   A literal backslash (\`\\').

The interpreted directives are:

%%
   A literal percent sign.

%B
   The clip index (incremented on every file examination).

%C
   The class of the application (**Mylene1f4**).

%E
   The release size (the number of bytes of PCM data resulting from
   audio frame decoding).

%G
   The ID3 tag information (version 2 is supported only if the matching
   plugin is found) (subformat directive).

%L
   The file information as returned by **lstat**\ (2) (subformat
   directive).

%M
   The setup information (subformat directive).

%N
   The path of the loaded file (escapable).

%O
   The current byte offset in the file.

%P
   The play index (incremented on every file playback).

%Q
   The controller information (subformat directive).

%R
   The invocation name of the application (the basename with which
   **Mylene1f4** was called).

%S
   The file information as returned by **stat**\ (2) (subformat
   directive).

%V
   The output information (subformat directive).

%X
   The filter information (subformat directive).

%Y
   The version.

%Z
   The process information (subformat directive).

%a
   The audio device frame index (approximation).

%b
   The basename of the loaded file (escapable).

%c
   The index of the loaded file in the current files list.

%d
   The directory name of the loaded file (escapable).

%e
   The cached PCM data size in bytes.

%f
   The index of the next audio frame to be decoded.

%i
   The index of the loaded file in the initial files list.

%l
   The files list information (subformat directive).

%o
   The load order information (subformat directive).

%r
   The read index in the cache buffer.

%s
   The sampling rate (number of audio samples played per second).

%t
   The stream time information (subformat directive).

%v
   The audio stream coding-specific information (subformat directive).

%w
   The write index in the cache buffer.

%x
   The 1f42 map information (subformat directive).

%y
   The detailed version information (subformat directive).

%z
   The remaining bytes to end of file.

The ID3 (**%G**) directives are:

%A
   The album (escapable).

%a
   The artist (escapable).

%c
   The comment (escapable).

%g
   The genre (unimplemented; currently outputs nothing).

%t
   The title (escapable).

%y
   The year (escapable).

%z
   The non-zero length fields information (subformat directive).

The ID3 non-zero length fields (**%G(%z)**) directives are:

%A
   The album (subformat directive, no interpreted subdirectives, valid
   when the album field has a non-zero length).

%a
   The artist (subformat directive, no interpreted subdirectives, valid
   when the artist field has a non-zero length).

%c
   The comment (subformat directive, no interpreted subdirectives, valid
   when the comment field has a non-zero length).

%g
   The genre (subformat directive, no interpreted subdirectives, never
   valid).

%t
   The title (subformat directive, no interpreted subdirectives, valid
   when the title field has a non-zero length).

%y
   The year (subformat directive, no interpreted subdirectives, valid
   when the year field has a non-zero length).

The file information as returned by **lstat**\ (2) (%L) and the file
information as returned by **stat**\ (2) (%S) directives are:

%B
   The blocksize for filesystem I/O (where available, \`0' where not).

%G
   The group information (subformat directive).

%M
   The protection.

%U
   The user information (subformat directive).

%a
   The time of last access (subformat directive).

%b
   The number of blocks allocated (where available, \`0' where not).

%c
   The time of last change (subformat directive).

%d
   The device (subformat directive).

%g
   The group ID of owner.

%i
   The inode.

%m
   The time of last modification (subformat directive).

%n
   The number of hard links.

%r
   The device type (if inode device) (subformat directive).

%s
   The total size, in bytes.

%u
   The user ID of owner.

The interpreted group (%L(%G) and %S(%G)) directives are:

%n
   The name.

The interpreted user (%L(%U) and %S(%U)) directives are:

%n
   The name.

The interpreted time of last access (%L(%a) and %S(%a)), time of last
change (%L(%c) and %S(%c)), time of last modification (%L(%m) and
%S(%m)) and process start and current times (%Z(%s) and %Z(%t))
directives are:

%A
   The date information (subformat directive).

%D
   The date as returned by **ctime**\ (3).

%T
   The time of day information (subformat directive).

%d
   The date expressed as YYYYMMDD, where YYYY is the year, MM is the
   month and DD is the month day.

%e
   The date expressed as YYYY-MM-DD, where YYYY is the year, MM is the
   month and DD is the month day.

%t
   The time expressed as HH:MM:SS, where HH is the hour of the day, MM
   is the minute of the hour and SS is the second of the minute. Same as
   %T(%t).

The interpreted date (%L(%a(%A)), %L(%c(%A)), %L(%m(%A)), %S(%a(%A)),
%S(%c(%A)), %S(%m(%A)), %Z(%s(%A)) and %Z(%t(%A))) directives are:

%Y
   The year.

%d
   The day of the month (1 .. 31).

%m
   The month of the year (1 .. 12).

The file timestamps (%L(%a(%T)), %L(%c(%T)), %L(%m(%T)), %S(%a(%T))),
%S(%c(%T)) and %S(%m(%T))), the play start stream time (%M(%S(%t))), the
process start, current and up broken down times (%Z(%s(%T)) and
%Z(%t(%T))), the stream time (%t) and the 1f42 map available stream
length (%x(%l)) and time to end of stream (%x(%t)) directives are:

%F
   The frame of the second (0 .. 59).

%H
   The hour of the day (0 .. 23).

%M
   The minute of the hour (0 .. 59).

%S
   The second of the minute (0 .. 59).

%f
   The frame.

%h
   The hour.

%m
   The minute.

%s
   The second.

%t
   The time expressed as HH:MM:SS, where HH is the hour of the day, MM
   is the minute of the hour and SS is the second of the minute. Same as
   %<0>2H:%<0>2M:%<0>2S.

%x
   The time expressed as HH:MM:SS:FF, where HH is the hour of the day,
   MM is the minute of the hour, SS is the second of the minute and FF
   is the frame of the second. Same as %<0>2H:%<0>2M:%<0>2S:%<0>2F.

%y
   The time expressed as HH:MM:SS,FF, where HH is the hour of the day,
   MM is the minute of the hour, SS is the second of the minute and FF
   is the frame of the second. Same as %<0>2H:%<0>2M:%<0>2S,%<0>2F.

The interpreted device (%L(%d) and %S(%d)) and device type (%L(%r) and
%S(%r)) directives are:

%M
   The major.

%m
   The minor.

The interpreted setup (%M) directives are:

%S
   The start information (subformat directive).

%V
   The output setup information (subformat directive).

%l
   The loaders setup information (subformat directive).

The interpreted start (%M(%S)) directives are:

%f
   The play start frame index information (subformat directive).

%t
   The play start stream time information (subformat directive).

The interpreted play start frame index (%M(%S(%f))) directives are:

%i
   The frame index.

The interpreted output setup (%M(%V)) directives are:

%d
   The "device" output setup information (subformat directive, valid
   when output type was via **-o** or **--output** set to *device*).

The interpreted "device" output setup (%M(%V(%d))) directives are:

%n
   The name (set via **-C** or **--device-name**, same as %V(%d(%n))).

%p
   The path (set via **-d** or **--device**, same as %V(%d(%p))).

The interpreted loaders setup (%M(%l)) directives are:

%m
   The MPEG audio stream loader setup information (subformat directive).

The interpreted MPEG audio stream loader setup (%M(%l(%m))) directives
are:

%a
   The amplification information (subformat directive).

%e
   The equalizer information (subformat directive, valid when the
   amplification is not zero).

The interpreted MPEG audio stream amplification (%M(%l(%m(%a))))
directives are:

%l
   The level (real).

The interpreted MPEG audio stream equalizer (%M(%l(%m(%e)))) directives
are:

%b
   The equalizer band information (iterable subformat directive).

%c
   The equalizer bands count.

The interpreted MPEG audio stream equalizer band (%M(%l(%m(%e(%b)))))
directives are:

%f
   The frequency (real).

%i
   The index.

%l
   The level (real).

The interpreted controller (%Q) directives are:

%L
   The lanes controller information (subformat directive).

%N
   The no controller information (subformat directive).

%S
   The UNIX or INET server controller information (subformat directive).

The interpreted UNIX or INET server controller (**%Q(%S)**) directives
are:

%C
   The cycling status (*off* if the file will not be replayed, *on* if
   the file is repeated indefinitely, or a non-zero cardinal
   representing the remaining number of times the file will be
   replayed).

%a
   The repeat count (the number of times the currently playing file has
   been continuously repeated as a result of repeat requests).

%e
   The program execution information (subformat directive, no
   interpreted subdirectives, valid when a program is executing or
   queued for execution).

%q
   The program execution queue information (subformat directive).

%t
   The context program execution lists information (subformat
   directive).

%x
   The running program information (subformat directive, valid when a
   program is currently executing).

The interpreted UNIX or INET server controller execution queue
(%Q(%S(%q))), file play back start execution list (%Q(%S(%t(%p)))) and
file play back finish execution list (%Q(%S(%t(%q)))) directives are:

%l
   The queue or list length (the number of programs in the queue or
   list).

%p
   The program information (iterable subformat directive).

The interpreted UNIX or INET server controller execution queue program
(%Q(%S(%q(%p)))), file play back start execution list program
(%Q(%S(%t(%p(%p))))) and file play back finish execution list program
(%Q(%S(%t(%q(%p))))) directives are:

%d
   The ID (subformat directive).

%i
   The index (0 for the first to be executed).

%m
   The memory statistics information (subformat directive).

%t
   The title information (subformat directive, valid when a title is
   defined for the program).

The interpreted UNIX or INET server controller execution queue program
ID (%Q(%S(%q(%p(%d))))), file play back start execution list program ID
(%Q(%S(%t(%p(%p(%d)))))) and file play back finish execution list
program ID (%Q(%S(%t(%q(%p(%d)))))) directives are:

%M
   The major.

%m
   The minor.

The interpreted UNIX or INET server controller execution queue program
memory statistics (%Q(%S(%q(%p(%m))))), file play back start execution
list program memory statistics (%Q(%S(%t(%p(%p(%m)))))) and file play
back finish execution list program memory statistics
(%Q(%S(%t(%q(%p(%m)))))) directives are:

%s
   The allocated size, expressed in bytes.

The interpreted UNIX or INET server controller execution queue program
title (%Q(%S(%q(%p(%t))))), file play back start execution list program
title (%Q(%S(%t(%p(%p(%t)))))) and file play back finish execution list
program title (%Q(%S(%t(%q(%p(%t)))))) directives are:

%t
   The title text.

The interpreted UNIX or INET server controller context execution lists
(%Q(%S(%t))) directives are:

%p
   The file play back start execution list information (subformat
   directive).

%p
   The file play back finish execution list information (subformat
   directive).

The interpreted UNIX or INET server controller running program
(%Q(%S(%x))) directives are:

%d
   The ID (subformat directive).

%m
   The memory statistics information (subformat directive).

%t
   The title information (subformat directive, valid when a title is
   defined for the running program).

The interpreted UNIX or INET server controller running program ID
(%Q(%S(%x(%d)))) directives are:

%M
   The major.

%m
   The minor.

The interpreted UNIX or INET server controller running program memory
statistics (%Q(%S(%x(%m)))) directives are:

%s
   The allocated size, expressed in bytes.

The interpreted UNIX or INET server controller running program title
(%Q(%S(%x(%t)))) directives are:

%t
   The title text.

The interpreted output (%V) directives are:

%d
   The "device" output information (subformat directive, valid when
   output type was via **-o** or **--output** set to *device*).

%n
   The "null" output information (subformat directive, no interpreted
   directives, valid when output type was via **-o** or **--output** set
   to *null*).

%s
   The "skip" output information (subformat directive, no interpreted
   directives, valid when output type was via **-o** or **--output** set
   to *skip*).

The interpreted "device" output (%V(%d)) directives are:

%d
   The reported state (the internal acknowledged state of the program to
   which **Mylene1f4** redirects PCM data as reported by itself).

%n
   The name (set via **-C** or **--device-name**, same as
   %M(%V(%d(%n)))).

%p
   The path (set via **-d** or **--device**, same as %M(%V(%d(%p)))).

%q
   The process id.

The interpreted "device" output reported state (%V(%d(%d))) directives
are:

%A
   The "is system audio device attached" information (subformat
   directive, no interpreted directives, valid when the system audio
   device is opened).

%p
   The "is playing" information (subformat directive, no interpreted
   directives, valid when the commanded state is playing if system audio
   device attached).

The interpreted filter (%X) directives are:

%A
   The album match target (initially set via **-A** or **--album**).

%B
   The basename match target (initially set via **-B** or
   **--basename**).

%P
   The path match target (initially set via **-P** or **--path**).

%a
   The artist match target (initially set via **-a** or **--artist**).

%c
   The comment match target (initially set via **-c** or **--comment**).

%t
   The title match target (initially set via **-t** or **--title**).

%x
   The complex filter expression (initially set via **-F** or
   **--filter**).

The interpreted process (%Z) directives are:

%p
   The process id, as returned by **getpid**\ (2).

%s
   The process (approximate) start time (subformat directive).

%t
   The process (approximate) current time (subformat directive).

%u
   The process (approximate) up time (subformat directive).

The process (approximate) up time (%Z(%u)) directives are:

%T
   The time of day information (subformat directive).

%d
   The day.

%s
   The second day (subformat directive, no interpreted directives, valid
   when the up time day count is 1).

The interpreted files list (%l) directives are:

%f
   The files information (iterable subformat directive).

%l
   The list length.

%s
   The "is shuffled" information (subformat directive, no interpreted
   directives, valid when the files list is shuffled).

%t
   The "is tagged" information (subformat directive, no interpreted
   directives, valid when the files list is tagged, i.e. when was
   specified via one of **-i** or **--tagged-list** options).

The interpreted files in files list information (%l(%f)) directives are:

%N
   The name of the file (escapable).

%b
   The basename of the file (escapable).

%c
   The index in the current files list of the file.

%d
   The dirname of the file (escapable).

%i
   The index in the initial files list of the file.

%t
   The file tags (subformat directive, valid when the files list is
   tagged, i.e. when was specified via one of **-i** or
   **--tagged-list** options).

The interpreted file tags (%l(%f(%t))) directives are:

%A
   The album (escapable).

%a
   The artist (escapable).

%c
   The comment (escapable).

%g
   The genre (unimplemented, for now it outputs nothing).

%t
   The title (escapable).

%y
   The year (escapable).

%z
   The non zero length fields information (subformat directive).

The interpreted non zero length file tags (%l(%f(%t(%z)))) directives
are:

%A
   The album (subformat directive, no interpreted directives, valid when
   the album tag has a non zero length).

%a
   The artist (subformat directive, no interpreted directives, valid
   when the artist tag has a non zero length).

%c
   The comment (subformat directive, no interpreted directives, valid
   when the comment tag has a non zero length).

%g
   The genre (subformat directive, no interpreted directives, never
   valid).

%t
   The title (subformat directive, no interpreted directives, valid when
   the title tag has a non zero length).

%y
   The year (subformat directive, no interpreted directives, valid when
   the year tag has a non zero length).

The interpreted load order (%o) directives are:

%n
   The "next" order information (subformat directive, no interpreted
   directives).

%p
   The "previous" order information (subformat directive, no interpreted
   directives).

%z
   The "random" order information (subformat directive, no interpreted
   directives).

The interpreted source specific (%v) directives are:

%M
   The mpeg system stream information (subformat directive).

%a
   The A52a information (subformat directive).

%h
   The 1f42 map information (subformat directive).

%l
   The DVD audio private_stream_1 Linear PCM track information
   (subformat specifier).

%m
   The mpeg audio stream information (subformat directive).

The mpeg system stream (%v(%M)) directives are:

%S
   The specification (one of *ISO/IEC 11172-1* and *ISO/IEC 13818-1*).

The A52a (%v(%a)) directives are:

%E
   The frame rate denominator.

%e
   The frame rate numerator.

The 1f42 map (%v(%h)) directives are:

%A
   The multiple entries map information (subformat directive, valid when
   the entry count is not 1).

The multiple entries 1f42 map (%v(%h(%A))) directives are:

%E
   The entry count.

%e
   The entry index.

The DVD audio private_stream_1 Linear PCM track (%v(%l)) directives are:

%E
   The frame rate denominator.

%e
   The frame rate numerator.

%y
   The system information (subformat directive).

The DVD audio private_stream_1 Linear PCM track system (%v(%l(%y)))
directives are:

%t
   The private_stream_1 track id.

The mpeg audio stream (%v(%m)) directives are:

%B
   The bitrate.

%E
   The frame rate denominator.

%L
   The layer (arabic digits).

%M
   The mode extension.

%S
   The specification (one of *ISO/IEC 11172-3* and *ISO/IEC 13818-3*).

%e
   The frame rate numerator.

%f
   The frame rate (real periodic representation).

%l
   The layer.

%m
   The mode (one of *dual_channel*, *joint_stereo*, *single_channel* and
   *stereo*).

The 1f42 map (%x) directives are:

%F
   The frame count.

%l
   The stream length information (subformat directive).

%t
   The time to end of stream information (subformat directive).

The detailed version (%y) directives are:

%a
   The **Mylene1f4** MPEG audio decoder version.

%i
   The **aime** interpreter version (**aime** is the interpreter
   **Mylene1f4** uses for complex filtering and program execution).

After the **%**, the following modifiers may appear in sequence:

:order:
   A decimal digit string enclosed in colons (**:**) specifying the
   parent scope level in which the current format directive should be
   interpreted.

#. A flag specifying that the field should be displayed in an alternate
   form.

<string>
   A string enclosed in angle brackets (**<** and **>**) specifying the
   padding pattern used when the field width is less than the indicated
   minimum field width. The padding is applied by repeating the string
   as many times as necessary.

[+\|-]...width
   A decimal digit string preceded by zero or more **+** (plus) or **-**
   (minus) signs specifying a minimum field width.

.precision...
   One or more precision specifiers, each consisting of a leading **.**
   (period) and a decimal digit string. For real-number directives, the
   first precision specifier sets the minimum number of characters
   following the integral part minus 1, and the second sets the maximum
   number of digits in the fractional part.

Escapable string fields are printed in escaped form when the alternate
form (**#**) is requested.

This manual page may be incomplete or out of date regarding the
available formatting directives. Use **--list-directives**,
**--pack-directives**, or **--tree-directives** to inspect the compiled
list of supported directives.

FORMATTING EXAMPLES
===================

The following format string is suitable for stream header progress
information (it is the built-in *x1f4* progress type format for stream
headers). It concatenates:

   **"%b"**

..

   **"%x( \\(%l(%<0>2H:%<0>2M:%<0>2S:%<0>2F), %7F frames\\))\\n"**

which prints the basename of the file, followed by the 1f42 map stream
length and frame count (if the file has a 1f42 map);

   **"%G(%a\\n%t\\n)"**

which prints the artist and title if an ID3 tag was read; and

   **"%v(%m(%S, layer %3l))"**

..

   **"%v(%m(, %3B kbits/s, %:2:5s Hz, %14m, %M\\n))"**

which prints MPEG audio stream details.

List all files in the playlist along with their current index:

   **"%l(%f(%6c\\t%N\\n))"**

List a subset of files (indices 48‚63 and 80‚95), showing both their
current and initial indices along with artist and title:

   **"%l(%{48-63,80-95}f(%-5c%-5i%t(%-30a%-30t)\\n))"**

SERVER MODE
===========

If requested, **Mylene1f4** can establish a TCP/IP or UNIX domain socket
server to accept remote commands (see **--inet-server** and
**--unix-server**).

**Mylene1f4** is capable of serving only one client at a time. A
multi-client proxy server (such as **Mylene200**\ (1)) can be used to
allow simultaneous connections from multiple clients.

MAPPED STREAMS SUPPORT
======================

Apart from plain or system embedded MPEG audio streams **Mylene1f4**
understands 1f42 mapped plain or system embedded MPEG audio streams. The
1f42 stream maps contain information such as stream frame rate, stream
duration and number of frames, file position of audio stream frames.
Though not required when playing an MPEG audio stream, the 1f42 stream
map availability greatly extends **Mylene1f4** capabilities in
performing random access, providing stream information and so forth.

MPEG audio streams can be 1f42 mapped using **mapmaudio**\ (1).

SEE ALSO
========

**Mylene0fa**\ (1), **Mylene200**\ (1), **Mylene3e8**\ (1),
**aime**\ (1), **ctime**\ (3), **express17**\ (1), **lstat**\ (2),
**mapmaudio**\ (1), **stat**\ (2)

AUTHORS
=======

Ciprian Niculescu
