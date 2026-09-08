# Cheatsheet
## C++
### Importing
```cpp
#include <package_name>
using namespace std;      // This will be assumed for the remaining of this cheatsheet
```
Sometimes you may want to import everything available to your compiler. This is usually a bad idea in the case of software engineering, but may be convenient for quick testing and simple codes. To do so:
```cpp
#include <bits/stdc++.h>
```
For a list of what libraries are included with that import, see [here](https://gcc.gnu.org/onlinedocs/gcc-4.6.2/libstdc++/api/a01040_source.html)

### Data types
Below are some commonly used primitive types. For the full list, please refer to [here](https://en.cppreference.com/w/cpp/language/types).

| Data Type   | Size (Bytes)  | Typical Range*            |
| ----------- |:-------------:| :------------------------:|
| `bool`      | 1             | \[`false`, `true`\]       |
| `char`      | 1             | ±127 OR \[0, 255\]        |
| `int`       | 4             | ±(9.10^6)                 |
| `long`      | 4             | ±(2^32 - 1) OR ±(2^64 - 1)|
| `long long` | 8             | ±(9.10^18)                |
| `float`     | 4             | ±3.4e(±38)  (~7 digits)   |
| `double`    | 8             | ±1.7e(±308) (~15 digits)  |

#### Character
Horizontal tab `'\t'`
Newline (LF) `'\n'`
Vertical tab (VT) `'\v'`
Feed (FF) `'\f'`
Carriage return (CR) `'\r'`
Converting between character of numbers to character:
```cpp
char char_five = '5';
int  int_five  = char_five - '0'; //=> 5
```
#### Type casting
```cpp
int i;
float pi = 3.14;

// Both methods below are equivalent
i = (int) pi;
i = int (pi);
```
### Alias
typedef <kiểu dữ liệu> <tên>;
#define <tên> <kiểu dữ liệu/cấu trúc/câu lệnh> (nhiều ứng dụng hơn)
**C++ 11 trở lên** có thể dùng **using**
```cpp
typedef long long ll;
typedef vector<tuple<nusnet_id, name, grade>> grades;
grades CS2040C;
CS2040C.push_back(make_tuple("A0123456I", "Kattis", "A+"));
#define ll long long
#define FOR(a, b, i) for(int i = a, i < b; i++)
using ll = long long;
```
### Function
Viết hàm và gọi hàm :
  ```cpp
  int check(int a, int b /*Tham số hình thức*/){
      return a;
  }

  int main(){
      int x = 5, y = 10; //Tham số chính thức
      check(x, y); // lời gọi hàm 
  }
  ```
Nạp chồng hàm : tên hàm có thế giống nhau nhưng kiểu trả về khác nhau 
  ```cpp
  void Tinh_tong(){
  }
  long long Ting_tong(){
  }
  ```
Truyền giá trị mặc định tham số vào hàm : truyền vào phần tử cuối bên phải nếu chỉ truyền 1 tham số || truyền hết cho n tham số
  - `void f(int a = 5, long long b = 7, float c = 7.6);`
  - `void f(int a, int b, char c = 'c');`
Viết hàm có tham số là con trỏ hoặc mảng :
```cpp
// con trỏ
void f(int *a){
  code
}
int main(){
  f(&a);
}

// mảng 1 chiều
void f(int a[], int n){
  code
}
int main(){
  f(a, 10);
}

// mảng 2 chiều
void f(int a[][1000], int n){
  code
}
int main(){
  int n, m; cin >> n >> m;
  int a[n][m];
  f(a, n);
} 
```
### Operators
See [here](http://www.cplusplus.com/doc/tutorial/operators/) for the full list of C++ operators.
#### Assignment
```cpp
y = x;
```
Assigns variable y with the value of x. Note however that if `Y` is [copy assignable](https://en.cppreference.com/w/cpp/named_req/CopyAssignable), then `=` will be a [copy assignment operator](https://en.cppreference.com/w/cpp/language/copy_assignment) so `Y` will take on a copy of `x` at the end of the operation.

#### Arithmetic
| Operator | Description   |
|:--------:|:-------------:|
| `+`      | Addition      |
| `-`      | Subtraction   |
| `*`      | Multiplication|
| `/`      | Division      |
| `%`      | Modulo        |

#### Compound Assignment
|Expression | Equivalence  |
|:---------:|:------------:|
| `y += x;` | `y = y + x;` |
| `y -= x;` | `y = y - x;` |
| `y *= x;` | `y = y * x;` |
| `y /= x;` | `y = y / x;` |
| `y %= x;` | `y = y % x;` |

#### Increment and Decrement
|Expression | Equivalence        |
|:---------:|:------------------:|
| `y = ++x;` | `x += 1; y  = x;` |
| `y = x++;` | `y  = x; x += 1;` |
| `y = --x;` | `x -= 1; y  = x;` |
| `y = x--;` | `y  = x; x -= 1;` |

#### Relational and comparison operators
| Operator | Description              |
|:--------:|:------------------------:|
| `==`     | Equal to                 |
| `!=`     | Not equal to             |
| `<`      | Less than                |
| `>`      | Greater than             |
| `<=`     | Less than or equal to    |
| `>=`     | Greater than or equal to |

#### Logical operators
| Operator | Description  |
|:--------:|:------------:|
| `!`      | Logical NOT  |
| `&&`     | Logical AND  |
| `\|\|`   | Logical OR   |

#### Conditional ternary operator
```cpp
x = (predicate)? consequent: alternative;
```
Is equivalent to:
```cpp
if (predicate)
  x = consequent;
else
  x = alternative;
```
#### Comma operator
> In the C and C++ programming languages, the comma operator (represented by the token ,) is a binary operator that evaluates its first operand and **discards the result**, and then evaluates the second operand and returns this value (and type). The use of the comma token as an operator is distinct from its use in function calls and definitions, variable declarations, enum declarations, and similar constructs, where it acts as a separator. [source](https://en.wikipedia.org/wiki/Comma_operator)

Example:
```cpp
int a,b;
a = (b=3, b+2); //=> a = 5;
```
This can be particularly useful for reading inputs that are terminated by a given condition, for instance:
```cpp
// While loop terminates when a == b == c == d == 0
while(cin >> a >> b >> c >> d, a || b || c || d) {
  // Do something
}
```
#### Bitwise operators
| Operator | Description     |
|:--------:|:---------------:|
| `&`      | Bitwise AND     |
| `\|`     | Bitwise OR      |
| `^`      | Bitwise XOR     |
| `~`      | NOT             |
| `<<`     | Bit-shift left  |
| `>>`     | Bit-shift right |

### Control flow
#### Selection
##### If-else statement
```cpp
if (condition_1) {
  // Do something
}
else if (condition_2) {
  // Do something
}
else {
  // Do something
}
```
##### Switch statement
```cpp
int test = 1;
switch(test) {
  case 1 : cout << '1'; // prints "1"
           break;       // and exits the switch
  case 2 : cout << '2';
           break;
  // ...
  default:
           cout << 'Default case';
           break;
}
```
If declaration statement exists within a case, it has to be scoped. e.g.
```cpp
switch(1) {
  case 1: {  int x = 0;
             std::cout << x << '\n';
             break;
          } // scope of 'x' ends here
  default: std::cout << "default\n"; // no error
           break;
}
```
#### Repetition
##### For-loop
```cpp
for (int i=0; i<n; ++i) {
  // ...
}

// a is a copy of the items we are iterating over
for (auto a : s) {
  cout << a << " ";
}

// a is a reference of the items we are iterating over
for (auto &a : s) {
  cout << a << " ";
}  
```
##### While-loop
```cpp
while(loop_conditon) {
   // ...
}

int n = 5;
while(--n) {
  // n = 4, 3, 2, 1
}

int m = 5;
while(m--) {
  // m = 4, 3, 2, 1, 0
}
```
##### Do-while-loop
```cpp
do {
   // ...
}
while (loop_condition);
```
### Array
#### One dimension
- Các hàm có sẵn
  - Tìm phần tử lớn nhất : `*max_element(a, a + n)`
  - Tìm phần tử nhỏ nhất : `*min_element(a, a + n)`
  - Hàm max, min của C++ :
      - `max(a, b)`
      - `min({a, b, ...})`
  - Hàm memset(tên_mảng, giá trị (0, -1), sizeof(tên_mảng)) 
      - `memset(a, 0, sizeof(a));`
      - `memset(b, false, sizeof(b));`
- Cài đặt :
  ```cpp
  int a[1000006], n;

  void input(){
      cin >> n;
      for(int i = 0; i < n; i++) cin >> a[i];
  }
  void output(){
      for(int i = 0; i < n; i++) cout << a[i] << ' '; 
  }

  int find(int val){
      for(int i = 0; i < n; i++){
          if(a[i] == val) return i;
      }
      return -1;
  }

  void insert(int pos, int val){
      if(pos >= 1e6) return;
      for(int i = n - 1; i >= pos; i--){
          a[i + 1] = a[i];
      }
      a[pos] = val;
      n++;
  }

  void erase(int val){
      int pos = find(val);
      if(pos == -1) return;
      for(int i = pos + 1; i < n; i++){
          a[i - 1] = a[i];
      }
      n--;
  }

  int sz(){
      return n;
  }
  ```
- Ứng dụng :
  ```cpp
  //Đếm phân phối => khai báo mảng đánh dấu ngoài main -> tránh bị tràn bộ nhớ
  int cnt[10000001] = {0}; 
  int mark[10000001] = {0};
  int used[10000001] = {0};

  //Liệt kê theo thứ tự tăng dân
  void counting1(int a[], int n){
      int min_val = INT_MAX; max_val = INT_MIN;
      for(int i = 0; i < n; i++){
          cnt[a[i]]++;
          max_val = max(max_val, a[i]);
          min_val = min(max_val, a[i]);
      }
      for(int i = min_val; i <= max_val; i++){
          if(cnt[i]) cout << i << ' ' << cnt[i];
      }
  }

  //Liệt kê theo thứ tự xuất hiện của các phần tử trong mảng
  void counting2(int a[], int n){
      for(int i = 0; i < n; i++){
          mark[a[i]]++;
      }
      for(int i = 0; i < n; i++){
          if(used[a[i]] == 0){
              cout << a[i] << ' ' << mark[a[i]] << endl;
              used[a[i]] = 1;
          }
      }
  }

  // Đảo ngược máng
  void reverse(int a[], int n){
      int l = 0; r = n - 1;
      while(l <= r){
          swap(a[l], a[r]); //int temp = a[l]; a[l] = a[r]; a[r] = temp;
          l++; r--;
      }
  }
  ```
#### Two dimension
- Cài đặt:
  ```cpp
  ll res[1000][1000];
  int n, m, p;

  int main(){
      //Nhập mảng 2 chiều
      cin >> m >> n >> p;
      int a[n][p], b[p][m], sum[n][m], dif[n][m];
      ll product[n][m];
      for(int i = 0; i < n; i++){
          for(int j = 0; j < p; j++){
              cin >> a[i][j];
          }
      }

      for(int i = 0; i < p; i++){
          for(int j = 0; j < m; j++){
              cin >> b[i][j];
          }
      }

      //Cộng 2 ma trận
      for(int i = 0; i < n; i++){
          for(int j = 0; j < m; j++){
              sum[i][j] = a[i][j] + b[i][j]; 
          }
      }

      //Hiệu 2 ma trận
      for(int i = 0; i < n; i++){
          for(int j = 0; j < m; j++){
              dif[i][j] = a[i][j] - b[i][j]; 
          }
      }

      //Nhân 2 ma trận
      for(int i = 0; i < n; i++){
          for(int j = 0; j < m; j++){
              product[i][j] = 0;
              for(int k = 0; k < p; k++){
                  product[i][j] += 1ll * a[i][k] * b[k][j];
              }
          }
      }

      //Xuất ma trận
      for(int i = 0; i < n; i++){
          for(int j = 0; j < m; j++){
              cout << sum[i][j] << ' ';
          }
          cout << endl;
      }
      cout << endl;

      for(int i = 0; i < n; i++){
          for(int j = 0; j < m; j++){
              cout << dif[i][j] << ' ';
          }
          cout << endl;
      }
      cout << endl;

      for(int i = 0; i < n; i++){
          for(int j = 0; j < m; j++){
              cout << product[i][j] << ' ';
          }
          cout << endl;
      }
      cout << endl;

      //Giá trị min và max với vị trí của chúng trong mảng 2 chiều
      int max_val = INT_MIN; 
      int min_val = INT_MAX;
      
      for(int i = 0; i < n; i++){
          for(int j = 0; j < m; j++){
              max_val = max(a[i][j], max_val);
              min_val = min(a[i][j], min_val);
          }
      }
      cout << min_val << endl;
      for(int i = 0; i < n; i++){
          for(int j = 0; j < m; j++){
              if(min_val == a[i][j]) cout << i + 1 << " " << j + 1 << endl;
          }
      }
      cout << max_val << endl;
      for(int i = 0; i < n; i++){
          for(int j = 0; j < m; j++){
              if(max_val == a[i][j]) cout << i + 1 << " " << j + 1 << endl;
          }
      }
  }
  ```
### Pointer and Reference
Con trỏ b quản lý biến a 
  - `int a = 6;`
  - `int *b = &a;` 
Gán giá trị khác cho a qua con trỏ 
  - `*b = 100;  //(a = 100)`
In ra giá trị biến a :
  - `cout << a;`
  - `cout << *b;` 
In ra địa chỉ biến a :
  - `cout << &a;`
  - `cout << b;`
Cấp phát động, con trỏ quản lý mảng và mảng các con trỏ 
  ```cpp
  int a[10000000] // mảng tĩnh => vùng nhớ stack 1e6 phần tử liên tiếp bị tràn
  int *a = new int[100000000] //mảng động => vùng nhớ heap 1e6 phần tử ko liên tiếp và ko bị tràn 
  int *c = NULL;
  int d[100];
  c = d = &d[0];
  d[1] == *(c + 1);
  //mảng con trỏ (con trỏ cấp 2)
  int **a = new int*[1000000]
  //di chuyển ô nhớ
  int *ptr = NULL;
  ++ptr;
  ptr -= 2;
  //giải phóng con trỏ và mảng quản lý con trỏ
  delete a;
  delete[] a;
  ```
#### Pointer
##### Basics
```cpp
int a = 3;
int *ptr1;              // Declaration
ptr1 = &a;              // Assignment
cout << *ptr1 << endl;  // De-reference to retrieve value
*ptr1 = 4;              // Modify value
cout << a << endl;      //=> 4

int b = 6;
int *ptr2 = &b;
ptr1 = ptr2;            // Re-assignment
cout << *ptr1 << endl;  //=> 6
cout << a << endl;      //=> 4
```
##### Object pointers
```cpp
MyObject* my_object = new MyObject();

/* The following are equivalent when accessing attributes of an object pointer */
(*my_object).attr1;
my_object->attr1;
```
#### Reference
[Pointers vs references](https://stackoverflow.com/questions/57483/what-are-the-differences-between-a-pointer-variable-and-a-reference-variable-in)
##### Basics
```cpp
int a = 3;
int &ref = a;
cout << ref;    // Acess value of reference
```
##### Examples
Swap function
```cpp
void swap(int &a, int &b) {
  int temp = a;
  a = b;
  b = temp;
}
```
By default C++ passes by value (i.e make copy). Thus when writing functions to modify objects, be sure to pass (and return) them via reference!
```cpp
void update_val(vector<int> &vect, int index, int val) {
  vect[index] = val;
}

queue<int> & get_curr_queue(bool on_left, queue<int> &left_queue, queue<int> &right_queue) {
  return (on_left)? left_queue : right_queue;
}
queue<int> &curr_queue = get_curr_queue(on_left, left_queue, right_queue); // Important! If not assigning to a reference, a copy is made instead
```
### I/O
#### [`iostream`](https://en.cppreference.com/w/cpp/io/basic_iostream)
Import
```cpp
#include <iostream>
```
##### Optimization for competitive programming
If only `cin`/`cout` will be used in place of `printf`/`scanf`, I/O can be optimized via:
```cpp
ios:sync_with_stdio(false); cin.tie(NULL);
```
See [here](https://www.geeksforgeeks.org/fast-io-for-competitive-programming/) for the reason.

##### [`std::cin`](https://en.cppreference.com/w/cpp/io/cin)
```cpp
// Read 2 numbers
int l, r;
cin >> l >> r;
// Read a word (terminated with space/tab)
string str;
cin >> str;
// Read an entire line
getline(cin, str);
```
##### [`std::cout`](https://en.cppreference.com/w/cpp/io/cout)
```cpp
cout << "Hello World!" << endl;
```
##### [`get`](https://en.cppreference.com/w/cpp/io/basic_istream/get)
Low level input that extracts a single character from stream:
```cpp
char c; cin.get(c);
```
It is also a common method to consume the newline character:
```cpp
cin.get(); // Consumed character discarded when no arguments given
```
##### `std::setprecision`
Sets the desired output precision format for real number representations e.g. `float`, `double` etc.
```cpp
std::cout << std::setprecision(5) << 1.123456789 << endl;  //=> 1.12345
```
#### `fstream`
```cpp
fstream f;
ifstream in;
ofstream out;
in.open("input.txt");
in.open("D:/CNTT/Code/C++/SOURCE/input.txt");
out.output("output.txt");
if(in.is_open()){
    code
}
in.close();
out.close();
```
#### [`stdio.h`](https://en.cppreference.com/w/cpp/header/cstdio)
##### Import
```cpp
#include <stdio.h>
```
##### [`printf`](https://en.cppreference.com/w/cpp/io/c/fprintf)
```cpp
printf("Hello World!\n");
printf ("Characters: %c %c \n", 'a', 65);
printf ("Decimals: %d %ld\n", 1977, 650000L);
printf ("Preceding with blanks: %10d \n", 1977);
printf ("Preceding with zeros: %010d \n", 1977);
printf ("Some different radices: %d %x %o %#x %#o \n", 100, 100, 100, 100, 100);
printf ("floats: %4.2f %+.0e %E \n", 3.1416, 3.1416, 3.1416);
printf ("Width trick: %*d \n", 5, 10);
printf ("%s \n", "A string");
```
Outputs the following
```sh
Characters: a A
Decimals: 1977 650000
Preceding with blanks:       1977
Preceding with zeros: 0000001977
Some different radices: 100 64 144 0x64 0144
floats: 3.14 +3e+000 3.141600E+000
Width trick:    10
A string
```

##### [`scanf`](https://en.cppreference.com/w/cpp/io/c/fscanf)
```cpp
int l, r;
scanf("%d %d", &l, &r);
```

#### [`sstream`](https://en.cppreference.com/w/cpp/header/sstream)
##### Import
```cpp
#include <sstream>
```
##### [`std::stringstream`](https://en.cppreference.com/w/cpp/io/basic_stringstream)
Output
```cpp
stringstream ss;
ss << "Hello";
ss << " ";
ss << "World!";
cout << s.str();  /=> Hello World!
```
Input
```cpp
string str = "John Doe Johnson Mary";
stringstream ss(str);
string word;
while(ss >> word) {
  cout << word; //=> John, Doe, Johnson, Mary
}
```
##### [`std::istringstream`](https://en.cppreference.com/w/cpp/io/basic_istringstream)
```cpp
string line = "5 6 22 8";
istringstream iss;
iss.clear();
iss.str(line);
int num1, num2, num3, num4;
iss >> num1 >> num2 >> num3 >> num4;
```

#### [`freopen`](https://en.cppreference.com/w/cpp/io/c/freopen), [`fclose`](https://en.cppreference.com/w/cpp/io/c/fclose)
##### Reading
```cpp
freopen("input.txt", "r", stdin);   // redirects standard input
string x;
cin >> x;                           // reads from input.txt
```
##### Writing
```cpp
freopen("output.txt", "w", stdout); // redirects standard output
string x = "Write this out to file";
cout << x << endl;                  // writes to output.txt
fclose(stdout);                     // end standard output redirection to file
```
#### C++ Template
``` cpp
#include <bits/stdc++.h> 
//các toán tử được định nghĩa trong namespace
using namespace std; 
using ll = long long;

int main(){
	freopen("input.txt", "r", stdin);
	freopen("output.txt", "w", stdout);
	ios::sync_with_stdio(false); 
	cin.tie(nullptr); cout.tie(nullptr);	
}
```
### Math libraries
#### [`cmath`](https://en.cppreference.com/w/cpp/header/cmath)
```cpp
#include <cmath>
```
##### [`floor`](https://en.cppreference.com/w/cpp/numeric/math/floor)
```cpp
floor(+2.7);      //=> 2.000000
floor(-2.7);      //=> -3.000000
floor(-0.0);      //=> -0.000000
floor(-INFINITY); //=> -INFINITY
```
##### [`ceil`](https://en.cppreference.com/w/cpp/numeric/math/ceil)
```cpp
ceil(+2.4);       //=> 3.000000
ceil(-2.4);       //=> -2.000000
ceil(-0.0);       //=> -0.000000
ceil(-INFINITY);  //=> -INFINITY
```
##### [`pow`](https://en.cppreference.com/w/cpp/numeric/math/pow)
```cpp
pow(2, 10);   //=> 1024
pow(2, 10.0); //=> 1024.0
```
##### [`Infinity`](https://en.cppreference.com/w/c/numeric/math/INFINITY)
`INFINITY` is a macro for numberic types supporting floating-point infinites.
```cpp
int     inf_int = INFINITY; //=> 4196512 (unsupported)
long    inf_lng = INFINITY; //=> 160     (unsupported)
float   inf_flt = INFINITY; //=> inf
double  inf_dbl = INFINITY; //=> inf
```

#### [`numberic_limits`](https://en.cppreference.com/w/cpp/types/numeric_limits/infinity)
Example
```cpp
#include <limits>
double max = numeric_limits<double>::max();
double inf = numeric_limits<double>::infinity();
assert(inf > max);  //=> assertion true
```
### Basic techniques
#### String to char array
```cpp
char* to_char_array(string str) {
    int n = str.length();
    char* char_array = new char[n+1];
    strcpy(char_array, str.c_str());
    return char_array;
}
```

#### Substring
```cpp
// Returns the index of the first occurence of the substring, else -1
int find(string str, string subStr) {
  for (int i = 0; i < str.length() - subStr.length() + 1; i++) {
    bool match = true;
    for (int j = 0; j < subStr.length(); j++) {
      if (str[i + j] != subStr[j]) {
        match = false;
        break;
      }
    }
    if (match) return i;
  }
  return -1;
}
```

#### Tokenize
For single delimiter
```cpp
#include <sstream>
// ...
vector<string> tokenizer(string str, char delimiter) {
  string token;
  istringstream iss(str);
  vector<string> tokens(str.length());
  while(getline(iss, token, delimiter)) {
    tokens.push_back(token);
  }
  return tokens;
}
```
For multiple delimiters
```cpp
vector<string> tokenizer(string str, string delimiters) {
    vector<string> tokens;
    char* str_chars = to_char_array(str);
    char* del_chars = to_char_array(delimiters);
    char *token = strtok(str_chars, del_chars); 
    while (token != NULL) {
        tokens.push_back(token);
        token = strtok(NULL, del_chars);
    }
    return tokens;
}
```
#### Reading until empty line
```cpp
vector<string> read_till_empty() {
  vector<string> lines;
  string line; 
  while(getline(cin, line), line.length()){
    lines.push_back(line);
  }
  return lines;
}
```

#### Reading 2D char array
```cpp
char** read_2D_char_array(int rows, int cols) {
  char **char_array_2D = new char *[rows];
  for (int r=0; r<rows; r++) {
    char_array_2D[r] = new char[cols];
    for (int c=0; c<cols; c++) {
      cin >> char_array_2D[r][c];
    }
  }
  return char_array_2D;
}
```

#### Printing 1D int array
```cpp
void print_array(int* arr, int n) {
  for (int* i=arr; i<arr+n; i++) {
    cout << *i;
    if (i<arr+n-1) cout << ", ";
  }
  cout << endl;
}
```

#### Index to iterator
```cpp
vector<int>::iterator get_iterator(int index, vector<int> & vect) {
  return vect.begin() + index;
}
```

#### Iterator to index
```cpp
int get_index(vector<int>::iterator it, vector<int> & vect) {
  return distance(vect.begin(), it);
}
```

#### Binary search
```cpp
int binary_search(vector<int> & sorted_vect, int val) {
  vector<int>::iterator it = lower_bound(sorted_vect.begin(), sorted_vect.end(), val);
  return (it == sorted_vect.end())
    ? -1
    : (*it == val)
      ? distance(sorted_vect.begin(), it)
      : -1;
}
```

