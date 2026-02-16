# ドキュメントのビルド

Jancyには、以下の四冊から成るドキュメントパッケージが同梱されています

 * ビルドガイド
 * 言語マニュアル
 * 標準ライブラリリファレンス
 * API リファレンス

ドキュメントをビルドするために必要な[前提条件](prerequisites-tools.md)が揃っていれば、CMakeは必要なシェル・スクリプトをすべて `./build/jancy/doc/<ドキュメントのパッケージ>` の下に作成しているはずです。

`sphinx-build` は常に必要ですし、 `pdflatex` も PDF をビルドするために必要です。

生成される HTML ページは、 `./build/jancy/doc/<ドキュメントのパッケージ>/html` に配置されます。

生成される PDF は、 `./build/jancy/doc/<ドキュメントのパッケージ>/pdf` に配置されます。

## ビルドガイド

これは、現在お読みになられているものです。

ドキュメントのソースは、 `./doc/build-guide` にあります。

ビルド手順:
``` bash
cd ./build/jancy/doc/build-guide
./build-html
./build-pdf
```

## 言語マニュアル

Jancy 言語の機能についてまとめた本です。

ドキュメントのソースは、 `./doc/language` にあります。

ビルド手順:
``` bash
cd ./build/jancy/doc/language
./build-html
./build-pdf
```

## 標準ライブラリリファレンス

Jancy 標準ライブラリのリファレンスです。

ドキュメントのソースは、 `./doc/stdlib` にあります。

stdlib ドキュメントをビルドする前に、まず Jancy をビルドする必要があります。 `jancy` コンパイラは stdlib ソースを分析し、ドキュメントコメントを抽出するために必要です。

また、 `doxyrest` は Doxygen XML データベースを reStructuredText に変換する必要があります (これはさらに `sphinx-build` に渡されます)。

`jancy` と `doxyrest` の準備ができたら、スクリプトを実行できます (`Release`  構成で `jancy` と `doxyrest` をビルドした場合は、  `Debug` を `Release` に置き換えます )。

ビルド手順:
``` bash
cd ./build/jancy/doc/stdlib
./build-xml Debug # jancy を Release でビルドした場合は Release に置き換え
./build-rst Debug # doxyrest を Release でビルドした場合は Release に置き換え
./build-html
./build-pdf
```

## API リファレンス

C/C++ 言語向けの Jancy API に関するリファレンスです。これは静的または動的拡張ライブラリを書きたい人や、 Jancy をスクリプトエンジンとして C/C++ アプリケーションに埋め込む計画をしている人のためのドキュメントです。

ドキュメントのソースは、 `./doc/api` にあります。

なお、API ヘッダの解析とドキュメントコメントの抽出には、 `doxygen` が必要です。

また、 `doxyrest` は Doxygen XML データベースを reStructuredText に変換する必要があります (これはさらに `sphinx-build` に渡されます)。


`doxygen` と `doxyrest` の準備ができたら、スクリプトを実行できます (`Release`  構成で `doxyrest` をビルドした場合は、  `Debug` を `Release` に置き換えます )。

ビルド手順:
``` bash
cd ./build/jancy/doc/api
./build-xml
./build-rst Debug # doxyrest を Release でビルドした場合は Release に置き換え
./build-html
./build-pdf
```

---
©Copyright 2012-2026, Tibbo Technology Inc.
