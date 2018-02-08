budir
-----
::

  Usage: budir [DIR]

  Summary: Compress the contents of a directory into a .tgz file
           saved in a dedicated backup directory.

  'budir' performs the following actions:

  1. Obtain the path to the destination directory by invoking 'rabot-vars backupDir'
     and create this directory if it doesn't already exist.

  2. Run the script 'tgzdir' to compress the contents of DIR into a .tgz file
     located in the destination directory. If DIR is omitted, the current working
     directory is used as DIR.

  See also: 'tgzdir', 'zipdir', 'cleandir'

cleandir
--------
::

  Usage: cleandir [DIR]

  Clean up intermediate files in a directory

  'cleandir' performs the following actions:

  1. If DIR is omitted, use the current working diretory
     as DIR.

  2. Look for an executable file named 'clean' in DIR
     and run it, if found.

  3. Look for a file named 'clean.txt' in DIR and - if
     found - delete all files and directories named in
     this file.

  4. Look for a file named 'Makefile' in DIR and - if
     found - run the command 'make clean'.

decrypt
-------
::

  Usage: decrypt INFILE [OUTFILE]

  'decrypt' decrypts INFILE with the aes-256-cbc
  cipher and saves the result as OUTFILE. If INFILE ends
  with '.enc' then you can omit OUTFILE, and the output
  filename will be INFILE without its '.enc' suffix.
  If the output file exists, it will be overwritten.
  You will be prompted for a password.

  See also: 'encrypt'

docScripts
----------
::

  Usage: docScripts [DIR]

  'docScripts' prints the documentation of all executable files found in a given
  directory tree DIR or, by default, in the Current Working Directory (CWD).
  First the contents of a file 'DIR/readme-head.rst' (if it exists) are printed to
  stdout. Then every executable file in DIR is called with the option '--help' and
  additional reStructuredText markup is added to the output. Finally, the contents of
  a file 'DIR/readme-tail.rst' (if it exists) are printed to stdout.

  Example:

  Create a README file in reStructuredText format, documenting all scripts in the CWD:

      $ docScripts > README.rst

encrypt
-------
::

  Usage: encrypt FILENAME [OUTFILE]

  'encrypt' encrypts FILENAME with the aes-256-cbc
  cipher and saves the result as either FILENAME.enc or OUTFILE.
  If the output file exists, it will be overwritten.
  You will be prompted twice for a password.

  See also: 'decrypt'

flon
----
::

  Usage: FINDCOMMAND | flon

  'flon' takes the output of a 'find' command
  (or a command with similar output) and opens it in the
  'quickfix' window of a 'vim' session.

  This allows for easy navigation through all files found
  by FINDCOMMAND.

  At startup, 'vim' will map the ':cn' and ':cp' commands
  to the 'F6' and '<SHIFT>-F6' key combinations,
  respectively. You can change this mapping either by
  editing the file 'vimnav' or by editing/overriding
  the variable 'vimNav' in 'rabot-vars'.

  The 'quickfix' window will assume that the output
  contains nothing but filenames, as 'vim' will be
  started with 'errorformat' equal to '%f'.

  Example:

      $ find . -type f | flon

  This will open 'vim' and display the 'quickfix' window.
  The latter window will contain a list of every file in
  the current working directory and all its subdirectories.

  See also: 'glon'

fnd
---
::

  Usage: fnd ['find' arguments]

  'fnd' wraps the 'find' tool by adding options that make it
  exclude directories with the following names:

      '.git', '.hg', '.svn', '.bzr' and 'CVS'

  See also: 'fnd0', 'grp'

fnd0
----
::

  Usage: fnd0 ['find' arguments]

  'fnd0' is similar to 'fnd' but adds a '-print0' option to
  the 'find' command.

  For more info, see 'fnd --help'.

