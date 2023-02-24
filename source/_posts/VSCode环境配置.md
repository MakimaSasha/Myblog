---
title: VSCode环境配置
date: 2022-05-12 9:21:45
tag: 其他
---

## task.json

```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "type": "shell",
            "label": "task g++",    //修改此项
            "command": "F:\\Program Files\\Cmove\\mingw64\\bin\\g++.exe",	//修改为当前g++路径
            "args": [
                "-g",
                "${file}",
                "-o",
                "${fileDirname}\\${fileBasenameNoExtension}.exe"
            ],
            "options": {
                "cwd": "F:\\Program Files\\Cmove\\mingw64\\bin"	//修改为当前mingw64路径
            },
            "problemMatcher": [
                "$gcc"
            ],
            "group": "build"
        }
    ]
}
```

## launch.json

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "g++.exe build and debug active file",
            "type": "cppdbg",
            "request": "launch",
            "program": "${fileDirname}\\${fileBasenameNoExtension}.exe",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": false,      //修改此项，让其弹出终端
            "MIMode": "gdb",
            "miDebuggerPath": "F:\\Program Files\\Cmove\\mingw64\\bin\\gdb.exe", //修改为当前gdb路径
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "task g++" //修改此项
        }
    ]
}
```

