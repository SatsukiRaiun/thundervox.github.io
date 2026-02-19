## 2. チュートリアル

### 2.1 Hello world

以下は、 Seed7 の hello world プログラムです。

``` pascal
$ include "seed7_05.s7i";

const proc: main is func
  begin
    writeln("hello world");
  end func;
```

このプログラムを hello.sd7 ファイルに保存し、コンソールで次のように起動します。

``` indent
s7 hello
```

Seed7 インタプリタは以下のように記述します。

``` indent
SEED7 INTERPRETER Version 5.1.790  Copyright (c) 1990-2023 Thomas Mertes
hello world
```

Seed7 インタプリタと hello.sd7 プログラムの出力に関する情報が表示されます。

``` indent
hello world
```

オプション **`-q`** を使用すると、 Seed7 インタプリタの情報行を抑制できます。

``` indent
s7 -q hello
```

プログラムの最初の行には、

``` pascal
$ include "seed7_05.s7i";
```

標準ライブラリのすべての定義が含まれます。他のライブラリとは対照的に、 seed7_05.s7i ライブラリには関数宣言だけでなく、ステートメントと演算子の宣言も含まれています。さらに、 seed7_05.s7i ライブラリは、 `main` 関数を Seed7 プログラムのエントリポイントとして定義しています。

上の例では、`main`は定数として宣言されているため、 `proc` は`main`型となります。 `proc` 型で `main` を宣言すると、プロシージャが作成されます。`main`オブジェクトは、

``` pascal
$ include "seed7_05.s7i";
```
値として構文を取り込みます。

