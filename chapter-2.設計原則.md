 <h1>chapter-2.設計原則</h1>

- 言語のパラダイムに依存しない、普遍的な設計原則について触れる

<!-- TOC -->

- [1. DRY : Don't Repeat Yourself](#1-dry--dont-repeat-yourself)
    - [1.1. リテラルの重複](#11-%E3%83%AA%E3%83%86%E3%83%A9%E3%83%AB%E3%81%AE%E9%87%8D%E8%A4%87)
        - [1.1.1. NG](#111-ng)
        - [1.1.2. OK](#112-ok)
    - [1.2. 処理の流れの重複](#12-%E5%87%A6%E7%90%86%E3%81%AE%E6%B5%81%E3%82%8C%E3%81%AE%E9%87%8D%E8%A4%87)
        - [1.2.1. NG](#121-ng)
        - [1.2.2. OK](#122-ok)
- [2. KISS : Keep It Short and Simple](#2-kiss--keep-it-short-and-simple)
    - [2.1. 単純なはずの処理を長く難しく実装してしまう](#21-%E5%8D%98%E7%B4%94%E3%81%AA%E3%81%AF%E3%81%9A%E3%81%AE%E5%87%A6%E7%90%86%E3%82%92%E9%95%B7%E3%81%8F%E9%9B%A3%E3%81%97%E3%81%8F%E5%AE%9F%E8%A3%85%E3%81%97%E3%81%A6%E3%81%97%E3%81%BE%E3%81%86)
        - [2.1.1. NG](#211-ng)
        - [2.1.2. OK](#212-ok)
- [3. YAGNI : You Are'nt Going to Need It](#3-yagni--you-arent-going-to-need-it)
    - [3.1. あらゆる変更を予測し、どんな拡張もOKな汎用性を最初から追及してしまう](#31-%E3%81%82%E3%82%89%E3%82%86%E3%82%8B%E5%A4%89%E6%9B%B4%E3%82%92%E4%BA%88%E6%B8%AC%E3%81%97%E3%81%A9%E3%82%93%E3%81%AA%E6%8B%A1%E5%BC%B5%E3%82%82ok%E3%81%AA%E6%B1%8E%E7%94%A8%E6%80%A7%E3%82%92%E6%9C%80%E5%88%9D%E3%81%8B%E3%82%89%E8%BF%BD%E5%8F%8A%E3%81%97%E3%81%A6%E3%81%97%E3%81%BE%E3%81%86)
- [4. PIE : Programming Intently and Expressively](#4-pie--programming-intently-and-expressively)
    - [4.1. 処理の塊に名前を付けない](#41-%E5%87%A6%E7%90%86%E3%81%AE%E5%A1%8A%E3%81%AB%E5%90%8D%E5%89%8D%E3%82%92%E4%BB%98%E3%81%91%E3%81%AA%E3%81%84)
        - [4.1.1. NG](#411-ng)
        - [4.1.2. OK](#412-ok)

<!-- /TOC -->

# 1. DRY : Don't Repeat Yourself
- 意図、概要
	- 繰り返すな、重複するな、コピペするな
	- 対義語にWET : Write Every Time(毎回書く)がある
- 何故？何のために？
	- 重複した箇所に修正が必要になった時の負荷が高いため
		- **修正の手間が重複の数だけ倍増する**
		- **重複の数だけ修正漏れのリスクが増える**
- 稀に意図して冗長性を持たせる場合もある

## 1.1. リテラルの重複
- 同じ意味を持つ値を直接2か所以上に書き込むのはNG
	- 値の修正が必要になった場合、同じ修正を複数個所に対して行う必要が出てしまう
	- 修正漏れがあった場合、バグの原因となってしまう
- 対策 : **定数の定義を1箇所に固め、必要な範囲で共有する**
	- 出現回数が1回のリテラルであっても、出来る限り数値の意図を変数名で説明するべき
	- リテラルと変数を比較する、関数の引数としてリテラルを入力する、等の文脈においては比較する変数や仮引数の名前からリテラルの意図が推測できる場合もある

### 1.1.1. NG
```cpp
void test(int value){
	if(value > 10){
		// do something
	}else if(value < 10){
		// do something
	}
}
```

### 1.1.2. OK
```cpp
void test(int value){
	static constexpr int MAX_VALUE {10};
	if(value > MAX_VALUE){
		// do something
	}else if(value < MAX_VALUE){
		// do something
	}
}
```


## 1.2. 処理の流れの重複
- 一部を除いた一連の手続きが同じ処理をコピペするのはNG
- 対策 : **処理が異なる部分を抽象化する**
	- 処理が同じ部分と異なる部分に分ける
	- 異なる部分は外から指定できるようにする
		- 引数で指定する
			- enum等の値だけでなく、関数オブジェクトで処理を渡すことも可能
		- 型引数(template)で指定する

### 1.2.1. NG
```cpp
enum EventType{
	A,
	B
};

class EventA{
	int data{};
};
class EventB{
	float value{};
};

void sendEventA(){
	EventA* event = (EventA*)createEvent(EventType::A);
	if(event != NULL){
		event->data = 0;
		sendEvent(event);
	}
}

void sendEventB(){
	EventB* event = (EventB*)createEvent(EventType::B);
	if(event != NULL){
		event->value = 0.5f;
		sendEvent(event);
	}
}
```

### 1.2.2. OK
```cpp
#include <functional>

enum EventType{
    A,
    B
};

class EventA{
    int data{};
};
class EventB{
    float value{};
};

template<class Event>
void sendEvent(EventType type, std::function<void(Event&)> prepare){
	auto event = static_cast<Event*>(createEvent(type));
	if(event != nullptr){
		prepare(*event);
		sendEvent(event);
	}
}

void sendEventA(){
	sendEvent<EventA>(EventType::A, [](EventA& event){
		event.data = 0;
	});
}

void sendEventB(){
	sendEvent<EventB>(EventType::B, [](EventB& event){
		event.value = 0.5f;
	});
}
```


# 2. KISS : Keep It Short and Simple
- 意図、概要
	- 簡潔かつ単純にせよ
- 何故？何のために？
	- 指針の無いコードは自然と無秩序に向かって複雑になってしまうため
- 対策 : もっと短く、もっと簡単にできないかを考え続ける
	- DRYに反したコードは無いか？減らせるコピペはまだあるのでは？
	- 標準ライブラリで出来ることを自前で開発していないか？車輪の再開発になってないか？
	- そもそも標準ライブラリで出来ることが何か、を調べたか？知っているか？
	- 「ぼくがかんがえたさいきょうのあるごりずむ」になっていないか？
- 新しい機能、古い機能のバランス
	- 新しい機能を使うべきか否かについては意見が分かれやすい
	- 以下の要素を天秤に掛けて判断する必要がある
		- 古い機能を使う場合
			- 始めて1か月の新人みたいな初心者でもコードが読めるというメリット
				- 但しこれは**古い知識から順を追って勉強している場合**に限る
				- 最新の機能や方法から勉強し始める人には当てはまらない
			- 安全でない旧機能を安全に使いこなさねばならないデメリット
			- 新機能に相当する処理を旧機能の組み合わせで再開発してしまうデメリット
		- 新しい機能を使う場合
			- 新機能により享受できる安全性、保守性のメリット
			- 新機能の使い方を覚えるための工数がかかるデメリット


## 2.1. 単純なはずの処理を長く難しく実装してしまう
- 引数にstart, end両方を渡しているが、yyyymmddを渡してDateを返す関数を1個作り、それを2回呼べば良い
- for >> for >> switch >> switch とネストが深すぎ、かつ繰り返しと分岐の組み合わせで読みにくい
- enumの定義も役割が不明確
	- Y1~D2は桁数offsetに使っているが、名前から役割が分からず、使わない要素があるのもNG
	- NUM_SEは略称が意味不明、恐らくstart/endか
	- NUM_YMDに至ってはstart/endとは関係無く、ここで定義する意味が読み取れない
- int[][]とchar[]について
	- 名前がtmp, tempなので一時変数と思われるが、関数の最も広いスコープで生存するためNG
	- intは0初期化されるだろうが、charは保証されない、宣言と同時に初期化するべき
- bool chk は最早何のチェックなのか分からない
	- コードの参照のされ方を見ないと役割が分からないのはNG
- switchの各caseについて
	- strncpy() >> NULL terminate >> atoi()でint[][]に代入 の流れがコピペされているのでNG
	- 処理が重複しているなら適切な名前と共に関数化するべき
- 数値のvalidationについて
	- 整形しているが、OR条件4個は多いし読みにくい

### 2.1.1. NG
```cpp
struct Date{
	int	year;
	int	month;
	int	day;
};

void analysisDate(char* get_start, char* get_end, Date* startDate, Date* endDate){
	enum {
		Y1 = (0),
		Y2,
		Y3,
		Y4,
		M1,
		M2,
		D1,
		D2
	};

	enum {
		START = (0),
		END,
		NUM_SE,
		NUM_YMD
	};

	enum{
		Y = (0),
		M,
		D
	};
	int tmp[NUM_SE][NUM_YMD];
	int i, j;
	char temp[512];
	bool chk = true;
	//有効期限取得
	for(i = START; i <= END; i++){
		for(j = Y; j <= D; j++){
			temp[0] = '\0';
			switch (i) {
				case START:{
					switch (j) {
						case Y:{
								strncpy(temp, get_start, 4);
								temp[4] = '\0';
								tmp[i][j] = atoi(temp);
								break;
						}
						case M:{
								strncpy(temp, get_start + M1, 2);
								temp[2] = '\0';
								tmp[i][j] = atoi(temp);
								break;
						}
						case D:{
								strncpy(temp, get_start + D1, 2);
								temp[2] = '\0';
								tmp[i][j] = atoi(temp);
								break;
						}
					}
					break;
				}
				case END:{
					switch (j) {
						case Y:{
								strncpy(temp, get_end, 4);
								temp[4] = '\0';
								tmp[i][j] = atoi(temp);
								break;
						}
						case M:{
								strncpy(temp, get_end + M1, 2);
								temp[2] = '\0';
								tmp[i][j] = atoi(temp);
								break;
						}
						case D:{
								strncpy(temp, get_end + D1, 2);
								temp[2] = '\0';
								tmp[i][j] = atoi(temp);
								break;
						}
					}
					break;
				}
			}
		}
	}
	// 許容値かチェック
	for(int se = START; se < NUM_SE; se++) {
		if(tmp[se][Y] < 1900 || 9999 < tmp[se][Y]) {
			chk = false;
			break;
		}
		if(		tmp[se][M] < 1 || 12 < tmp[se][M]
			||	tmp[se][D] < 1 || 31 < tmp[se][D]
		) {
			chk = false;
			break;
		}
	}

	if(chk) {
		startDate->year 	= tmp[START][Y];
		startDate->month 	= tmp[START][M];
		startDate->day 		= tmp[START][D];

		endDate->year 		= tmp[END][Y];
		endDate->month 		= tmp[END][M];
		endDate->day 		= tmp[END][D];
	}
	return;
}
```

### 2.1.2. OK
```cpp
#include <iostream>
#include <string>

template<class T>
void print(const T& t){
    std::cout << t << std::endl;
}

bool isContained(int value, int start, int end){
    return value >= start && value <= end;
}

struct Date{
    int year {1900};
    int month {1};
    int day {1};
    bool isValid(){
        return  isContained(year, 1900, 9999) && 
                isContained(month, 1, 12) && 
                isContained(day, 1, 31);
    }
    void debug(){
        print(year);
        print(month);
        print(day);
    }
};

bool isValidDateStr(const std::string& yyyymmdd){
    constexpr size_t sourceSize {8};
    return yyyymmdd.size() == sourceSize;
}

int convert(const std::string& source, size_t pos, size_t length){
    return std::stoi(source.substr(pos, length));
}

int getYear(const std::string& yyyymmdd){
    return convert(yyyymmdd, 0, 4);
}

int getMonth(const std::string& yyyymmdd){
    return convert(yyyymmdd, 4, 2);
}

int getDay(const std::string& yyyymmdd){
    return convert(yyyymmdd, 6, 2);
}

Date createDate(const std::string& yyyymmdd){
    if(!isValidDateStr(yyyymmdd)) return {};
    auto year = getYear(yyyymmdd);
    auto month = getMonth(yyyymmdd);
    auto day = getDay(yyyymmdd);
    Date date { year, month, day };
    if(!date.isValid()) return {};
    return date;
}

int main(void){
    auto start = createDate("20190101");
    auto end = createDate("20191231");
    start.debug();
    end.debug();
}
```


# 3. YAGNI : You Are'nt Going to Need It
- 意図、概要
	- きっと必要にならないから、今作るのはやめておけ
- 何故？何のために？
	- コードの予測は外れやすいため
- 対策 : 最初はシンプルに
	- 最初は汎用性よりも単純性を重視する
	- 汎用性が必要な変更を要求された時に、汎用性を持たせる


## 3.1. あらゆる変更を予測し、どんな拡張もOKな汎用性を最初から追及してしまう
- [引用元](https://nekogata.hatenablog.com/entry/2018/09/10/163206)
- [GitHub - EnterpriseQualityCoding/FizzBuzzEnterpriseEdition: FizzBuzz Enterprise Edition is a no-nonsense implementation of FizzBuzz made by serious businessmen for serious business purposes.](https://github.com/EnterpriseQualityCoding/FizzBuzzEnterpriseEdition)
 > ぼくの大好きなリポジトリに「FizzBuzzEnterpirseEdition」っていうのがあるんですけど、これすごくて、FizzBizzのあらゆるところに拡張ポイントを入れたものなんですね。ループすら抽象化されている。これは考えうる限り最悪のFizzBuzzで、そもそもそういうジョークなんですけど、これってまさに「問題を見誤った例」だと思うんです。FizzBuzzっていう本来シンプルな問題に対して、「ここも変わるかも」「ここも変わるかも」を適用しまくった例なんですよね。でも、逆にいうと、本当にその「ここも変わる」が高い確率で起こるのであれば、そこに対して準備しておく、つまり、それを元に設計原則と照らし合わせていったら、このリポジトリは大変に良いコードになるんです。このリポジトリは、そんな「問題を吟味することの大切さ」と「どこが変わりやすいポイントなのかによって設計原則を適用した結果は変わる」を体現するリポジトリで、本当にいいリポジトリっていうか、めちゃめちゃ面白いのでぜひ見てみてください。


# 4. PIE : Programming Intently and Expressively
- 意図、概要
	- 意図を表現してプログラミングせよ
- 何故？何のために？
	- コードを読むのはコンパイラではなく人間であるため
		- 読む人に意図の伝わらないコードは役割を果たせていない
	- コードだけが唯一信用できる手掛かりであるため
		- コードが仕様書や設計書と一致しているとは限らない
	- コードを読む時間の方が、書く時間よりも長いため
		- 書く効率よりも、読む効率を重視してコーディングするべき
- 対策 : 初見の人にも理解できるかを考えながら書く
	- 自分以外の人間が読んでも理解できるように書かないと意味が無い
	- コードだけで処理の目的が読み取れるのが理想的
		- クラス、関数、変数の名前から役割が読み取れるか？
	- コードで表現できないことは、コメントで補足する
		- ある処理が必要になった理由、事情、経緯、背景が網羅できているか？

## 4.1. 処理の塊に名前を付けない
- コメントには`// XXXする`と書くがXXXに相当するコードはべた書きで関数化されていない
	- 関数化は共通化の手段ではあるが、呼び出し元が1箇所しかなくても関数化するべき時はある
	- 関数化は、**ひとかたまりの処理に名前を付け、その詳細を呼び出し元から隠蔽する**行為に相当する
	- 利用者は関数が実行する内部の詳細を知っていてはならないし、知るべきではない
		- "関数func()を呼び出すと中でstatic変数のXXXが○○されるから..."のような、実装の詳細を期待したり、利用の前提条件にするような関数は作るべきではない
	- 利用者が知っているべきは、**関数に何を入力したら何が出力されるのか**、ただ1点に尽きる
	- 仮に関数の中身がくそコードであったとしても、適切な名前と入出力を持つ関数で包まれていれば良い

### 4.1.1. NG
```cpp
// 超長い関数
void veryLongFunction(void){
	// 処理Aを実行する
	// ...複数行に渡るコード
	
	// 処理Bを実行する
	int i, j;
	for(i = j = 0; path[i] != 0; i++){
		if(path[i] == '\\' || path[i] == '/' || path[i] == ':'){
			j = i + 1;
		}
	}
	path[j] = 0;
	std::cout << path;
	
	// 処理Cを実行する
	// ...複数行に渡るコード
	
	// 以下、処理D~Zまで続く
	// 処理Zを実行する
	// ...複数行に渡るコード
}
```

### 4.1.2. OK
```cpp
#include <iostream>

void printDirPath(const std::string& path){
	std::cout << path.substr(0, path.find_last_of("\\/:"));
}

// 超長い関数
void veryLongFunction(void){
	// 処理Aを実行する
	// ...複数行に渡るコード
	
	// 処理Bを実行する
	printDirPath("C:\\ProgramData\\hogehoge\\data.txt");
	
	// 処理Cを実行する
	// ...複数行に渡るコード
	
	// 以下、処理D~Zまで続く
	// 処理Zを実行する
	// ...複数行に渡るコード
}
```