fnt
---
::

  Usage: fnt DIRECTORY [FILE EXTENSIONS]

  'fnt' wraps the 'fnd' script by searching in
  DIRECTORY for filenames with extensions given as a list
  of arguments.

  Directory names having any of the given extensions are
  not listed.

  The string matching of extensions is case-insensitive.

  Example:
      $ fnt . cpp h
      ./utils.h
      ./main.cpp
      ./utils.cpp

  See also: 'fnd', 'fnd0', 'grp'

fullts
------
::

  Usage: fullts [FILE]

  'fullts' displays the current time in the format:
  'YYYMMDDhhmmss'. If the argument FILE is given, it displays
  the timestamp of FILE in this format.

  See also: 'timestamp-id'

glon
----
::

  Usage: GREPCOMMAND | glon

  'glon' takes the output of a 'grep' command
  (or a command with similar output) and opens it in the
  'quickfix' window of a 'vim' session.

  This allows for easy navigation through all matching
  lines found by GREPCOMMAND.

  At startup, 'vim' will map the ':cn' and ':cp' commands
  to the 'F6' and '<SHIFT>-F6' key combinations,
  respectively. You can change this mapping either by
  editing the file 'vimnav' or by editing/overriding
  the variable 'vimNav' in 'rabot-vars'.

  The 'quickfix' window will assume the following format
  for the output lines:

      '%f:%l:%m'

  where '%f' is the filename, '%l' is the linenumber and
  '%m' is the rest of the line.

  If 'grep' is used as the command, the option '-n' must
  be used in order to produce this format.

  Example:

       $ grp rabot . | glon

  This makes use of the 'grep' wrapper script called 'grp'.
  Vim will be started and the quickfix window will be
  displayed, containing a list of all occurences of the
  search term 'rabot' found in files of the current working
  directory and its subdirectories.

  See also: 'flon'

grp
---
::

  Usage: grp [OPTIONS] REGEX DIRECTORY

  'grp' wraps the 'grep' tool by adding the options: '-nrIP'.

  This means, respectively: display line numbers, search recursively
  through the directory tree, skip binary files and use the PCRE regex
  flavour.

  Additionally, directories named '.git', '.hg', '.svn', '.bzr' or 'CVS'
  will be skipped during the search and output will be displayed in
  colour.

  See also: 'fnd'

lfiles
------
::

  Usage: lfiles [DIR]

  'lfiles' outputs the full paths to all regular files in
  the directory DIR (or the current working directory if
  the argument is omitted), one file per line and sorted by
  modification time in reverse chronological order (newest
  first). This output can be useful as input to other tools,
  like 'pickn' or 'pick1'.

  Examples:

  List log files created by 'logop':

      $ lfiles ~/log
      /home/user/log/20161120205104_tBS.txt
      /home/user/log/20161120153503_zyW.txt
      /home/user/log/20161120153224_sFf.txt

  Choose one log file to view:

      $ lfiles ~/log | pick1 | xargs -r less

  See also: 'pick1', 'pickn', 'logop'

