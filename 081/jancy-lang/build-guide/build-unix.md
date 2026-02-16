# Unix でのビルド

Unix ベースのシステムで Jancy を構築するには、 `./build` フォルダで以下を実行するだけです。

``` bash
make
```

また、以下のように、 `-j <n>` を追加することで複数のCPUコアを利用し、ビルドプロセスを高速化することもできます。

``` bash
make -j 4
```

`make` ベースのビルドにおいて、  `Debug` から `Release` への構成の変更は、 CMake の構成段階〔設定時〕で行う必要があることに注意してください (Xcode と Visual Studio は複数構成のビルドシステムです)。

Jancy のビルドが完了すると、 `./build/jancy/lib/${CMAKE_BUILD_TYPE}` ディレクトリに Jancy 静的ライブラリファイルが作成されます。コマンドラインコンパイラ、動的拡張ライブラリ、およびサンプルソースの実行可能バイナリは、 `./build/jancy/bin/${CMAKE_BUILD_TYPE}` にあります。

## テスト

では、コンパイルされたバイナリをテストしてみましょう。

以下を  `./build` フォルダで実行します。

``` bash
make test
```

すべてが順調に進んだ場合は、以下のように表示されます。

``` text
100% tests passed, 0 tests failed out of 123
```

おめでとうございます！　これで Jancy のビルドに成功しました。

---
©Copyright 2012-2026, Tibbo Technology Inc.
