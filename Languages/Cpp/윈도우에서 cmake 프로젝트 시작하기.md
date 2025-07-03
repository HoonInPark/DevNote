

2025-04-05

----
#Cpp #cmake 

## 개요
컴퓨터를 osx에서 windows로 바꿨다. 
기존에 쓰던 vscode 환경에서 만든 cmake 프로젝트를 Visual Studio에서 사용하고 싶다. 

## 내용
`CMakePresets.json`을 수정하자. 
```json
{
    "version": 3,
    "configurePresets": [
        {
            "name": "windows-base",
            "hidden": true,
            "generator": "Ninja",
            "binaryDir": "${sourceDir}/build/${presetName}",
            "installDir": "${sourceDir}/install/${presetName}",
            "cacheVariables": {
                "CMAKE_C_COMPILER": "cl.exe",
                "CMAKE_CXX_COMPILER": "cl.exe"
            },
            "condition": {
                "type": "equals",
                "lhs": "${hostSystemName}",
                "rhs": "Windows"
            }
        },
        {
            "name": "x64-debug",
            "displayName": "x64 Debug",
            "inherits": "windows-base",
            "architecture": {
                "value": "x64",
                "strategy": "external"
            },
            "cacheVariables": {
                "CMAKE_BUILD_TYPE": "Debug"
            }
        },
        {
            "name": "x64-release",
            "displayName": "x64 Release",
            "inherits": "x64-debug",
            "cacheVariables": {
                "CMAKE_BUILD_TYPE": "Release"
            }
        },
        {
            "name": "x86-debug",
            "displayName": "x86 Debug",
            "inherits": "windows-base",
            "architecture": {
                "value": "x86",
                "strategy": "external"
            },
            "cacheVariables": {
                "CMAKE_BUILD_TYPE": "Debug"
            }
        },
        {
            "name": "x86-release",
            "displayName": "x86 Release",
            "inherits": "x86-debug",
            "cacheVariables": {
                "CMAKE_BUILD_TYPE": "Release"
            }
        }
    ]
}
```

새롭게 생성한 Visual Studio의 CMake 프로젝트의 동제 파일에서 다음 부분만 수정했다. 
```json
"binaryDir": "${sourceDir}/build/${presetName}",
"installDir": "${sourceDir}/install/${presetName}",
```

기존 파일구조랑 잘 맞도록 고친 것임.