logop
-----
::

  Usage:
      first form:
          logop COMMAND [ARG1]...

      second form:
          COMMAND [ARG1]... | logop

  In the first form, 'logop' invokes the command string and sends
  its output (both stdout and stderr) to two different targets: stdout and a
  logfile.

  In the second form, the stdout of the command is piped to 'logop',
  where it is duplicated over stdout and a logfile. If you want to log stderr
  too, redirect it to stdout first, like this:

      COMMAND [ARG1]... 2>&1 | logop

  In addition to passing on the output of the command, 'logop'
  adds a header and a footer section with supplementary information. If the
  second form is used however, this information will not contain the command
  string that has been invoked nor the exit status of the command.

  The logfile is saved in the folder obtained from invoking 'rabot-vars logDir'.
  The filename of the logfile has the following form:

      YYYYMMDDhhmmss_RND.txt

  The part before the extension is the current time and a random alphanumerical
  string, as explained in 'timestamp-id --help'.

  In the log directory a symbolic link called 'latest' will be created or updated
  pointing to the newly created logfile.

  Examples:

  A minimal sample of the first form:

      user@host ~ $ logop echo Hello
      ==== Start log: 2014-05-23 22:31:09
      ==== Logscript: /home/user/tools/rabot/logop/logop
      ==== Command: echo Hello
      ==== Working directory: /home/user
      ==== Logfile: /home/user/log/20140523223109_f4w.txt

      Hello

      ==== Exit status: 0
      ==== Elapsed: 0.00 seconds
      ==== End log: 2014-05-23 22:31:09

  A minimal sample of the second form:

      user@host ~ $ echo Hello | logop
      ==== Start log: 2014-05-23 22:34:24
      ==== Logscript: /home/user/tools/rabot/logop/logop
      ==== Working directory: /home/user
      ==== Logfile: /home/user/log/20140523223423_q5n.txt

      Hello

      ==== Elapsed: 0.00 seconds
      ==== End log: 2014-05-23 22:34:24

  See also: 'logopd', 'logopf'

logopd
------
::

  Usage:
      first form:
          logopd DIR COMMAND [ARG1]...

      second form:
          COMMAND [ARG1]... | logopd DIR

  The behavior of 'logopd' is similar to 'logop', with the
  following differences:

  - An extra 'DIR' argument will override the value provided by
    'rabot-vars logDir'.

  - The symlink called 'latest.txt' in the default log directory will
    not be updated. Instead, a 'latest.txt' symlink is created/updated
    in the 'DIR' directory.

  For more info, see: 'logop --help'

  A minimal sample of the first form:

      user@host ~ $ logopd mylogdir echo Hello
      ==== Start log: 2014-05-23 22:37:40
      ==== Logscript: /home/user/tools/rabot/logop/logopd
      ==== Command: echo Hello
      ==== Working directory: /home/user
      ==== Logfile: /home/user/mylogdir/20140523223740_8yo.txt

      Hello

      ==== Exit status: 0
      ==== Elapsed: 0.00 seconds
      ==== End log: 2014-05-23 22:37:40

  A minimal sample of the second form:

      user@host ~ $ echo Hello | logopd mylogdir
      ==== Start log: 2014-05-23 22:38:17
      ==== Logscript: /home/user/tools/rabot/logop/logopd
      ==== Working directory: /home/user
      ==== Logfile: /home/user/mylogdir/20140523223817_0r0.txt

      Hello

      ==== Elapsed: 0.00 seconds
      ==== End log: 2014-05-23 22:38:17

  See also: 'logop', 'logopf'

logopf
------
::

  Usage:
      first form:
          logopf FILE COMMAND [ARG1]...

      second form:
          COMMAND [ARG1]... | logopf FILE

  The behavior of 'logopf' is similar to 'logop', with the
  following differences:

  - An extra 'FILE' argument specifies the logfile. 'logopf'
    never deletes the contents of this file but only appends to it.

  - No symlink 'latest.txt' is created or updated.

  For more info, see: 'logop --help'

  A minimal sample of the first form:

      user@host ~ $ logopf mylogfile.txt echo Hello
      ==== Start log: 2014-05-23 22:43:03
      ==== Logscript: /home/user/tools/rabot/logop/logopf
      ==== Command: echo Hello
      ==== Working directory: /home/user
      ==== Logfile: /home/user/mylogfile.txt

      Hello

      ==== Exit status: 0
      ==== Elapsed: 0.00 seconds
      ==== End log: 2014-05-23 22:43:03

  A minimal sample of the second form:

      user@host ~ $ echo Hello | logopf mylogfile.txt
      ==== Start log: 2014-05-23 22:43:18
      ==== Logscript: /home/user/tools/rabot/logop/logopf
      ==== Working directory: /home/user
      ==== Logfile: /home/user/mylogfile.txt

      Hello

      ==== Elapsed: 0.00 seconds
      ==== End log: 2014-05-23 22:43:18

  See also: 'logop', 'logopd'

