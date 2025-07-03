

2025-01-21

----
#cmake #Cpp #C #vscode

## 개요
회사 다닐 적에 서드파티 라이브러리를 패키징할 때 가장 힘든 점은 CMake를 이용해서 빌드하는 것이었다. 
개인적으로 사용하는 맥북에선 CLion 라이선스가 종료돼서 VSCode를 써서 C++ 공부를 계속해 나가야 한다. 
대다수의 오픈소스들도 CMake를 사용하고 있기에 지금이 CMake를 익혀 보기에 적기이다. 

원래 CLion에서 사용하던 폴더구조는 다음과 같다. 
```txt
 changjoonlee@Changjoonui-MacBookPro  ~/Documents/Cpp/ThisIsCpp/Chapter11   main ±  tree -a -L 2
.
├── .DS_Store
├── .idea
│   ├── .gitignore
│   ├── Chapter11.iml
│   ├── misc.xml
│   ├── modules.xml
│   ├── vcs.xml
│   └── workspace.xml
├── AddressBook.c
├── AddressBook_OOP.cpp
├── CMakeLists.txt
├── DataManager.c
├── DataManager.h
├── LinkedList.c
├── LinkedList.h
├── USERDATA.cpp
├── USERDATA.h
├── UserInterface.c
├── UserInterface.h
└── cmake-build-debug
    ├── .DS_Store
    ├── .cmake
    ├── .ninja_deps
    ├── .ninja_log
    ├── Address.dat
    ├── AddressBook
    ├── AddressBook_OOP
    ├── CMakeCache.txt
    ├── CMakeFiles
    ├── Testing
    ├── build.ninja
    └── cmake_install.cmake

7 directories, 31 files
```

## 내용
먼저 vscode에서 설치해야 할 플러그인들이 있다. 
아래 플러그인들을 설치하자.
![[Pasted image 20250123154704.png|400]]

그리고 `CMakeLists.txt`가 있는 디렉터리에 `CMakePresets.json`을 만들고 다음과 같이 구성.
```json
{
    "version": 8,
    "configurePresets": [
        {
            "name": "macos-clang",
            "displayName": "도구 체인 파일을 사용하여 사전 설정 구성",
            "description": "Ninja 생성기, 빌드 및 설치 디렉터리 설정",
            "generator": "Ninja",
            "binaryDir": "${sourceDir}/build",
            "cacheVariables": {
                "CMAKE_BUILD_TYPE": "Debug",
                "CMAKE_MAKE_PROGRAM": "/opt/homebrew/bin/ninja",                 
                "CMAKE_INSTALL_PREFIX": "${sourceDir}/out/install/${presetName}",
                "CMAKE_C_COMPILER": "/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/clang",
                "CMAKE_CXX_COMPILER": "/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/clang++"
            }
        }
    ],
    "buildPresets": [
        {
          "name": "macos-clang-build",
          "configurePreset": "macos-clang"
        }
    ]    
}
```

CMake의 빌드 단축키인 `F7`을 누르면, 빌드 결과는 다음과 같이 뜬다. 
![[Pasted image 20250122150548.png]]

여기까지만 설정해도 `ctrl + F5`를 눌러서 실행은 가능하다. 
디버깅도 가능하긴 하다. 
`shift + F5`를 누르면 다음과 같이 CMake 전용 디버깅 콘솔에서 실행할 수 있다. 
![[Pasted image 20250123155223.png|400]]

디버그 포인트를 찍어서 변수 내부의 값을 확인할 수 있다. 
문제는 `Unable to perform this action because the process is running.`가 뜨면서 사용자 입력을 받는 부분이 안된다는 것. 

공식문서를 보면, CMake는 MakeFile, Ninja와 같이 프로젝트의 빌드시스템을 생성하는 소프트웨어이다. 
> CMake is a tool to manage building of source code. Originally, CMake was designed as a generator for various dialects of `Makefile`, today CMake generates modern buildsystems such as `Ninja` as well as project files for IDEs such as Visual Studio and Xcode.

즉, 컴파일러가 어느 파일을 어떻게 컴파일할지 결정하도록 만드는 시스템인 것. 
CMake가 빌드를 완료하면, 실행파일들이 지정한 폴더(우리의 경우 `${sourceDir}/build`에)에 생성된다. 
이걸 디버깅하는 건 또 다른 프로그램이 필요한데, 맥os의 경우 lldb가 있다. 
터미널에 `lldb --version`을 쳤을 때 (만약 이미 설치가 돼 있다면) 버전을 확인할 수 있고, `which lldb`를 통해 실행 파일 위치를 알 수 있다. 
![[Pasted image 20250123161003.png]]

이 실행파일을 이용하여 디버깅을 하기 위해선 앞서 설치한 CodeLLDB를 쓰면 된다. 
좌측의 Activity Bar에서 디버깅 창을 열고 `create a launch.json file`을 누른다. 
![[Pasted image 20250123162817.png|400]]

눌렀을 때 다음과 같은 파일이 생성된다. 
```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
    ]
}
```

`configurations` 안에 다음 오브젝트를 추가.
```json
        {
            "type": "lldb",
            "request": "launch",
            "name": "Debug",
            "program": "${fileDirname}/build/${fileBasenameNoExtension}",
            "args": [],
            "cwd": "${workspaceFolder}"
        }
```

이제 파일 구조는 다음과 같이 구성됐다. 
```bash
 changjoonlee@Changjoonui-MacBookPro  ~/Documents/Cpp/ThisIsCpp/Chapter11   main ±  tree -a -L 2
.
├── .DS_Store
├── .idea
│   ├── .gitignore
│   ├── Chapter11.iml
│   ├── misc.xml
│   ├── modules.xml
│   ├── vcs.xml
│   └── workspace.xml
├── .vscode
│   ├── .DS_Store
│   └── launch.json
├── AddressBook.c
├── AddressBook_OOP.cpp
├── CMakeLists.txt
├── CMakePresets.json
├── DataManager.c
├── DataManager.h
├── LinkedList.c
├── LinkedList.h
├── USERDATA.cpp
├── USERDATA.h
├── UserInterface.c
├── UserInterface.h
├── build
│   ├── .cmake
│   ├── .ninja_deps
│   ├── .ninja_log
│   ├── AddressBook
│   ├── AddressBook_OOP
│   ├── CMakeCache.txt
│   ├── CMakeFiles
│   ├── build.ninja
│   └── cmake_install.cmake
└── cmake-build-debug
    ├── .DS_Store
    ├── .cmake
    ├── .ninja_deps
    ├── .ninja_log
    ├── Address.dat
    ├── AddressBook
    ├── AddressBook_OOP
    ├── CMakeCache.txt
    ├── CMakeFiles
    ├── Testing
    ├── build.ninja
    └── cmake_install.cmake

10 directories, 37 files
```

내가 실행하고자 하는 `main` 함수가 있는 파일을 포커싱한 뒤 `F5`를 누르면 `integrated terminal`에서 입출력까지 정상적으로 된다.
![[Pasted image 20250123164539.png]]

*역시 난 프로다.* 
![[Pasted image 20250123164442.png]]