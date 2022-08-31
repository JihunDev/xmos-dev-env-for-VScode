# Development environment for VScode

## Install
- 드라이버 다운로드 : [Software Tools](https://www.xmos.ai/software-tools)

### for Mac OS
- 다운로드 된 .dmg 파일 오픈 후 Applications Folder로 복사
- Terminal을 이용하여 설치한 디렉토리로 이동 후 아래의 커맨드 입력
```shell
$ SetEnv.command
```
- 환경 설정 구성 확인
```shell
$ xcc --help
```

### for Windows
- 다운로드 받은 설치 프로그램에 지침에 따라 프로그램 설치
- Bash 환경을 사용하기 위한 Git for Windows 설치
	- Git 다운로드 : [Git for Windows](https://git-scm.com/downloads).
	- 설치 시 표준 지침에 따라 설치하고 아래 이미지와 같이 선책
		![install windows setting](/image/git_for_windows.png)
- Start Button ->  XMOS -> Command Prompt(15.x.x) 실행
- bash 환경 실행을 위한 커맨드 입력
```shell
> bash
```
- 환경 설정 구성 확인
```shell
$ xcc --help
```

# Configration common to IDE
## Install VScode
- [Visual Studio Code](https://code.visualstudio.com/).

## Install Extensions
- VScode 설치후 C/C++ 확장 프로그램 설치 : [C/C++ extension for VS Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools).

## Bash Scripts
- 프로젝트 폴더에 빌드 커맨드 입력용 Bash Scripts 3개 생성

**build.sh**
```shell
#!/bin/bash

CWD=`pwd`
xcc -target=XCORE-200-EXPLORER -g $CWD/main.c
```

**run.sh**
```shell
#!/bin/bash

xrun --io a.xe
```

**debug.sh**
```shell
#!/bin/bash

xgdb a.xe
```

## Configure VScode
- Project 폴더에 `.vscode` 생성
- 생성된 폴더에 하단 2개의 파일 생성

**tasks.json**
```json
{
	"version": "2.0.0",
	"tasks": [
		{
			"label": "Build active file",
			"type": "shell",
			"command": "bash",
			"args": [
				"./build.sh"
			],
			"options": {
				"cwd": "${fileDirname}"
			},
			"problemMatcher": [
				"$gcc"
			],
			"group": {
				"kind": "build",
				"isDefault": true
			}
		},
		{
			"label": "Run active file",
			"type": "shell",
			"command": "bash",
			"args": [
				"./run.sh"
			],
			"options": {
				"cwd": "${fileDirname}"
			},
			"problemMatcher": []
		},
		{
			"label": "Debug active file",
			"type": "shell",
			"command": "bash",
			"args": [
				"./debug.sh"
			],
			"options": {
				"cwd": "${fileDirname}"
			},
			"problemMatcher": []
		}
	]
}
```

**c_cpp_properties.json**
```json
{
    "version": 4,
    "configurations": [
        {
            "name": "Xcore",
            "includePath": [
                "${workspaceFolder}/**"
            ],
            "defines": ["XSCOPE_IMPL"],
            "compilerPath": "${XMOS_TOOL_PATH}/bin/xcc",
            "cStandard": "gnu11",
            "cppStandard": "gnu++14",
            "intelliSenseMode": "clang-x64",
            "compilerArgs": [
                "-save-temps"
            ]
        }
    ]
}
```

# VScode Command
## Build
`Ctrl + Shift + B`

## Run
`Ctrl + P` 
Typing `task Run`

## Debug
`Ctrl + P` 
Typing `task Debug`