---
{"dg-publish":true,"permalink":"/c/","tags":["gardenEntry"],"dg-note-properties":{}}
---

### W1
#### 课堂1
//1. **强制显示正号**​：输出整数 `n`，要求如果是正数则必须显示 `+` 号（使用 `showpos`）；noshowpos是终止showpos用的；showpos只对≥0的数有用
`if(n>0)cout<<showpos<<n<<noshowpos<<endl;`
`else cout<<n<<endl;`
//2.​​**十六进制与八进制**​：在一行内输出整数 `n` 的十六进制形式和八进制形式，中间用空格分隔。
`cout<<hex<<n<<' '<<oct<<n<<endl;`
//3。**科学计数法**​：输出浮点数 `f`，要求使用科学计数法表示（使用 `scientific`）
`cout<<scientific<<f<<endl;`
#### 课后1
编写一个程序，输出下表。要求使用 `iostream` 和 `iomanip` 提供的操纵符进行格式化：
```
Apple         5.20
Banana        3.14
```
1. 第一列显示名称，左对齐，宽度为 10 。
2. 第二列显示价格，右对齐，宽度为 8，且保留两位小数。
输入：两个浮点数（float）
`#include<iomaiop>`
`cout<<left<<setw(10)<<"Apple"<<setw(8)<<right<<fixed<<setprecision(2)<<a<<endl;` `cout<<left<<setw(10)<<"Banana"<<right<<setw(8)<<fixed<<setprecision(2)<<b<<endl;`


### W2
#### 课堂2
给定一个字符串，请使用auto关键字结合for循环遍历该字符串，并将每个字符逐行输出。
	for(auto ch:s){cout<<ch<<' '<<endl;}
##### auto
编译器会根据初始化表达式自动推导变量的类型
###### 1. 处理复杂的迭代器类型
`std::vector<std::pair<int, std::string>>::iterator it = data.begin()`->`auto it = data.begin()`
###### 2. 处理 lambda 表达式(匿名函数对象)
每个 lambda 表达式都有自己独特的、不可写的类型。`auto` 是保存 lambda 的唯一方式：
`auto comparator = [](int a, int b) { return a > b; };`
`std::vector<int> nums = {5, 2, 8, 1, 9};`
`std::sort(nums.begin(), nums.end(), comparator);`
你无法写出 `comparator` 的具体类型，因为它是由编译器生成的唯一类型名称。
###### 3.其他
类型名很长且不影响可读性的情况（如智能指针：`auto ptr = std::make_unique<Widget>()`）
模板代码中类型依赖于模板参数时

#### 课后1
给定一个字符串`email`，请编写程序：
1. 找到字符 `@` 的位置，无需输出，若字符串中没有`@`，则输出`wrong`
2. 提取出 `@` 之前的所有字符并输出。
`#include <iostream>`
`#include <string>`
`using namespace std;`
`int main() {`
    `string email;`
    `cin >> email;`
    `size_t pos = email.find('@');`
        `if (pos == string::npos) {`
        `cout << "wrong" << endl;`
    `} else {`
         `string username = email.substr(0, pos);`
         `cout << username << endl;`
    `}`
    `return 0;`
