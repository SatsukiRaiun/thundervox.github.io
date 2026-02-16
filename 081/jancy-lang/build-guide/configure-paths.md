# paths.cmake

Jancyビルドシステムは、特定のツールまたはライブラリを検索する必要がある場合、 `paths.cmake` ファイルを主な参照として使用します。パスが指定されていない場合、 `find_package` を使用してフォールバック検索が行われます。

これにより、すぐに使用できる _デフォルト_ のビルドが可能になると同時に、依存関係の場所を _きめ細かく_ 制御できる。ここTibboでは、1台のビルドマシンに複数のバージョンのツールとライブラリがインストールされており、同時に、特定のプロジェクトをビルドするときにどのツールやライブラリを使用するかを`常に`完全に制御している。

`paths.cmake`ファイルは**カスケード**されています。つまり、現在のディレクトリの上の任意の場所に配置すると、そのファイルが検索されて使用されます。そこから、次の `paths.cmake` などをチェーンインクルードできます。これにより、すべてのプロジェクトに既定の場所を指定できますが、サブプロジェクトのパスを上書きすることもできます。

マシン固有の` paths.cmake` ファイルは `.gitignore` に追加され、Git内で追跡されることはない。したがって、構成プロセスの最初のステップとして `paths.cmake` ファイルを書き込む必要がある。では、中には何が入っているべきなのでしょうか？

この質問に答えるには、 `dependencies.cmake` ファイルを確認する必要があります。このファイル内の `AXL_PATH_LIST` という変数には、ビルド中に使用されるすべてのパスが含まれています。

``` bash
LUA_INC_DIR         # Lua C インクルードディレクトリのパス
LUA_LIB_DIR         # Lua ライブラリディレクトリのパス
LUA_LIB_NAME        # Lua ライブラリ名 (lua/lua51/lua52/lua53)
LLVM_INC_DIR        # LLVM インクルードディレクトリのパス (リスト可)
LLVM_LIB_DIR        # LLVM ライブラリディレクトリのパス
LLVM_CMAKE_DIR      # LLVM CMake モジュールディレクトリのパス
PCAP_INC_DIR        # (オプション) Pcap インクルードディレクトリのパス
PCAP_LIB_DIR        # (オプション) Pcap ライブラリディレクトリのパス
LIBSSH2_INC_DIR     # (オプション) LibSSH2 インクルードディレクトリのパス
LIBSSH2_LIB_DIR     # (オプション) LibSSH2 ライブラリディレクトリのパス
OPENSSL_INC_DIR     # (オプション) OpenSSL インクルードディレクトリのパス
OPENSSL_LIB_DIR     # (オプション) OpenSSL ライブラリディレクトリのパス
QT_CMAKE_DIR        # (オプション) Qt CMake モジュールディレクトリのパス
QT_DLL_DIR          # (オプション) Qt 動的ライブラリディレクトリのパス (Windows のみ)
RAGEL_EXE           # Ragel 実行可能ファイルのパス
7Z_EXE              # 7-Zip 実行可能ファイルのパス
DOXYGEN_EXE         # (オプション) Doxygen 実行可能ファイルのパス
DOXYREST_CMAKE_DIR  # (オプション) Doxyrest CMake モジュールディレクトリのパス
SPHINX_BUILD_EXE    # (オプション) Sphinx コンパイラの実行可能ファイル sphinx-build のパス
PDFLATEX_EXE        # (オプション) Latex-to-PDF コンパイラのパス
```

一部の依存関係は自動検出される場合があります。Unixシステムでは、インストールされたライブラリとツールは自動的に検出される可能性があります。WindowsではJancyビルドシステムは、ツール実行可能ファイルが`PATH` (`where` コマンドの使用) に追加されている場合、自動的にツール実行可能ファイルを検出します。

ただし、自動検出がすぐに機能する場合でも、`paths.cmake` を使用して特定のツール/ライブラリの場所を微調整できます。個人的には、常にすべてのパスを明示的に指定することを好みます。

