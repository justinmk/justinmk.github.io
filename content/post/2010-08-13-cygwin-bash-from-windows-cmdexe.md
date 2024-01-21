---
date: "2010-08-13T00:00:00Z"
published: true
title: How to call a Cygwin bash command from Windows CMD
---

Using Cygwin on Windows, sometimes you need to run a bash command or script from the Windows command line (CMD, DOS, shell). This is particularly useful if you want <b>to use the Task Scheduler</b> instead of <a href="http://www.devdaily.com/blog/post/linux-unix/get-crontab-started-running-when-using-cygwin">messing with cron on cygwin</a>. If you're going to schedule tasks on a Windows system, then the Windows&nbsp;Task Scheduler is probably the sane way to do it.

To run a bash script or command from outside of cygwin, you need to pass it to the bash executable with a little prep work. That "prep work" is contained in the following script (adapted from <a href="http://zenovations.com/blog/2009/06/how-to-run-a-cygwin-command-from-windows-scheduler-scheduled-tasks/">here</a>), called `cygrun.bat`:

    ::Use this script to run bash commands 
    ::from the Windows CMD (DOS) prompt.
    ::Pass the bash script or commands as arguments to this .bat.
    ::If no arguments are passed, a bash interactive shell starts.

    @echo off
        if "%DEF_PATH%"=="" set DEF_PATH=%PATH%
        ::Do _not_ surround path with quotes, even if paths have whitespace.
        set PATH=c:\cygwin\bin;%DEF_PATH%
        set args="%*"
        if "%args%"=="" goto noarg
        bash --rcfile %HOMEPATH%/.bashrc -i -c "%args%"
        sleep 1
        goto exit

        :noarg
        bash --login -i

        :exit

Then call that script, passing the bash script (or commands) to it as arguments.

    C:\path\to\cygrun.bat /cygdrive/c/path/to/bash_script.sh

Note:

- the argument(s) to cygrun.bat uses Cygwin bash (Unix) path syntax.
- If you have msysgit cygwin (instead of 'vanilla' cygwin), its bin directory is located somewhere like <code>c:\Program Files\Git\bin</code>