`}`
#### 课后2
给定一个带`C`字符串（带空格），编写程序实现以下字符串操作：
1. 在字符串末尾追加"language"。
2. 将其中的 "C" 替换为 "C++"。
3. 在开头插入 "Programming: "
`#include<iostream>
`#include<string>`
`#include<cctype>`
`using namespace std;
`int main()`
`{`
    `string input;`
    `getline(cin, input);  // 读取带空格的整行`
    `// 1. 在末尾追加 "language"`
    `input += " language";`
    `// 2. 将所有的 "C" 替换为 "C++"`
    `size_t pos = 0;`
    `while ((pos = input.find("C", pos)) != string::npos) {`
        `input.replace(pos, 1, "C++");`
        `pos += 3;  // 跳过新插入的 "C++"否则陷入无限循环`
    `}`
    `// 3. 在开头插入 "Programming: "`
    `input.insert(0, "Programming: ");`
    `cout << "处理后的字符串: " << input << endl;`
    `return 0;`
`}`
#### 课后4
判断字母、转化为大写
`if (isalpha(c)) { s1 += toupper(c)}`
//转化为小写：tolower(c);
//判断大小写：isupper(c)、islower(c)

### W3
#### 课后3
创建一个 `Time` 类，私有成员：`hour`, `minute`, `second`，完成以下函数：
1. `bool setTime(int h, int m, int s)`，设置时间并检查时间的合法性，若合法，设置时间，返回`true`；反之，不设置时间，返回`false`。
2. `void addSeconds(int s)`：在该时间上增加若干秒。如果增加后秒数超过60，需进位到分钟；分钟超过60需进位到小时；小时超过24需从0重新开始计算。
3. `void display()`：按照形如`22:9:20`的格式输出时间
提示：你需要完成Time.h，以
```
#ifndef TIME_H
#define TIME_H
```
为开头，`#endif`为结尾
```
#ifndef TIME_H
#define TIME_H
#include <iostream>
using namespace std;
class Time {
private:
    int hour;
    int minute;
    int second;
public:
    bool setTime(int h, int m, int s) {
        if (h < 0 || h >= 24 || m < 0 || m >= 60 || s < 0 || s >= 60) {//先判断false的情况，因为这个条件最容易说全
            return false;
        }
        //对应题目的设置时间
        hour = h;
        minute = m;
        second = s;
        return true;
    }
    void addSeconds(int s) {
        if (s < 0) return;
        int totalSeconds = hour * 3600 + minute * 60 + second + s;
        //判断秒数有没有超过一天
        totalSeconds %= (24 * 3600);
        hour = totalSeconds / 3600;
        //秒数减去已经判给小时的部分
        totalSeconds %= 3600;
        minute = totalSeconds / 60;
        second = totalSeconds % 60;
    }
    void display() {
        cout << hour << ":" << minute << ":" << second;
    }
};
#endif
```

#### 课后4
实现一个功能完善的 `DATE` 类，`year`, `month`, `day` 为私有成员：实现以下函数：
1. `bool setDate(int y, int m, int d)`，如果输入日期非法（如2月30日或13月），则拒绝修改并返回`false`，反之返回`true`。
2. 实现 `void increment()` 函数，使日期增加一天。必须正确处理跨月以及跨年的情况。
3. 实现`void print()`函数，按照`2024-3-23`的格式输出日期。
提示：你需要完成Date.h，以`#ifndef DATE_H #define DATE_H`为开头，`#endif`为结尾

`data.h`
`#ifndef DATE_H`
`#define DATE_H`
`#include <iostream>`
`class Date {`
`private:`
    `int year;`
    `int month;`
    `int day;`
    `bool isLeapYear(int y) const {`
        `return (y % 4 == 0 && y % 100 != 0) || (y % 400 == 0);`
    `}`
    `int getDaysInMonth(int y, int m) const {`
        `int days[] = { 0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31 };`
        `if (m == 2 && isLeapYear(y)) {`
            `return 29;`
        `}`
        `return days[m];`
    `}`
`public:`
    `Date() : year(1), month(1), day(1) {}`
    `bool setDate(int y, int m, int d) {`
        `if (y < 1) return false;`
        `if (m < 1 || m > 12) return false;`
        `if (d < 1 || d > getDaysInMonth(y, m)) return false;`
        `year = y;`
        `month = m;`
        `day = d;`
        `return true;`
    `}`
    `void increment() {`
        `day++;`
        `if (day > getDaysInMonth(year, month)) {`
            `day = 1;`
            `month++;`
            `if (month > 12) {`
                `month = 1;`
                `year++;`
            `}`
        `}`
    `}`
    `void print() const {`
        `std::cout << year << "-" << month << "-" << day << std::endl;`
    `}`
`};`
`#endif`

### W4
#### 课堂2
编写一个完整的 C++ 程序，开发一个简单的文本处理软件。
程序首先输入一个长度不超过 `10000` 的字符串，作为初始文档。之后可以对该文档进行以下操作：
1. **在文档尾部插入字符串**
2. **截取文档中的一部分**
3. **查找子串在文档中最先出现的位置**

为完成上述功能，请定义一个类来表示文档。
**类的要求**
定义一个类 `File`，包含以下成员：
**私有成员**
- `char text[10000]`：用于存储文档内容
- `int length`：当前文档长度
**公有成员**
1. **构造函数**  
    以输入的字符串作为参数，初始化文档内容和长度。
2. **`insertRear(char *str)`**  
    在文档末尾插入字符串 `str`，并输出插入后的整个文档。
3. **`intercept(int a, int n)`**  
    只保留文档中从第 `a` 个字符开始的 `n` 个字符，并输出截取后的整个文档。
4. **`find(char *str)`**  
    查找字符串 `str` 在当前文档中最先出现的位置，并输出该位置。  
    如果找不到，则输出 `-1`。  
    位置编号从 `1` 开始。
**输入格式**
第一行输入一个字符串，表示初始文档。  
第二行输入一个整数 `m`，表示接下来有 `m` 次操作。
接下来输入 `m` 行，每行表示一次操作，格式如下：
- `1 str`：在文档尾部插入字符串 `str`
- `2 a n`：截取文档中从第 `a` 个字符开始的 `n` 个字符
- `3 str`：查找字符串 `str` 在文档中最先出现的位置
**输出格式**
对于每次操作：
- 若操作为 `1`，输出插入后的文档
- 若操作为 `2`，输出截取后的文档
- 若操作为 `3`，输出查找结果
**编程要求**
请编写完整程序，并完成以下内容：
- 定义并实现类 `File`
- 使用字符数组存储文档内容
- 在构造函数中完成文档初始化
- 按要求实现插入、截取、查找三个成员函数
- 在 `main` 函数中完成输入、操作处理和输出
**数据说明**
- 初始文档长度不超过 `10000`
- 所有输入字符串均不含空格和换行
- 保证截取操作合法
- 保证插入后文档总长度不超过 `10000`
##### Note1构造函数：
- 函数名 **必须和类名完全相同**（这样的只有构造函数和析构函数）
- **没有返回类型**（连 `void` 都不能写）
- 用于创建对象时初始化
##### Note2c++`<string>` VS  c `<cstring>`
`#include<string>`
用的是***面向对象接口***:

|功能|写法|
|---|---|
|长度|`s.length()` / `s.size()`|
|拼接|`s += "world"`|
|插入|`s.insert(pos, str)`|
|截取|`s.substr(pos, len)`|
|查找|`s.find(str)`|
`#include<cstring>`
用的是**函数库操作**：

|功能|写法|
|---|---|
|长度|`strlen(s)`|
|拼接|`strcat(s, "world")`|
|复制|`strcpy(s, t)`|
|比较|`strcmp(a, b)`|
|查找|`strstr(s, sub)`|
`.insert()` vs `strcat()` 的本质区别
	`string.insert()`可以插入到**任意位置**
	`strcat()`只能在**末尾拼接**
###### 什么时候用哪个
用 `string`（强烈推荐场景）
	 99% 情况应该用 `string`
必须用 `char[]` 的情况->才使用头文件`<cstring>`
1. **题目明确要求**（你这道题就是）
2. 做底层开发（比如操作系统、嵌入式）
3. 和 C 接口交互
```
#include<iostream>

#include<cstring>

using namespace std;

class file{

    private:

    char text[10001];

    int length;

    public:

    file(char *str)

    {

        strcpy(text,str);

        length=strlen(text);

    }

    void insertRear(char *str)

    {

        strcat(text,str);

         length=strlen(text);

         cout<<text<<endl;

    }

    void intercept(int a, int n)

    {

        char temp[10001];

        strncpy(temp,text+(a-1),n);

        temp[n]='\0';

  

        strcpy(text,temp);

        length=n;

        cout<<text<<endl;

    }

    void find(char *str)

    {

        char *ptr=strstr(text,str);

        if(ptr==NULL)

        {

            cout<<-1<<endl;

        }

        else

        {

            int pos=(ptr-text)+1;

            cout<<pos<<endl;

        }

    }

};

int main()

{

    char initStr[10001];

    if(!(cin>>initStr))return 0;

  

    file myfile(initStr);

    int m;cin>>m;

    for(int i=0;i<m;i++)

    {

        int type;

        cin>>type;

        if(type==1)

        {

            char s[10001];

            cin>>s;

            myfile.insertRear(s);

        }

        else if(type==2){

            int a,n;

            cin>>a>>n;

            myfile.intercept(a,n);

        }

        else if(type==3){

            char s[10001];

            cin>>s;

            myfile.find(s);

        }

    }

    return 0;

}
```

#### 课后2
编写一个完整的 C++ 程序，定义一个 Person 类，用于表示一个人。

Person 类包含以下两个私有数据成员：

name：姓名，类型为 string
age：年龄，类型为 int
请根据要求实现 Person 类的以下成员：

默认构造函数
将姓名初始化为 "Unknown"，年龄初始化为 0，并输出：
Default constructor called

带参数构造函数
使用给定的姓名和年龄初始化对象，并输出：
Parameterized constructor called

拷贝构造函数
使用一个已存在的 Person 对象初始化新对象，并输出：
Copy constructor called

成员函数 display()
按如下格式输出对象信息：

Name: 姓名, Age: 年龄

输入格式
输入一行，包含一个姓名和一个年龄。

John 25
​
输出格式
按照题目要求输出构造函数调用信息，以及对象的信息。

Default constructor called
Name: Unknown, Age: 0
Parameterized constructor called
Name: John, Age: 25
Copy constructor called
Name: John, Age: 25
​
编程要求
请编写完整程序，并完成以下内容：

定义并实现 Person 类
在 main 函数中读入姓名和年龄
创建对象 p1，调用默认构造函数
创建对象 p2(name, age)，调用带参数构造函数
创建对象 p3 = p2，调用拷贝构造函数
分别调用这三个对象的 display() 函数输出信息
需要程序逻辑正确，且输入输出格式符合示例。


#### 课后3
编写一个完整的 C++ 程序，定义一个 Stack 类，用数组实现一个顺序栈，并完成压栈、弹栈、查询栈顶元素等操作。

栈中元素为整数，最大容量为 1000。

类的要求
定义一个 Stack 类，包含以下成员：

私有成员
int stack[1000]：用于存储栈中元素
int top：栈顶指针
公有成员
默认构造函数
将栈初始化为空栈。
push(int x)
将整数 x 压入栈中。
pop()
弹出栈顶元素。
如果栈为空，则输出：
Empty!
getTop()
返回当前栈顶元素。
如果栈为空，则输出：
Empty!
isEmpty()
判断栈是否为空。若为空返回 true，否则返回 false。
输入格式
第一行输入一个整数 n，表示接下来共有 n 次操作。

接下来输入 n 行，每行表示一次操作，格式如下：

1 x：将整数 x 压入栈中
2：弹出栈顶元素
3：查询当前栈顶元素
输出格式
对于每次操作：

若操作为 2 且栈为空，输出 Empty!
若操作为 3：
如果栈非空，输出当前栈顶元素
如果栈为空，输出 Empty!
编程要求
请编写完整程序，并完成以下内容：

定义并实现 Stack 类
使用数组实现顺序栈
在默认构造函数中完成空栈初始化
在 main 函数中读入操作并依次执行
按题目要求输出结果
要求不能使用 vector。

数据说明
1 <= n <= 100
栈中元素均为整数
保证压栈操作不会导致栈溢出
输入输出示例 1
输入
5
1 10
1 20
3
2
3
​
输出
20
10
```
#include <iostream>

  

using namespace std;

  

class Stack {

private:

    int stack[1000];

    int top;        

  

public:

    Stack() {

        top = -1;

    }

  

    bool isEmpty() {

        return top == -1;

    }

  

    void push(int x) {

        stack[++top] = x;

    }

  

    void pop() {

        if (isEmpty()) {

            cout << "Empty!" << endl;

        } else {

            top--;

        }

    }

  

    void getTop() {

        if (isEmpty()) {

            cout << "Empty!" << endl;

        } else {

            cout << stack[top] << endl;

        }

    }

};

  

int main() {

    int n;

    if (!(cin >> n)) return 0;

  

    Stack s;

  

    for (int i = 0; i < n; ++i) {

        int op;

        cin >> op;

        if (op == 1) {

            int x;

            cin >> x;

            s.push(x);

        } else if (op == 2) {

            s.pop();

        } else if (op == 3) {

            s.getTop();

        }

    }

  

    return 0;

}
```
#### 课后4
编写一个完整的 C++ 程序，实现字符串的基本压缩功能。

压缩规则如下：

对于字符串中连续重复出现的字符，用“字符 + 重复次数”来表示这一段内容。

例如：

aaabbbbbcc 压缩后为 a3b5c2
woohhheeeennnniiii 压缩后为 w1o2h3e4n4i4
如果压缩后的字符串长度没有变短，则输出原字符串。

题目保证输入字符串只包含大小写英文字母，可以使用 string 类型。

输入格式
输入一行，一个只包含大小写英文字母的字符串。

输出格式
输出压缩后的结果。
如果压缩后的字符串长度不小于原字符串长度，则输出原字符串。

编程要求
请编写完整程序，并完成以下内容：

读入一个字符串
按题目要求对字符串进行压缩
如果压缩后字符串更短，则输出压缩后的字符串
否则输出原字符串
数据说明
字符串长度大于等于 1
字符串中只包含大小写英文字母
输入输出示例 1
输入
aaabbbbbcc
​
a3b5c2
```
​#include <iostream>
#include <string>
using namespace std;
string compressString(string s) {
    int n = s.length();
    if (n <= 1) return s;
    string compressed = "";
    int count = 1;
    for (int i = 0; i < n; ++i) {
        if (i + 1 < n && s[i] == s[i + 1]) {
            count++;
        } else {
            compressed += s[i];
            compressed += to_string(count);
            count = 1;
        }
    }
    if (compressed.length() >= s.length()) {
        return s;
    }
    return compressed;
}
int main() {
    string input;
    if (!(cin >> input)) return 0;
    cout << compressString(input) << endl;
    return 0;
}
```

### W6
#### 课后1
请编写一个完整的 C++ 程序，实现一个二维整数矩阵类 IntMatrix，包含以下成员。

私有数据成员
int** data = nullptr：指向二维动态数组
int rows = 0：矩阵行数
int cols = 0：矩阵列数
公有成员函数
IntMatrix() = default;
默认构造函数。要求显式写出 =default。
IntMatrix(const IntMatrix& other)
拷贝构造函数。要求实现深拷贝。
IntMatrix& operator=(const IntMatrix& other)
赋值运算符重载。要求实现深拷贝，并正确处理自赋值。
~IntMatrix()
析构函数，释放二维动态数组空间。
void init(int r, int c)
将当前矩阵重置为一个 r × c 的新矩阵。若之前已有动态空间，应先正确释放旧空间，再申请新空间。
void read()
读入当前矩阵中的 rows × cols 个整数。
void set(int x, int y, int v)
将第 x 行第 y 列元素改为 v。
int sum() const
返回整个矩阵所有元素之和。
int rowSum(int x) const
返回第 x 行所有元素之和。
int colSum(int y) const
返回第 y 列所有元素之和。
void print() const
按矩阵形式输出全部元素。每行输出一行，同行元素之间用一个空格分隔。
功能说明
程序开始时先创建若干个默认构造的 IntMatrix 对象，编号为 0 到 k-1。

之后根据输入的操作指令完成各种操作。

操作共有 8 种。

1. 初始化矩阵
INIT i r c
​
表示将编号为 i 的矩阵对象重置为一个 r × c 的新矩阵。
接下来输入 r 行，每行 c 个整数，作为矩阵内容。

2. 赋值
ASSIGN i j
​
表示执行赋值操作：

obj[i] = obj[j];
​
要求赋值后两个对象互不共享动态内存。

3. 按值传参输出
COPY i
​
将编号为 i 的对象按值传递给函数 PrintByValue，并输出矩阵内容。

也就是逻辑上执行：

PrintByValue(obj[i]);
​
其中：

void PrintByValue(IntMatrix x) {
    x.print();
}
​
这一步会触发拷贝构造函数。

4. 修改单个元素
SET i x y v
​
将编号为 i 的矩阵的第 x 行第 y 列元素修改为 v。

5. 整矩阵求和
SUM i
​
输出编号为 i 的矩阵所有元素之和。

6. 某一行求和
ROWSUM i x
​
输出编号为 i 的矩阵第 x 行所有元素之和。

7. 某一列求和
COLSUM i y
​
输出编号为 i 的矩阵第 y 列所有元素之和。

8. 输出矩阵
PRINT i
​
按矩阵形式输出编号为 i 的矩阵全部元素，每行一行。

输入格式
第一行输入两个整数 k 和 m，分别表示矩阵对象个数和操作数。

接下来输入 m 条操作指令，格式见题目说明。

题目保证：

1 <= k <= 10
1 <= m <= 100
所有被查询、被赋值、被修改的矩阵对象，在此之前一定已经执行过 INIT
INIT 中的 r >= 1，c >= 1
下标 i、j、x、y 均合法
矩阵元素及修改值的绝对值不超过 1000
输出格式
对于每条需要输出的操作，按要求输出结果。

对于 SUM、ROWSUM、COLSUM 操作，输出一个整数，占一行。
对于 PRINT、COPY 操作，按矩阵形式输出，共输出对应矩阵的 rows 行。
输入输出示例
输入
2 9
INIT 0 2 3
1 2 3
4 5 6
COPY 0
ASSIGN 1 0
SET 0 0 1 100
PRINT 0
PRINT 1
ROWSUM 1 1
COLSUM 0 1
​
输出
1 2 3
4 5 6
1 100 3
4 5 6
1 2 3
4 5 6
15
105
​
提示
本题重点考查二维动态数组的申请与释放。
如果只使用默认拷贝，会导致多个对象共享同一块矩阵内存，后续修改或析构都会出问题。
COPY 操作会通过按值传参触发拷贝构造函数。
sum、rowSum、colSum、print 都应定义为 const 成员函数。
赋值运算符中要注意处理自赋值。

#### 课后2
Q：两个拷贝构造函数为什么都要有if条件，一个if (other.capacity > 0) 、if (this != &other) 为什么要有if条件？没有怎么样？怎么判断要添加怎么样的if条件
Ans：如何总结“什么时候该加什么样的 if”？遵循以下三个原则：
A. “自毁”原则 —— 针对 operator=
判断条件：if (this == &other) 或 if (this != &other)。
场景：凡是类里面有 new 指针成员，且在赋值函数里有 delete 旧指针的操作，必加。

B. “合法/有效性”原则 —— 针对所有函数
判断条件：if (ptr != nullptr) 或 if (size > 0)。
场景：在访问指针指向的内容前，判断指针是否为空。
在分配资源前，判断请求的资源量是否大于 0。
你的 if (other.capacity > 0) 就属于这一类。

C. “边界/越界”原则 —— 针对索引操作
判断条件：if (index >= 0 && index < size)。
场景：凡是通过下标访问数组（如你的 addItem 函数中判断 size >= capacity），必加。
#### 课后4
+链表请定义类 PrintQueue，包含以下成员。

私有数据成员
struct Node：表示一个打印任务结点，包含以下字段
char name[21]：任务名称，长度不超过 20，不含空格
int pages：该任务需要打印的页数
Node* next：指向下一个结点
Node* head = nullptr：队首指针
Node* tail = nullptr：队尾指针
int size = 0：当前队列中的任务个数
公有成员函数
PrintQueue() = default;
默认构造函数。要求显式写出 =default。
PrintQueue(const PrintQueue& other);
拷贝构造函数。要求实现深拷贝。
PrintQueue& operator=(const PrintQueue& other);
赋值运算符重载。要求实现深拷贝，并正确处理自赋值。
~PrintQueue();
析构函数。释放整条链表。
void enqueue(const char* name, int pages);
在队尾加入一个新的打印任务。
bool dequeue();
删除队首任务。若原队列非空，删除成功并返回 true；否则返回 false。
int count() const;
返回当前任务个数。
int totalPages() const;
返回当前队列中所有任务页数之和。
bool front(char* nameOut, int& pagesOut) const;
读取队首任务信息。若队列非空，则将队首任务名称写入 nameOut，页数写入 pagesOut，并返回 true；否则返回 false。
void print() const;
按队首到队尾的顺序输出全部任务。每个任务输出格式为：
name pages
​
若队列为空，输出：

EMPTY
​
功能说明
程序开始时先创建 m 个默认构造的 PrintQueue 对象，编号为 0 到 m-1。

之后根据输入的操作指令完成各种操作。

操作共有 7 种。

1. 添加打印任务
ADD i name pages
​
向编号为 i 的打印队列队尾加入一个新任务。

输出：

OK
​
题目保证：任务名称长度不超过 20，且不含空格；pages > 0。

2. 删除队首任务
POP i
​
删除编号为 i 的打印队列队首任务。

若删除成功，输出 OK
若该队列为空，输出 EMPTY
3. 查看队首任务
FRONT i
​
输出编号为 i 的打印队列队首任务信息，格式为：

name pages
​
若该队列为空，输出：

EMPTY
​
4. 统计任务个数
COUNT i
​
输出编号为 i 的打印队列中任务个数。

5. 统计总页数
TOTAL i
​
输出编号为 i 的打印队列中所有任务页数之和。

6. 赋值复制
ASSIGN i j
​
执行赋值操作，使编号为 i 的打印队列变成编号为 j 的打印队列的一份拷贝。

也就是逻辑上执行：

queue[i] = queue[j];
​
7. 快照输出
SNAPSHOT i
​
将编号为 i 的打印队列按值传参给函数 PrintByValue，并输出其内容。

也就是逻辑上执行：

PrintByValue(queue[i]);
​
其中：

void PrintByValue(PrintQueue q) {
    q.print();
}
​
这一步会触发拷贝构造函数。

输入格式
第一行输入两个整数 m 和 q，分别表示打印队列个数和操作条数。
接下来输入 q 行操作指令，格式见题目说明。

题目保证：

1 <= m <= 10
1 <= q <= 200
任务名称只包含大小写字母、数字和下划线，不含空格
1 <= pages <= 100
输出格式
对于每条需要输出的操作，按要求输出结果，每个结果占一行。

对于 SNAPSHOT 操作：

若队列为空，输出一行 EMPTY
否则按队首到队尾顺序输出全部任务，每个任务占一行
输入输出示例
输入
2 12
ADD 0 hw1 12
ADD 0 report 8
FRONT 0
COUNT 0
TOTAL 0
ASSIGN 1 0
POP 0
FRONT 0
SNAPSHOT 1
TOTAL 1
COUNT 1
POP 1
​
输出
OK
OK
hw1 12
2
20
OK
report 8
hw1 12
report 8
20
2
OK
​
提示
本题重点考查链式结构的深拷贝策略。
拷贝构造函数和赋值运算符都应重新复制整条链表，而不是直接复制头指针。
析构函数中需要释放所有结点。
count、totalPages、front、print 都应定义为 const 成员函数。
赋值运算符中应注意自赋值问题。
复制链表时要同时维护好 head、tail 和 size。
```
#include <iostream>

#include <cstring>

using namespace std;

  

class PrintQueue {

private:

    struct Node {

        char name[21];

        int pages;

        Node* next;

    };

    Node* head = nullptr;

    Node* tail = nullptr;

    int size = 0;

public:

    PrintQueue() = default;

    PrintQueue(const PrintQueue& other);

    PrintQueue& operator=(const PrintQueue& other);

    ~PrintQueue();

    void enqueue(const char* name, int pages);

    bool dequeue();

    int count() const;

    int totalPages() const;

    bool front(char* nameOut, int& pagesOut) const;

    void print() const;

};

  

PrintQueue::PrintQueue(const PrintQueue& other) {

    head = tail = nullptr;

    size = 0;

    Node* p = other.head;

    while (p) {

        enqueue(p->name, p->pages);

        p = p->next;

    }

}

  

PrintQueue& PrintQueue::operator=(const PrintQueue& other) {

    if (this == &other) return *this;

    while (dequeue());

    Node* p = other.head;

    while (p) {

        enqueue(p->name, p->pages);

        p = p->next;

    }

    return *this;

}

  

PrintQueue::~PrintQueue() {

    while (dequeue());

}

  

void PrintQueue::enqueue(const char* name, int pages) {

    Node* newNode = new Node;

    strcpy(newNode->name, name);

    newNode->pages = pages;

    newNode->next = nullptr;

    if (!head) {

        head = tail = newNode;

    } else {

        tail->next = newNode;

        tail = newNode;

    }

    size++;

}

  

bool PrintQueue::dequeue() {

    if (!head) return false;

    Node* temp = head;

    head = head->next;

    delete temp;

    size--;

    if (!head) tail = nullptr;

    return true;

}

  

int PrintQueue::count() const {

    return size;

}

  

int PrintQueue::totalPages() const {

    int sum = 0;

    Node* p = head;

    while (p) {

        sum += p->pages;

        p = p->next;

    }

    return sum;

}

  

bool PrintQueue::front(char* nameOut, int& pagesOut) const {

    if (!head) return false;

    strcpy(nameOut, head->name);

    pagesOut = head->pages;

    return true;

}

  

void PrintQueue::print() const {

    if (!head) {

        cout << "EMPTY" << endl;

        return;

    }

    Node* p = head;

    while (p) {

        cout << p->name << " " << p->pages << endl;

        p = p->next;

    }

}

  

void PrintByValue(PrintQueue q) {

    q.print();

}

  

int main() {

    int m, q;

    cin >> m >> q;

    PrintQueue queues[10];

    while (q--) {

        char op[10];

        cin >> op;

        if (strcmp(op, "ADD") == 0) {

            int i, pages;

            char name[21];

            cin >> i >> name >> pages;

            queues[i].enqueue(name, pages);

            cout << "OK" << endl;

        } else if (strcmp(op, "POP") == 0) {

            int i;

            cin >> i;

            if (queues[i].dequeue()) cout << "OK" << endl;

            else cout << "EMPTY" << endl;

        } else if (strcmp(op, "FRONT") == 0) {

            int i, pages;

            char name[21];

            cin >> i;

            if (queues[i].front(name, pages)) cout << name << " " << pages << endl;

            else cout << "EMPTY" << endl;

        } else if (strcmp(op, "COUNT") == 0) {

            int i;

            cin >> i;

            cout << queues[i].count() << endl;

        } else if (strcmp(op, "TOTAL") == 0) {

            int i;

            cin >> i;

            cout << queues[i].totalPages() << endl;

        } else if (strcmp(op, "ASSIGN") == 0) {

            int i, j;

            cin >> i >> j;

            queues[i] = queues[j];

        } else if (strcmp(op, "SNAPSHOT") == 0) {

            int i;

            cin >> i;

            PrintByValue(queues[i]);

        }

    }

    return 0;

}
```

### W7
#### 课堂1
##### `*this`指针
```
#include <iostream>
#include "TicketNo.h"

/**
 * 构造函数
 * 使用公式将传入的初始号码规范化到 0-99 范围内
 */
TicketNo::TicketNo(int n) {
    number = (n % 100 + 100) % 100;
}

/**
 * operator+ (const)
 * 返回一个增加步长后的新对象，不改变当前对象
 */
TicketNo TicketNo::operator+(int step) const {
    // 利用构造函数自动进行规范化处理
    return TicketNo(number + step);
}

/**
 * operator+=
 * 修改当前对象的号码，并返回自身的引用
 */
TicketNo& TicketNo::operator+=(int step) {
    number = ( (number + step) % 100 + 100) % 100;
    return *this;
}

/**
 * 前缀自增 (++counter)
 * 先加 1 再返回
 */
TicketNo& TicketNo::operator++() {
    return *this += 1; // 复用 operator+= 的逻辑
}

/**
 * 后缀自增 (counter++)
 * 先保存备份，再自增，返回备份
 */
TicketNo TicketNo::operator++(int) {
    TicketNo temp = *this; // 保存旧值
    *this += 1;            // 当前对象自增
    return temp;           // 返回旧值对象
}

/**
 * 输出当前号码
 */
void TicketNo::print() const {
    std::cout << number << std::endl;
}
```
#### 课后4
```
#include <iostream>
#include <string>
#include <iomanip>

using namespace std;

class ClockTime {
private:
    int hour;
    int minute;

    // 辅助函数：将任意时分规范化为 24 小时制
    void normalize(int h, int m) {
        // 计算总分钟数
        long long totalMinutes = (long long)h * 60 + m;
        // 1440 是一天的总分钟数 (24 * 60)
        // 使用数学取模，确保结果为正
        int normalizedMinutes = (totalMinutes % 1440 + 1440) % 1440;
        
        this->hour = normalizedMinutes / 60;
        this->minute = normalizedMinutes % 60;
    }

public:
    // 构造函数
    ClockTime(int h = 0, int m = 0) {
        normalize(h, m);
    }
    // 设置时间
    void set(int h, int m) {
        normalize(h, m);
    }
    // operator+ 不修改原对象
    ClockTime operator+(int x) const {
        return ClockTime(this->hour, this->minute + x);
    }
    // operator- 不修改原对象
    ClockTime operator-(int x) const {
        return ClockTime(this->hour, this->minute - x);
    }
    // operator+= 修改当前对象
    ClockTime& operator+=(int x) {
        normalize(this->hour, this->minute + x);
        return *this;
    }
    // 前缀自增 ++cur
    ClockTime& operator++() {
        normalize(this->hour, this->minute + 1);
        return *this;
    }
    // 后缀自增 cur++
    ClockTime operator++(int) {
        ClockTime temp = *this; // 保存原副本
        normalize(this->hour, this->minute + 1); // 修改当前值
        return temp; // 返回原副本
    }
    // 相等判断
    bool operator==(const ClockTime& other) const {
        return (this->hour == other.hour && this->minute == other.minute);
    }
    // 不等判断
    bool operator!=(const ClockTime& other) const {
        return !(*this == other);
    }
    // 格式化输出 HH:MM
    void print() const {
        cout << setfill('0') << setw(2) << hour << ":"
             << setfill('0') << setw(2) << minute;
    }
};

int main() {
    int n;
    if (!(cin >> n)) return 0;

    ClockTime cur(0, 0);

    while (n--) {
        string cmd;
        cin >> cmd;

        if (cmd == "SET") {
            int h, m;
            cin >> h >> m;
            cur.set(h, m);
        } else if (cmd == "ADD") {
            int x;
            cin >> x;
            cur = cur + x;
        } else if (cmd == "SUB") {
            int x;
            cin >> x;
            cur = cur - x;
        } else if (cmd == "MOVE") {
            int x;
            cin >> x;
            cur += x;
        } else if (cmd == "PREINC") {
            ClockTime t = ++cur;
            t.print();
            cout << " ";
            cur.print();
            cout << endl;
        } else if (cmd == "POSTINC") {
            ClockTime t = cur++;
            t.print();
            cout << " ";
            cur.print();
            cout << endl;
        } else if (cmd == "CHECK") {
            int h, m;
            cin >> h >> m;
            if (cur == ClockTime(h, m)) {
                cout << "true" << endl;
            } else {
                cout << "false" << endl;
            }
        } else if (cmd == "PRINT") {
            cur.print();
            cout << endl;
        }
    }

    return 0;
}
```