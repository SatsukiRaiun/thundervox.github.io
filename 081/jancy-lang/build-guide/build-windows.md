# Windows でのビルド

Windows で Jancy をビルドするには、新しく生成されたソリューションファイル `jancy_b.sln` をダブルクリックして Visual Studio を起動し、 IDE からビルドします。

コマンドラインからビルドしたいのでしたら、以下を  `./build` フォルダで実行します。

``` powershll
cmake --build .
```

デフォルトでは、 `msbuild.exe` は `Debug` 構成でプロジェクトをビルドします。 `Release` 構成でビルドするには、以下を実行します。

``` powershell
cmake --build . --config Release
```

`--switch` を使用して、追加のコマンドライン引数を `msbuild.exe` に渡すことができます。たとえば、 `Release` 構成のマルチコア・ビルドを使用する場合は、次のコマンドを実行します:

``` powershell
cmake --build . --config Release -- /maxcpucount
```

コマンドラインパラメータの詳細については、 `msbuild` ドキュメンテーションを参照してください:  [https://msdn.microsoft.com/en-us/library/ms164311.aspx](https://msdn.microsoft.com/en-us/library/ms164311.aspx)

Jancy のビルドが完了すると、 `./build/jancy/lib/$(構成)` ディレクトリに Jancy 静的ライブラリファイルが作成されます。コマンドラインコンパイラ、動的拡張ライブラリ、およびサンプルソースの実行可能バイナリは、 `./build/jancy/bin/$(構成)` にあります。

## テスト

では、コンパイルされたバイナリをテストしてみましょう。

Visual Studio IDE を使用して Jancy をビルドした場合は、 IDE 内からテストを実行できます。実行するには、 Visual Studio のソリューションエクスプローラ/ナビゲータで `RUN_TESTS` 擬似プロジェクトを右クリックし、`ビルド`を選択します。

コマンドラインからビルドしたいのでしたら、以下を  `./build` フォルダで実行します。

``` powershell
ctest -C Debug
ctest -C Release
```

Windows でテストを実行する別の方法は、以下の通りです。

``` powershell
cmake --build . --target RUN_TESTS --config Debug
cmake --build . --target RUN_TESTS --config Release
```

もちろん、 `Debug` または `Release` 構成をテストできるのは、この構成がすでにビルドされている場合だけです。

すべてが順調に進んだ場合は、以下のように表示されます。

``` text
100% tests passed, 0 tests failed out of 123
```

おめでとうございます！　これで Jancy のビルドに成功しました。

---
©Copyright 2012-2026, Tibbo Technology Inc.