LLVMパスを指定する必要があります(リポジトリで使用可能な新しいバージョンではなく、LLVM 3.4.2をビルドしてインストールする場合を除く)。

Windowsでは、必要なライブラリへのパスも指定する必要があります。ライブラリが自動的に検索されることはほとんどありません。

**Linux 用 paths.cmake のサンプル**
```  cmake
set(LLVM_VERSION 3.4.2)

set(LLVM_INC_DIR  /home/vladimir/Develop/llvm/llvm-${LLVM_VERSION}/include)

if("${TARGET_CPU}" STREQUAL "amd64")
        set(LLVM_CMAKE_DIR  /home/vladimir/Develop/llvm/llvm-${LLVM_VERSION}/build/make-amd64/${CONFIGURATION}/share/llvm/cmake)
        set(LLVM_INC_DIR    ${LLVM_INC_DIR} /home/vladimir/Develop/llvm/llvm-${LLVM_VERSION}/build/make-amd64/${CONFIGURATION}/include)
        set(LLVM_LIB_DIR    /home/vladimir/Develop/llvm/llvm-${LLVM_VERSION}/build/make-amd64/${CONFIGURATION}/lib)
else()
        set(LLVM_CMAKE_DIR  /home/vladimir/Develop/llvm/llvm-${LLVM_VERSION}/build/make-x86/${CONFIGURATION}/share/llvm/cmake)
        set(LLVM_INC_DIR    ${LLVM_INC_DIR} /home/vladimir/Develop/llvm/llvm-${LLVM_VERSION}/build/make-x86/${CONFIGURATION}/include)
        set(LLVM_LIB_DIR    /home/vladimir/Develop/llvm/llvm-${LLVM_VERSION}/build/make-x86/${CONFIGURATION}/lib)
endif()
```

**Windows 用 paths.cmake のサンプル**
``` cmake
set(LUA_VERSION   5.2.1)
set(LUA_LIB_NAME  lua52)
set(LLVM_VERSION  3.4.2)
set(RAGEL_VERSION 6.7)

set(7Z_EXE       "c:/Program Files/7-Zip/7z.exe")
set(PERL_EXE     c:/Develop/ActivePerl/bin/perl.exe)
set(RAGEL_EXE    c:/Develop/ragel/ragel-${RAGEL_VERSION}/ragel.exe)
set(LUA_INC_DIR  c:/Develop/lua/lua-${LUA_VERSION}/include)
set(LLVM_INC_DIR c:/Develop/llvm/llvm-${LLVM_VERSION}/include)

if("${TARGET_CPU}" STREQUAL "amd64")
        set(LUA_LIB_DIR    c:/Develop/lua/lua-${LUA_VERSION}/lib/amd64/${CONFIGURATION})
        set(LLVM_INC_DIR   ${LLVM_INC_DIR} c:/Develop/llvm/llvm-${LLVM_VERSION}/build/msvc10-amd64/include)
        set(LLVM_LIB_DIR   c:/Develop/llvm/llvm-${LLVM_VERSION}/build/msvc10-amd64/lib/${CONFIGURATION})
        set(LLVM_CMAKE_DIR c:/Develop/llvm/llvm-${LLVM_VERSION}/build/msvc10-amd64/share/llvm/cmake)
else()
        set(LUA_LIB_DIR    c:/Develop/lua/lua-${LUA_VERSION}/lib/x86/${CONFIGURATION})
        set(LLVM_INC_DIR   ${LLVM_INC_DIR} c:/Develop/llvm/llvm-${LLVM_VERSION}/build/msvc10/include)
        set(LLVM_LIB_DIR   c:/Develop/llvm/llvm-${LLVM_VERSION}/build/msvc10/lib/${CONFIGURATION})
        set(LLVM_CMAKE_DIR c:/Develop/llvm/llvm-${LLVM_VERSION}/build/msvc10/share/llvm/cmake)
endif()
```

---
©Copyright 2012-2026, Tibbo Technology Inc.
