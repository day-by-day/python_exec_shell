# python_exec_shell
#python执行shell命令



#!/usr/bin/env python
import shlex
import datetime
import time
import subprocess



def execute_command(cmdstring,cwd=None,timeout=None,shell=False):
    if shell:
        cmdstring_list = cmdstring
    else:
        cmdstring_list = shlex.split(cmdstring)
    if timeout:
        end_time = datetime.datetime.now() + datetime.timedelta(seconds=timeout)

    sub = subprocess.Popen(cmdstring_list, cwd=cwd, stdin=subprocess.PIPE, shell=shell, bufsize=4096)
    while sub.poll() is None:
        time.sleep(0.1)
        if timeout:
            if end_time <= datetime.datetime.now():
                raise Exception("Timeout:%s" % cmdstring)

    return str(sub.returncode)
