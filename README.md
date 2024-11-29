## Attempt to compile QGNSS for GNU/Linux

Although a Qt5 application, the QGNSS source code includes many Microsoft 
specific C++ compiler features not availabe in GCC (``g++``) and hardly 
available in LLVM (``clang++``). Although the executable is generated, it is
not functional at the moment since most functions in
``QGNSS_FW_UPG/QGNSS_FW_UPG/uart/QG_Uart_win.cpp`` have been commented out,
meaning serial communication is *not* functional. Other functions that were
removed to reach compilation but should be inserted back are
```bash
$ grep -r JMF * | grep cpp
QGNSS/deviation_map.cpp:    ui->customPlot->axisRect(0)->setAutoMargins(QCP::msNone); // 去除边框 JMF
QGNSS_FW_UPG/QG_Common.cpp:   int size; // = WideCharToMultiByte(CP_ACP, 0, &wstr[0], (int)wstr.size(), NULL, 0, NULL, NULL); JMF
QGNSS_FW_UPG/QG_Common.cpp:   int len; // = MultiByteToWideChar(CP_UTF8, 0, str.c_str(), -1, NULL, 0); JMF
QGNSS_FW_UPG/QG_Common.cpp://   MultiByteToWideChar(CP_UTF8, 0, str.c_str(), -1, wide, len);  JMF
QGNSS_FW_UPG/QGNSS_FW_UPG/QG_Print.cpp:#define _itoa_s(a,b,c) sprintf(b, "%d", a) // JMF CORRECT (not using the base)
QGNSS_FW_UPG/QGNSS_FW_UPG/QG_Print.cpp:                 // std::transform(strtemp, strtemp + 8, strtemp, std::tolower); JMF
QGNSS_FW_UPG/QGNSS_FW_UPG/QG_Print.cpp:                 // std::transform(strtemp, strtemp + 8, strtemp, std::toupper); JMF
```

See the [patch](patch) file generated with ``git diff`` after modifying the original source code so
it compiles under GNU/Linux.

# Requirements:

Dependencies (Debian GNU/Linux unstable): at least
```
sudo apt install qtwebengine5-dev qttools5-dev
```
and a functional ``clang/clang++`` compiler

# Compiling:

```
qmake QGNSS.pro  # generate Makefile in current directory
make             # generate Makefile in QGNSS: check that CC = clang and CXX = clang++
```
