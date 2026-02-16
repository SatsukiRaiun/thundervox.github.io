# ライブラリ

これらのライブラリはJancyコンパイラのビルドに**必要**です。

 * LLVM 3.4.2

 Jancy は LLVM をバックエンドとして使用しています。 LLVM は、すぐにコンパイラのバックエンド実装のための事実上の標準フレームワークとなったコンパイラライブラリのコレクションです。

Jancy および [IO Ninja](https://ioninja.com/) は依然として Visual Studio 2010 上に公式にビルドされており、 LLVM のより新しいバージョンは残念ながら Visual Studio 2010 上にビルドすることが**できない**ので (C++11 サポートがないため)、 Visual Studio 2010 を依然としてサポートするために最新のLLVMバージョンである LLVM 3.4.2 をダウンロードして構築する必要があります。

LLVM 3.4.2のソースは [http://llv.org/releases/download.html#3.4.2](http://llv.org/releases/download.html#3.4.2) から入手できます。

LLVMソースをダウンロードして解凍したら、[http://llv.org/docs/CMake.html](http://llv.org/docs/CMake.html) にある LLVM CMake ビルドガイドに従ってください。

⚠️注記: Linux システム上で静的 LLVM ライブラリを構築する場合、 C/C++ フラグ[¹]()に `-fvisibility=hidden` を追加することを強く推奨します。

⚠️注記: Linux システムでは、 `libncurses` への不要な依存を避けるために、 `LLVM_ENABLE_TERMINFO` を `OFF` に設定することも推奨します。

⚠️注記: LLVM をインストールする必要はありません。静的ライブラリを構築するだけで十分です。

 * Lua

文法コンパイラ Graco は、 LL(k) 文法から C++ コードを生成するために Lua 文字列テンプレートを使用します。したがって、 Graco を構築するには Lua ヘッダとライブラリが必要です。
  
Lua の実行ファイル、ヘッダファイル、および静的ライブラリと動的ライブラリは、ほとんどの Linux ディストリビューションの公式リポジトリで入手できます。

Luaのソース、およびコンパイル済みバイナリ (実行可能ファイルとライブラリの両方) へのリンクは、 Lua の公式ウェブサイト: [http://www.lua.org](http://www.lua.org) にあります。

5.2.x と 5.3.x の両方のブランチで問題ありません。

これらのライブラリは**オプション**であり、完全なJancyソースパッケージを構築するためにのみ必要です。

 * OpenSSL & LibSSH2

jnc_io_ssh.jncx 動的拡張ライブラリをビルドするには、 LibSSH2 および OpenSSL ライブラリが必要です。このライブラリは、クライアント側の SSH 接続を管理するための io.SshChannel クラスを提供します。

この機能が必要ない場合は、 OpenSSL も LibSSH2 も必要ありません。

OpenSSL と LibSSH2 はどちらも、ほとんどの Linux ディストリビューションの公式リポジトリで入手できます。あるいは、公式ウェブサイトで入手可能なソースからビルドすることもできます。

OpenSSL 公式ウェブサイト: http://www.OpenSSL.org

LibSSH2 公式ウェブサイト: http://www.LibSSH2.org

ウェブ上には、OpenSSLとLibSSH2の両方のプリコンパイル済みライブラリを提供する非公式のプロジェクトも多数あります。

 * Pcap/WinPcap

Pcap (Windows では WinPcap )ライブラリは、 `jnc_io_pcap.jncx` 動的拡張ライブラリを構築するために必要です。このライブラリは、ネットワークトラフィックのフィルタリング、キャプチャ、およびインジェクションを含む低レベルのネットワークパケット管理用の `io.Pcap` クラスを提供します。

この機能が必要ない場合、Pcapライブラリは必要ありません。

Pcap ライブラリは、ネットワークトラフィックのフィルタリング、キャプチャ、および注入を含む低レベルのネットワークパケット管理機能を提供します。

Pcap は Mac OS X SDK の一部であり、通常は多くの Linux ディストリビューションでもすぐに使用できます。 Windows システムソースでは、コンパイル済みのライブラリとドライバを WinPcap の公式ウェブサイト[http://www.WinPcap.org](http://www.winpcap.org) からダウンロードできます。

 * Qt 5.x
 
Qt は包括的なクロスプラットフォームC++フレームワークです。 Jancy は、 Qt を使用して、 GUI ベースのテストとサンプルでユーザーインターフェイスを提供しています。

 * `test_qt` は、ユーザコードをコンパイルして実行することができるシンプルな Jancy エディタです。
 * `jnc_sample_03_dialog` は、 Jancy リアクティブプログラミング概念のQt ウィジェットへの適用を実証する GUI サンプルである。

しかしながら、 Qt のより新しいバージョンは、 Visual Studio 2010 と互換性がない場合がある。ここで、 Tibbo では、 Qt 5.4.2 を使用して、 IO Ninja の公式パッケージをビルドしています。

Qt 公式ダウンロードアーカイブは、[http://download.qt.io/archive/qt](http://download.qt.io/archive/qt) から入手できます。

**脚注:**

［[¹]()］これをしないと、リンカは LLVM シンボルのサブセットを、結果の `ELF` 実行可能ファイルの `.dynsym` テーブルに追加する可能性がある。これにより、システムにインストールされた LLVM との競合によるクラッシュが発生する可能性がある。たとえば、 mesa OpenGL ライブラリを使用するシステム上の Qt アプリケーションで発生する可能性がある (Qt はウィンドウを作成し、レンダリングに mesa を使用する。 mesa はシェーダのコンパイルに共有 LLVM,を使用する)。

---
©Copyright 2012-2026, Tibbo Technology Inc.
