## 3 宣言

宣言は、識別子、型、および変数、定数、関数などの言語要素のその他の側面を指定します。
Seed7では、使用する前にすべてを宣言する必要があります。
すべての宣言は特定のキーワードで導入され、共通のパターンに従います。
キーワードの後にタイプが続く(サンプルコード[赤]{.type}が使用される)・下表中[`aType`]{.type}は、任意ののプレースホルダです。[タイプ](#types_file_start){.link}

| ------- | -------------- |
| 宣言 | コメント |
| [`var`](#decls_Variable_declarations) _**`aType`**_: `name` `is` ` ...` | [変数宣言](#decls_Variable_declarations) |
| [`const`](#decls_Constant_declarations) _**`aType`**_: `name` `is` ` ...` | [定数宣言](#decls_Constant_declarations) |
| [`val`](#params_val_parameter) _**`aType`**_: `name` | [値渡しパラメータ](#params_val_parameter)の宣言 |
| [`ref`](#params_ref_parameter) _**`aType`**_: `name` | [参照渡しパラメータ](#params_ref_parameter)の宣言 |
| [`in`](#params_in_parameter) _**`aType`**_: `name` | [in パラメータ](#params_in_parameter)の宣言 |
| [`in var`](#params_in_var_parameter) _**`aType`**_: `name` | [in-var パラメータ](#params_in_var_parameter)の宣言 |
| [`inout`](#params_inout_parameter) _**`aType`**_: `name` | [inout パラメータ](#params_inout_parameter)の宣言 |
| [`in`](#params_in_parameter) [`func`](#types_func) _**`aType`**_: `name` | [名前渡しパラメータ](#params_call_by_name_parameter)の宣言 |
| [`const`](#decls_Type_declarations) [`type`](#types_type): `name` `is` ` ...` | [型宣言](#decls_Type_declarations) |
| [`const`](#decls_Procedure_declarations)  [`proc`](#types_proc): `name` `is` ` ...` | [プロシージャ宣言](#decls_Procedure_declarations) |
| [`const`](#decls_Function_declarations) [`func`](#types_func) _**`aType`**_: `name` `is` ` ...` | [関数宣言](#decls_Function_declarations) |
| [`const`](#decls_Abstract_data_types) [`func`](#types_func) [`type`](#types_type): `name` `is` ` ...` | [抽象データ型](#decls_Abstract_data_types)の宣言 |
| [`const`](#decls_Procedure_declarations)  [`proc`](#types_proc): `name`  `(` [`in`](#params_in_parameter) [`type`](#types_type): _**`aType`**_ `is` ` ...` `)` | [テンプレート](#decls_Templates)の宣言|
| [`syntax`](#decls_Syntax_declarations) [`expr`](#types_expr) `: pattern` `is` ` ...` | [シンタックス宣言](#decls_Syntax_declarations) |

### 3.1 変数の宣言

変数とは、値の場所の名前です。
変数の値は[譲渡](#stats_Assignment){.link}変数は使用する前に宣言する必要があります。
例:

``` pascal
var integer: number is 0;
```

これは[`整数`](#types_integer){.type}変わりやすい`'数'`変数は値0で初期化されます。
その後、変数の値を次のように変更できます。

``` pascal
number := 1;
````

変数は、その型の値のみを保持できます。
間違った型の値を代入しようとすると、コンパイルエラーになります。

``` text
*** tst545.sd7(7):57: Match for {number := "hello" } failed
    number := "hello";
```

### 3.2 定数宣言

定数は、実行中に変更できない名前付きの値です。
定数は、使用する前に宣言する必要があります。 例:

``` pascal
const integer: ONE is 1;
```

これは[`整数`](#types_integer){.type}一定した`「1つ」`の値は1です。
定数を変更しようとすると、コンパイルエラーになります。

``` pascal
***tst544.sd7(7):58:

ONE:=2;
{ONE:=2}で予期される変数が定数integer:ONEを検出しました
```

#### 3.2.1 型宣言

型宣言は、型の名前を定義します。
型宣言は定数宣言として行われ、宣言された定数の型は次のとおりです。[`タイプ`](#types_type){.type}例:

``` pascal
const type: myChar is char;
```

その後、新しい型を他の宣言で使用することができます。 例:

``` pascal
var myChar: aChar is 'x';
```

#### 3.2.2 プロシージャ宣言

プロシージャのコードは実行時に変更されません。
したがって、プロシージャは定数宣言で宣言されます。

``` pascal
const proc: helloWorld is func
  begin
    writeln("hello world");
  end func;
```

プロシージャに `local` キーワードを付け加えることでローカル宣言になります。

``` pascal
const proc: helloWorld is func
  local
    const string: greeting is "hello world";
  begin
    writeln(greeting);
  end func;
```

パラメータ(`greeting`)は次のように定義されています。

``` pascal
const proc: hello (in string: greeting) is func
  begin
    writeln(greeting);
  end func;
```

プロシージャと関数パラメータは、に説明があります。[第6章(パラメータ)](#params_file_start){.link}

#### 3.2.3 関数の宣言

手続きと同様に、関数のコードは実行時に変更されません。
したがって、関数は定数宣言でも宣言されます。

``` pascal
const func boolean: flipCoin is
  return rand(FALSE, TRUE);
```

に注意してください。[`リターン`]{.keywd_no_ul}は文ではありません。
代わりに[`リターン`]{.keywd_no_ul}は[result 変数]{.link}:

``` pascal
const func boolean: flipCoin is func
  result
    var boolean: coinState is FALSE;
  begin
    coinState := rand(FALSE, TRUE);
  end func;
```

[result 変数]{.link}は、関数の結果として使用される特定のローカル変数です。
もしそうでなければ[譲渡](#stats_Assignment){.link}を実行して[初期値](#decls_Initialization){.link}結果値のは関数の結果です。

``` pascal
const func integer: countDigits is func
  result
    var integer: count is 0;
  begin
    while getc(IN) in {'0' .. '9'} do
      incr(count);
    end while;
  end func;
```

`local` キーワードでは、ローカル宣言を導入できます。

``` pascal
const func string: alphabet is func
  result
    var string: abc is "";
  local
    var char: ch is ' ';
  begin
    for ch range 'a' to 'z' do
      abc &:= ch;
    end for;
  end func;
```

パラメータ (`number`) は次のように定義されています。

``` indent
const func float: inverse (in float: number) is
  return 1.0 / number;
```

プロシージャと関数パラメータは、[第6章(パラメータ)](#params_file_start)に説明があります。

#### 3.2.4 前方宣言

Seed7 では、使用する前にすべてを宣言する必要があります。

これにより、コードの追跡が容易になり、コンパイラの実装が容易になります。

完全に宣言する前に使用する必要がある場合は、前方宣言が必要です。

``` pascal
const func boolean: isOdd (in integer: number) is forward;

const func boolean: isEven (in integer: number) is
  return number = 0 ? TRUE : isOdd(pred(number));

const func boolean: isOdd (in integer: number) is
  return number = 0 ? FALSE : isEven(pred(number));
```

#### 3.2.5 インタフェース宣言

Seed7 の[オブジェクト指向](#objects_file_start)は、インタフェース型とインタフェース関数に基づいています。
例:

``` pascal

const type: file is sub object interface;                             # インターフェース型の宣言

const proc: write (inout file: outFile, in string: stri) is DYNAMIC;  # インターフェース関数の宣言
```　

実際の型と関数は、次のインタフェースを実装しています。

``` pascal
const type: striFile is sub null_file struct                              # 実装型
    var string: content is "";
    var integer: position is 1;
  end struct;

type_implements_interface(striFile, file);                                # 実装型からインターフェイス型へ接続

const proc: write (inout striFile: outStriFile, in string: stri) is func  # 実装関数
  ...
```

関数は、インタフェース型と実装型を使用して、新しいインタフェース値を作成します。

``` pascal
const func file: openStriFile (in string: content) is func
  result
    var file: newFile is STD_NULL;
  local
    var striFile: new_striFile is striFile.value;
  begin
    new_striFile.content := content;
    newFile := toInterface(new_striFile);
  end func;
```

機能`toInterface`から所有権を移す`new_stritFile`へ`新しいファイル`

#### 3.2.6 抽象データ型

抽象データ型は付加情報を必要とする型である。
例:抽象データ型[`配列`](#types_array){.type}は、付加情報として配列要素の型を必要とします。
事前定義された抽象データ型は次のとおりである。[`配列`](#types_array){.type}[`副型`]{.type}[`構造体`](#types_struct){.type}[`部分範囲`]{.type}[`ハッシュ`](#types_hash){.type}[`設定する`](#types_set){.type}[`インターフェース`]{.type}および[`列挙型`]{.type}・定義済み抽象データ型の定義[`配列`](#types_array){.type}図書館で[s7i]{.lib}は次のように始まります。

``` pascal
const func type: array (in type: baseType) is ...
```

パラメータ[`baseType`]{.type}のタイプを指定します。[`配列`](#types_array){.type}要素。
抽象データ型[`配列`](#types_array){.type}は、次のように宣言で使用できます。

``` pascal
var array integer: numbers is [] (1);
```

他の抽象データ型は以下のように宣言されます。[`配列`](#types_array){.type}

ユーザ定義の抽象データ型も使用できます。

#### 3.2.7 テンプレート

テンプレートは、特定の型を宣言します。
テンプレートはコンパイル時に実行されます。
機能`FOR_ENUM_DECLS`図書館から[forloop.s7i]{.lib}は、特定の列挙型のすべての値をループするfor-loopを定義します。

``` pascal
const proc: FOR_ENUM_DECLS (in type: aType) is func
  begin

    const proc: for (inout aType: variable) range (attr aType) do
        (in proc: statements) end for is func
      begin
        for variable range aType.first to aType.last do
          statements;
        end for;
      end func;

  end func;
```

テンプレート`FOR_ENUM_DECLS`ライブラリで呼び出されます。[列挙型.s7i]{.lib}変更後:

``` pascal
FOR_ENUM_DECLS(enumType);
```

### 3.3 初期化

で宣言された各オブジェクトは`'`[`const`]{.keywd}`'`または`'`[`var`](#decls_Variable_declarations){.keywd_no_ul}`'`宣言は初期値を取得します。
使用できない`'`[`const`]{.keywd}`'`または`'`[`var`](#decls_Variable_declarations){.keywd_no_ul}`'`初期値のない宣言。
初期値の型は変数や定数の型に適合していなければなりません。
そうでない場合、コンパイルエラーがトリガーされます。

``` bash
*** tst546.sd7(3):57: Match for {number ::= ' ' } failed
var integer: number is ' ';
---------------------------^
*** tst546.sd7(3):42: Declaration of "number" failed
var integer: number is ' ';
```

初期値として式を使用することができます。 例:

``` pascal
var string: fileName is NAME & ".txt";
```

式が評価され、結果が新しいオブジェクトに代入されます。
これは、インタプリタまたはコンパイラによってコンパイル時に行われます。
初期設定式には、任意の関数(または演算子)呼び出しを含めることができます。
そうすることで、ユーザ定義関数を使用して定数や変数を初期化することもできます。

``` pascal
const boolean: maybe is flipCoin;
```

#### 3.3.1初期化の仕組み

変数と定数の初期化はコンパイル時に行われます。
初期化では[作成演算子](#types_creator){.link}([`::=`](#types_creator){.op_no_ul})。
に注意してください。[`::=`](#types_creator){.op_no_ul}はインタプリタとコンパイラによって内部的に使用されます。
の明示的な呼び出し[`::=`](#types_creator){.op_no_ul}ユーザプログラム内では使用できません。
その[作成演算子](#types_creator){.link}は[代入文](#stats_Assignment){.link}([`:=`](#stats_Assignment){.op_no_ul})、しかし2つの重要な違いがあります。

1.  割当て先は正当な値であると仮定できる。[オペレータを作成する](#types_creator){.link}デスティネーションに未定義の値があると仮定する。A[作成演算子](#types_creator){.link}必要なすべての初期化を実行する必要があります。
2.  代入は、値を変数に代入する場合にのみ使用できます。[オペレータを作成する](#types_creator){.link}は変数に限定されません。
    インタプリタとコンパイラの使用[`::=`](#types_creator){.op_no_ul}定数を初期化します。

オブジェクトのライフタイムは次のようになります。

1.  メモリーは、新しいオブジェクト用に予約されます(スタック・メモリーまたはヒープメモリによる違いはありません)。
2.  新しいメモリの内容は未定義である(ゴミが含まれている可能性がある)。[作成演算子](#types_creator){.link}([::=](#types_creator){.op_no_ul})が必要です。[代入文](#stats_Assignment){.link}([:=](#stats_Assignment){.op_no_ul})。
3.  その[作成演算子](#types_creator){.link}([::=](#types_creator){.op_no_ul})は、左の式が未定義であることを考慮して、右の式を左の式にコピーします。
4.  オブジェクトが可変の場合、他の値は[代入文](#stats_Assignment){.link}([:=](#stats_Assignment){.op_no_ul})。
    この代入では、代入先に正当な値が含まれていると仮定できます。
5.  文字列(およびその他の一部のタイプ)は、データが存在するメモリ領域への参照にすぎません。
    これらの参照がメモリ領域の唯一の所有者となります。
    これにより、割当てによってメモリー領域を再割当てできます。
6.  オブジェクトの存続期間が終了すると[破壊操作](#types_destroyer){.link}を実行する。
    文字列(およびメモリ領域への単なる参照であるその他のタイプ)の場合、参照されたメモリは解放されます。
7.  オブジェクトのメモリーは解放されます。

最初の3つのステップは通常、宣言文の中に隠されています。
宣言文が実行されます。

``` pascal
ONE ::= 1
```

割り当てる`1`その物体に`ONE`その[破壊操作](#types_destroyer){.link}は[作成演算子](#types_creator){.link}オブジェクトの存続期間が終了すると自動的に実行されます。
ランタイムライブラリが実行されます。

``` pascal
destroy(fileName)
```

で参照されるメモリを解放します。[`文字列`](#types_string){.type_no_ul}変わりやすい`fileName`次のような単純なタイプの場合[`整数`](#types_integer){.type_no_ul}その[破壊操作](#types_destroyer){.link}は何もしない。

createとdestroyを使用したメカニズムは、値渡しパラメータ([`val`](#params_val_parameter){.keywd_no_ul}[`var内`](#params_in_var_parameter){.keywd_no_ul}および[`で`](#params_in_parameter){.keywd_no_ul}(値渡しを使用している場合)。
関数が呼び出されると、値渡しパラメータは[作成演算子](#types_creator){.link}:

``` pascal
formalParameter ::= actualParameter
```

関数の最後に[破壊操作](#types_destroyer){.link}を実行する。

``` pascal
destroy(formalParameter)
```

すべての定義済みタイプについて[作成演算子](#types_creator){.link}([`::=`](#types_creator){.op_no_ul})および[破壊操作](#types_destroyer){.link}はすでに定義されています。
新しいユーザー定義型のオブジェクトを宣言できるようにするには、この型のcreate操作とdestroy操作を定義する必要があります。

### 3.4 シンタックスの宣言

シンタックス宣言は、演算子、ステートメント、宣言、およびその他の構成要素の構文、優先順位、および結合性を指定するために使用されます。
シンタックス宣言。`'+'`演算子:

``` pascal
$ syntax expr: .(). + .()   is ->  7;
```

ほとんどの構文定義は次のファイルにあります。[`構文.s7i`]{.lib}構文宣言の詳細な説明は[第9章(構造化構文定義)](#syntax_file_start){.link}また、括弧で囲まれたパラメータリストを持つ関数呼び出しのハードコードされた構文もあります。この場合、パラメータはカンマで区切られます。
ハードコードされた構文は[第11章(式)](#expr_file_start){.link}ここでは、より複雑な構文記述を使用します。

### 3.5 system 宣言

system 宣言により、アナライザとインタプリタは、様々なシステム内部の目的のためにどのオブジェクトを使用すべきかを知ることができます。
system 宣言の例を以下に示します。

``` pascal
$ system "integer" is integer;
```

これは、すべての整数リテラルの型が[`整数`](#types_integer){.type}さらに[`整数`](#types_integer){.type}は、プリミティブアクションで生成されるすべての整数の型として使用されます。
システム宣言で定義されるさまざまなオブジェクトがあります。

- リテラルと単純表現のタイプ例:[`文字列`](#types_string){.type}文字列と[`整数`](#types_integer){.type}整数の場合
- プリミティブアクションの結果値として使用するオブジェクト。例:
  [`TRUE`]{.var}[`FALSE`]{.var}および[`空の`](#types_void){.var}
- プリミティブなアクションによって発生する例外。例えば:
  [`NUMERIC_ERROR`](#errors_NUMERIC_ERROR){.exception}および[`メモリエラー`](#errors_MEMORY_ERROR){.exception}
- いくつかの暗黙的アクションに使用するオブジェクトの例:
  `:=``::=`[`破壊する`]{.func}[`書く`]{.func}および[`フラッシュ`]{.func}

次のシステム宣言が存在します。

``` pascal
$ system "expr" is expr;
$ system "f_param" is f_param;
$ system "integer" is integer;
$ system "bigInteger" is bigInteger;
$ system "char" is char;
$ system "string" is string;
$ system "proc" is proc;
$ system "float" is float;

$ system "true" is TRUE;
$ system "false" is FALSE;
$ system "empty" is empty;

$ system "memory_error" is MEMORY_ERROR;
$ system "numeric_error" is NUMERIC_ERROR;
$ system "overflow_error" is OVERFLOW_ERROR;
$ system "range_error" is RANGE_ERROR;
$ system "index_error" is INDEX_ERROR;
$ system "file_error" is FILE_ERROR;
$ system "database_error" is DATABASE_ERROR;
$ system "graphic_error" is GRAPHIC_ERROR;
$ system "illegal_action" is ILLEGAL_ACTION;

$ system "assign" is := ;
$ system "create" is ::= ;
$ system "destroy" is destroy;
$ system "ord" is ord;
$ system "in" is in;
$ system "prot_outfile" is PROT_OUTFILE;
$ system "flush" is flush;
$ system "write" is write;
$ system "writeln" is writeln;
$ system "main" is main;
```

[]{#decls_Pragmas}

### 3.6プラグマ

プラグマは、プログラムの処理方法を指定する。
いいね[システム宣言](#decls_System_declarations){.link}プラグマは、ドル記号(\$)で始まり、その後にプラグマの名前が続きます。
次のプラグマが存在します。

| プラグマ | パラメータ | コメント |
| -------------- | ----------------- | -------------- |
|  `$` `library` | [`string`](#types_string) | \*.s7i ファイル用の追加ディレクトリを指定してください。 |
|  `$` `message` |[`string`](#types_string)  | 解析中にメッセージを書き込みます。 |
|  `$` `info` | `on` / `off` | コンパイル情報のオン/オフを切り替えてください。 |
|  `$` `trace` | [`string`](#types_string) | コンパイル時のトレース・フラグを設定します。|
|  `$` `decls` | \- | 宣言をトレースします。 |
|  `$` `names` | `unicode` / `ascii` | Unicode (または ASCII) 識別子を使用できます。 |

未知のプラグマが[構文解析エラー](#errors_Parsing_errors){.link}:

``` bash
*** pragma.sd7(1):8: Illegal pragma "unknownPragma"
$ unknownPragma
---------------^
```

[プラグマ`メッセージ`メッセージを書くのに用いられる]{#decls_pragma_message}解析中に生成されます。
書くこと[`"hello world"`]{.stri}解析時には以下を使用します。

``` pascal
$ message "hello world";
```

[プラグマ`info`冗長レベルを変更するために使用できる]{#decls_pragma_info}解析フェーズの。
これは[`-vn`]{.link}の[通訳]{.link}with

``` pascal
$ info on;
```

パーサはライブラリ名と現在処理されている行番号に関する情報を書き込みます。
有

``` pascal
$ info off;
```

そのような情報は書き込まれません。

[プラグマ`形跡`インタプリタのトレースを実行するために使用できる]{#decls_pragma_trace}プログラムの解析中はonまたはoff。
これは[`-dx`]{.link}の[通訳]{.link}その[`文字列`](#types_string){.type}パラメータを使用して[`形跡`]{.keywd}プラグマは、文字の連続を許す[`+`]{.keywd}[`-`]{.keywd}[`a`]{.keywd}[`c`]{.keywd}[`d`]{.keywd}[`e`]{.keywd}[`h`]{.keywd}[`m`]{.keywd}[`u`]{.keywd}[`s`]{.keywd}および[`*`]{.keywd}これらの文字は次の意味を持ちます。

- **+** 以下のフラグをオン(デフォルト)にします。
- **-** 以下のフラグをオフにします。
- **a** プリミティブアクションをトレースする
- **c** アクションのチェックはしない
- **d** 動的呼び出しをトレースする
- **e** トレース例外とハンドラ
- **h** トレースヒープサイズ(\'a\'との組み合わせ)
- **m** 式のマッチングをトレースする
- **u** トレース実行ユーティリティ機能
- **s**  トレースシグナル
- **\*** すべてのフラグ

[プラグマ`名前`Unicodeを可能にするために使用できる]{#decls_pragma_names}で[名前の識別子](#tokens_Name_identifiers){.link}:

``` pascal
$ names unicode;
```

これにより、ドイツ語のウムラウト文字やキリル文字などの変数を使用できます。
これにより、初心者は母国語の変数名や関数名を使用することができます。