pick1
-----
::

  Usage: pick1 [TEXT]

  'pick1' reads from stdin and after reaching EOF
  displays a dialog box with all lines read as items in a
  menu. When you select an item, 'pick1' prints
  the item on stdout and terminates.

  When pressing "Cancel" 'pick1' terminates without
  printing any output.

  With the optional argument TEXT you can put an explanatory
  text in the dialog. The default is the empty string.

  No dialog is shown and no output is printed when the input is
  empty.

  This script depends on the command-line tool 'dialog'.

  Example:

  Choose one file in ~/log to view with 'less':

      $ lfiles ~/log | pick1 | xargs -r less

  See also: 'pickn', 'lfiles'

pickn
-----
::

  Usage: pickn [TEXT] [STATUS]

  'pickn' reads from stdin and after reaching EOF displays
  a dialog box with all lines read as selectable items.
  You can select or deselect every item individually.
  When you press "OK", 'pickn' prints a list of the selected
  items on stdout, one item per line, and terminates.

  When pressing "Cancel" 'pickn' terminates without printing
  any output.

  With the optional argument TEXT you can put an explanatory
  text in the dialog. The default is the empty string.

  STATUS determines the initial state of the items. 'off' for
  deselected and 'on' for selected. Default is 'off'.

  No dialog is shown and no output is printed when the input is
  empty.

  This script depends on the command-line tool 'dialog'.

  Examples:

  Choose a couple of files in ~/log to remove:

      $ lfiles ~/log | pickn | xargs -r rm

  In a git working directory, stage some of the modified files
  for the next commit:

      $ git diff --name-only | pickn | xr git add

  See also: 'pick1', 'lfiles'

rabot-vars
----------
::

  Usage: rabot-vars VARNAME

  'rabot-vars' collects some configuration settings of 'rabot'.

  It will output the value of the variable whose name is specified
  as a command-line argument.

  These values can be overridden outside 'rabot-vars' by redefining
  the variable before calling this script. For example:

      $ rabot-vars logDir
      MyNormalLogDir
      $ export logDir=MySpecialLogDir
      $ rabot-vars logDir
      MySpecialLogDir

  The value of the variables can also be changed permanently by editing
  'rabot-vars'.

  For a list of all variables defined by 'rabot-vars' and
  their values, see the source code of the script.

  If you are a first-time user of rabot, you probably might want to edit
  this script to change the default values of some of the variables.

randid
------
::

  Usage: randid [LENGTH]

  'randid' prints a random alphanumerical string of
  LENGTH characters (3 by default).

  Example:

      $ randid 5
      mx2ft

tgzdir
------
::

  Usage: tgzdir [DIR] [DESTDIR] [PREFIX] [ROOTDIR]

  'tgzdir' compresses the directory DIR to a .tgz file and saves
  the latter in the directory DESTDIR.

  The filename has the following pattern:

      'NAME_YYYMMDDhhmmss_RND.tgz'

  where 'NAME' is either equal to the name of 'DIR' or to 'PREFIX' if the
  latter argument is given, 'YYYMMDDhhmmss' is the current datetime and 'RND'
  is a 3-character random alphanumerical string.

  If omitted, DESTDIR will be the parent directory of DIR. If DIR is omitted
  too, the current working directory is taken as DIR.

  Paths inside the .tgz file will be relative to DIR's parent directory,
  unless you specify ROOTDIR, then they will be relative to ROOTDIR. DIR must
  be a subdirectory of ROOTDIR though. If you want to specify ROOTDIR but not
  PREFIX you can use '_' (underscore) as value for PREFIX.

  Examples:

  Compress the current working directory and put the file in the parent
  directory:

      user@host ~/projects/myprj $ tgzdir
      /home/user/projects/myprj_20180103163250_o5c.tgz

  Compress the directory '~/somedir/mydir' and put the file in the current
  working directory:

      user@host ~ $ tgzdir somedir/mydir .
      /home/user/mydir_20180103162834_tyh.tgz

  See also: 'zipdir', 'tgzfiles'