``'func'`` 構文は、 PASCAL の `begin` ... `end` および C の `{ ... }` に類似している。 ``'func'``の内部には、 [`"hello world"`](#tokens_String_literals) 文字列を有する `writeln` ステートメントがあり、 `writeln` ステートメントは、[`文字列`](#types_string)に続いて改行文字を書き込むために使用される。

**FIXME:** 

### 2.2 挨拶

以下のプログラムは、小さなダイアログを起動します。

``` inden
$ include "seed7_05.s7i";

const proc: main is func
  local
    var string: name is "";
  begin
    write("What's your name? ");
    readln(name);
    writeln("Hi " <& name <& "!");
  end func;
```

このプログラムを「greeting.sd7」ファイルに保存し、コンソールで次のように起動します。

``` indent
s7 greeting
```

次のようにお名前をお尋ねします。

``` indent
What's your name?
```

名前を入力してEnterを押すと、挨拶が表示されます。
このプログラムは、変数を使用する`'name'`入力した名前を保存します。
変数を定義する必要があります。
関数のすべての変数を定義および初期化する場所は、キーワードの後です。[地域の]{.keywd}

``` indent
var string: name is "";
```

これは[`文字列`](#types_string){.type}変わりやすい`'name'`この定義では、初期値も割り当てられます。[\"\"](#tokens_String_literals){.stri}へ`'name'`・値[\"\"](#tokens_String_literals){.stri}が空の文字列の場合(何も文字が含まれていない場合)
Seed7では、変数を定義し、常に初期値を取得する必要があります。

その[`書く`]{.func}ステートメントは次のようになります。[`writeln`]{.func}と同じですが、改行文字を書き込みません。
その[`readln`]{.func}文は、標準入力ファイルから行を読み込み、その行を指定された変数に代入します。
この機能では、バックスペースを使用して入力を訂正できます。
入力を押すと、行がプログラムに送信されます。
最後の[`writeln`]{.func}文は演算子を含む[`<&`]{.op}と入力します。
必要に応じて[`<&`]{.op}演算子は値を[`文字列`](#types_string){.type}

上記のグリーティング・プログラムには問題があります。
誰かが名前の入力を拒否して\[Enter\]だけを押した場合、プログラムは次のように書き込みます。

``` indent
Hi !
```

これを回避するために、特殊なケースをチェックするようにプログラムを改善します。

``` indent
$ include "seed7_05.s7i";

const proc: main is func
  local
    var string: name is "";
  begin
    write("What's your name? ");
    readln(name);
    if name = "" then
      writeln("Greetings to the person who pressed enter!");
    elsif name = "name" then
      writeln("Interesting, your name is name.");
    else
      writeln("Hi " <& name <& "!");
    end if;
  end func;
```

ゼロまたはそれ以上になり得ます。[`elsif`]{.keywd}部品、および[`else`]{.keywd}パートはオプションです。
見ての通り、文字列が等しいかどうかは[`=`]{.op_no_ul}の条件[if文](#stats_if-statement){.link}何をするか決めなければいけない
偶然にも上の例の両方の条件が変数を使用する`名前`この特別なケースは [case 文](#stats_case-statement) 代わりに:

``` indent
$ include "seed7_05.s7i";

const proc: main is func
  local
    var string: name is "";
  begin
    write("What's your name? ");
    readln(name);
    case name of
      when {""}:
        writeln("Greetings to the person who pressed enter!");
      when {"name"}:
        writeln("Interesting, your name is name.");
      when {"Linus", "Torvalds"}:
        writeln("Are you the inventor of Linux?");
      otherwise:
        writeln("Hi " <& name <& "!");
    end case;
  end func;
```

ご覧のように、キーワード[`いつ`](#stats_case-statement)この後に、中括弧で囲まれた値のカンマ区切りリストが続きます。
これは、このwhen部分の対象となる値の集合です。
\'name\'の値に応じて、when-partが実行されます。
whenパートが適合しない場合、otherwiseパートが実行されます。

挨拶プログラムを改善する方法はこれだけではありません。
あるいは、ループを使用して名前の入力を要求することもできます。

``` indent
$ include "seed7_05.s7i";

const proc: main is func
  local
    var string: name is "";
  begin
    repeat
      write("What's your name? ");
      readln(name);
    until name <> "";
    writeln("Hi " <& name <& "!");
  end func;
```

その[`繰り返す`]{.keywd}[`まで`]{.keywd}loopは、条件が満たされるまで2つのキーワード間のステートメントを繰り返します。`名前`[`<>`]{.op}[`""`](#tokens_String_literals){.stri}は[`TRUE`]{.var}なお[`繰り返す`](#stats_repeat-statement){.keywd}ループを少なくとも1回実行する。
を使用したソリューション[`一方で`](#stats_while-statement){.keywd}loop:

``` indent
$ include "seed7_05.s7i";

const proc: main is func
  local
    var string: name is "";
  begin
    write("What's your name? ");
    readln(name);
    while name = "" do
      write("Just pressing enter is not okay. What's your name? ");
      readln(name);
    end while;
    writeln("Hi " <& name <& "!");
  end func;
```

### 2.3 代入

次のプログラムは、1から10までの数を書き込みます。

``` indent
$ include "seed7_05.s7i";

const proc: main is func
  local
    var integer: count is 1;
  begin
    while count <= 10 do
      writeln(count);
      count := count + 1;
    end while;
  end func;
```

このプログラムは、 [`integer`](#types_integer) 型のローカル変数を宣言する。

``` indent
    var integer: count is 1;
```

変数 `count` は `1` で初期化される。前述の用例において変数の値は[代入](#stats_Assignment)( `:=` )によって変更できます。

``` indent
    count := count + 1;
```

右側の式は `:=` シンボルが評価され `:=` シンボル。
変数の上の場合 `count` を 1 増やします。
この特殊なケースでは、代入の代わりに使用できるショートカットがあります。

``` indent
    incr(count);
```

このプログラムによる出力は次のようになります。

``` indent
1
2
3
4
5
6
7
8
9
10
```

代わりに 10 から 0 までカウントダウンするには、どのような変更が必要ですか？

[]{#tutorial_Constants}

### 2.4 定数

華氏から摂氏への変換テーブルを記述するには、次のプログラムを使用します。

``` indent
(* 華氏-摂氏の表を印刷する
   華氏の値が0から300*の場合 300 *)

$ include "seed7_05.s7i";

const proc: main is func
  local
    const integer: upper is 300;
    const integer: increment is 20;
    var integer: fahr is 0;
    var integer: celsius is 0;
  begin
    while fahr <= upper do
      celsius := 5 * (fahr - 32) div 9;
      writeln(fahr <& " " <& celsius);
      fahr +:= increment;
    end while;
  end func;
```

間のすべて[(\*および\*)](#tokens_Comments){.comment}は[コメント](#tokens_Comments){.link}は無視されます。
このプログラムには、タイプのローカル定数および変数が含まれています。[`整数`](#types_integer){.type}定数宣言は次のように導入されます。[`const`](#decls_Constant_declarations){.keywd_no_ul}:

``` indent
    const integer: upper is 300;
    const integer: increment is 20;
```

Like variables constants must be initialized with a value that is
specified after the keyword[`は`]{.keywd}Constants
like`上`は変更できません。 すべての定数は初期値のみを維持します。
定数を変更しようとすると[構文解析エラー](#errors_Parsing_errors){.link}:

``` indent
*** tst352.sd7(14):58: Variable expected in {lower := 10 } found constant integer: lower
    lower := 10;
```

エラーはプログラム実行開始前に報告されることに注意してください。

プログラムには[while文](#stats_while-statement){.link}を計算するための式と`「摂氏」` 値.
変数`'fahr'`がインクリメントされる。 `+:=` 演算子。 次の文:

``` indent
      fahr +:= increment;
```

は、次の文のショートカットです。

``` indent
      fahr := fahr + increment;
```

を計算するための式`「摂氏」`値は整数除算(`div`)。
このプログラムによる出力は次のようになります。

``` indent
0 -17
20 -6
40 4
60 15
80 26
100 37
120 48
140 60
160 71
180 82
200 93
220 104
240 115
260 126
280 137
300 148
```

### 2.5 for ループと float 式

華氏から摂氏への変換テーブルを書き込むためのプログラムの改良版は次のとおりです。

``` indent
# 浮動小数点数を使用して華氏-摂氏の表を印刷します。

$ include "seed7_05.s7i";  # これを最初に記述します。
  include "float.s7i";     # 後続の include には $ は不要です。

const proc: main is func
  local
    const integer: lower is 0;
    const integer: upper is 300;
    const integer: increment is 20;
    var integer: fahr is 0;
    var float: celsius is 0.0;
  begin
    for fahr range lower to upper step increment do
      celsius := float(5 * (fahr - 32)) / 9.0;
      writeln(fahr lpad 3 <& " " <& celsius digits 2 lpad 6);
    end for;
  end func;
```

間のすべて[`#`](#tokens_Line_comments){.comment}行の終わりは[行コメント](#tokens_Line_comments){.link}は無視されます。
タイプを使用するには[`浮かぶ`](#types_float){.type}以下を含む必要がある[`float.s7i`]{.lib}その[`浮かぶ`](#types_float){.type}変わりやすい`「摂氏」`は(0ではなく)0.0で初期化する必要があります。
その[`for-loop`](#stats_for-statement){.link}ループ本体を異なる値で実行します。`fahr`(0,
20, 40.280, 300)
の省略[`ステップ`]{.keywd}パートは、1のステップに相当します。

``` indent
for fahr range lower to upper do
  celsius := float(5 * (fahr - 32)) / 9.0;
  writeln(fahr lpad 3 <& " " <& celsius digits 2 lpad 6);
end for;
```

`downto` キーワードは [`for ループ`](#stats_for-statement) を逆方向から実行します。

``` indent
for fahr range upper downto lower do
  celsius := float(5 * (fahr - 32)) / 9.0;
  writeln(fahr lpad 3 <& " " <& celsius digits 2 lpad 6);
end for;
```

Seed7 は強い型なので[`integer`](#types_integer)および[`float`](#types_float) 式の中で値を混在させることはできません。
したがって[`整数`](#types_integer){.type}表現`'5*(fahr-32)'`に変換されます。[`浮かぶ`](#types_float){.type}関数で[`浮かぶ`]{.func}同様の理由により`'`[`/`]{.func}`'`除算と値`'9.0'`を使用しなければならない。
その[`<&`]{.op}演算子は、書き込む前に要素を連結するために使用されます。[`<&`]{.op}オペレータがタイプを持っていません[`文字列`](#types_string){.type}に変換されます。[`文字列`](#types_string){.type}を使用して`'str'`関数を使用します。
その[`パッド`]{.op}演算子は`'fahr'`を文字列に変換し、文字列の長さが3になるまで左に空白を埋めます。
その[`指`]{.op}演算子は`「摂氏」`10進数2桁の文字列に変換されます。
結果のストリングは、6の長さまでパディングされます。

### 2.6 配列

配列では、変数が複数値を持つことができます。
例:配列を使用して、日番号から日名へのマッピングを格納できます。

``` indent
$ include "seed7_05.s7i";

const proc: main is func
  local
    var array string: weekdayName is 7 times "";
    var integer: number is 0;
  begin
    weekdayName[1] := "monday";
    weekdayName[2] := "tuesday";
    weekdayName[3] := "wednesday";
    weekdayName[4] := "thursday";
    weekdayName[5] := "friday";
    weekdayName[6] := "saturday";
    weekdayName[7] := "sunday";
    for number range 1 to 7 do
      writeln(weekdayName[number]);
    end for;
  end func;
```

以来 `weekdayName` は、値が割り当てられた後に変更されないため、配列リテラルで初期化された定数として宣言できます。

``` indent
$ include "seed7_05.s7i";

const proc: main is func
  local
    const array string: weekdayName is [] ("monday", "tuesday", "wednesday", "thursday", "friday", "saturday", "sunday");
    var integer: number is 0;
  begin
    for number range 1 to 7 do
      writeln(weekdayName[number]);
    end for;
  end func;
```

上記の例では配列リテラルを使用しています。

``` indent
[] ("monday", "tuesday", "wednesday", "thursday", "friday", "saturday", "sunday")
```

この配列リテラルは[`配列`](#types_array){.type}[`文字列`](#types_string){.type}1から始まるインデックスが付けられます。
対応する0から始まる配列リテラルは次のようになります。

``` indent
[0] ("monday", "tuesday", "wednesday", "thursday", "friday", "saturday", "sunday")
```

その[for-loop](#stats_for-statement){.link}上記では、リテラル`7`を上限とする。
機能[`長さ`]{.func}を代わりに使用することもできます。

``` indent
for number range 1 to length(weekdayName) do
```

これは、配列が `weekdayName` インデックスは1から始まります。
そうでない場合は、その機能は[`minIdx`]{.func}および[`maxIdx`]{.func}を使用することができる。

``` indent
for number range minIdx(weekdayName) to maxIdx(weekdayName) do
```

この場合には、便利な[for-key-loop](#stats_for-key-statement){.link}:

``` indent
$ include "seed7_05.s7i";

const proc: main is func
  local
    const array string: weekdayName is [] ("monday", "tuesday", "wednesday", "thursday", "friday", "saturday", "sunday");
    var string: name is "";
  begin
    for name range weekdayName do
      writeln(name);
    end for;
  end func;
```

A[for-each-loop](#stats_for-each-statement){.link}の要素を反復する[`配列`](#types_array){.type}場合によっては、要素とそれに対応する索引が必要になります。
これはサポートされています。[キーごとのループ](#stats_for-each-key-statement){.link}:

``` indent
$ include "seed7_05.s7i";

const proc: main is func
  local
    const array string: weekdayName is [] ("monday", "tuesday", "wednesday", "thursday", "friday", "saturday", "sunday");
    var string: name is "";
    var integer: index is 0;
  begin
    for name key index range weekdayName do
      writeln("day " <& index <& ": " <& name);
    end for;
  end func;
```

別の [`for-each-loop`](#stats_for-each-statement) の用例 は、

``` indent
$ include "seed7_05.s7i";

const proc: main is func
  local
    var integer: number is 0;
  begin
    for number range [] (0, 1, 2, 3, 5, 8, 13, 20, 40, 100) do
      write(number <& " ");
    end for;
    writeln;
  end func;
```

上記の例では配列リテラルを使用しています。

``` indent
[] (0, 1, 2, 3, 5, 8, 13, 20, 40, 100)
```

この配列リテラルは[`配列`](#types_array){.type}[`整数`](#types_integer){.type}配列の添字型は整数に限定されない。
A type
like[`char`](#types_char){.type}への1:1マッピングを持つ[`整数`](#types_integer){.type}また、インデックスとしても使用できます。

``` indent
$ include "seed7_05.s7i";

const proc: main is func
  local
    const array [char] string: digitName is
        ['0'] ("zero", "one", "two", "three", "four", "five", "six", "seven", "eight", "nine");
    var integer: number is 0;
    var char: ch is ' ';
  begin
    number := rand(1, 9999999);
    write(number <& ": ");
    for ch range str(number) do
      write(digitName[ch] <& " ");
    end for;
    writeln;
  end func;
```

上記の例では配列リテラルを使用しています。

``` indent
['0'] ("zero", "one", "two", "three", "four", "five", "six", "seven", "eight", "nine")
```

この配列リテラルは[`配列`](#types_array){.type_no_ul}`[`[`char`](#types_char){.type_no_ul}`]`[`文字列`](#types_string){.type_no_ul}この配列の最小のインデックスは文字[`'0'`](#tokens_Character_literals){.stri}

### 2.7ハッシュ

A[`ハッシュ`](#types_hash){.type}は[`配列`](#types_array){.type}ただし、インデックスは任意のタイプにすることができます(単にインデックスを[`整数`](#types_integer){.type})。
型式[`ハッシュ`](#types_hash){.type}[`[`]{.type}[`文字列`](#types_string){.type}[`]`]{.type}[`整数`](#types_integer){.type}でハッシュを定義します。[`文字列`](#types_string){.type}引数indexおよび[`整数`](#types_integer){.type}価値として。
この型は型宣言で使用できます。

``` indent
const type: nameToDigitType is hash [string] integer;
```

ハッシュリテラルは、ハッシュ定数または変数を初期化するために使用できます。

``` indent
const nameToDigitType: nameToDigit is [] (
    ["zero" : 0], ["one" : 1], ["two"   : 2], ["three" : 3], ["four" : 4],
    ["five" : 5], ["six" : 6], ["seven" : 7], ["eight" : 8], ["nine" : 9]);
```

配列と同様に、ハッシュ内の要素には

``` indent
nameToDigit[name]
```

次の例では、数字名を要求し、対応する数字を記述します。

``` indent
$ include "seed7_05.s7i";

const type: nameToDigitType is hash [string] integer;

const proc: main is func
  local
    const nameToDigitType: nameToDigit is [] (
        ["zero" : 0], ["one" : 1], ["two"   : 2], ["three" : 3], ["four" : 4],
        ["five" : 5], ["six" : 6], ["seven" : 7], ["eight" : 8], ["nine" : 9]);
    var string: name is "";
  begin
    write("Enter the name of a digit: ");
    readln(name);
    if name in nameToDigit then
      writeln("The value of " <& name <& " is " <& nameToDigit[name]);
    else
      writeln("You entered " <& name <& ", which is not the name of a digit.");
    end if;
  end func;
```

上の例では

``` indent
name in nameToDigit
```

キー`名前`ハッシュ表にある`nameToDigit`これにより、対応する値を得ることが保証される。

``` indent
nameToDigit[name]
```

成功します。 for-eachループはハッシュテーブルでも使用できます。
以下の例では[`keyDescription`]{.var}これは次のように定義されてい[`ハッシュ`](#types_hash){.type}[`[`]{.type}[`char`](#types_char){.type}[`]`]{.type}[`文字列`](#types_string){.type}で[`"`]{.lib}[`keydescr.s7i`]{.lib}[`"`]{.lib}キーボードのキーに関する説明文が含まれています。
A[`for-loop`]{.link}はaの値をループすることができる。[`ハッシュ`](#types_hash){.type}:

``` indent
$ include "seed7_05.s7i";
  include "keydescr.s7i";

const> proc: main is func
  local
    var string: description is "";
  begin
    for description range keyDescription do
      write(description <& " ");
    end for;
    writeln;
  end func;
```

A[`for-loop`]{.link}のキー(インデックス)と値をループすることもできます。[`ハッシュ`](#types_hash){.type}:

``` indent
$ include "seed7_05.s7i";
  include "keydescr.s7i";

const proc: main is func
  local
    var char: aChar is ' ';
    var string: description is "";
  begin
    for description key aChar range keyDescription do
      writeln("const char: " <& description <& " is " <& literal(aChar));
    end for;
  end func;
```

### 2.8 for ループとコンテナ

`for ループ` は [`set`](#types_set) の各要素に対して繰り返す (イテレート) こともできます。

``` indent
$ include "seed7_05.s7i";

const proc: main is func
  local
    var string: innerPlanet is "";
  begin
    for innerPlanet range {"Mercury", "Venus", "Earth", "Mars"} do
      write(innerPlanet <& " ");
    end for;
    writeln;
  end func;
```

上の例では`{`[`「水銀」`](#tokens_String_literals){.stri}[`「金星」`](#tokens_String_literals){.stri}[`「地球」`](#tokens_String_literals){.stri}[`「火星」`](#tokens_String_literals){.stri}`}`はセット・リテラルです。
このリテラルのタイプは[`設定する`](#types_set){.type}[`の`](#types_set){.type}[`文字列`](#types_string){.type}その他のセットリテラルは次のとおりです。

``` indent
{1, 2}
{'a', 'e', 'i', 'o', 'u'}
```

[`forループ`](#stats_for-each-statement)は [`string`](#types_string) の文字を繰り返すことができる。

``` indent
$ include "seed7_05.s7i";

const proc: main is func
  local
    const set of char: vowels is {'a', 'e', 'i', 'o', 'u'};
    var char: letter is ' ';
  begin
    for letter range "the quick brown fox jumps over the lazy dog" do
      if letter not in vowels then
        write(letter);
      end if;
    end for;
    writeln;
  end func;
```

A[`for-loop`]{.link}のキー(インデックス)と値をループすることができます。[`配列`](#types_array){.type}:

``` indent
$ include "seed7_05.s7i";

const proc: main is func
  local
    var integer: number is 0;
    var string: name is "";
  begin
    for name key number range [0] ("zero", "one", "two", "three", "four",
                                   "five", "six", "seven", "eight", "nine") do
      writeln(number <& ": " <& name);
    end for;
  end func;
```

コンテナ内で繰り返されるループはすべて[`まで`]{.func}条件:

``` indent
$ include "seed7_05.s7i";

const proc: main is func
  local
    var string: testText is "";
    var char: ch is ' ';
    var boolean: controlCharFound is FALSE;
  begin
    write("Enter text: ");
    readln(testText);
    for ch range testText until controlCharFound do
      controlCharFound := ord(ch) < 32;
    end for;
    if controlCharFound then
      writeln("The text contains control chars.");
    end if;
  end func;
```

### 2.9 関数

以下のプログラムは、その`flipCoin` 関数を使用しており表が出るまでコインを投げます。

``` indent
$ include "seed7_05.s7i";

const func boolean: flipCoin is
  return rand(FALSE, TRUE);

const proc: main is func
  local
    var boolean: heads is FALSE;
  begin
    repeat
      write("Press enter to flip the coin. ");
      readln;
      heads := flipCoin;
      writeln(heads ? "heads" : "tails");
    until heads;
  end func;
```

上記の例では[`flipCoin`]{.func}は定数として宣言される。
As[`const`]{.keywd}のコード[`flipCoin`]{.func}実行時に変更されません。
型式[`機能`](#types_func){.type}[`ブール`](#types_boolean){.type}は、この関数が[`ブール`](#types_boolean){.type}value.
職務のタスク[`flipCoin`]{.func}は、乱数を返すことである。[`ブール`](#types_boolean){.type_no_ul}value.
これは次のようにして行います。

``` indent
return rand(FALSE, TRUE);
```

実際の乱数は以下の式で計算される。

``` indent
rand(FALSE, TRUE);
```

ステートメント

``` indent
heads := flipCoin;
```

の結果を割り当てる。[`flipCoin`]{.func}へ`頭`その後`頭`で上書きされます。[三項演算子]{.link}([`:`]{.op_no_ul})。
もし`頭`は[`TRUE`]{.var}と書いている。[`「ヘッド」`](#tokens_String_literals){.stri}そうでなければ[`「尻尾」`](#tokens_String_literals){.stri}

に注意してください。[`リターン`]{.keywd_no_ul}は文ではありません。
代わりに[`リターン`]{.keywd_no_ul}は[結果変数]{.link}:

``` indent
const func boolean: flipCoin is func
  result
    var boolean: coinState is FALSE;
  begin
    coinState := rand(FALSE, TRUE);
  end func;
```

A[結果変数]{.link}は、関数の結果として使用される特定のローカル変数です。

の宣言[`flipCoin`]{.func}上記は、次の構文を含みます。

``` indent
func
...
end func
```

機能[`flipCoin`]{.func}を取得します。[`機能`]{.keywd}[`関数終了`]{.keywd}値として構築します。
このコンストラクトは、関数およびプロシージャの初期化に使用できます。
の内部[`機能`]{.keywd}[`関数終了`]{.keywd}キーワードを構成する[`結果`]{.keywd_no_ul}[`地域の`]{.keywd}および[`begin`]{.keywd}を使用することができる。

- キーワード[`結果`]{.keywd_no_ul}の宣言を紹介する。[結果変数]{.link}関数が終了すると、結果変数の現在の値が返されます。
  もしそうでなければ[譲渡](#stats_Assignment){.link}を実行して[初期値](#decls_Initialization){.link}結果値のが関数の結果です。
- キーワード[`地域の`]{.keywd}は、関数に対してローカルな定数宣言と変数宣言を導入します。
- キーワード[`begin`]{.keywd}は、関数が呼び出されたときに実行される一連のステートメントを導入します。

の宣言からわかるように[`主要な`]{.func}上記の[`結果`]{.keywd_no_ul}および[`地域の`]{.keywd}partsはオプションです。関数タイプが[`proc`](#types_proc){.type}[`結果`]{.keywd_no_ul}を省略しなければならない。
その他すべての機能については[`結果`]{.keywd_no_ul}部分は必須です。

必ず関数の結果を使用します([割り当てられた](#stats_Assignment){.link}、書かれた、パラメータとして使用されるなど)。
関数を宣言文として使用して、関数の結果を無視する事はできません。
使用しようとする試み[`flipCoin`]{.func}asステートメント:

``` indent
const proc: main is func
  begin
    flipCoin;
  end func;
```

結果は[構文解析エラー](#errors_Parsing_errors){.link}:

``` indent
*** tst609.sd7(8):35: Procedure expected found "func boolean" expression
    flipCoin;
```

[]{#tutorial_Parameters}

### 2.10 パラメータ

ほとんどのパラメータは、関数内で変更されません。
Seed7使用`'`[`で`](#params_in_parameter){.keywd_no_ul}`'`パラメータを使用します。

``` indent
const func integer: negate (in integer: num1) is
  return -num1;

const func integer: fib (in integer: num1) is func
  result
    var integer: fib is 1;
  begin
    if num1 <> 1 and num1 <> 2 then
      fib := fib(pred(num1)) + fib(num1 - 2);
    end if;
  end func;
```

上記の機能は`'`[`in`](#params_in_parameter){.keywd_no_ul}`'`というパラメータ`'num1'`の譲渡`'num1'`は使用できません。
正式な`'`[`in`](#params_in_parameter){.keywd_no_ul}`'`パラメータlike`'num1'`定数のように動作します。
フォーマルなものを変えようとすること`'`[`in`](#params_in_parameter){.keywd_no_ul}`'`パラメータ:

``` indent
const proc: wrong (in integer: num2) is func
  begin
    num2 := 0;
  end func;
```

結果は[構文解析エラー](#errors_Parsing_errors)になります。

``` indent
*** tst77.sd7(5):53: Variable expected in {num2 := 0 } found parameter (in integer: num2)
    num2 := 0;
```

関数が実パラメータの値を変更したい場合`'`[`inout`](#params_inout_parameter){.keywd_no_ul}`'`パラメータ:

``` indent
const proc: reset (inout integer: num2) is func
  begin
    num2 := 0;
  end func;
```

この関数を

``` indent
reset(number);
```

変数`'数'`その後の値は0になります。
使命`'リセット'`変数の代わりに定数を使用して

``` indent
reset(8);
```

結果は[構文解析エラー](#errors_Parsing_errors)

``` indent
*** tst77.sd7(12):53: Variable expected in {8 reset } found constant integer: 8
    reset(8);
```

場合によっては`'`[`で`](#params_in_parameter){.keywd_no_ul}`'`パラメータは必要ですが、実際のパラメータに影響を与えずに関数内の仮パラメータを変更する必要があります。
この場合は`'`[`var内`](#params_in_var_parameter){.keywd_no_ul}`'`パラメータ:

``` indent
const func string: oct_str (in var integer: number) is func
  result
    var string: stri is "";
  begin
    if number >= 0 then
      repeat
        stri := str(number mod 8) & stri;
        number := number mdiv 8;
      until number = 0;
    end if;
  end func;
```

ご覧のように、これは`'`[`in`](#params_in_parameter){.keywd_no_ul}`'`パラメータをローカル変数`'`[`var`](#decls_Variable_declarations){.keywd_no_ul}`'`

通常、2種類のパラメータがあります。`「値渡し」`および`「参照渡し」`アクセス権(定数または変数)を考慮すると、次の表が得られます。

  パラメーター                                                 渡し方  アクセス権
  ----------------------------------------------------------- ------------ ------------
  ` `[`val`](#params_val_parameter)` `              値渡し     const
  ` `[`ref`](#params_ref_parameter)` `             参照     const
  ` `[`in`](#params_in_parameter)` `              値・参照渡し    const
  ` `[`in var`](#params_in_var_parameter)` `         値渡し     var
  ` `[`inont`](#params_inout_parameter)` `       参照渡し     var

すでにわかっているパラメータに加えて、この表では次のパラメータについても説明します。`'`[`val`](#params_val_parameter){.keywd_no_ul}`'`および`'`[`参照`](#params_ref_parameter){.keywd_no_ul}`'`\'constant\'と\'constant\'を使用し、値渡しな仮パラメータを持つ参照渡し。
その`'`[`で`](#params_in_parameter){.keywd_no_ul}`'`パラメータが呼び出されました。`'val/ref'`この表で簡単に説明できます。

An`'`[`で`](#params_in_parameter){.keywd_no_ul}`'`パラメータは`'`[`val`](#params_val_parameter){.keywd_no_ul}`'`または`'`[`参照`](#params_ref_parameter){.keywd_no_ul}`'`パラメータのタイプによって異なります。

パラメータ

``` indent
in integer: number
```

は、\'val\'パラメータです。

``` indent
val integer: number
```

一方、パラメータ

``` indent
in string: stri
```

は「ref」パラメータで

``` indent
ref string: stri
```

の意味`'`[`in`](#params_in_parameter){.keywd_no_ul}`'`パラメータは、ほとんどのタイプに事前定義されています。
通常はデータ使用量が少ないタイプ`'`[`val`](#params_val_parameter){.keywd_no_ul}`'`as`'`[`で`](#params_in_parameter){.keywd_no_ul}`'`より大きなデータを持つ型が使用するパラメータ`'`[`参照`](#params_ref_parameter){.keywd_no_ul}`'`ほとんどの場合、注意する必要はありません。`'`[`で`](#params_in_parameter){.keywd_no_ul}`'`パラメータは実際には`'`[`val`](#params_val_parameter){.keywd_no_ul}`'`または`'`[`参照`](#params_ref_parameter){.keywd_no_ul}`'`パラメータを使用します。

まれにa`'`[`参照`](#params_ref_parameter){.keywd_no_ul}`'`パラメータがグローバル変数やその他の`'`[`参照`](#params_ref_parameter){.keywd_no_ul}`'`パラメータ。
このような場合は、明示的な`'`[`val`](#params_val_parameter){.keywd_no_ul}`'`パラメータを使用します。`'`[`で`](#params_in_parameter){.keywd_no_ul}`'`パラメータは意味をなします。

すべての正常な場合`'`[`で`](#params_in_parameter){.keywd_no_ul}`'`パラメータは明示的なものよりも優先されるべきです。`'`[`val`](#params_val_parameter){.keywd_no_ul}`'`および`'`[`参照`](#params_ref_parameter){.keywd_no_ul}`'`パラメータを使用します。

### 2.11 オーバーロード

関数は識別子だけでなく、パラメータの型によっても識別されます。
したがって、いくつかのバージョンの関数を定義することができます。

``` indent
$ include "seed7_05.s7i";
  include "float.s7i";
  include "bigint.s7i";
  include "bigrat.s7i";

const func float:       tenPercent (in float: amount)      is return amount / 10.0;
const func float:       tenPercent (in integer: amount)    is return float(amount) / 10.0;
const func bigRational: tenPercent (in bigInteger: amount) is return amount / 10_;

const proc: main is func
  begin
    writeln(tenPercent(123));
    writeln(tenPercent(123.0));
    writeln(tenPercent(123_));
  end func;
```

上記の例は関数を定義する。`tenPercent`タイプ用[`float`](#types_float){.type}[`整数`](#types_integer){.type}および[`bigInteger`](#types_bigInteger){.type}これらの関数のうち2つは[`浮かぶ`](#types_float){.type}を返し[`bigRational`](#types_bigRational){.type}このように同じ関数名を再利用することを、オーバーロードと呼ぶ。
リテラルは`123``123.0`および`123_`タイプがある[`浮かぶ`](#types_float){.type}[`整数`](#types_integer){.type}および[`bigInteger`](#types_bigInteger){.type}である。
これにより`tenPercent`関数を使用する。
に注意してください。[`writeln`]{.func}オーバーロードされる。
それ以外の場合[`writeln`]{.func}受け入れることができない[`浮かぶ`](#types_float){.type}および[`bigRational`](#types_bigRational){.type}引数。

多重定義では関数の結果を考慮しない。
次の例では、多重定義を試みます。`addOne`パラメータタイプが同じで結果タイプが異なる場合:

``` indent
const func integer: addOne (in integer: num) is return num + 1;
const func float:   addOne (in integer: num) is return float(num) + 1.0;
```

[戻り値の型のオーバーロード]{.link}はサポートされていません。
したがって、上記の試行は[翻訳時エラー](#errors_Parsing_errors){.link}:

``` indent
*** tst344.sd7(5):34: Redeclaration of "addOne (val integer: num)"
const func float:   addOne (in integer: num) is return float(num) + 1.0;
------------------------------------------------------------------------^
*** tst344.sd7(4):35: Previous declaration of "addOne (val integer: num)"
const func integer: addOne (in integer: num) is return num + 1;
```

の欠如[戻り型の多重定義]{.link}以下の理由により、読みやすさが向上します

  ----------------------------------------------------------------
  すべての式(および副式)の型は、コンテキストから独立しています。
  ----------------------------------------------------------------

式が使用されている場所を調べる必要はありません。
この式だけで、式の型を判断するのに十分な情報が得られます。

`+` などの演算子は以下で多重定義することができる。

``` indent
$ include "seed7_05.s7i";
  include "float.s7i";

const func float: (in integer: summand1) + (in float: summand2) is
    return float(summand1) + summand2;

const func float: (in float: summand1) + (in integer: summand2) is
    return summand1 + float(summand2);

const proc: main is func
  begin
    writeln(123 + 123.456);
    writeln(123.456 + 123);
  end func;
```

の定義は[`+`]{.op}上記の演算子は、の混合を可能にする[`整数`](#types_integer){.type_no_ul}および[`浮かぶ`](#types_float){.type_no_ul}引数。
過負荷の[`+`]{.op}上記の演算子は[`mixarith.s7i`]{.lib}ライブラリ。

### 2.12 テンプレート

テンプレートを使用すると、実際の型が後で指定される関数を宣言できます。
関数の宣言は、型をパラメータとするプロシージャ内で行います。
ライブラリ[integer.s7i]{.lib}テンプレートを定義します。`DECLARE_MIN_MAX`as:

``` indent
const proc: DECLARE_MIN_MAX (in type: aType) is func
  begin

    const func aType: min (in aType: value1, in aType: value2) is
        return value1 < value2 ? value1 : value2;

    const func aType: max (in aType: value1, in aType: value2) is
        return value1 > value2 ? value1 : value2;

  end func;
```

テンプレート手順`DECLARE_MIN_MAX`タイプパラメータを使用します。[`aType`]{.type_no_ul}関数を宣言する[`分`]{.func}および[`最大`]{.func}テンプレートは、以下を必要とするすべての実型に対してインスタンス化されます。[`分`]{.func}および[`最大`]{.func}例:



``` indent
DECLARE_MIN_MAX(integer);     # integer.s7i ライブラリでインスタンス化
DECLARE_MIN_MAX(bigInteger);  # bigint.s7i ライブラリでインスタンス化
DECLARE_MIN_MAX(float);       # float.s7i ライブラリでインスタンス化
```

これにより、次のような式が可能になります。`min(2, 5)`または `min(PI, E)`

### 2.13宣言

このサンプルプログラムは、引数を記述します。

``` indent
$ include "seed7_05.s7i";       # 標準 Seed7 ライブラリ

const proc: main is func
  local
    var string: stri is "";
  begin
    for stri range argv(PROGRAM) do
      write(stri <& " ");
    end for;
    writeln;
  end func;
```

その[`for文`](#stats_for-statement){.link}繰り返される[`argv`](#os_argv_PROGRAM){.func}`(プログラム)`・機能[`argv`](#os_argv_PROGRAM){.func}`(プログラム)`は[`配列文字列`](#types_array){.type}(=[`配列`](#types_array){.type}の[`文字列`](#types_string){.type}要素)。
その[`for文`](#stats_for-statement){.link}様々なコレクション型に対してオーバーロードされます。
標準Seed7ライブラリに[`seed7_05.s7i`]{.lib}その[`for文`](#stats_for-statement){.link}の[配列](#types_array){.type}sは次のように宣言される。

``` indent
const proc: for (inout baseType: variable) range (in arrayType: arr_obj) do
              (in proc: statements)
            end for is func
  local
    var integer: number is 0;
  begin
    for number range 1 to length(arr_obj) do
      variable := arr_obj[number];
      statements;
    end for;
  end func;
```

この構文は、次のとおりです。[`for文`](#stats_for-statement){.link}は次のように宣言されます。

``` indent
$ syntax expr: .for.().range.().to.().do.().end.for is              -> 25;
```

さらに、誰もが[`for文`](#stats_for-statement){.link}も同様です。
これらの強力な機能のために、Seed7はイテレーターを必要としません。

### 2.14文を宣言するテンプレート

テンプレートは通常の関数です。[タイプ](#types_type){.type}sをパラメータとする。
次のテンプレート関数は[`for文`](#stats_for-statement){.link}:

``` indent
const proc: FOR_DECLS (in type: aType) is func
  begin

    const proc: for (inout aType: variable) range (in aType: low) to (in aType: high) do
        (in proc: statements) end for is func
      begin
        variable := low;
        if variable <= high then
          statements;
          while variable < high do
            incr(variable);
            statements;
          end while;
        end if;
      end func;

  end func;

FOR_DECLS(char);
FOR_DECLS(boolean);
```

FOR_DECLS関数の本体には、次の宣言が含まれます。[`for文`](#stats_for-statement){.link}タイプの[`aType`]{.type}で\'FOR_DECLS\'を呼び出す[`char`](#types_char){.type}および[`ブール`](#types_boolean){.type}asパラメータは、対応する宣言を作成します。[`for文`](#stats_for-statement){.link}上記の例は、ライブラリの一部を簡略化したものです。[`forloop.s7i`]{.lib}



