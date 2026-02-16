# ツール

Jancy ビルドシステムは、 `paths.CMake` ファイルを介した独自のパス設定に依存しているため、これらのツールを `PATH` に追加する必要はありません。ただし、 CMake はビルドプロセス全体を開始するために使用されるため (`PATH` または `CMake-gui` へのショートカットなどを介して)、 `CMake` を簡単に利用できるようにすることをお勧めします。

## 必要なツール

Jancyコンパイラをビルドするには、以下のツールが**必要**です。

 * `CMake 3.3 以降`\
   \
   Jancy は CMake ベースのビルドシステムを使用しています。\
   \
   CMake は、ビルドスクリプトを使用してプラットフォーム固有のメイクファイルを生成する、クロスプラットフォームの C\/C++ 中心のビルドシステムです。クロスプラットフォームの C\/C++ アプリケーションの構築に関しては、 CMake は急速に事実上の標準になりつつあります。\
   \
   CMake は、ほとんどの Linux ディストリビューションの公式リポジトリで入手できます。ソースとコンパイル済みバイナリは、公式ウェブサイト [http://cmake.org/](http://cmake.org/) から直接ダウンロードすることもできます。\
   \
   CMake 3.3 以降のバージョンであれば動作するはずです。 CMake 3.3 より前のバージョンでは、Jancy ビルドシステムが依存している `CMake_PARENT_LIST_FILE` 変数が正しく展開されません。

 * `Ragel`\
   \
   Jancy はフロントエンドのトークン化段階の字句解析器・スキャナ生成器として Ragel を使用しています。\
   \
   Ragel はクロスプラットフォームの有限状態マシンコンパイラです。Ragel は、入力言語の利便性と出力コードの優れたパフォーマンスのために、字句解析器・スキャナ生成に最適であることがわかりました〔基盤です〕。\
   \
   Ragel は、ほとんどの Linux ディストリビューションの公式リポジトリで入手できます。ソースは、公式ウェブサイト [http://www.colm.net/open-source/ragel](http://www.colm.net/open-source/ragel) からダウンロードできます。\
   \
   残念ながら、公式Webサイトではコンパイル済みのバイナリを配布していないため、Ragel を自分でビルドするか、ウェブ上で入手可能な非公式のコンパイル済みのバイナリのいずれかを選択する必要があります。

 * `Perl`\
   \
   Jancy は Perl を使用して、標準型と関数の定義を含む Jancy ソースファイルを C++ コードスニペットに変換し、 Jancy コンパイラの C++ ソースファイルに組み込みます。\
   \
   Perl は、ほとんどの Linux ディストリビューションと Mac OSX でそのまま使用できます。 Windowsでは、[ActivePerl](http://www.activestate.com/ActivePerl) または [StraberryPerl](http://strawberryperl.com) を使用できます。\
   \
   どちらも問題なく動作するはずです。

 * `7-Zip`\
   \
   Jancy は、動的拡張をパッケージ化するために 7-Zip ファイルアーカイバを使用しています。\
   \
   7-Zip は、 p7zip として多くの Linux ディストリビューションの公式リポジトリで入手できます。 Mac OSX の場合は、[Homebrew](http://brew.sh) から p7zip をインストールするか、非公式のコンパイル済みバイナリ (7zx など) をダウンロードできます。 Windows 用のコンパイル済みバイナリは、公式ウェブサイト [http://www.7-zip.org](http://www.7-zip.org) で入手できます。\

## オプションツール

これらのツールはオプションであり、 Jancy ドキュメントをビルドする場合にのみ必要です。

 * `Python`\
   \
   Jancy のドキュメントは Sphinx をバックエンドとして使用しているため、 Python も必要です。\
   \
   Python は、ほとんどの Linux ディストリビューションおよび Mac OS X でそのまま利用可能です。Windows では、[https://www.python.org](https://www.python.org) で利用可能な公式バイナリをインストールするか、または ActivePython: [http://www.activestate.com/ActivePython](http://www.activestate.com/ActivePython) を使用できます。\
   \
   3.5.x と 2.7.x の両方〔どちら、いずれ〕のブランチで問題ないはずです (Sphinx のドキュメントには、必要なバージョンは 2.6 以上と記載されています)。
 
 * `Doxygen`\
   \
   Jancy のドキュメントでは、 API ヘッダの分析、宣言とコメントの抽出、 XML データベースの構築に Doxygen を使用しています。\
   \
   Doxygen は、ほとんどの Linux ディストリビューションの公式リポジトリで利用できる。ソースおよびコンパイル済みのバイナリは、公式ウェブサイト [http://www.stack.nl/~dimitri/doxygen/](http://www.stack.nl/~dimitri/doxygen/) から直接ダウンロードすることもできます。

 * `Doxyrest Jancy jancy`\
   \
   Jancy のドキュメントでは、 `Doxygen-doxyrest-sphinx` または ` jancy-doxyrest-sphinx` パイプラインのミドルエンドとして Doxyrest を使用しています。\
   \
   Doxyrest は Lua 文字列テンプレートに基づく〔ベースの〕ツールで、 Doxygen XML データベースを reStructuredText に変換することを目的〔用途〕としています。\
   \
   Doxyrestは、 [http://tibbo.com/downloads/archive/doxyrest](http://tibbo.com/downloads/archive/doxyrest) で入手可能なソースからビルドする必要があります。

 * `Sphinx`\
    \
    Jancyのドキュメントでは、HTMLまたはPDF出力の生成にSphinxを使用しています。\
    \
    Sphinx は、 Python パッケージ管理インフラストラクチャ `pip` を介して利用できます。多くのLinuxディストリビューションでも、スタンドアロンのバイナリパッケージとして利用できるはずです。\
    \
    Sphinxの公式サイト: http://www.sphinx-doc.org

---
©Copyright 2012-2026, Tibbo Technology Inc.

