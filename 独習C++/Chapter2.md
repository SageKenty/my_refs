# 2.1 構造体・共用体・列挙体
## 2.1.1 構造体（独習C参照）
複数の変数を一つの変数としてまとめて扱うための型を作る機能

言い換えればデータベースの形を作る機能

**構文**
```
struct struct-name
{
    member-type1 member-name1;
    member-type2 member-name2;
}
```
- 構造体でまとめられた変数をその構造体の**メンバ変数**という。
- メンバ変数としてほかの構造体の変数を宣言することもできる
- 構造体へのポインタをメンバーにできる

構造体のメンバーにアクセスする際、**ドット演算子`.`** もしくは**アロー演算子`->`** を用いる。

両者の違いはアクセスするのが**構造体変数**なのか**構造体変数へのポインタ** なのかという違いである。

**構文**
```
structure-variable.member-name

pointer-to-strucure-variable -> member-name
```

### プログラム例
```cpp
#include<iostream>

struct product
{
    int id; //商品ID
    int price; //商品単価
    int stock; //商品在庫
};

int main()
{
    product pen; //ペンに関するデータを持つ変数

    //ドット演算子でペンに関する情報メンバ変数に格納
    pen.id = 0;
    pen.price = 100;
    pen.stock = 200;

    product *ptr = &pen;

    //アロー演算子で構造体のメンバ変数を取得
    std::cout << "商品ID" << ptr -> id << std::endl;
    std::cout << "単価" << ptr -> price << std::endl;
    std::cout << "在庫数" << ptr -> stock << std::endl;

}
```
構造体変数は複数のメンバ変数を持つため、`{}`を使って初期値を以下のようにまとめて渡す。
```cpp
product pen = 
{
    0, //商品ID
    100, //単価
    200, //在庫数
}

```
`{}`の中には構造体の**定義で書いた順番通り** に**メンバ変数の数だけ** 初期値を記述する。

### 構造体と関数
構造体は関数の引数や戻り値に使うことができる。

引数が複数ある場合、
- どんな順番で並んでいたのか
- 引数にどんな意味があるのか

ということを見失うこともある。

構造体を使って引数をまとめることで変数の意味や目的を関数を使う人に伝えやすくなりバグも減る

```cpp
void show_product(product product)
{
    std::cout << "商品ID：" << product.id << std::endl;
    std::cout << "単価：" << product.price << std::endl;
    std::cout << "在庫数：" << product.stock << std::endl;
}
int main()
{
    /*penを定義*/

    show_product(pen);
}
```
## 2.1.3 列挙体(独習C参照)
列挙体がとりえる値を列挙しておき、そのうちいずれかの値を持っている変数を作るための型

インデックス自体に意味を持たせず、とりあえず数を列挙したい場合に使うことが多い。

**構文**
```
enum class enum-name
{
    enumerator1,
    enumerator2 = value
    .....
}
```
- 値を指定することができる
- 何も指定しなければ0から順番に割り当てられる。

また列挙体名と列挙値を**スコープ解決演算子`::`** でつなげるとその値をつなげることができる。

```
enum-name::enumerator
```

プログラム例
```cpp
#include <iostream>

enum class Category
{
    Value1, // 先頭は明示的に指定しない限り暗黙的に0
    Value2, // 値を省略した場合には1つ上の整数の次（これは1）
    Value3 = 100, // 1つ上の次の整数だと困る場合に明示的に指定できる
    Value4, // 再度省略した場合には1つ上の整数の次（これは101）
};

int main()
{
    // 列挙体の変数を宣言してValue3で初期化
    Category cat = Category::Value3;

    // 列挙体の値に対応した整数を得る
    std::cout << static_cast<int>(cat) << std::endl;
}

```

- 列挙体変数は整数値を持っているが整数型へ暗黙変換はできないので`int`型にキャストする必要がある。

列挙体で扱える整数の範囲を扱えるようにする整数の範囲を指定するには
```
enum class enumn-name : underlying-type
```
例えば整数の範囲を1バイトに収めたかったら、charは必ず1バイトであることを保証するため、
```cpp
enum class Category : char
```
とすればいい。この表現はハードウェアを扱うときによく使う。

# 2.2 クラス概要
データと処理をひとまとめにして扱う機能

- クラスと関連付けられた関数を**メンバ関数**という。
- 定義しただけでは利用不可能。クラスを使い変数を作る必要がある
- クラスで変数を作ることを**実体化**や**インスタンス化**という。
- 実体化した変数は**インスタンス**や**オブジェクト**という

**構文**
```cpp
class class_name
{
    //class-body
}

class_name instance_name; //インスタンス化
```
## 2.2.1 アクセス指定子
クラス内部処理用のデータや処理がクラス外部からアクセスできてしまえば意図しない動作を引き起こす可能性がある。

そこで外部からのアクセス可能性を**アクセス指定子**を使い指定できる。

- クラスはデフォルトでは外部からの**アクセス不可能**な`private`となっている
- 一方外部に公開する必要があれば**public**にすればよい

**構文**
```cpp
class class_name
{
    // デフォルトではprivate

public: //公開のアクセス指定子
    //公開するメンバ変数やメンバ関数

private: //非公開のアクセス指定子
    //非公開のメンバ変数やメンバ関数
};
```
- 非公開となっているメンバ変数やメンバ関数にアクセスしようとするとコンパイルエラーになる。

## 2.2.2 メンバー関数
メンバー関数はそれ単体で使うことは不可能であるが、関連付けられたクラスのインスタンスと合わせて使うことができる

**コード例**
```cpp
class class-name
{
    //メンバー関数宣言
    return-type menber-function-name(parameters...);
}

//メンバー関数定義

return-type class-name::menber-function-name(parameters...)
{
    function-body...
}

//インスタンスを使ったメンバー関数呼び出し
instance.member-function-name(arguments...)

//インスタンスへのポインタ経由でメンバ関数呼び出し
pointer -> member-function-name(arguments...);
```

- メンバー関数の定義は関連付けられたクラスを指定する必要がある
- 呼び出しではインスタンスから直接呼び出せる
- 同じクラスのメンバー関数であれば非公開メンバー関数・変数などにも直接アクセスが可能

## getterとsetter
メンバ変数を`private`にし、直接アクセスを許可しないものの**getter**(取得用)や**setter**といったメンバ関数を通してのみ操作できるようにすることがある。

```cpp
class product
{
    int price; //単価

public:
    int get_price(); //単価のgetter
    void set_price(int new_price); //単価のsetter
}

int product::get_price()
{
    return price;
}

void product::set_price()
{
    //Setterを使うと新しい値が不正な値でないかチェックができる
    assert(new_price < 0)
    price = new_price;
}

int main()
{
    product pen; //ペンに関するデータを持つ変数

    //メンバ変数は非公開なのでsetterを使って値を格納
    pen.set_price(100);

    product* ptr = &pen //インスタンスへのポインタ
    
    //アロー演算子を使いgetterから値取得
    std::cout << "単価" << ptr->get_price() << std::endl;
} 

```

結果
```
単価:100
```
# 2.3 参照
## 1.変数の別名
変数に別名をつける機能
構文
```
type-name& reference-name = variable-name;
```
- 初期化が必須
- 参照の型は基本的に同じにしなければならない
- `,`で区切られてそのまま定義した場合ただの変数定義となる
## 2.参照とポインタの違い
- 参照は変数の直接の別名となる
- `*`を使わず直接アクセス可能
- しかし初期化時に指定した変数以外への参照に変更することはできない

## 3.const参照
- const変数を参照する場合はconst参照を利用する必要がある
- 