tgzfiles
--------
::

  Usage: tgzfiles FILELIST DESTDIR [PREFIX]

  'tgzfiles' reads the file FILELIST and creates a .tgz file
  (with the command 'tar') containing all files and directories
  listed in FILELIST.

  FILELIST must contain one path to a file or directory per line.
  Paths can be either absolute or relative to the current working
  directory.

  If a path starts with '~', the tilde will be
  replaced with the value of \/home/bert (on this system: /home/bert)
  before being passed to 'tar'.

  Inside the created .tgz file, all paths will be absolute, even
  the paths that were relative in the FILELIST.

  The directory DESTDIR will be created if it does not exist.

  The name of the destination file will be in the format:
      YYYYMMDDhhmmss_rnd.tgz
  where 'YYYYMMDDhhmmss' is the creation time of the .tgz file
  and 'rnd' is a random 3-character string consisting of numerals
  and/or lowercase letters. If a third argument 'PREFIX' is
  specified, the filename will be:
      PREFIX_YYYYMMDDhhmmss_rnd.tgz

  Example:

  With a file 'filelist.txt' containing the following two lines:
      one.txt
      two.txt

  The command and its output look like this:
      user@host ~ $ tgzfiles filelist.txt .
      /home/user/one.txt
      /home/user/two.txt
      /home/user/20140519142819_5sp.tgz

  See also: 'tgzdir'

timestamp-id
------------
::

  Usage: timestamp-id

  'timestamp-id' will print the current time plus a
  3-character random alphanumerical string in the following way:

      YYYYMMDDhhmmss_RND

  where 'YYYYMMDDhhmmss' is the timestamp (produced by 'fullts')
  and 'RND' is the random string (produced by 'randid').

  Example:

      user@host ~ $ timestamp-id
      20140328133629_1oy

  See also: 'fullts', 'randid'

walkdir
-------
::

  Usage: walkdir DIR COMMAND [ARG1]...

  'walkdir' performs COMMAND with its arguments in
  every directory of the tree rooted in DIR.

  Example:

      user@host / $ walkdir ~ pwd
      /home/user
      /home/user/mydir
      /home/user/myotherdir

zipdir
------
::

  Usage: zipdir [DIR] [DESTDIR] [PREFIX] [ROOTDIR]

  'zipdir' compresses the directory DIR to a .zip file and saves
  the latter in the directory DESTDIR.

  The filename has the following pattern:

      'NAME_YYYMMDDhhmmss_RND.zip'

  where 'NAME' is either equal to the name of 'DIR' or to 'PREFIX' if the
  latter argument is given, 'YYYMMDDhhmmss' is the current datetime and 'RND'
  is a 3-character random alphanumerical string.

  If omitted, DESTDIR will be the parent directory of DIR. If DIR is omitted
  too, the current working directory is taken as DIR.

  Paths inside the .zip file will be relative to DIR's parent directory,
  unless you specify ROOTDIR, then they will be relative to ROOTDIR. DIR must
  be a subdirectory of ROOTDIR though. If you want to specify ROOTDIR but not
  PREFIX you can use '_' (underscore) as value for PREFIX.

  Examples:

  Compress the current working directory and put the file in the parent
  directory:

      user@host ~/projects/myprj $ zipdir
      /home/user/projects/myprj_20180103163250_o5c.zip

  Compress the directory '~/somedir/mydir' and put the file in the current
  working directory:

      user@host ~ $ zipdir somedir/mydir .
      /home/user/mydir_20180103162834_tyh.zip

  See also: 'tgzdir', 'tgzfiles'

