# STL
## Functions of STL
Import
```cpp
#include <algorithm>
#include <functional> // if using lambda
```
### [`max`](https://en.cppreference.com/w/cpp/algorithm/max)
```cpp
cout << min(10, 20) << endl; // tương tự với max()
cout << max({10, 20, 30, 50}) << endl; // tương tự vs min()
```
### [`max_element`](https://en.cppreference.com/w/cpp/algorithm/max_element)
```cpp
//trả về con trỏ và iterator đến phần tử => dùng * để giải tham chiếu
cout << *max_element(a, a + n) << endl;
cout << *min_element(v.begin() + 3, v.begin() + 5) << endl;
//lambda function
auto it_abs_max = max_element(v.begin(), v.end(), [](int a, int b){ return abs(a) < abs(b); });
```
### `accumulate`
```cpp
//accumulate => tính tổng
int sum = accumulate(a, a + n, 0); //giống như khởi tạo s = 0 -> đi cộng vào
cout << sum << endl;
cout << accumulate(v.begin(), v.end(), 0) << endl;
```
### `swap`
```cpp
//swap => hoán vị giá trị 2 phần tử 
int x = 100, y = 200;
cout << x << ' ' << y << endl;
swap(x, y);
cout << x << ' ' << y << endl;
```
### `find`
```cpp
//find() tìm 1 phần tử có xuất hiện trong mảng, vector, set hay ko => trả về iterator hoặc con trỏ
if(find(a, a + n, 5) != a + n) cout << "FOUND" << endl;
else cout << "NOT FOUND" << endl;
if(find(v.begin(), v.end(), 4) != v.end()) cout << "FOUND" << endl;
else cout << "NOT FOUND" << endl;
```
### `memset`
```cpp
//memset() => gán tất cả các phần tử trong mảng, mảng 2 chiều cho 0 hoặc -1
int b[n];
memset(b, 0, sizeof(a)); 
for(int x : b) cout << x << ' ';
cout << endl;
```
### `fill`
```cpp
//fill() => gán tất cả các phần tử trong mảng, vector cho 1 giá trị bất kì
vector<int> vi(5);
fill(begin(vi), end(vi), 200);
for(int x : vi) cout << x << ' ';
cout << endl;
```
### `merge`
```cpp
//merge => trộn 2 dãy tăng dần vào 1 dãy
vector<int> res(10);
vector<int> c = {1, 2, 3, 6, 8};
vector<int> d = {2, 4, 7, 10, 11};
merge(c.begin(), c.end(), d.begin(), d.end(), res.begin());
for(int x : res) cout << x << ' ';
cout << endl;
```
### `reverse`
```cpp
//reverse() => lật ngược mảng hoặc vector hoặc string
string s = "ngocty";
reverse(s.begin(), s.end());
cout << s << endl;
```
### [`sort`](https://en.cppreference.com/w/cpp/algorithm/sort) and[`stable_sort`](https://en.cppreference.com/w/cpp/algorithm/)
```cpp
//mảng
sort(a, a + n);                 // Sorts ascending (default)
sort(a, a + n, less<int>());    // Sorts descending
sort(a, a + n, greater<int>()); // Sorts descending
sort(a + x, a + y + 1); // sắp xếp trên [x,y] theo thứ tự tăng dần
stable_sort(a, a + n);          // Stable sorts ascending
//comparator
bool cmp(int a, int b){
    return abs(a) < abs(b);
}
stable_sort(a, a + n, cmp);
//lambda
stable_sort(a, a + n, [](int a, int b){
    return abs(a) < abs(b);
});
//vector
sort(vi.begin(), vi.end()); // sắp xếp trên cả vector
sort(vi.begin(), vi.end(), greater<int>()); // sorts descending
sort(vi.begin() + x, vi.begin() + y + 1); // sắp xếp trên [x, y] của vector
```
### [`binary_search`](https://en.cppreference.com/w/cpp/algorithm/binary_search)
```cpp
//sort trước khi dùng
//mảng 
int n; cin >> n;
int a[n];
for(int &x : a) cin >> x;
int x; cin >> x;
if(binary_search(a, a + n, x)) cout << 1 << endl;
else cout << 0 << endl;

//vector
vector<int> v;
for(int &x : a){
    cin >> x;
    v.push_back(x);
}
bool found_4 = binary_search(v.begin(), v.end(), 4);  //=> true
bool found_3 = binary_search(v.begin(), v.end(), 3);  //=> false
```
### [`lower_bound`](https://en.cppreference.com/w/cpp/algorithm/lower_bound)
Tìm vị trí đầu tiên của phần tử >= x trong mảng, vector sắp xếp tăng dần **CÓ THỨ TỰ**
```cpp
auto it = lower_bound(v.begin(), v.end(), x);  //=> iterator poiting at index position x
cout << *it << endl; 

it--; it += k;
cout << *it << endl; // phần tử < x

if(it == v.end()) cout << "NO\n"; //không tìm thấy
```
### [`upper_bound`](https://en.cppreference.com/w/cpp/algorithm/upper_bound)
Tìm vị trí đầu tiên của phần tử > x trong mảng, vector sắp xếp tăng dần **CÓ THỨ TỰ**
```cpp
it = upper_bound(a, a + n, x);  //=> iterator poiting at index position x
cout << *it << endl;

it--;
cout << *it << endl; // phần tử <= x

if(it == a + n) cout << "No\n"; // không tìm thấy
```
### Note `lower_bound` and `upper_bound`
- 2 hàm trên trả về con trỏ nếu là mảng, và iterator nếu là vector => tính chỉ số : `auto it = lower_bound(a, a + n, x) - a/a.end()`
- Nếu ko tìm thấy phần tử thỏa điều kiện thì trả về con trỏ `a + n` hoặc `a.end()`
- Cũng áp dụng được cho **set với map** nhưng phải dùng hàm `distance(se.begin(), it)` để tính chỉ số **(khoảng cách của phần tử tìm đc vs phần tử đầu tiên)**

### Sets function
Đều phải sort trước khi sử dụng => các phép trong tập hợp
- `set_union`
- `set_intersection`
- `set_difference`
- `set_symmetric_difference`
```cpp
//set_union tương tự vs các hàm set_ khác
int z[] = {1, 2, 3, 4, 5};
int t[] = {1, 3, 4, 9, 10};
vector<int> u(11);
auto it = set_union(z, z + 5, t, t + 5, u.begin()); // tương tự những cái còn lại
// phải resize vector u đi vì sau khi giao hợp hiệu thì vector u kích thước sẽ thay đổi chứ ko như ban đầu
u.resize(it - u.begin());
for(auto x : u) cout << x << ' ';
cout << endl;
```
### [`std::copy`, `std::copy_if`](https://en.cppreference.com/w/cpp/algorithm/copy)
```cpp
vector<int> v1{1,2,3,4,5};
vector<int> v2(5);
copy(v1.begin(), v1.end(), v2.begin());
//v1 == v2

vector<int> v_geq3{3,4,5};
vector<int> v3(3);
copy_if(v1.begin(), v1.end(), v3.begin(), [](int i){ return i >= 3;});
//v3 == v_geq3
```
### [`std::count`, `std::count_if`](https://en.cppreference.com/w/cpp/algorithm/count)
```cpp
vector<int> v{1,2,1,3,1,4,1,5,1,6};
cout << count(v.begin(), v.end(), 1);  //5
cout << count_if(v.begin(), v.end(), [](int i){ return i >= 3; });  //4
```
### [`std::equal_range`](https://en.cppreference.com/w/cpp/algorithm/equal_range)
```cpp
vector<pair<int, char>> v_ic = { {1,'A'}, {2,'B'}, {2,'C'}, {2,'D'}, {4,'G'}, {3,'F'} };
pair<int, char> value = {2, '?'};
auto [it_begin, it_end] = equal_range(v_ic.begin(), v_ic.end(), value);
auto it_lower = lower_bound(v_ic.begin(), v_ic.end(), value);
auto it_upper = upper_bound(v_ic.begin(), v_ic.end(), value);
//it_begin == it_lower
//it_end == it_upper
```
### [`std::remove`, `std::remove_if`](https://en.cppreference.com/w/cpp/algorithm/remove)
```cpp
string str1 = "Hello   World!";
str1.erase(remove(str1.begin(), str1.end(), ' '), str1.end());
//str1 == "HelloWorld!"

string str2 = " Hello \t World! \n";
str2.erase(remove(str2.begin(), str2.end(), [](char x){ return isspace(x); }), str2.end());
//str2 == "HelloWorld!"
```
## Iterator
- Các hàm 
    - `v.begin()` : phần tử đầu
    - `v.rbegin()` : phần tử cuối
    - `v.end()` : sau phần tử cuối
    - `v.rend()` : trước phần tử đầu
- Dùng iterator để duyệt set, map, vector, pair
    - [x; y) = (v.begin() + x, v.begin() + y);
    - [x; y] = (v.begin() + x, v.begin() + y + 1);
    - v[x] = v.begin() + x 
``` cpp
for(int i = 0; i < vi.size(); i++) cin >> v[i] ;
for(int &x : vi) cin >> x;
for(int i = 0; i < vi.size(); i++) cout << v[i] ;
for(int x : vi) cout << x; // iterator thông minh và ko cần *it để truy cập giá trị
for(vector<int>::iterator it = vi.begin(); it ≠ vi.end(); it++) cout << *it;
vector<int>::iterator it = auto it  //auto thay cho mọi kiểu dữ liệu
```
## Pair
- Khai báo : pair<kiểu_dữ_liệu, kiểu_dữ_liệu>; 
    - `pair<int, int> pi;`   
    - `pair<pair<bool, char>, string> pi2;` 
- Nhập và xuất pair :
``` cpp
cin >> a. first >> a.second  // pi = {100, 200} = make_pair(100, 200);
for(int i = 0; i < n; i++){
    cin >> a[i].first >> a[i].second; 
    cout << a[i].first << a[i].second;
}
```
## Vector
- Khai báo và nhập : 
    ```cpp
    vector<int> vi; 
    vector <pi> vii; 

    for(int i = 0; i < n; i++) {
        int temp; cin >> temp; 
        vi.push_back(temp);
    }
    // khai báo vector có n pt như mảng
    const n = 1e6;
    vector<int> a(n); 
    for(int i = 0; i < n; i++){
        cin >> a[i]; // hoặc dùng push_back() vẫn đc 
    }
    // gán giá trị sẵn cho vector 
    vector<kiểu dữ liệu> a(n, giá trị); // n pt có giá trị bằng giá trị gán
    vector<int> a(n, 6);
    ```
- Các hàm :
    - `vi.push_back()` : thêm vào sau vector và truy cập phần tử như mảng
    - `vi.size()` : cho biết số lượng phần tử vector
    - `vi.pop_back()` : xóa phần tử ở cuối vector
    - `vi.erase(vị trí)` : xóa phần tử thông qua iterator `vi.erase(vi.begin() + x)` 
    - `vi.erase(it.begin() + x, it.begin() + y + 1)` : xóa 1 đoạn [x, y] 
    - `vi.insert(vị trí, giá trị)` : thêm phần tử vào vị trí nào đó `vi.insert(vi.begin() + x, k)`
    - `vi.clear()` : xóa toàn bộ phần tử trong vector `vi.clear();` : `vi.size() = 0`
- Ứng dụng :
    ```cpp
    int mark[1000001] = {0};
    //In các phần tử khác nhau theo thứ tự xuất hiện
    //Code trâu
    int n; cin >> n;
    int a[n];
    for(int &x : a) cin >> x;
    for(int i = 0; i < n; i++){
        bool check = true;
        for(int j = 0; j < i; j++){
            if(a[i] == a[j]){
                check = false;
                break;
            }
        }
        if(check) cout << a[i] << ' ';
    }
    cout << endl;

    //Đếm phân phối -> nhanh
    for(int i = 0; i < n; i++){
        if(mark[a[i]] == 0){
            cout << a[i] << " ";
            mark[a[i]] = 1;
        }
    }
    cout << endl;

    //Trộn 2 mảng đã sx vào 1 mảng + tìm giao và hợp giữa 2 mảng
    int m, n; cin >> m >> n;
    int c[m], d[n];
    vector<int> v;
    for(int &x : c) cin >> x;
    for(int &x : d) cin >> x;
    int i = 0, j = 0;
    vector<int> giao, hop;
    while(i < m && j < n){
        if(c[i] == d[j]){
            v.push_back(c[i]);
            hop.push_back(c[i]);
            giao.push_back(c[i]);
            i++; j++;
        }
        else if(c[i] > d[j]){
            v.push_back(d[j]);
            hop.push_back(d[j]);
            j++;
        }
        else{
            v.push_back(c[i]);
            hop.push_back(c[i]);
            i++;
        }
    }
    while(i < m) v.push_back(c[i++]); hop.push_back(c[i++]);
    while(j < n) v.push_back(d[j++]); hop.push_back(d[j++]);

    for(int i = 0; i < v.size(); i++){
        cout << v[i] << ' ';
    }
    for(int i = 0; i < hop.size(); i++){
        cout << hop[i] << ' ';
    }
    for(int i = 0; i < giao.size(); i++){
        cout << giao[i] << ' ';
    }
    ```
## Set
### set
- Khai báo : set<kiểu_dữ_liêu> tên_biến  `set<int> se`
- Các hàm :
    - `se.insert(giá trị)` : thêm phần tử vào set  `se.insert(3)`
    - `se.size()` : số lượng phần tử trong set
    - `se.find(x)`: tìm phần tử trong set trả về iterator trỏ tới phần tử trong set
        - `auto it = se.find(3);`
        - `vector<int>::iterator it = se.find(3);`
    - `se.count(x)` : đếm số phần tử xuất hiện trong set cho ra giá trị là 0 hoặc 1
    - `se.erase(x)` : xóa phần tử trong set
### multiset
- Khai báo : multiset<kiểu_dữ_liệu> tên_biến `multiset<long long> se;`
- Các hàm giống set trừ hàm find :
    - `se.find(x)` : trả về iterator của phần tử đầu tiên nếu các phần tử trùng nhau
### unordered_set
- Khai báo : unordered_set<kiểu_dữ_liệu> tên_biến `unordered_set<string> se;` 
- Các hàm giống set
## Map
### map
- Khai báo : map<kiểu dữ liệu, kiểu dữ liệu> tên_biến `map<int, int> mp;`
- Truy xuất phần tử : mp[key]  `mp[5];`
- Duyệt map :
    - **for each** : `for(pair<KDL, KDL> x : mp){ cout << x.first << x.second)` 
    - **iterator** : tương tự như set, vector NHƯNG chú ý (*it).first
    - Có thể thay `(*it).first = it → first`
- Các hàm :
    - `mp.insert({x, y})` : thêm 1 cặp phần tử `mp.insert({1, 5}) == map[1] = 5;`
    - `mp[key] = value` : có thể thay đổi giá trị của key đã cho trước
    - `mp[key]++` : tăng giá trị của value có trước lên 1 còn nếu chưa có key với value trong map thì tự động thêm vào và cho `mp[key] = 1`
    - `mp.size()` : số lượng cặp pt
    - `mp.find(x)` : tìm key có nằm trong map hay không và trả về iterator như vector hoặc set
    - `mp.count(x)` : đếm xem key trong map xuất hiện bao nhiêu lần
    - `mp.erase(x)` : xóa cả key và value và xóa thông qua key
### multimap
- Giống map
- **KO HỖ TRỢ mp[key] = value**  không truy cập và gán
### unordered_map 
- Giống map
- Ứng dụng hash_table : Truy xuất O(1)
### Ứng dụng map 
``` cpp
//Đếm tần suất xuất hiện của các ptu trong mảng
//Đếm theo thứ tự tăng dần
map<ll, int> mp;
int n; cin >> n;
ll a[n];
for(int i = 0; i < n; i++){
    cin >> a[i];
    mp[a[i]]++;
}
//Duyệt bình thường bằng iterator
for(auto it = mp.begin(); it != mp.end(); it++){
    cout << (*it).first << " " << (*it).second << endl;
}
//Duyệt bằng for each
// bản thân it là 1 pair trong map rồi chứ ko phải iterator nữa nên ko cần (*it) -> trỏ đến 1 pair trong map nữa rồi mới (*it).first
for(auto it : mp){
    cout << it.first << ' ' << it.second << endl;
}

//Đếm theo thứ tự xuất hiện của mảng
map<ll, int> mp;
int n; cin >> n;
ll a[n];
for(int i = 0; i < n; i++){
    cin >> a[i];
    mp[a[i]]++;
}
for(int i = 0; i < n; i++){
    if(mp[a[i]] != 0){
        cout << a[i] << ' ' << mp[a[i]] << endl;
        mp[a[i]] = 0;
    }
}
```
## List
- Khai báo : list<kiểu_dữ_liệu> tên_biến;
    ```cpp
    list<int> lst;              
    list<int> lst({1, 2, 3});   // Initialize with items {1, 2, 3}
    list<int> lst = {1, 2, 3};  // Initialize with items {1, 2, 3}
    ```
- Các hàm
    - `lst.empty()` : Checks if list is empty
    - `lst.size()` : Returns size of list
    - `lst.front()` : Get head item
    - `lst.back()` : Get tail item
    - `lst.clear()` : Clear all contents of list
    - `lst.push_back(item)` : Appends item to the rear
    - `lst.push_front(item)` : Prepends item at head
    - `lst.insert(it, item)` : Inserts item before iterator position
    - `lst.pop_back()` : Removes last item
    - `lst.reverse()` : Reverses the entire list
    - `lst.remove(v)` : Removes all elements equal to v
    - `lst.remove_if([](int x){ return x > 10; })` : Removes all elements greater than 10
    - [Splice](http://www.cplusplus.com/reference/list/list/splice/)
    ```cpp
    list<int> lst1 = {1, 2, 3};
    list<int> lst2 = {4, 5, 6};
    lst1.splice(lst1.end(), lst2); // O(1)
    list<int> lst3 = {1, 2, 3, 4, 5, 6};
    //lst1 == lst3
    ```
## Stack
- Khai báo : stack<kiểu_dữ_liệu> tên_biến; `stack<int> st`;
- Các hàm 
    - `st.empty()` : Checks if stack is empty
    - `st.size()` : Returns current size on stack
    - `st.top()` : Returns the topmost element
    - `st.push(item)` : Push item to top of stack
    - `st.pop(item)` : Pop item off top of stack
    - `st1.swap(st2)` : Swap two values of two stacks
## Queue 
- Khai báo : queue<kiểu_dữ_liệu> tên_biến; `queue<int> q;`
- Các hàm 
    - `q.empty()` : Checks if queue is empty
    - `q.size()` : Returns current size on queue
    - `q.front()` : Returns front item of queue
    - `q.back()` : Returns rear item of queue
    - `q.push(item)` : Enqueue item to rear of queue
    - `q.pop(item)` : Dequeue item from front of queue
    - `print_queue(q)`

## Deque
- Khai báo : deque<kiểu_dữ_liệu> tên_biến  `deque<int> deq;`
- Các hàm
    - `deq.empty()` : Checks if deque is empty
    - `deq.size()` : Returns current size on deque
    - `deq.front()` : Returns front item of deque
    - `deq.back()` : Returns rear item of deque
    - `deq.at(i)` : Returns the item at position i in O(1)
    - `deq.push_front(item)` : Push item to front of deque
    - `deq.pop_front(item)` : Pop item from front of deque
    - `deq.push_back(item)` : Inject item at rear of deque
    - `deq.pop_back(item)` : Eject item at rear of deque
## Priority_queue
- Khai báo 
    ```cpp
    // Creates a max-heap
    priority_queue<int> pq;                             

    // Creates a min-heap
    priority_queue<int, vector<int>, greater<int>> pq;  

    // Initialize max-heap with {1,2,3} | O(N)
    priority_queue<int> pq(less<int>(), {1,2,3});       

    // Initialize max-heap with vector | O(N)
    vector<int> vect = {1,2,3};
    priority_queue<int> pq(less<int>(), vect);          

    // comparator
    bool cmp(int lhs, int rhs) {
        return lhs < rhs; // This entails a max heap
    }
    priority_queue<int, vector<int>, cmp> pq;
    ```
- Các hàm
    - `pq.empty()`
    - `pq.size()`
    - `pq.top()`
    - `pq.push(item)` : Add
    - `pq.pop()` : Remove
    - `print_queue(pq)`

## String
- Khai báo : string tên_biến `string s = ”Noi dung”` (trong dấu ngoặc kép lưu 1 chuỗi kí tự)
- Nhập :
    - `cin >> s` : xâu không có dấu cách `s = "Python";`
    - `getline(cin, s, <có thể thêm kí tự phân cách khác>)` : xâu có dấu cách
    - `cin.ignore()` : xóa dấu cách của cin trước khi dùng getline
- Các hàm : 
    - `s.size()` hoặc `s.length()` : trả số lượng kí tự trong xâu s
    - `s.front() == s[0]` : kí tự đầu tiên
    - `s.back() == s[s.size() - 1]` → kí tự cuối cùng
    - `s.compare(t)` nếu s == t thì 0; nếu s > t thì 1; nếu s < t thì -1;
    - `+` : concate nối 2 xâu lại và cũng có thể nối xâu với kí tự
    - `isdigit(char c)` : kiểm tra chữ số
    - `islower(char c)` : kiểm tra chữ in thường
    - `isupper(char c)` : kiểm tra in hoa
    - `isalpha(char c)` : kiểm tra chữ cái
    - `int tolower(char c)` : chuyển thành chữ in thường mã ASCII ép kiểu char
    - `int toupper(char c)` : chuyển thành chữ in hoa mã ASCII ép kiểu char ta thường dùng `s[i] = toupper(s[i])` => gán lại cho s[i]
    - `stoi (string to integer)` : đổi string sang số int `int t = stoi(s);`
    - `stoll (string to ll)` : chuyển string sang long long `ll a = stoll(s);`
    - `to_string()` : đổi số sang string `string s = to_string(t);`
    - `stringstream ss(s)` : dùng để tách các xâu có nhiều dấu cách hoặc xâu con có các kí tự phân biệt
        ```cpp
        string s = "hoc lap trinh   Python";
        string tmp;
        stringstream ss(s);
        while(ss >> tmp){
            //code
        }
- Ứng dụng :
    ```cpp
    //Chuyển đổi chuỗi sang full in hoa
    void toUpper(string &s){
        for(int i = 0; i < n; i++){
            s[i] = toupper(s[i]);
        }
    }

    //Chuyển đổi chuỗi sang full in thường
    void toLower(string &s){
        for(int i = 0; i < n; i++){
            s[i] = tolower(s[i]);
        }
    }

    //Đối vs các số lớn có nhiều chữ số (>= 10^6 chữ số) lưu vào string
    void solve(string s){
        cin >> s;
        int sum = 0;
        for(char x : s){
            sum += (int) x; // sai vì nó sẽ cộng mã ASCII của x vào sum chứ ko phải số cần tính
            sum += x - '0'; //Lấy mã ASCII của char x trừ đi mã ASCII 0 sẽ ra số tách ra từ xâu
        }
        cout << sum;
    }

    //Tách chuỗi nhỏ trong xâu -> áp dụng tách số vẫn được nhưng phải convert qua
    string s; getline(cin, s);
    stringstream ss(s); //khởi tạo stringstream tách chuỗi trong xâu
    string word;
    vector<string> vi;
    //while(getline(ss, word, ts3)) ts3 : là kí hiệu sẽ ngắt luồng cin
    while(ss >> word){ // luồn nhập liên tục cứ tới dấu cách là ngưng
        vi.push_back(word);
    }
    //có thể làm bất cứ thứ gì với vector<string>
    sort(vi.begin(), vi.end());  
    ```

# String
## Xử lý xâu căn bản

### Tổng chữ số của số nguyên lớn

```cpp
ll sumOfNum(string s){
    ll sum = 0;
    for(int i = 0; i < s.size(); i++){
        sum += s[i] - '0';
    }
    return sum;
}
```

### Kiểm tra xâu con t trong xâu s

```cpp
bool checkSubString(string s, string t){
    if(s.find(t) != string::npos) return true;
    else return false;
}
```

### In hoa

```cpp
string Upper(string s){
	for(int i = 0; i < s.size(); i++){
		s[i] = toupper(s[i]);
	}
	return s;
}
// Cách khác
void upper(string &s){
    for(int i = 0; i < s.size(); i++){
        s[i] = toupper(s[i]);
    }
}
```

### In thường

```cpp
string Lower(string s){
	for(int i = 0; i < s.size(); i++){
		s[i] = tolower(s[i]);
	}
	return s;
}
// Cách khác
void lower(string &s){
    for(int i = 0; i < s.size(); i++){
        s[i] = tolower(s[i]);
    }
}
```

### Kiểm tra in hoa và in thường

```cpp
void check(char c){
	if(c >= 65 && c <= 90){
		cout << "UPCASE : ";
		cout << (char)(c + 32) << endl;
    } 
	else if(97 <= c <= 122){
		cout << "DOWNCASE : ";
		cout << (char)(c - 32);
	} 
}
```

### Chuẩn hóa ngày tháng năm sinh

- Theo form dd/mm/yyyy => ứng dụng để viết comparator so sánh tuổi theo thứ tự từ điển

```cpp
void stringMod(string &s){
    if(s[2] != '/') s = "0" + s;
    if(s[5] != '/') s.insert(3, "0");
}
```

## Phân loại kí tự

```cpp
void solve1(){
    string s;
    getline(cin, s);
    int num = 0, character = 0, special = 0;
    for(int i = 0; i < s.size(); i++){
        if(isdigit(s[i])) num++;
        else if(isalpha(s[i])) character++;
        else special++;
    }
    cout << character << " " << num << " " << special << endl;
}
```

### Đếm tần xuất xuất hiện của các từ trong xâu

```cpp
void solve3(){
    string s;
    getline(cin, s);
    //cin.ignore();
    //C1
    int cnt[256] = {0};
    for(int i = 0; i < s.size(); i++){
        cnt[s[i]]++;
    }
    //chỉ duyệt 0 - 255 hoặc bé hơn 256 nếu dùng <= 256 sẽ sai ngay
    for(int i = 0; i < 256; i++){
        if(cnt[i] != 0){
            cout << (char)i << " " << cnt[i] << endl;
        }
    }
    cout << endl;
    for(int i = 0; i < s.size(); i++){
        if(cnt[s[i]] != 0){
            cout << s[i] << " " << cnt[s[i]] << endl;
            cnt[s[i]] = 0;
        }  
    }
    cout << endl;
    //C2
    map<char, int> mp;
    for(auto x : s) mp[x]++;
    for(auto it : mp) cout << it.first << ' ' << it.second << endl;
    cout << endl;
    for(auto x : s){
        if(mp[x] != 0){
            cout << x << " " << mp[x] << endl;
            mp[x] = 0;
        }
    }
    cout << endl;
}
```

### Tần suất xuất hiện nhiều nhất, ít nhất

- Kí tự có số lần xuất hiện nhiều nhất và thứ tự xuất hiện ít nhất nếu trùng tần xuất thì in theo thứ tự từ điển lớn nhất

```cpp
bool cmp(pair<char, int> a, pair<char, int> b){
    if(a.second != b.second) return a.second < b.second;
    else return a.first < b.first; 
}
void solve4(){
    string s;
    cin >> s;

    map<char, int> mp;
    for(auto x : s) mp[x]++;

    vector<pair<char, int>> v;
    for(auto it : mp) v.push_back(it);
    sort(v.begin(), v.end(), cmp);
  
    for(auto x : v) cout << x.first << " " << x.second << endl;
    cout << endl;

    cout << v[v.size() - 1].first << " " << v[v.size() - 1].second << endl;
    char res = v[0].first; 
    int cnt = v[0].second;
    for(int i = 1; i < s.size(); i++){
        if(cnt == v[i].second) res = v[i].first;
    } 
    cout << res << " " << cnt << endl;
}
```

### Kí tự xuất hiện ở cả 2 xâu và xuất hiện 1 trong 2 xâu

```cpp
void solve5(){
    string s; cin >> s;
    string t; cin >> t;
    set<char> se1, se2, se3;
    for(int i = 0; i < s.size(); i++){
        se1.insert(s[i]);
        se2.insert(s[i]);
    }
    for(int i = 0; i < t.size(); i++){
        if(se1.count(t[i]) != 0) se3.insert(t[i]);
        else se2.insert(t[i]);
    }
    for(auto x : se3) cout << x;
    cout << endl;
    for(auto x : se2) cout << x;
    cout << endl;
}
```

### Kí tự chỉ xuất hiện trong xâu 1 mà ko có trong xâu 2 và ngược lại

```cpp
void solve6(){
    string s1; cin >> s1;
    string s2; cin >> s2;
    set<char> se1, se2;
    for(auto x : s1) se1.insert(x);
    for(auto x : s2) se2.insert(x);
    for(auto x : se1){
        if(!se2.count(x)) cout << x;
    }
    cout << endl;
    for(auto x : se2){
        if(!se1.count(x)) cout << x;
    }
    cout << endl;
}
```

### Xâu đối xứng

```cpp
void solve7(){
    string s; cin >> s;
    string t = "";
    for(int i = s.size() - 1; i >= 0; i--){
        t += s[i];
    }
    if(t == s) cout << "YES\n";
    else cout << "NO\n";
}
```

### Kiểm tra kí tự

```cpp
void solve8(){
    char c; cin >> c;

   //in ra chữ cái liền sau
    if(c == 'z') cout << 'a' << endl;
    else cout << (char)(c + 1) << endl;

   //in ra chữ liền sau nhưng luôn luôn là chữ thường
    if(c == 'z' || c == 'Z') cout << 'a' << endl;
    else if(c >= 'A' && c <= 'Z') cout << (char)(c + 33) << endl;
    else cout << (char)(c + 1);
}
```

### Kiểm tra xâu có đầy đủ 26 kí tự (Xâu Pangram)

```cpp
void solve9(){
    string s; cin >> s;
    set<char> se;

    for(int i = 0; i < s.size(); i++) s[i] = tolower(s[i]); 
    for(auto x : s) se.insert(x);

    if(se.size() == 26) cout << "YES\n";
    else cout << "NO\n";
}
```

### Đếm số lượng từ

```cpp
void solve10(){
    string s; 
    getline(cin, s); 
    cin.ignore();
    stringstream ss(s);
    string w;
    int cnt = 0;
    while(ss >> w){
        cnt++;
    }
    cout << cnt << endl;
}
```

### Liệt kê các từ khác nhau trong xâu

```cpp
void solve11(){
    string s; 
    getline(cin, s);
    set<string> se;
    vector<string> v;
    map<string, int> mp;
    stringstream ss(s);
    string w;
    while(ss >> w){
        //C1
        if(!se.count(w)) v.push_back(w);
        se.insert(w);
        //C2
        mp[w]++;
        v.push_back(w);
        se.insert(w);
    }
    //C1
    for(auto it : se) cout << it << " "; cout << endl;
    for(auto x : v) cout << x << ' ';

    //C2
    for(auto x : se) cout << x << " "; cout << endl;
    for(auto x : v){
        if(mp[x] != 0){
            cout << x << " ";
            mp[x] = 0;
        }
    }
}
```

### Sắp xếp xâu theo thứ tự từ điển và độ dài xâu

```cpp
void solve12(){
    string s; 
    getline(cin, s);
    vector<string> v;
    stringstream ss(s);
    string w;
    while(ss >> w) v.push_back(w);

    sort(v.begin(), v.end());
    for(auto x : v) cout << x << " "; 
    cout << endl;
    sort(v.begin(), v.end(), [](string a, string b)-> bool {
        if(a.size() != b.size()) return a.size() < b.size();
        else return a < b;
    });
    for(auto x : v) cout << x << ' ';
}
```

### Sắp xếp xâu đối xứng khác nhau theo độ dài và theo thứ tự xuất hiện

```cpp
bool palindrome(string s){
    string t = s;
    reverse(t.begin(), t.end());
    return s == t;
}
void solve13(){
    string s; 
    getline(cin, s);
    vector<string> v;
    set<string> se;
    stringstream ss(s);
    string w;
    while(ss >> w){
        if(!se.count(w) && palindrome(w)){
            v.push_back(w);
            se.insert(w);
        }
    }
    stable_sort(v.begin(), v.end(), [](string a, string b)-> bool {
        return a.size() < b.size();
    });
    for(auto x : v) cout << x << ' ';
}
```

### Tần xuất các từ trong xâu

```cpp
void solve14(){
    string s; 
    getline(cin, s);
    map<string, int> mp;
    vector<string> v;
    stringstream ss(s);
    string w;
    while(ss >> w){
        mp[w]++;
        v.push_back(w);
    }
    for(auto x : mp) cout << x.first << " " << x.second << endl;
    cout << endl;
    for(auto x : v){
        if(mp[x] != 0){
            cout << x << ' ' << mp[x] << endl;
            mp[x] = 0;
        }
    }
}
```

### Từ xuất hiện nhiều nhất, ít nhất

```cpp
void solve15(){
    string s; 
    getline(cin, s);
    map<string, int> mp;
    vector<pair<string, int>> v;
    stringstream ss(s);
    string w;
    while(ss >> w) mp[w]++;
    for(auto x : mp) v.push_back(x);
    sort(v.begin(), v.end(), [](pair<string, int> a, pair<string, int> b)-> bool {
        if(a.second != b.second) return a.second < b.second;
        else return a.first < b.first;
    });
  
    cout << v[v.size() - 1].first << " " << v[v.size() - 1].second << endl;
    string res = v[0].first;
    int cnt = v[0].second;
    for(int i = 1; i < v.size(); i++){
        if(v[i].second == cnt) res = v[i].first;
    }
    cout << res << " " << cnt << endl;
}
```

# Linked list
- Xây dựng các cấu trúc Node (data, reference) ⇒ data : bất cứ dữ liệu gì, reference : lưu địa chỉ của node tiếp theo, node cuối tham chiếu trỏ vào NULL
- Quản lý dslk = node đầu tiên : head
- Cấu trúc Node tự trỏ → tham chiếu trỏ đến địa chỉ của phần tử giống hệt nó đều là Node ⇒ mang tính đệ quy
- Gắn 1 node vào dslk ⇒ cấp phát động ⇒ khai báo con trỏ kiểu Node
## Danh sách liên kết đơn
```cpp
//Singly Linked list (Danh sách liên kết đơn)
struct Node{
    //data -> có thể bất cứ KDL gì
    int data;
    //reference -> là 1 con trỏ kiểu Node => lưu địa chỉ của Node tiếp theo nên bản thân nó phải là con trỏ kiểu Node
    Node* next; //Cấu trúc tự trỏ
};
// mỗi node trong dslk đều là cấp phát động (con trỏ kiểu node) => quản lý thông qua địa chỉ => lưu địa chỉ thằng tiếp theo
typedef Node* node;  

//Tạo 1 node mới từ thông tin đã có
node makeNode(int newData){
    //Cấp phát động 1 node 
    node tmp = new Node(); // Node* tmp = new Node();
    tmp->data = newData; //gán data
    tmp->next = NULL; //trỏ địa chỉ vào NULL -> có thể thay đổi sau khi đưa vào linked list
    return tmp;
}

//Tính số lượng pt trong dslk
int sz(node head){
    int cnt = 0;
    while(head != NULL){ //địa chỉ của node head chưa tới node cuối
        cnt++;
        head = head->next; //head->next : địa chỉ của node tiếp theo => node head sẽ nhảy sang node tiếp theo để quản lý
    }
    return cnt;
}

//In dslk
void print(node head){
    while(head != NULL){
        cout << head->data << ' '; // In giá trị data
        head = head->next; //nhảy sang node tiếp theo
    }
}

//Thêm đầu dslk
void pushFront(node &head, int newData){ //Sau khi kết thúc hàm muốn nó làm đổi dslk thì phải tham chiếu 
    node tmp = makeNode(newData);
    //Dslk rỗng hoặc dslk đã có phần tử => kt trước khi thêm
    if(head == NULL){ //DSLK rỗng
        head = tmp; // Vì head cũng là con trỏ kiểu Node nên chỉ việc gán cho nhau là đc
        return;
    }
    else{ //DSLK có nhiều pt
        //Cập nhật phần tham chiếu trỏ đến node head đầu tiên trước rồi cập nhật head = tmp
        tmp->next = head;
        head = tmp; // giá trị mới đc thêm vào đầu tiên dslk
    }
}

//Thêm cuối dslk
void pushBack(node &head, int newData){
    node tmp = makeNode(newData);
    if(head == NULL){
        head = tmp;
        return;
    }
    else{
        //Tìm đc node cuối cùng -> cho head->next = tmp
        node find = head;
        //lặp đến code cuối cùng
        while(find->next != NULL){
            find = find->next; //Gán địa chỉ cho node tiếp theo
        }
        find->next = tmp; //Cập nhật node cuối = tmp
    }
}

//Thêm giữa dslk
void insert(node &head, int newData, int pos){
    int n = size(head); //đếm số lượng pt dslk;
    if(pos < 1 || pos > n + 1) return; // Chèn các vị trí hợp lệ nằm trong dslk và vị trí bắt đầu từ 1 -> n
    if(pos == 1) pushFront(head, newData); // vị trí đầu
    else if(pos == n + 1) pushBack(head, newData); // vị trí sau vị trí n (cuối cùng)
    else{ //chèn vào giữa
        node find = head;
        for(int i = 1; i <= pos - 2; i++){ //ra đc vị trí của node trước node cần chèn
            find = find->next;
        }
        node tmp = makeNode(newData);//Tham chiếu cái node tmp này vào node cần chèn trc để giữ được cái mạch dslk
        tmp->next = find->next; // gán địa chỉ của node chỗ cần chèn vào trước
        find->next = tmp;//cập nhật node tmp vào vị trí cần chèn
    }
}

//Xóa đầu dslk
void popFront(node &head){
    if(head == NULL) return; // dslk rỗng
    node tmp = head; // tạo 1 node tmp = node head
    head = head->next; // nhảy sang node tiếp theo
    delete tmp; // xóa thẳng node tmp đang chứa node head ban đầu
}

//Xóa cuối dslk
void popBack(node &head){
    if(head == NULL) return; // dslk rỗng
    node p = head, q = NULL; // dùng kĩ thuật 2 con trỏ chạy trước và chạy sau
    while(p->next != NULL){
        q = p; // q sẽ là node kế cuối
        p = p->next; // khi p là node cuối
    }
    delete p; // xóa p là xóa node cuối
    if(q == NULL) head = NULL; //dslk chỉ có 1 pt
    else q->next = NULL; // dslk có 2 pt trở lên node q trở thành node cuối
}
//Sử dụng ý tưởng thuần túy ko dùng kĩ thuật 2 con trỏ
void popBack2(node &head){
    if(head == NULL) return;
    node tmp = head;
    if(tmp->next == NULL){
        head = NULL;
        delete tmp;
        return;
    }
    while(tmp->next->next != NULL) tmp = tmp->next;
    node last = tmp->next;
    tmp->next = NULL;
    delete last;
}

//Xóa giữa dslk
void erase(node &head, int pos){
    int n = size(head); // đếm số lượng pt trong dslk
    if(pos < 1 || pos > n) return; // vị trí ko hợp lệ
    // kĩ thuật 2 con trỏ
    node p = head, q = NULL;
    for(int i = 1; i < pos; i++){ //con trỏ p đến vị trí pos cần xóa và con trỏ q sẽ đứng trc p
        q = p;
        p = p->next;
    }
    if(q != NULL) q->next = p->next; // con trỏ q trỏ vào con trỏ đừng sau p
    else head = head->next; // xóa pt đầu tiên của dslk
    delete p;
}
//Ý tưởng thuần túy ko dùng 2 con trỏ
void erase2(node &head, int pos){
    int n = size(head);
    if(pos < 1 || pos > n + 1) return;
    if(pos == 1) popFront(head);
    else{
        node find = head;
        for(int i = 1; i <= pos - 2; i++){
            find = find->next; //đến vị trí k - 1 => node thứ k - 1
        }
        node tmp = find->next; // node thứ k 
        //Kết nối node thứ k - 1 đến node thứ k + 1
        find->next = tmp->next; 
        delete tmp; 
    }
}

//Đảo dslk
node reverse(node &head){
    node prev = NULL;
    while(head != NULL){
        node tmp = head->next;
        head->next = prev;
        prev = head;
        head = tmp;
    }
    return prev;
}

//Tìm kiếm 1 pt trong dslk
bool search(node head, int val){
    while(head != NULL){
        if(head->data == val) return true;
        head = head->next;
    }
    return false;
}

//Sắp xếp trong dslk -> selection sort
void sort(node &head){
    for(node i = head; i != NULL; i = i->next){
        node min = i;
        for(node j = i->next; j != NULL; j = j->next){
            if(min->data > j->data){
                min = j;
            }
        }
        swap(i->data, min->data);
        // int tmp = min->data;
        // min->data = i->data;
        // i->data = tmp;
    }
}

//Thêm các pt vào dslk đến khi dừng
void input(node &head){
    head = NULL;
    while(1){
        int x; cin >> x;
        if(x == -1) break; // hoặc có thể là điều kiện x là khác để dừng
        else pushBack(head, x);
    }
}
```
## Danh sách liên kết đôi
```cpp
//Doubly linked list (Danh sách liên kết đôi)
struct Node{
    int data;
    Node* next;
    Node* prev;
};
typedef Node* node;

node makeNode(int x){
    node tmp = new node;
    tmp->data = x;
    tmp->next = tmp->prev = NULL;
    return tmp;
}

void print(node head){
    while(head != NULL){
        cout << head->data << ' ';
        head = head->next;
    }
}

int sz(node head){
    int cnt = 0;
    while(head != NULL){
        cnt++;
        head = head->next;
    }
    return cnt;
}

void pushFront(node &head, int x){
    node tmp = makeNode(x);
    tmp->next = head;
    if(head != NULL){
        //Tránh bị lỗi truy cập vào dslk rỗng
        head->prev = tmp;
    }
    head = tmp;
}

void pushBack(node &head, int x){
    node tmp = makeNode(x);
    node find = head;
    //Tránh trường hợp dslk rỗng => lỗi truy cập
    if(head == NULL){
        head = tmp;
        return;
    }
    while(find->next != NULL){
        find = find->next;
    }
    find->next = tmp;
    tmp->prev = find;
}

void insert(node &head, int x, int k){
    int n = size(head);
    if(k < n || k > n + 1) return;
    if(k == 1){
        pushFront(head, x);
        return;
    }
    else{
        node find = head;
        for(int i = 1; i <= k - 1; i++){
            find = find->next;
        }
        node tmp = makeNode(x);
        tmp->next = find;
        find->prev->next = tmp;
        tmp->prev = find->prev;
        find->prev = tmp;
    }
}

void popFront(node &head){
    if(head == NULL) return; // DSLK rong
    node deleteNode = head;
    //Cho node head thanh node thu 2 trong DSLK
    head = head->next; 
    //Neu head != NULL thi tro ngược lại vào NULL
    if(head != NULL){
        head->prev = NULL;
    }
    //Giai phong vung nho
    delete deleteNode;
}

void popBack(node &head){
    if(head == NULL) return; // DSLK rong
    if(head->next == NULL){
        delete head;
        head = NULL;
    }
    else{
        node tmp = head;
        //Duyệt đến node thứ 2 từ cuối về : tmp
        while(tmp->next->next != NULL){
            tmp = tmp->next;
        }
        //Lưu lại node cuối để giải phóng
        node delNode = tmp->next;
        //Node tmp = NULL
        tmp->next = NULL;
        //Giải phóng node cuối
        delete delNode;
    }
}

void erase(node &head, int k){
    if(k < 1 || k > len(head)) return; // vi tri xoa ko hop le
    if(k == 1){
        popFront(head);
    }
    else{
        node tmp = head;
        //Duyet den node k - 1
        for(int i = 1; i <= k - 1; i++){
            tmp = tmp->next;
        }
        //Luu lai node thu k
        node delNode = tmp;
        //Cho k - 1 => tro vao k + 1
        tmp->prev->next = tmp->next;
        //Cho k + 1 => tro nguoc k - 1
        if(tmp->next != NULL)
            tmp->next->prev = tmp->prev;
        //Giai phong node thu k
        delete delNode;
    }
}
```


# Hash table
## Cài đặt Seperate Chaining
```cpp
const int cap = 1e6 + 3;
vector<pair<int, int>> bucket[cap]; // mảng 2 chiều pair
struct hashTable{
    int function(int key){
        return key % cap;
    }
    void insert(int key, int val){
        int k = function(key);
        bucket[k].push_back({key, val});
    }
    bool find(int key, int val){
        int k = function(key);
        for(auto x : bucket[k]){
            if(x.first == key && x.second == val) return true;
        }
        return false;
    }
    void get(int key){
        int k = function(key);
        for(auto x : bucket[k]){
            if(x.first == key) cout << x.second << ' ';
        }
        cout << endl;
    }
    void remove(int key, int val){
        int k = function(key);
        for(int i = 0; i < bucket[k].size(); i++){
            if(bucket[k][i].first == key && bucket[k][i].second == val){
                bucket[k].erase(bucket[k].begin() + i);
                return;
            }
        }
    }
    void removeAll(int key){
        int k = function(key);
        bucket[k].clear();
        return;
    }
    void print(int n){
        for(int i = 0; i < n; i++){
            cout << i << " : " << '[' << bucket[i][0].first << " : ";
            for(int j = 0; j < bucket[i].size(); j++){
                if(j == bucket[i].size() - 1) cout << bucket[i][j].second;
                else cout << bucket[i][j].second << ", ";
            }
            cout << ']' << endl;
        }
    }
};

int main(){
    //thử nghiệm
    int a[10] = { 12, 3, 23, 4, 11, 32, 26, 33, 17, 19 };
    hashTable h;
    for(int i = 0; i < 10; i++){
        h.insert(i, a[i]);
    }
    h.insert(2, 89);
    h.insert(4, 78);
    h.removeAll(4);
    h.get(2);
    cout << h.find(7, 17) << endl;
    h.print(10);
}
```
## Cài đặt Open addressing
```cpp
const int cap = 1e6 + 3;
vector<pair<int, int>> bucket(cap, {-1, -1}); //mảng pair
int cnt;
struct hashTable{
    int function(int key){
        return key % cap;
    }
    void insert(int key, int val){
        int k = function(key);
        while(bucket[k].first != -1 && bucket[k].first == key){
            k = (k + 1) % cap;
        }
        bucket[k] = {key, val};
        cnt = max(cnt, k);
    }
    int find(int key, int val){
        int k = function(key);
        while(bucket[k].second != val){
            k = (k + 1) % cap;
        }
        return k;
    }
    void remove(int key, int val){
        int i = find(key, val);
        bucket[i] = {-1, -1};
    }
    void print(){
        for(int i = 0; i <= cnt; i++){
            if(bucket[i].first != -1 && bucket[i].second != -1){
                cout << i << " : "<< "[" << bucket[i].first << ", " << bucket[i].second << "]" << endl;
            }
        }
    }

};

int main(){
    //thử nghiệm
    int a[10] = { 12, 3, 23, 4, 11, 32, 26, 33, 17, 19 };
    hashTable h;
    for(int i = 0; i < 10; i++){
        h.insert(i, a[i]);
    }
    h.insert(2, 89);
    h.insert(4, 78);
    h.remove(4, 78);
    cout << h.find(2, 89) << endl;
    h.print();
    
}
```

# Stack
Hoạt động theo cơ chế LIFO
## Cài đặt bằng mảng
```cpp
int n = 0, st[100001];

void push(int x){
    st[n] = x;
    n++;
}

void pop(){
    if(n >= 1) n--;
    else return;
}

int top(){
    return st[n - 1];
}

int sz(){
    return n;
}
```
## Cài đặt bằng danh sách liên kết
```cpp
struct Node{
    int data;
    Node* next;
    Node(int x){
        this->data = x;
        this->next = NULL;
    }
};
typedef Node* node;

node makeNode(int x){
    node *tmp = new node;
    tmp->data = x;
    tmp->next = NULL;
    return tmp;
}

void push(node &top, int x){
    node tmp = new node;
    if(top == NULL){
        top = tmp;
        return;
    }
    tmp->next = top;
    top = tmp;
}

void pop(node &top){
    if(top == NULL) return;
    else{
        node tmp = top;
        top = tmp->next;
        delete tmp;
    }
}

int top(node top){
    if(top != NULL) return top->data; 
}

int sz(node top){
    int res = 0;
    while(top != NULL){
        res++;
        top = top->next;
    }
    return res;
}
```
## Ký pháp ba lan 
### Biểu thức trung tố sang tiền tố - Khó nhất
```cpp
int priority(char c){
    if(c == '^') return 3;
    else if(c == '*' || c == '/') return 2;
    else if(c == '+' || c == '-') return 1;
    return 0;
}
void convert(string s){
    //string lưu kq
    string res = "";
    //dùng stack để thực hiện liên quan những bài này
    stack<char> st; // lưu các toán tử và toán hạng
    //duyệt từ đầu đến cuối string
    for(int i = 0; i < s.length(); i++){
        if(isalpha(s[i])) res += s[i]; //là toán hạng push vào res
        else if(s[i] == '(') st.push(s[i]); // là dấu mở ngoặc push vào stack là mốc để thực hiện vòng lặp
        else if(s[i] == ')'){ 
            // bắt đầu lặp để thực hiện push vào cho đến khi gặp dấu mở ngoặc tương ứng
            while(!st.empty() && st.top() != '('){
                res += st.top(); // đẩy các toán tử trong stack vào res
                st.pop(); 
            }
            st.pop(); //xóa nốt dấu mở ngoặc
        }
        else{ // xử lí các toán tử
            while(!st.empty() && priority(s[i]) <= priority(st.top())){ // để thực hiện phép tính các phép tính có độ ưu tiên lớn còn nếu ngang nhau sẽ thực hiện từ trái sang phải
                res += st.top();
                st.pop();
            }
            st.push(s[i]); //đưa kí tự có độ ưu tiên thấp vào
        }
    }
    //thực hiện hết vòng lặp thì còn những toán tử nào trong stack thì pop hết ra
    while(!st.empty()){
        res += st.top();
        st.pop();
    }
    cout << res << endl;
} 
```
### Biểu thức tiền tố sang trung tố - Duyệt từ cuối về đầu
```cpp
void convert(string s){
    stack<string> st; //stack string để lưu kq
    for(int i = s.size() - 1; i >= 0; i--){
        if(isalpha(s[i])) st.push(string(1, s[i])); //hàm string(số kí tự, kí tự) -> biến đổi char thành string vs số lượng string là số kí tự truyền vào như copy lại
        else{
            string op1 = st.top();
            st.pop();
            string op2 = st.top();
            st.pop();
            string expression = '(' + op1 + s[i] + op2 + ')';
            //đẩy lại vào stack -> nguyên cụm (A+B)
            st.push(expression);
        }
    }
    cout << st.top() << endl;
} 
```
### Biểu thức tiền tố sang hậu tố - Duyệt từ cuối về đầu
```cpp
void convert(string s){
    stack<string> st; //stack string để lưu kq
    for(int i = s.size() - 1; i >= 0; i--){
        if(isalpha(s[i])) st.push(string(1, s[i])); //hàm string(số kí tự, kí tự) -> biến đổi char thành string vs số lượng string là số kí tự truyền vào như copy lại
        else{
            string op1 = st.top();
            st.pop();
            string op2 = st.top();
            st.pop();
            string expression = op1 + op2 + s[i];
            //đẩy lại vào stack -> nguyên cụm (A+B)
            st.push(expression);
        }
    }
    cout << st.top() << endl;
} 
```
### Biểu thức hậu tố sang tiền tố - Duyệt từ đầu tới cuối
```cpp
void convert(string s){
    stack<string> st; //stack string để lưu kq
    for(int i = 0; i < s.size(); i++){
        if(isalpha(s[i])) st.push(string(1, s[i])); //hàm string(số kí tự, kí tự) -> biến đổi char thành string vs số lượng string là số kí tự truyền vào như copy lại
        else{
            string op1 = st.top();
            st.pop();
            string op2 = st.top();
            st.pop();
            string expression = s[i] + op2 + op1;
            //đẩy lại vào stack -> nguyên cụm (A+B)
            st.push(expression);
        }
    }
    cout << st.top() << endl;
} 
```
## Kiểm tra ngoặc tổng quát
```cpp
bool isValid(string s){
    stack<char> st;
    for(char x : s){
        if(x == '(' || x == '{' || x == '[') st.push(x);
        else if(st.empty()) return false;
        else{
            if((x == ')' && st.top() != '(') || (x == '}' && st.top() != '{') || (x == ']' && st.top() != '[')) return false;
            st.pop();
        }
    }
    return st.empty();
}
```
## Tính toán các biểu thức
```cpp
int priority(char c){
    if(c == '*' || c == '/') return 2;
    else if(c == '+' || c == '-') return 1;
    return 0;
}

ll compute(ll a,ll b, char c){
    if(c == '+') return a + b;
    else if(c == '-') return a - b;
    else if(c == '*') return a * b;
    else return a/b;
}
void convert(string s){
    stack<char> st1;
    stack<ll> st2;
    for(int i = 0; i < s.size(); i++){
        if(s[i] == '('){
            st1.push(s[i]);
        }
        else if(isdigit(s[i])){
            ll tmp = 0;
            while(i < s.size() && isdigit(s[i])){
                tmp = tmp * 10 + s[i] - '0'; 
                ++i;
            }
            --i; //rất quan trong vì khi kết thúc vòng while thì i nó đã nhảy sang kí tự kế tiếp rồi phải lùi lại để tăng i lên lại sẽ đúng vị trí 
            st2.push(tmp);
        }
        else if(s[i] == ')'){
            while(!st1.empty() && st1.top() != '('){
                ll op1 = st2.top();
                st2.pop();
                ll op2 = st2.top();
                st2.pop();
                st2.push(compute(op2, op1, st1.top())); //tính toán từ toán hạng đứng trước là 2 rồi tới 1
                st1.pop();
            }
            st1.pop();
        }
        else{ //toán tử
            while(!st1.empty() && priority(s[i]) <= priority(st1.top())){
                ll op1 = st2.top();
                st2.pop();
                ll op2 = st2.top();
                st2.pop();
                st2.push(compute(op2, op1, st1.top())); 
                st1.pop();
            }
            st1.push(s[i]); //đẩy kí tự độ ưu tiên thấp vào stack
        }
    }
    //trong stack vẫn còn kí tự thì pop hết ra
    while(!st1.empty()){
        ll op1 = st2.top();
        st2.pop();
        ll op2 = st2.top();
        st2.pop();
        st2.push(compute(op2, op1, st1.top())); 
        st1.pop();
    }
    cout << st2.top() << endl;
}
void solve(){
    int q; cin >> q;
    while(q--){
        string s; cin >> s;
        convert(s);
    }
}
```

# Queue
Hoạt động theo cơ chế FIFO
## Cài đặt bằng mảng
```cpp
int a[100000], maxN = 100000;
int n = 0;

void push(int x){
    if(n == maxN) return;
    a[n] = x;
    n++;
}

void pop(){
    if(n == 0) return;
    for(int i = 0; i < n - 1; i++){
        a[i] = a[i + 1]; //dịch trái
    }
    n--;
}

int ssz(){
    return n;
}

bool empty(){
    return n == 0;
}

int front(){
    return a[0];
}
```
## Cài đặt bằng danh sách liên kết
```cpp
struct Node{
    int data;
    Node* next;
};
typedef Node* node;

node makeNode(int x){
    node *tmp = new node;
    tmp->data = x;
    tmp->next = NULL;
    return tmp;
}

void push(node &queue, int x){
    node tmp = makeNode(x);
    if(queue == NULL){
        queue = tmp;
        return;
    }
    node find = queue;
    while(find->next != NULL) find = find->next;
    find->next = tmp;
}

void pop(node &queue){
    if(queue = NULL) return;
    node tmp = node;
    queue = queue->next;
    delete tmp;
}

int sz(node queue){
    int res = 0;
    while(queue != NULL){
        res++;
        queue = queue->next;
    }
    return res;
}

bool empty(node queue){
    return queue == NULL;
}

int front(node queue){
    return queue->data;
}

void print(node queue){
    while(queue != NULL){
        cout << queue->data << ' ';
        queue = queue->next;
    }
}
```
# Priority queue 
Như `queue` nhưng lưu được dưới dạng max heap hoặc min heap => truy xuất phần tử bé nhất hoặc lớn nhất nhanh chóng
```cpp
// max-heap
priority_queue<int> Q;                             

// min-heap
priority_queue<int, vector<int>, greater<int>> Q;
```

# Graph
## Lưu trữ
3 loại : ma trận kề, danh sách cạnh, danh sách kề
```cpp
const int maxN = 10000;
//Số đỉnh và cạnh => duyệt số cạnh
int n, m;

//Cấu trúc dữ liệu lưu danh sách kề (Adjacency list) => mảng vector
vector<int> adj[maxN];
//Nhập danh sách cạnh sang danh sách kề
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        //Vô hướng
        adj[x].push_back(y);
        adj[y].push_back(x);
        //Có hướng
        adj[x].push_back(y);
        //Nhập vào ds cạnh
        edge.push_back({x, y});
    }
    //có thể sắp xếp sẵn sau khi nhập xong => để duyệt ds kề theo từ bé đến lớn
    for(int i = 0; i < m; i++){
        sort(adj[i].begin(), adj[i].end());
    }
    //reset mảng visited
    memset(visited, false, sizeof(visited));
}
//Xuất danh sách kề
void output(){
    //In theo thứ tự tăng dần của mỗi đỉnh
    for(int i = 1; i <= n; i++){
        //sắp xếp mỗi phần tử của mảng vector tăng dần
        sort(adj[i].begin(), adj[i].end());
        cout << i << ' ' <<  ':' << ' ';
        for(int j = 0; j < adj[i].size(); j++){
            cout << adj[i][j] << ' ';
        }
        cout << endl;
    }
    cout << endl;
}

//cấu trúc dữ liệu lưu ma trận kề 
int a[maxN + 1][maxN + 1];
//Nhập danh sách cạnh đến ma trận kề
void input(){
	cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        //vô hướng
        a[x][y] = 1;
        a[y][x] = 1;
        //có hướng 
        a[x][y] = 1;
    }
}
//Xuất ma trận kề
void output(){
    for(int i = 1; i <= n; i++){
        for(int j = 1; j <= n; j++){
            cout << a[i][j] << ' ';
        }
        cout << endl;
    }
    cout << endl;
}
```
## Disjoint set union - DSU
### Khởi tạo
```cpp
int n; //số đỉnh đồ thị
const int maxN = 1e4;
int parent[maxN + 1]; //truy vết thằng đại diện của mỗi đỉnh
int sz[maxN + 1];

//Khởi tạo DSU
void init(){
    cin >> n;
    //đánh dấu đại diện của mỗi đỉnh riêng biệt là chính nó
    for(int i = 1; i <= n; i++){
        parent[i] = i;
        sz[i] = 1;
    }

}
```
### Find
```cpp
//Chưa tối ưu
int Find(int u){
    while(u != parent[u]) u = parent[u]; //lấy cha của u gán cho u để truy vết ngược lại
    return u;
}

//Tối ưu
//Tìm đại diện của đỉnh u => i = parent[i]
int Find(int u){
    if(u == parent[u]) return u;
    else{
        int tmp = Find(parent[u]);
        parent[u] = tmp;
        return tmp;
    }
    //code gọn hơn
    else parent[u] = Find(parent[u]);
}
```
### Union
```cpp
//Chưa tối ưu
bool Union(int u, int v){
    u = Find(u); // tìm đại diện của u
    v = Find(v); // tìm đại diện của v
    if(u == v) return false; // 2 đỉnh này cùng tập hợp
    else{
        //gán đại diện cho tập hợp là phần tử nhỏ hơn
        if(u < v) parent[v] = u;
        else parent[u] = v;
        return true; // gộp đc
    }
}

// Tối ưu
//Gộp 2 đỉnh -> tìm đại diện cho 2 thằng và thay đổi đại diện
bool Union(int u, int v){
    u = Find(u); // tìm đại diện của u
    v = Find(v); // tìm đại diện của v
    if(u == v) return false; // 2 đỉnh này cùng tập hợp
    if(sz[u] < sz[v]) swap(u, v); //ưu tiên thằng lớn hơn là thằng u
    sz[u] += sz[v]; // size của u là lớn hơn v nên sẽ được đi nhập vào
    parent[v] = u; // v gọi u là cha
    return true;
}
```
### Struct DSU
```cpp
const int k = 1e4;
int n, m; 
int parent[k + 1];
int sz[k];

struct DSU{
    void init(){
        cin >> n >> m;
        for(int i = 1; i <= n; i++){
            parent[i] = i;
            sz[i] = 1;
        }
    }
    int Find(int u){
        if(u == parent[u]) return u;
        else return parent[u] = Find(parent[u]);
    }
    bool Union(int u, int v){
        u = Find(u);
        v = Find(v);
        if(u == v) return false;
        if(sz[u] < sz[v]) swap(u, v);
        sz[u] += sz[v];
        parent[v] = u;
        return true;
    }
};
```
### Ứng dụng
```cpp
//đếm số lượng tlpt -> disjoint set
int m; // số cạnh của đồ thị
int CC(){
    init();
    cin >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        Union(x, y);
    }
    int cnt = 0;
    for(int i = 1; i <= n; i++){
        //số tplt
        if(i == parent[i]) cnt++;
    }
    return cnt;
}
```

## Thuật toán đồ thị
### DFS
- DFS -> duyệt theo từng nhánh ⇒ đến đáy rồi quay lại
- Mã giả 
    ```cpp
    //Dùng danh sách kề => O(V + E) tối ưu và dễ code
    //Tìm kiếm theo chiều sâu DFS => đệ quy
    DFS(u){
        <Tham dinh u> //code
        visited[u] = true;//Đánh dấu đã thăm u
        //Duyệt danh sách kề của u
        for(int v : adj[u]){
            if(!visited[v]){
                DFS(v);
            }
        }
    }
    ```
```cpp
bool visited[maxN];
//Ds kề
void DFS(int u){
    cout << u << ' '; //in ra
    visited[u] = true; // đánh dấu đã thăm
    for(int v : adj[u]){ //duyệt danh sách kề của đỉnh u
        if(!visited[v]){
            DFS(v);
        }
    }
}

//Ma trận kề
void DFS1(int u){
    cout << u << ' ';
    visited[u] = true;
    //duyệt ds kề của đỉnh u = duyệt dòng thứ u trong ma trận kề
    for(int i = 1; i <= n; i++){ //cột 1 -> n
        if(mtx[u][i] == 1){ //xét từng cột của dòng u
            if(!visited[i]){
                DFS1(i);
            }
        }
    }
}

//Ds cạnh
void DFS2(int u){
    cout << u << ' ';
    visited[u] = true;
    //duyệt ds của kề đỉnh u = duyệt tất cả các cạnh trong ds cạnh => xét xem đỉnh đầu vs đỉnh cuối của cạnh đó nếu là u => có 1 cạnh kè với nó => gọi dfs 
    for(auto it : edge){ 
        if(it.first == u){ 
            if(!visited[it.second]){
                DFS1(it.second);
            }
        }
        if(it.second == u){
            if(!visited[it.first]){
                DFS2(it.first);
            }
        }
    }
}
```
### BFS
- BFS → duyệt theo từng hàng cùng bậc
- Mã giả
    ```cpp
    //Tìm kiếm theo chiều rộng BFS => hàng đợi
    BFS(u){
        //STEP 1: Khởi tạo
        queue = ∅; //Tạo một hàng đợi rỗng
        push(queue, u); // Đẩy đỉnh u vào hàng đợi
        visited[u] = true; // Đánh dấu đỉnh u đã được thăm
        //STEP 2: Lặp khi mà hàng đợi vẫn còn phần tử
        while (queue != ∅){
            v = queue.front(); // Lấy ra đỉnh ở đầu hàng đợi
            queue.pop(); //Xóa đỉnh khỏi đầu hàng đợi
            <Thăm đỉnh v>
        // Duyệt các đỉnh kề với v mà chưa được thăm và đẩy vào hàng đợi
            for(int x: ke[v]){
                if(!visited[x]){ // Nếu x chưa được thăm
                    push(queue, x);
                    visited[x] = true;
                }
            }
        }
    }
    ```
```cpp
bool visited[maxN];
//Ds kề
void BFS(int u){
    queue<int> q;
    q.push(u);
    visited[u] = true;
    while(!q.empty()){
        //Thăm đỉnh u
        int x = q.front();
        q.pop();
        cout << x << ' ';
        //Duyệt danh sách kề của đỉnh u
        for(int v : adj[x]){
            if(!visited[v]){
                q.push(v);
                visited[v] = true;
            }
        }
    }
}
```
### Kĩ thuật di chuyển trên mảng 2 chiều
```cpp
int dx[8] = {-1, -1, -1, 0, 0, 1, 1, 1};
int dy[8] = {-1, 0, 1, -1, 1, -1, 0, 1};

void DFS(int i, int j){
    visited[i][j] = true;
    for(int k = 0; k < 8; k++){
        int i1 = i + dx[k], j1 = j + dx[k];
        if(i1 >= 1 && i1 <= n && j1 >= 1 && j1 <= m && !visited[i1][j1]){
            DFS(i1, j1);
        }
    }
}

void BFS(int i, int j){
    queue<pair<int, int>> q;
    q.push({i, j});
    visited[i][j] = true;
    while(!q.empty()){
        pair<int, int> x = q.front(); 
        q.pop();
        for(int k = 0; k < 8; k++){
            int i1 = x.first + dx[k], j1 = x.second + dx[k];
            if(i1 >= 1 && i1 <= n && j1 >= 1 && j1 <= m && !visited[i1][j1]){
                q.push({i1, j1});
                visited[i1][j1] = true;
            }
        }
    }   
}
```
### Sắp xếp Topo
```cpp
//Topological sort -> đồ thị có hướng
//sắp xếp topo => những đỉnh liên quan vs nhau thì đỉnh nào bắt buộc phải đứng trước đỉnh kia sẽ đc đứng trc(giống như môn tiên quyết ở trường ĐH) -> những thằng ko liên quan ko xét đứng đâu cũng đc
//miễn xuất hiện đường nối có hướng giữa u và v -> sắp xếp topo => u phải xếp đứng trước v -> ứng dụng stack
//ứng dụng DFS
vector<int> topo;
stack<int> st;
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        adj[x].push_back(y);
    }
    memset(visited, false, sizeof(visited));
}
void DFStopo(int u){
    visited[u] = true;
    for(int v : adj[u]){
        if(!visited[v]) DFStopo(v);
    }
    //đã duyệt xong đỉnh u -> push vào vector hoặc stack
    //topo.push_back(u);
    st.push(u);
}
void topoSort(){
    //input();
    //duyết hết tất cả các đỉnh của đồ thị
    for(int i = 1; i <= n; i++){
        if(!visited[i]) DFStopo(i);
    }
    // reverse(topo.begin(), topo.end());
    // for(int x : topo) cout << x << ' ';
    while(!st.empty()){
        int x = st.top();
        st.pop();
        cout << x << ' ';
    }

}
```
### Đồ thị liên thông
#### Kiểm tra đồ thị lt + Đếm số lượng tplt đồ thị vô hướng  
```cpp
//Mỗi lần lời gọi là 1 lần xác định tp liên thông
void DFSconnect(int u){
    visited[u] = true; 
    for(int v : adj[u]){ 
        if(!visited[v]){
            DFSconnect(v);
        }
    }
}
void BFSconnect(int u){
    queue<int> q;
    q.push(u);
    visited[u] = true;
    while(!q.empty()){
        int x = q.front();
        q.pop();
        for(int v : adj[x]){
            if(!visited[v]){
                q.push(v);
                visited[v] = true;
            }
        }
    }
}
int connectivity(){
    //Tránh mỗi lần q truy vấn thì mảng chưa được reset => tùy lúc hẵn xài
    //memset(visited, false, sizeof(visited));
    int cnt = 0;
    for(int i = 1; i <= n; i++){
        if(!visited[i]){
            //chọn 1 trong 2
            DFSconnect(i);
            BFSconnect(i);
            cnt++;
        }
    }
    return cnt;
}
void connect(){
    if(connectivity() == 1) cout << "TRUE" << endl;
    else cout << "FALSE" << endl;
}
```
#### Kiểm tra tplt mạnh của đồ thị có hướng
```cpp
//Brute force -> kt tất cả các đỉnh có được thăm khi gọi dfs hoặc bfs ko
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        adj[x].push_back(y);
    }
    memset(visited, false, sizeof(visited));
}
void strongConnect(){
    input();
    for(int i = 1; i <= n; i++){
        memset(visited, false, sizeof(visited));
        DFSconnect(i); //BFSconnect(i);
        //duyệt hết tất cả các đỉnh của đồ thị để xem có được thăm hết ko
        for(int j = 1; j <= n; j++){
            if(visited[j] == false){
                cout << "NO\n";
                return;
            }
        }
    }
    cout << "YES\n";
}

//Kosaraju -> liệt kê các tplt mạnh
vector<int> tAdj[k + 1]; // dùng 1 mảng để lưu đồ thị chuyển vị -> quay ngược chiều toàn bộ các phần tử ban đầu
stack<int> st; //đẩy các phần tử khi gọi DFS vào stack như sắp xếp topo 
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        adj[x].push_back(y);
        //lật ngược ds kề và lưu vào tAdjs
        tAdj[y].push_back(x);
    }
    memset(visited, false, sizeof(visited));
}
void DFS1(int u){
    visited[u] = true;
    for(int v : adj[u]){
        if(!visited[v]) DFS1(v);
    }
    //sau khi gọi xong DFS1 của ds kề của u thì push các đỉnh lần lượt vào stack
    st.push(u);
}
void DFS2(int u){
    visited[u] = true;
    cout << u << ' '; //xuất giá trị của u
    //duyệt đồ thị chuyển vị 
    for(int v : tAdj[u]){
        if(!visited[v]) DFS2(v);
    }
}
void kosaraju(){
    input();
    //duyệt hết tất cả các tplt có thể có của đồ thị
    for(int i = 1; i <= n; i++){
        if(!visited[i]) DFS1(i);
    }
    //duyệt xong đã có các pt trong stack
    memset(visited, false, sizeof(visited));
    int scc = 0; //đếm số tplt mạnh
    while(!st.empty()){
        //lấy đỉnh đầu stack ra và duyệt DFS của đỉnh đó nhưng trong đồ thị transpose
        int x = st.top();
        st.pop();
        for(int v : tAdj[x]){
            if(!visited[v]){
                scc++; // tăng số tplt mạnh
                cout << scc << " : "; //liệt kê các tplt mạnh
                DFS2(v);
                cout << endl; // sau khi in xong mỗi tplt thì xuống dòng
            }
        }
    }
    cout << scc << endl;
}
//Kiểm tra đồ thị có phải liên thông mạnh => số tplt = 1 là đồ thi liên thông mạnh còn nhiều hơn thì ko phải
bool strongConntect(){
    //viết lại hàm kosaraju trả về kiểu int
    if(kosaraju() == 1) return true;
    else return false;
}

//Thuật toán Tarjan : dùng 2 mảng disc[] và low[], timer -> thể hiện thời gian thăm => tìm tplt mạnh
int low[k + 1], disc[k + 1];
int timer = 0, scc = 0;
stack<int> st;
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        adj[x].push_back(y);
        //adj[y].push_back(x);
    }
    memset(visited, false, sizeof(visited));
}
void DFStarjan(int u){
    visited[u] = true;
    //đánh dấu mảng disc vs low khi thăm lần đầu
    disc[u] = low[u] = ++timer; //ko được timer++ => nó ko cập nhật timer giá trị đúng
    //đẩy u vào trong stack
    st.push(u);
    for(int v : adj[u]){
        if(!visited[v]){
            DFStarjan(v);
            //đỉnh chưa được thăm thì so sánh và cập low của u hoặc v
            low[u] = min(low[u], low[v]);
        }
        //đỉnh được thăm thì phải so sánh và cập nhật low u vs disc v
        else low[u] = min(low[u], disc[v]);
    }
    //sau khi gọi DFS và cập nhật mảng low vs disc ở mỗi đỉnh xong
    if(low[u] == disc[u]){ // đỉnh thăm sớm nhất của tp liên thông mạnh
        scc++;
        while(st.top() != u){
            cout << st.top() << ' ';
            st.pop();
        }
        cout << st.top() << ' ';
        st.pop();
        cout << endl;
    }
}
void SCC(){
    input();
    for(int i = 1; i <= n; i++){
        if(!visited[i]) DFStarjan(i);
    }
}
```
### Đỉnh trụ - cạnh cầu
#### Cơ bản
```cpp
//Tìm đỉnh trụ -> Articulation point(cut vertice)
//Xóa 1 đỉnh khỏi đồ thị => visited[i] = true trước khi gọi DFS hoặc BFS
int connectivity(){
    int cnt = 0;
    for(int i = 1; i <= n; i++){
        if(!visited[i]){
            DFSconnect(i);
            //BFSconnect(i);
            cnt++;
        }
    }
    return cnt;
}
void AP(){
    //đếm số tp liên thông ban đầu
    int prevCnt = connectivity();
    //Duyệt từng đỉnh để xem nếu bỏ từng đỉnh đó ra thì nó có tạo thêm tplt ko
    for(int i = 1; i <= n; i++){
            //reset lại mảng visited sau khi đếm số tplt ban đầu
        memset(visited, false, sizeof(visited));
        //loại đỉnh đó khỏi đồ thị
        visited[i] = true;
        if(prevCnt < connectivity()){ // số tplt sau khi loại đỉnh i
            //i là đỉnh trụ
            cout << i << ' ';
        }
    }
    cout << endl;
}
//Đếm số lượng cạnh cầu => mảng set và vector<pair> và thực hiện 2 thao tác
//xóa x ở danh sách kề của y và xóa y ở ds kề của x sau đó insert lại
//Viết riêng ra đừng để chồng chất nhiều hàm thì thuật toán này sai
set<int> adjList[maxN];
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        edge.push_back({x, y});
        adjList[x].insert(y);
        adjList[y].insert(x);
    }
}
void bridge(){
    input();
    //đếm số lượng tplt ban đầu
    int prevCnt = connectivity();
    int cnt = 0;
    for(auto it : edge){
        int x = it.first, y = it.second;
        //loại bỏ cạnh của 2 đỉnh ra khỏi đồ thị mà ko xóa 2 đỉnh đó
        adjList[x].erase(y);
        adjList[y].erase(x);
        //làm mới lại mảng visited
        memset(visited, false, sizeof(visited));
        if(prevCnt < connectivity()){
            cnt++;
        }
        //Thêm cạnh lại vào ds để đảm bảo ds còn nguyên
        adjList[x].insert(y);
        adjList[y].insert(x);
    }
    cout << cnt << endl;
}

//Cạnh cầu ko cần dùng mảng set => đánh dấu rằng 2 đỉnh mình chuẩn bị xét là 2 đỉnh của cạnh liên thuộc cần xóa
//ko cần xóa hoặc thêm + code ngắn hơn
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        edge.push_back({x, y});
        adj[x].push_back(y);
        adj[y].push_back(x);
    }
}
void DFSbridge(int u, int s, int t){
    visited[u] = true;
    for(int v : adj[u]){
        //kiểm tra xem 2 đỉnh đó có phải là 2 đỉnh thuộc cạnh cầu => continue
        if(u == s && v == t || u == t && v == s) continue;
        if(!visited[v]){
            DFSbridge(v, s, t);
        }
    }
}
int connect(int s, int t){
    int ans = 0;
    for(int i = 1; i <= n; i++){
        if(!visited[i]){
            ans++;
            DFS(i, s, t);
        }
    }
    return ans;
}
void bridge(){
    input();
    int cc = connect(0, 0); //truyền 0 0 để ko ảnh hưởng đến lần xét tplt ban đầu
    int cnt = 0;
    for(auto it : edge){
        int x = it.first, y = it.second;
        memset(visited, false, sizeof(visited));
        //cạnh cầu bị loại bỏ -> truyền 2 đỉnh vào
        if(cc < connect(x, y)){
            cnt++;
        }
    }
    cout << cnt << endl;
}
```
#### Nâng cao
```cpp
//Tarjan dùng để tìm đỉnh trụ : đỉnh trụ => low[v] >= disc[u] (khác đỉnh nguồn) -> u là đỉnh trụ
int low[k + 1], disc[k + 1];
int timer = 0, scc = 0;
int AP[k + 1];
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        adj[x].push_back(y);
        adj[y].push_back(x);
    }
    memset(visited, false, sizeof(visited));
    memset(AP, false, sizeof(AP));
}
void DFSap(int u, int parent){
    visited[u] = true;
    //đánh dấu mảng disc vs low khi thăm lần đầu
    disc[u] = low[u] = ++timer; //ko được timer++ => nó ko cập nhật timer giá trị đúng
    //đếm số lượng con của cây DFS nếu lớn hơn hoặc bằng 2 con từ đỉnh u thì đó là đỉnh trụ 
    int child = 0;
    for(int v : adj[u]){
        if(v == parent) continue; //nếu nó là chu trình thì bỏ qua => ko xét đến
        if(!visited[v]){
            DFSap(v, u);
            //mỗi lần gọi dfs là tăng số cây con của đỉnh u
            child++;
            //đỉnh chưa được thăm thì so sánh và cập low của u hoặc v
            low[u] = min(low[u], low[v]);
            //kiểm tra đỉnh u ko phải là đỉnh gốc và có đặc điểm là disc[u] <= low[v] => là đỉnh trụ
            if(parent != -1 && disc[u] <= low[v]) AP[u] = true;
        }
        //đỉnh được thăm thì phải so sánh và cập nhật low u vs disc v
        else low[u] = min(low[u], disc[v]);
    }
    //sau khi gọi DFS và cập nhật mảng low vs disc ở mỗi đỉnh xong
    //đỉnh u là đỉnh gốc và có số con lớn hơn 2 -> đỉnh trụ
    if(parent == -1 && child > 1) AP[u] = true; 
}
void findAP(){
    input();
    for(int i = 1; i <= n; i++){
        if(!visited[i]) DFSap(i, -1);
    }
    for(int i = 1; i <= n; i++){
        if(AP[i]) cout << i << ' ';
    }
}

//Tarjan dùng để tìm cạnh cầu => low[v] > disc[u] -> tránh u là đỉnh nguồn của chu trình chứa v 
int low[k + 1], disc[k + 1];
int timer = 0;
vector<pair<int, int>> bridge;
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        adj[x].push_back(y);
        adj[y].push_back(x);
    }
    memset(visited, false, sizeof(visited));
}
void DFSbridge(int u, int parent){
    visited[u] = true;
    //đánh dấu mảng disc vs low khi thăm lần đầu
    disc[u] = low[u] = ++timer; //ko được timer++ => nó ko cập nhật timer giá trị đúng
    for(int v : adj[u]){
        if(v == parent) continue; //nếu nó là chu trình thì bỏ qua => ko xét nữa
        if(!visited[v]){
            DFSbridge(v, u);
            //đỉnh chưa được thăm thì so sánh và cập low của u hoặc v
            low[u] = min(low[u], low[v]);
            //kiểm tra disc[u] < low[v] => là cạnh cầu
            if(disc[u] < low[v])  bridge.push_back({u, v});
        }
        //đỉnh được thăm thì phải so sánh và cập nhật low u vs disc v
        else low[u] = min(low[u], disc[v]);
    }
}
void findBridge(){
    input();
    for(int i = 1; i <= n; i++){
        if(!visited[i]) DFSbridge(i, -1);
    }
    for(auto x : bridge) cout << x.first << ' ' << x.second << endl;
}
```
### Đường đi
#### Cơ bản
```cpp
//Kiểm tra đường đi 2 đỉnh -> mỗi lần gọi DFS hoặc BFS thì đánh dấu những đỉnh thăm thuộc tp liên thông nào => 2 đỉnh cùng thuộc 1 tp liên thông thì có đường đi với nhau
int ID[k + 1], cnt = 0;
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        adj[x].push_back(y);
        adj[y].push_back(x);
    }
    memset(visited, false, sizeof(visited));
}
void DFSid(int u){
    visited[u] = true;
    ID[u] = cnt; 
    for(int v : adj[u]){ 
        if(!visited[v]){
            DFSid(v);
        }
    }
}
void BFSid(int u){
    queue<int> q;
    q.push(u);
    visited[u] = true;
    while(!q.empty()){
        int x = q.front();
        ID[x] = cnt;
        q.pop();
        for(int v : adj[x]){
            if(!visited[v]){
                q.push(v);
                visited[v] = true;
            }
        }
    }
}
void checkPath(){
    input();
    for(int i = 1; i <= n; i++){
        if(!visited[i]){
            //đánh dấu đỉnh thuộc tplt nào
            cnt++;
            //loang ra hết tất cả các đỉnh kề của u
            DFSid(i);
            //BFSid(i);
        }
    }
    int q; cin >> q;
    while(q--){
        int x, y; cin >> x >> y;
        if(ID[x] == ID[y]) cout << "1" << ' ';
        else cout << "0" << ' ';
    }
}

//Tìm đường đi giữa 2 đỉnh trên đồ thị => dùng thêm 1 mảng cha -> parent[u] : đỉnh cha của đỉnh u
int parent[k + 1];
int s, t;

void DFSpath(int u){
    visited[u] = true;
    for(int v : adj[u]){ 
        if(!visited[v]){
            DFSpath(v);
            //v là ds kề của u -> mở rộng v => đánh dấu u là cha của v
            parent[v] = u;
        }
    }
}
void BFSpath(int u){
    queue<int> q;
    q.push(u);
    visited[u] = true;
    while(!q.empty()){
        int x = q.front();
        ID[x] = cnt;
        q.pop();
        for(int v : adj[x]){
            if(!visited[v]){
                q.push(v);
                visited[v] = true;
                //đánh dấu x là cha của v
                parent[v] = x;
            }
        }
    }
}
//Tìm đường đi bằng DFS là đường đi bình thường
void findPath(){
        input();
    //nhập 2 đỉnh (đầu, cuối để xác định đường đi)
    cin >> s >> t;
    DFSpath(s); //BFSpath(s);
    if(!visited[t]) cout << "-1" << endl;
    else{
        //Truy vết đường đi
        vector<int> res;
        while(t != s){
            res.push_back(t);
            //đi dò ngược lại cho đến khi thấy đỉnh đầu tiên 
            t = parent[t];
        }
        res.push_back(s);
        //lật ngược vector lại vì nãy giờ lần đi thì pushback vào theo thứ tự ngược
        reverse(res.begin(), res.end());
        for(int x : res) cout << x << ' ';
        cout << endl;
    }    
}
//Trên đồ thị ko có trọng số => BFS tìm được đường đi ngắn nhất giữa 2 đỉnh
void minPath(){
    input();
    //nhập 2 đỉnh (đầu, cuối để xác định đường đi)
    cin >> s >> t;
    BFSpath(s);
    if(!visited[t]) cout << "-1" << endl;
    else{
        //Truy vết đường đi
        vector<int> res;
        while(t != s){
            res.push_back(t);
            //đi dò ngược lại cho đến khi thấy đỉnh đầu tiên 
            t = parent[t];
        }
        res.push_back(s);
        //lật ngược vector lại vì nãy giờ lần đi thì pushback vào theo thứ tự ngược
        reverse(res.begin(), res.end());
        for(int x : res) cout << x << ' ';
        cout << endl;
    }    
}
```
#### Nâng cao
```cpp
//Tìm đường đi ngắn nhất
//Thuật toán Dijkstra => 1 -> mọi đỉnh : đồ thị có trọng số ko âm
typedef pair<int, int> ii;
bool taken[k + 1];
vector<ii> adj[k + 1];
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y, w; cin >> x >> y >> w;
        adj[x].push_back({y, w});
        adj[y].push_back({x, w});
    }
}
void dijkstra(int s){
    //tạo mảng lưu chi phí đường đi từ đỉnh nguồn đến đỉnh u
    vector<int> d(n + 1, 1e9);
    d[s] = 0; // chi phí ban đầu là 0
    priority_queue<ii, vector<ii>, greater<ii>> q;
    q.push({0, s}); //đẩy vào hàng đợi ưu tiên {giá trị, đỉnh}
    while(!q.empty()){
        //chọn ra đỉnh u có đường ngắn nhất => relaxation
        pair<int, int> t = q.top();
        q.pop();
        int u = t.second, dist = t.first;
        if(dist > d[u]) continue; // chi phí từ đỉnh nguồn đến đỉnh u ko phải là ngắn nhất => bỏ qua
        //relax
        for(auto it : adj[u]){
            int v = it. first, w = it.second;
            //kiểm tra chi phí đi từ đỉnh s -> v lớn hơn chi phí từ s -> u -> v
            if(d[v] > d[u] + w){
                //cập nhật chi phí
                d[v] = d[u] + w;
                q.push({d[v], v});
            }
        }
    }
    for(int i = 1; i <= n; i++) cout << d[i] << ' ';
}

//Thuật toán Bellman Ford => 1 -> mọi đỉnh : đồ thị ko có chu trình âm
struct edge{
    int x, y, w;
};
vector<edge> lst;
int d[k + 1];
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y, w; cin >> x >> y >> w;
        lst.push_back({x, y, w});
    }
}
void bellmanFord(int s){
    //Tất cả các đỉnh trừ đỉnh nguồn đều có giá trị max
    for(int i = 1; i <= n; i++) d[i] = INT_MAX;
    d[s] = 0; // đỉnh nguồn
    //lặp n - 1 lần
    for(int i = 0; i < n - 1; i++){
        //lặp m cạnh
        for(int j = 0; j < m; j++){
            edge tmp = lst[j];
            int x = tmp.x, y = tmp.y, w = tmp.w;
            //nếu đỉnh đã cập nhật giá trị rồi
            if(d[x] < INT_MAX){ // độ thị có cạnh là trọng số âm => rất quan trọng để khỏi bị cập nhật nhầm khi chưa tìm đc đường đi ngắn nhất
                d[y] = min(d[y], d[x] + w); //cập nhật chi phí đường đi thấp hơn giống dijkstra
            }
        }
    }
    //sau khi nó kết thúc nếu đồ thị có chu trình âm => làm thêm 1 vòng for như trên
    //nếu giá trị được cập nhật thì tức là đồ thị có chu trình âm
    for(int i = 1; i <= n; i++) cout << d[i] << ' ';
}

//Floyd : All pair -> 2 đỉnh bất kì trên đồ thị => số đỉnh thường bé hơn 400
int d[k + 5][k + 5]; //khoảng cách đường đi ngắn nhất từ i tới j
void floyd(){
    for(int k = 1; k <= n; k++){
        for(int i = 1; i <= n; i++){
            for(int j = 1; j <= n; j++){
                d[i][j] = min(d[i][j], d[i][k] + d[k][j]);
            }
        }
    }
    int q; cin >> q;
    while(q--){
        int x, y; cin >> x >> y;
        cout << d[x][y] << endl;
    }
}
```   
### Chu trình
```cpp
//Dồ thị có chu trình => ko có thứ tự topo
//Ứng dụng BFS (Kahn) : xóa dần đỉnh => đồ thị có chu trình -> ko đầy đủ các đỉnh khi thực hiện sx kahn => ứng dụng thực hiện kt chu trình trên đồ thị có hướng
//Quan tâm đến bán bậc vào
int inDeg[k + 1]; // lưu bán bậc vào
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        adj[x].push_back(y);
        //tăng số bán bậc vào cho đỉnh đồ thị => đỉnh y
        inDeg[y]++;
    }
    memset(visited, false, sizeof(visited));
}
void kahn(){
    input();
    queue<int> q;
    //duyệt tất cả các đỉnh => tìm thằng nào có bán bậc vào bằng 0 sẽ là thằng bắt đầu thuật toán
    for(int i = 1; i <= n; i++){
        //nếu bán bậc vào của đỉnh đó bằng 0 thì push vào queue
        if(inDeg[i] == 0) q.push(i);
    }
    while(!q.empty()){
        //lấy thằng đầu hàng đợi ra
        int u = q.front();
        q.pop();
        cout << u << ' ';
        //duyệt danh sách kề của u
        for(int v : adj[u]){
            //xóa đỉnh u khỏi đồ thị -> xóa cạnh nối u vs v => bán bậc vào của v bị giảm
            inDeg[v]--;
            //nếu bán bậc vào của v là 0 => push nó vào hàng đợi
            if(inDeg[v] == 0) q.push(v); 
        }
    }
}

//Kiểm tra chu trình trên đồ thị
//Vô hướng : DFS => tìm cạnh ngược (Nếu đỉnh u mở rộng ra đỉnh v đã được thăm rồi mà v ko phải cha trực tiếp của u -> u cạnh ngược)
int parent[k + 1];
bool DFS_Undirected_cycle(int u){
    //đỉnh truyền vào đc thăm
    visited[u] = true;
    for(int v : adj[u]){
        //đỉnh trong danh sách kề vs u chưa đc thăm
        if(!visited[v]){
            //cha của v là u
            parent[u] = v;
            //phải kiểm tra nhánh v của u đang xét có chu trình hay ko => ko có thì xét nhánh khác => ko được return DFS(v)
            if(DFS_Undirected_cycle(v)) return true;
        }
        //đỉnh trong ds kề u đã đc thăm => check xem nó có cha trực tiếp => ko có thì kết luận là cạnh ngược
        else if(v != parent[u]){
            return true;
        }
    }
    //duyệt hết các nhánh mà ko thấy xuất hiện -> ko có chu trình
    return false;
}
//Vô hướng : BFS
bool BFS_Undirected_cycle(int u){
    queue<int> q;
    //đỉnh truyền vào hàng đợi đc thăm
    q.push(u);
    visited[u] = true;
    //lặp cho đến khi hàng đợi rỗng
    while(!q.empty()){
        int x = q.front();  //thường là in ra x nếu duyệt
        q.pop();
        //duyệt ds kề của đỉnh u
        for(int v : adj[x]){
            //chưa được thăm
            if(!visited[v]){
                q.push(v);
                visited[v] = true;
                parent[v] = x; //đánh dấu x là cha của v
            }
            else if(v != parent[x]) return true;
        }
    }
    return false;
}
//Có thể thay bằng viết hàm void có biến toàn cục ok và check dựa vào biến ok đó 
int ok = 0;
void DFScycle(int u){
    visited[u] = true;
    for(int v : adj[u]){
        if(!visited[v]){
            parent[v] = u;
            DFScycle(v);
        }
        else if(v != parent[u]) ok = 1;
    }
}
void cycle(){
    input();
    //duyệt tổng quát-> một đồ thị có thể có 1 tplt hoặc nhiều hơn thì chỉ cần có ít nhất 1 tplt có chu trình thì đồ thị có chu trình
    for(int i = 1; i <= n; i++){
        //xét toàn bộ các đỉnh
        if(!visited[i]) checkCycle(i);
    }
    if(ok) cout << "YES\n";
    else cout << "NO\n";
}
//Code chu trình ko cần mảng parent
bool DFScycle(int u, int parent){
    visited[u] = true;
    for(int v : adj[u]){
        if(!visited[v]){
            //kiểm tra 1 nhánh đó có chu trình ko => ko có sang nhánh khác
            if(DFScycle(v, u)) return true;
        }
        //đỉnh v đã được thăm nhưng mà v ko là cha trực tiếp của u
        if(v != parent) return true;
    }
    //ko có chu trình
    return false;
}
void cycle(){
    input();
    bool check = false; // check tp liên thông nào có chu trình vì 1 đồ thị có thể có nhiều tplt
    for(int i = 1; i <= n; i++){
        if(!visited[i] && DFScycle(i, 0)){ //giá trị khởi đầu cho 0 hoặc -1 đều đc
            check = true;
            break;
        }
    }
    if(check == true) cout << "YES\n";
    else cout << "NO\n";
}

//Có hướng : DFS chuẩn -> xét 3 màu : trắng(0)->chưa thăm, xám(1)->đang thăm, đen(2)->đã thăm
int color[k + 1];
bool DFS_Directed_cycle(int u){
    //đỉnh truyền vào đc thăm -> xám
    color[u] = 1;
    for(int v : adj[u]){
        //đỉnh trong danh sách kề vs u chưa đc thăm
        if(color[v] == 0){
            //phải kiểm tra nhánh v của u đang xét có chu trình hay ko => ko có thì xét nhánh khác => ko được return DFS(v)
            if(DFS_Directed_cycle(v)) return true;
        }
        //đỉnh trong ds kề u đã đc thăm => check xem nó có màu xám ko => có kết luận là cạnh ngược
        else if(color[v] == 1){
            //ok = 1;
            return true;
        }
    }
    //Duyệt xong 1 nhánh DFS của 1 đỉnh thì cho đỉnh ban đầu truyền màu đen
    color[u] = 2;
    //duyệt hết các nhánh mà ko thấy xuất hiện -> ko có chu trình
    return false;
}
void cycle(){
    input();
    bool check = false; // check tp liên thông nào có chu trình vì 1 đồ thị có thể có nhiều tplt
    for(int i = 1; i <= n; i++){
        if(!color[i] && DFS_Directed_cycle(i, 0)){ //giá trị khởi đầu cho 0 hoặc -1 đều đc
            check = true;
            break;
        }
    }
    if(check == true) cout << "YES\n";
    else cout << "NO\n";
}

//Check chu trình trên đồ thị có hướng => thuật toán Kahn -> nếu Kahn ko duyệt được đầy đủ các đỉnh => có chu trình
int inDeg[k + 1];
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        adj[x].push_back(y);
        inDeg[y]++;
        //adj[y].push_back(x);
    }
    memset(visited, false, sizeof(visited));
}
bool BFS_Directed_cycle(){
    input();
    queue<int> q;
    //duyệt tất cả các đỉnh => tìm thằng nào có bán bậc vào bằng 0 sẽ là thằng bắt đầu thuật toán
    for(int i = 1; i <= n; i++){
        //nếu bán bậc vào của đỉnh đó bằng 0 thì push vào queue
        if(inDeg[i] == 0) q.push(i);
    }
    int cnt = 0;
    while(!q.empty()){
        //lấy thằng đầu hàng đợi ra
        int u = q.front();
        q.pop();
        cnt++; //đếm số lần đẩy đỉnh
        //duyệt danh sách kề của u
        for(int v : adj[u]){
            //xóa đỉnh u khỏi đồ thị -> xóa cạnh nối u vs v => bán bậc vào của v bị giảm
            inDeg[v]--;
            //nếu bán bậc vào của v là 0 => push nó vào hàng đợi
            if(inDeg[v] == 0) q.push(v); 
        }
    }
    if(cnt == n){   //đầy đủ các đỉnh => ko có chu trình
        return false;
    }
    else return true;
}
void cycle(){
    input();
    if(BFS_Directed_cycle()) cout << "YES\n";
    else cout << "NO\n";
}

//Kiểm tra chu trình âm => đồ thị có nhiều tplt thì kt hết tất cả các tplt đó
bool bellmanFordcheck(int s){
    d[s] = 0;
    for(int i = 1; i <= n - 1; i++){
        for(edge e : lst){
            if(d[e.x] < INT_MAX){
                d[e.y] = min(d[e.y], d[e.x] + e.w);
            }
        }
    }
    bool check = false;
    for(edge e : lst){
        if(d[e.x] < INT_MAX){
            if(d[e.y] > d[e.x] + e.w){
                check = true; 
                break;
            }
        }
    }
    return check;
}
bool negativeCycle(){
    fill(d + 1, d + n + 1, INT_MAX);
    bool res = false;
    for(int i = 1; i <= n; i++){
        if(d[i] == INT_MAX){
            if(bellmanFordcheck(i)){
                res = true; 
                break;
            }
        }
    }
    return res;
}
```
### Chu trình Hamilton
Thăm mỗi đỉnh 1 lần -> đồ thị Hamilton <=> bậc của các đỉnh trên đồ thị >= 2
```cpp
//Đường đi Hamilton => đi qua mỗi đỉnh 1 lần
set<int> adj[k + 1]; //dùng set để lưu ds kề mục đích để truy xuất đỉnh đầu tiên trong chu trình cho nhanh
int degree[k + 1]; // tính bậc đỉnh đồ thị
bool visited[k + 1];
int HC[k + 1]; //lưu vị trí các đường đi của Hamilton
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        adj[x].insert(y);
        adj[y].insert(x);
        degree[x]++;
        degree[y]++;
    }
}
void checkCycle(){
    input();
    //kt xem đồ thị có phải là chu trình hamilton ko bậc của 1 đỉnh bất kì < n/2
    for(int i = 1; i <= n; i++){
        if(degree[i] < n / 2){
            cout << "NO\n";
            return;
        }
    }
}
void hamilton(int pos, int u){
    visited[u] = true;
    HC[pos] = u; //lưu đường đi các đỉnh tại vị trí pos trong mảng
    if(pos == n){ //đã thăm hết các đỉnh của đồ thị
        if(adj[u].find(HC[1]) != adj[u].end()){ //tìm vị trí đầu tiên trong mảng ham là đỉnh đầu tiên xuất phát có kề vs đỉnh cuối ko, nếu kề là chu trình
            HC[++pos] = HC[1];
            //vị trí pos có thể khác nên phải cân nhắc đề chạy for duyệt cho chuẩn
            for(int i = 1; i <= pos; i++){
                cout << HC[i] << ' '; //in ra đường đi chu trình hamilton 
            }
            cout << endl;
            return; //khi duyệt xong chu trình thì kết thúc luôn đỡ phải duyệt tiếp
        }
    }
    //duyệt ds kề của u
    for(int v : adj[u]){
        if(!visited[v]){
            //gọi hamilton
            hamilton(pos + 1, v);
            //trả lại trả thái ban đầu cho v => nếu nhánh này ko phải hamilton => xét nhánh khác
            visited[v] = false;
        }
    }
    visited[u] = false; 
}
```
### Chu trình Euler 
Đi qua mỗi cạnh đúng 1 lần
```cpp
set<int> adj[k + 1];
int degree[k + 1]; 
vector<int> EC;
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        adj[x].insert(y);
        adj[y].insert(x);
        degree[x]++;
        degree[y]++;
    }
}
void euler(int v){
    stack<int> st;
    //đẩy đỉnh v tùy vào stack
    st.push(v);
    while(!st.empty()){
        //lấy ra phần tử đầu tiên trong stack
        int x = st.top();
        //xét ds kề của đỉnh x xem có khác rỗng ko
        if(adj[x].size() != 0){
            //lấy ra phần tử đầu trong ds kề của x
            int y = *adj[x].begin();
            //đẩy vào stack
            st.push(y);
            //xóa cặp(x, y) để nó ko đi lại nữa
            adj[x].erase(y);
            adj[y].erase(x);
        }
        else{
            //đẩy ra khỏi ngăn xếp
            st.pop();
            //bỏ đỉnh x vào mảng EC
            EC.push_back(x);
        }
    }
    //thích thì lật ngược ko thì để nguyên vẫn đc
    reverse(EC.begin(), EC.end());
    for(int x : EC) cout << x << ' ';
}
```
### Đồ thị 2 phía - lưỡng phân 
Dùng 2 màu -> vô hướng => 2 đỉnh kề nhau ko được tô cùng 1 màu
```cpp
int color[k + 1];
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        adj[x].push_back(y);
        adj[y].push_back(x);
    }
    memset(visited, false, sizeof(visited));
}
bool DFSbipartie(int u, int parent){
    color[u] = 3 - color[parent]; // dùng 2 màu : 1 -> trắng, 2 -> đen (0 là chưa tô màu)
    for(int v : adj[u]){
        if(color[v] == 0){
            //xét các tplt của đồ thị -> ko là đồ thị liên thông
            if(!DFSbipartie(v, u)) return false; 
        }
        //màu của đỉnh giống màu của cha nó => 2 đỉnh kề nhau chung 1 màu
        else if(color[v] == color[u]) return false;
    }
    return true;    
}
void bipartie(){
    input();
    //Đồ thị có tplt mà ko 2 phía thì đồ thị đó sẽ ko phải là đồ thị 2 phía
    bool check = true;
    //cho cha của đỉnh u ban đầu có màu là 2
    color[0] = 2;
    //duyệt hết các đỉnh
    for(int i = 1; i <= n; i++){
        if(color[i] == 0){
            if(!DFSbipartie(i, 0)){
                check = false;
            }
        }
    }
    cout << check << endl;
}
```
### Tô màu đồ thị
```cpp
//Tô màu đồ thị
vector<int> adj[k + 1];
int color[k + 1];
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        adj[x].push_back(y);
        adj[y].push_back(x);
    }
}
//KT có thể tô màu c cho đỉnh u hay ko
bool check(int u, int c){
    for(int v : adj[u]){
        if(color[v] == c) return false;
    }
    return true;
}
//Đếm số lượng màu tối đa mà màu c có thể tô đc
int cntColor(int c){
    int res = 0;
    for(int i = 1; i <= n; i++){
        if(!color[i] && check(i, c)){
            color[i] = c;
            res++;
        }
    }
    return res;
}
//Duyệt màu đồ thị
void printColor(){
    input();
    int c; cin >> c;
    int cnt = 0;
    while(cnt < n){ //chưa tô màu hết các đỉnh
        cnt += cntColor(c++);
    }
    for(int i = 1; i <= n; i++){
        cout << i << ' ' << color[i] << endl;
    }
}
//Test mẫu
// 9 8
// 1 2
// 1 6
// 2 3
// 2 4
// 3 5
// 6 7
// 7 8
// 7 9
// 4 7
// 3
// 2 3
// 4 8
// 6 7 
```
### Cây khung cực tiểu - Minimum spanning tree
```cpp
//Thuật toán Prim -> ứng dụng priority queue
struct edge{
    int x, y, w;
    //w là trọng số của cạnh
};
vector<edge> lst;
typedef pair<int, int> ii;
vector<ii> adj[k + 1];
bool taken[k + 1]; // check xem nó có nằm trong MST ko (True MST-False V)
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x , y, w; cin >> x >> y >> w;
        adj[x].push_back({y, w});
        adj[y].push_back({x, w});
    }
    memset(taken, false, sizeof(taken));
}
void prim(int s){
    //tạo 1 hàng đợi ưu tiên nhưng mà là min heap -> top luôn là thằng nhỏ nhất
    priority_queue<ii, vector<ii>, greater<ii>> q;
    //nạp giá trị ban đầu s vào cây khung (MST)
    taken[s] = true;
    //duyệt ds kề của s trong V để đẩy vào MST
    for(auto it : adj[s]){
        int t = it.first; // trong cặp pair giá trị thứ 1 là đỉnh thứ 2 là trọng số
        if(!taken[t]){
            q.push({it.second, t}); // đẩy trọng số vào trước trong hàng đợi ưu tiên để nó sắp xếp theo thứ tự tăng dần
        }
    }
    ll d = 0; // đếm trọng số
    ll cnt = 0; // đếm số lượng cành có bằng n - 1 đỉnh để thỏa đk cây khung ko
    while(!q.empty()){
        //lấy ra cạnh ngắn nhất
        pair<int, int> e = q.top();
        q.pop();
        int u = e.second, w = e.first;
        if(!taken[u]){ // bắt buộc phải có dòng này lúc đầu lọc thì thỏa điều kiện 2 đỉnh thuộc 2 tập khác nhau nhưng trong quá trình đẩy vào q có khả năng nó sẽ đẩy vào sai => phải check
            ++cnt; // tăng số lượng cạnh
            d += w; // cộng trọng số của cây
            taken[u] = true;
            //duyệt ds kề của đỉnh u xem mấy đỉnh khác có trong MST ko => ko có thì đẩy vào
            for(auto it : adj[u]){
                if(!taken[it.first]){
                    q.push({it.second, it.first});
                }
            }
        }
    }
    if(cnt == n - 1) cout << d << endl;
    else cout << "NO\n";
}
//Bản đầy đủ 
//Thuật toán Prim -> ứng dụng priority queue => lưu lại cạnh
struct edge{
    int x, y, w;//w là trọng số của cạnh
};
vector<edge> MST;
typedef pair<int, int> ii;
vector<ii> adj[k + 1];
bool taken[k + 1]; // check xem nó có nằm trong MST ko (True MST-False V)
int parent[k + 1], d[k + 1]; // lưu lại cạnh của cây khung
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x , y, w; cin >> x >> y >> w;
        adj[x].push_back({y, w});
        adj[y].push_back({x, w});
    }
    memset(taken, false, sizeof(taken));
    //Khởi tạo trọng số ban đầu cho các đỉnh là INT_MAX
    for(int i = 1; i <= n; i++) d[i] = INT_MAX;
}
void prim(int u){
    priority_queue<ii, vector<ii>, greater<ii>> q;
    int res = 0; // đếm trọng số
    q.push({0, u}); //push vào hàng đợi ưu tiên đỉnh nguồn
    while(!q.empty()){
        //lấy ra cạnh ngắn nhất
        pair<int, int> e = q.top();
        q.pop();
        int v = e.second, w = e.first;
        //trong hàng đợi ưu tiên có thể có nhiều cặp cạnh có đỉnh trùng nhau nên cần có câu lệnh này
        if(taken[v]) continue;
        res += w; // trọng số đc cộng
        taken[v] = true; // đưa vào MST
        if(u != v) MST.push_back({v, parent[v], w});
        //duyệt tất cả các đỉnh kề
        for(auto it : adj[v]){
            //it.first = đỉnh, it.second = trọng số
            if(!taken[it.first] && it.second < d[it.first]){
                q.push({it.second, it.first});
                //lưu vào mảng để xuất cạnh của cây khung
                d[it.first] = it.second; // kỉ lục ghi nhận => trọng số nhỏ nhất của tất cả các cạnh nối vs it.first
                parent[it.first] = v;
            }
        }
    }
    cout << res << endl;
    for(auto it : MST) cout << it.x << ' ' << it.y << ' ' << it.w << endl;
}

//Thuật toán Kruskal
struct edge{
    int x, y, w;
    //w là trọng số của cạnh
};
vector<edge> lst;

void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y, w; cin >> x >> y >> w;
        edge e{x, y, w};
        lst.push_back(e);//nhập ds cạnh vào có trọng số
    }
}
bool cmp(edge a, edge b){
    return a.w < b.w;
}
void kruskal(){
    vector<edge> MST; //lưu ds cạnh của cây khung
    int d = 0; // đếm trọng số của cây khung cực tiểu
    sort(lst.begin(), lst.end(), cmp); //sort danh sách theo thứ tự tăng dần
    /*
    có thể dùng biểu thức lambda chỗ này
    sort(list.begin(), list.end(), [](edge a, edge b)->bool{
        return a.w < b.w;
    });
    */
    for(edge e : lst){ //duyệt từng cạnh trong ds cạnh và chọn cạnh có trọng số nhỏ nhất và ko tạo thành chu trình bỏ vào cây khung
        if(MST.size() == n - 1) break; //cây khung đã đủ
        if(Union(e.x, e.y)){ //2 đỉnh này có thể kết nối với nhau
            MST.push_back(e);
            d += e.w;    
        }
    }
    if(MST.size() < n - 1) cout << "NO\n";
    else{
        cout << d << endl;
        for(edge e : MST) cout << e.x << ' ' << e.y << ' ' << e.w << endl;
    }
}
```

# Tree
## Cây - đồ thị
Có n đỉnh -> n - 1 cạnh, là đồ thị liên thông và ko có chu trình
```cpp
//Kiểm tra đồ thị phải là cây hay ko => đồ thị liên thông + ko có chu trình
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        adj[x].push_back(y);
        adj[y].push_back(x);
    }
    memset(visited, false, sizeof(visited));
}
bool DFScycle(int u, int parent){
    visited[u] = true;
    for(int v : adj[u]){
        if(!visited[v]){
            if(DFScycle(v, u)) return true;
        }
        else if(v != parent) return true;
    }
    return false;
}
void tree(){
    input();
    //có chu trình -> ko phải cây
    if(DFScycle(1, 0)) cout << "NO\n";
    else{
        //duyệt xem đồ thị có liên thông hay ko
        for(int i = 1; i <= n; i++){
            //có bất kì đỉnh nào chưa đc thăm -> ko liên thông -> ko phải dồ thị
            if(!visited[i]){
                cout << "NO\n";
                return;
            }
        }
        cout << "YES\n";
    }
}
//Tính độ cao của cây
//mảng chiều cao của các node trên cây
int he[k + 1];
void input(){
    cin >> n;
    for(int i = 0; i < n; i++){
        int x, y; cin >> x >> y;
        adj[x].push_back(y);
        adj[y].push_back(x);
    }
    memset(visited, false, sizeof(visited));
}
//dùng mảng parent -> khi cout thì chiều cao phải trừ đi 1 để thỏa yêu cầu chiều cao bắt đầu của bài toán
void DFSheight(int u, int parent){
    visited[u] = true;
    he[u] = he[parent] + 1;
    for(int v : adj[u]){
        if(!visited[v]){
            DFSheight(v, u);
        }
    }
}
//ko dùng mảng parent
void DFSheight(int u, int parent){
    visited[u] = true;
    he[u] = parent;
    for(int v : adj[u]){
        if(!visited[v]) DFSheight(v, parent + 1);
    }
}
void height(){
    input();
    DFSheight(1, 0);
    for(int i = 1; i <= n; i++){
        cout << i << ' ' << he[i] - 1 << endl;
    }
    DFSheight(1, 0);
    for(int i = 1; i <= n; i++){
        cout << i << ' ' << he[i] << endl;
    }
} 
```
## Cây nhị phân - BT
### Lưu trữ và khởi tạo
```cpp
struct node{
    //data -> có thể là bất cứ kiểu dữ liệu gì
    int data;
    //lưu địa chỉ 2 node con bên trái bên phải
    node *left;
    node *right;
    //Constructor mặc định
    node(){}
    //Constructor => tạo cấu trúc node
    node(int x){
        data = x;
        left = right = NULL;
    }
};

//Có thể tạo hàm makeNode trả về kiểu con trỏ -> node cấp phát động thay constructor => chậm và phiền phức hơn
node* makeNode(int x){
    node* newNode = new node;
    newNode->data = x;
    newNode->left = newNode->right = NULL;
    return newNode;
}

//Tìm ra node cha để có thể tạo thêm node mới cho cây
//u : node đang xét, v : node con trái hoặc phải
void makeRoot(node *root, int u, int v, char c){
    if(c == 'L') root->left = new node(v);
    else root->right = new node(v);
}

//tìm ra node cần tìm để thêm
void insertNode(node *root, int u, int v, char c){
    if(root == NULL) return; //kết thúc lời gọi đệ quy
    if(root->data = u) makeRoot(root, u, v, c); //tìm được node để thêm bên trái hoặc phải
    else{
        insertNode(root->left, u, v, c); //tìm bên cây con bên trái
        insertNode(root->right, u, v, c); //tìm bên cây con bên phải
    }
}

//Nhập node từ input
void input(){
    node *root = NULL;
    int n; cin >> n;
    for(int i = 0; i < n; i++){
        int u, v; char c;
        cin >> u >> v >> c;
        if(root == NULL){
            root = new node(u);
            makeRoot(root, u, v, c);
        }
        else insertNode(root, u, v, c);
    }
}
```
### Duyệt cây
#### Đệ quy
```cpp
//Duyệt giữa -> left - root - right
void inOrder(node *root){
    if(root == NULL) return;
    inOrder(root->left);
    cout << root->data << ' ';
    inOrder(root->right);
}

//Duyệt trước -> root - left - right
void preOrder(node *root){
    if(root == NULL) return;
    cout << root->data << ' ';
    preOrder(root->left);
    preOrder(root->right);
}

//Duyệt sau -> left - right - root
void postOrder(node *root){
    if(root == NULL) return;
    postOrder(root->left);
    postOrder(root->right);
    cout << root->data << ' ';
}

//Duyệt theo mức
void levelOrder(node *root){
    queue<node*> q;
    q.push(root);
    while(!q.empty()){
        node* tmp = q.front();
        q.pop();
        cout << tmp->data << ' ';
        if(tmp->left != NULL) q.push(tmp->left); //thăm cây con left
        if(tmp->right != NULL) q.push(tmp->right); //thăm cây con right 
    }
}

//Duyệt xoắn ốc
void spiralOrder(node *root){
    stack<node*> st1;
    stack<node*> st2;
    //stack 1 push root
    st1.push(root);
    while(!st1.empty() || !st2.empty()){
        while(!st1.empty()){
            node* tmp = st1.top(); 
            st1.pop();
            cout << tmp->data << ' ';
            //duyệt từ trái sang phải -> stack 2 push r -> l
            if(tmp->right != NULL) st2.push(tmp->right);
            if(tmp->left != NULL) st2.push(tmp->left);
        }
        while(!st2.empty()){
            node* tmp = st2.top(); 
            st2.pop();
            cout << tmp->data << ' ';
            //duyệt từ phải sang trái -> stack 1 push l -> r
            if(tmp->left != NULL) st1.push(tmp->left);
            if(tmp->right != NULL) st1.push(tmp->right);
        }
    }
}
```
#### Không đệ quy
```cpp
// NLR
void preorder(Node* root){
    if(root == NULL) return;
    stack<Node*> nodeList;
    nodeList.push(root);
    Node* curr;
    while(!nodeList.empty()){
        curr = nodeList.top();
        cout << curr->data << " ";
        nodeList.pop();
        if(curr->right != NULL) nodeList.push(curr->right);
        if(curr->left != NULL) nodeList.push(curr->left);
    }
}

// LRN
void postorder(Node* root){
    if(root == NULL) return;
    stack<Node*> s1, s2;
    s1.push(root);
    Node* node;

    while(!s1.empty()){
        node = s1.top();
        s1.pop();
        s2.push(node);

        if(node->left) s1.push(node->left);
        if(node->right) s1.push(node->right);
    }
    while(!s2.empty()){
        node = s2.top();
        s2.pop();
        cout << node->data << " ";
    }
}

// LNR
void inorder(Node* root){
    stack<Node*> s;
    Node* curr = root;
    while(curr != NULL || s.empty() == false){
        while(curr !=  NULL){
            s.push(curr);
            curr = curr->left;
        }
        curr = s.top();
        s.pop();
        cout << curr->data << " ";
        curr = curr->right;
    }
}
```
### Kiểm tra cây 
```cpp
//Cây nhị phân đầy đủ => 0 con hoặc 2 con
bool checkFullBT(node* root){
    //0 con
    if(root == NULL) return true;
    //2 con
    if(root->left == NULL && root->right == NULL) return true;
    if(root->left != NULL && root->right != NULL){
        return checkFullBT(root->left) && checkFullBT(root->right);
    }
    //1 trong 2 con left or right có xuất hiện -> sai 
    else return false;
}

//Cây nhị phân hoàn hảo
set<int> se;
bool check = true;
void track(node* root, int cnt){
    if(root == NULL) return;
    if(root->left == NULL && root->right == NULL) se.insert(cnt);
    else check = false;
    track(root->left, cnt + 1);
    track(root->left, cnt + 1);
}
void checkPerfectBT(){
    input();
    track(root, 0);
    if(check){
        if(se.size() == 1) cout << "YES";
        else cout << "NO";
    }
    else cout << "NO";
}

//Cây nhị phân tìm kiếm
bool isValid(node* root, int l, int r){
    if(root == NULL) return true;
    if(root->data > l && root->data < r){
        return isValid(root->left, l, root->data) && isValid(root->right, root->data, r);
    }
    else return false;
}
bool checkBST(node *root){
    return isValid(root, -INT_MAX, INT_MAX);
}
//code khác
bool checkBST(node* root){
    auto isBST = [](node* root, int left, int right, auto& isValid) -> bool{
        if(root == NULL) return true;
        if(root->data > left && root->data < right){
            return isValid(root->left, l, root->data, isValid) && isValid(root->right, root->data, r, isValid);
        }
        else return false;
    };
    return isBST(root, -INT_MAX, INT_MAX, isBST);
}
```
### Thao tác khác
```cpp
//Đếm node lá
int cntLeaf(node* root){
    if(root == NULL) return 0;
    if(root->left == NULL && root->right == NULL) return 1; //node lá
    int cnt = 0;
    //đếm lần lượt các cây con bên trái và bên phải của cây
    cnt += cntLeaf(root->left);
    cnt += cntLeaf(root->right);
    return cnt;
}

//Độ cao của cây
int height(node* root){
    if(root == NULL) return -1;
    return max(height(root->left) + 1, height(root->right) + 1);
    //if(root->left == NULL && root->right == NULL) return 0;
    //return max(height(root->left), height(root->right));
}

//Check các node lá cùng mức
set<int> level;
void cntLevel(node* root, int cnt){
    if(root == NULL) return;
    //là node lá
    if(root->left == NULL && root->right == NULL) level.insert(cnt);
    //duyệt các cây con bên trái và bên phải
    cntLevel(root->left, cnt + 1);
    cntLevel(root->right, cnt + 1);
}
void checkLevel(){
    input();
    cntLevel(root, 0);
    if(level.size() == 1) cout << "YES";
    else cout << "NO";
}
//check level lá cùng mức code khác => truyền tham chiếu để maxh thay đổi và tham gia vào các lần gọi đệ quy sau
bool checkSameLevel(node* root, int h, int &maxh){
    if(root == NULL) return true;
    //node lá
    if(root->left == NULL && root->right == NULL){
        if(maxh == x){ //x là giá trị muốn truyền vào ở hàm main mà thường là 0
            maxh = h;
            return true;
        }
        else return h == maxh;
    }
    else{
        return checkSameLevel(root->left, h + 1, maxh) && checkSameLevel(root->right, h + 1, maxh);
    }
}
```
## Cây nhị phân tìm kiếm - BST
```cpp
//Tìm kiếm trên BST
bool search(node *root, int key){
    if(root == NULL) return false;
    if(root->data == key) return true;
    else if(root->data < key) return search(root->right, key);
    else return search(root->left, key);
}

//Chèn trên BST -> giữ đc cây nhị phân tìm kiếm
node* insert(node* root, int key){
    if(root == NULL) return makeNode(key);
    else if(root->data >= key){
        root->left = insert(root->left, key);
    } 
    else root->right = insert(root->right, key);
    return root;
}

//Xóa trên BST
//tìm node nhỏ nhất lớn hơn node cần xóa => cây con bên trái
node* minNode(node* root){
    node* tmp = root;
    while(tmp != NULL && tmp->next != NULL) tmp = tmp->next; //kt trước khi thực hiện để ko bị lỗi
    return tmp;
}
node* erase(node* root, int key){
    if(root == NULL) return root; //chưa có node xóa
    //tìm node cần xóa
    if(key < root->data) root->left = erase(root, key);
    else if(key > root->data) root->data = erase(root, key);
    //Tìm đúng node cần xóa
    else{
        //TH1 : node có cây con bên phải
        if(root->left == NULL){
            node* tmp = root->right;
            delete root;
            return tmp;
        }
        //TH2 : node có cây con bên trái
        if(root->right == NULL){
            node*tmp = root->left;
            delete root;
            return tmp;
        }
        //TH3 : node trung gian
        else{
            //tìm node nhỏ nhất lớn hơn node cần xóa
            node* tmp = minNode(root->right);
            root->data = tmp->data;
            root->right = erase(root->right, tmp->data);
        }
    }
    return root;
}

// LCA : Tổ tiên chung gần nhất
node *LCA(node *root, int v1, int v2){
    int small = min(v1, v2);
    int large = max(v1, v2);
    while (root != NULL) {
        if (root->data > large) // p, q belong to the left subtree
            root = root->left;
        else if (root->data < small) // p, q belong to the right subtree
            root = root->right;
        else // Now, small <= root.val <= large -> This root is the LCA between p and q
            return root;
    }
    return NULL;
}
```
## Cây Fenwick - Segment
Thay thế bài toán truy vấn và tính tổng dùng mảng cộng dồn => nhiều truy vấn
### Segment
```cpp
//Cây Segment -> cập nhật giá tr và truy vấn tổng của i phần tử trong mảngị 
int const maxN = 1e6 + 5;
int a[maxN], SEG[4 * maxN], n;
//xây cây segment
void build(int v, int l, int r){
    //SEG[v] = sum của các pt từ l -> r
    if(l == r){ //điều kiện dừng
        SEG[v] = a[l]; //node lá 
    }
    else{
        //chưa phải node lá
        int m = (l + r) / 2;
        build(2 * v, l, m); //gọi cây con bên trái
        build(2 * v + 1, m + 1, r); //cây con bên phải
        //Bài toán khác vd như tìm giá trị nhỏ nhất trong đoạn l->r thì cài đặt từ chỗ này khác đi
        SEG[v] = SEG[2 * v] + SEG[2 * v + 1]; // node cha = tổng 2 node con
    }
}

//Truy vấn -> query : O(logN)
int getSum(int v, int tLeft, int tRight, int l, int r){
    if(l > r) return 0; //truy vấn trên 1 đoạn có thể ko có giá trị
    if(l == tLeft && r == tRight) return SEG[v]; //kết thúc đệ quy và trả kq
    else{
        int tMid = (tLeft + tRight) / 2;
        //l r có thể nằm lệch so vs tMid
        return getSum(2 * v, tLeft, tMid, l, min(tMid, r)) + getSum(2 * v + 1, tMid + 1, tRight, max(tMid + 1, l), r);
    }
}

//Cập nhật giá trị update : O(logN) a[pos] = val
void update(int v, int l, int r, int pos, int val){
    if(l == r) SEG[v] = val;
    else{
        int m = (l + r) / 2;
        //lệch bên trái
        if(pos <= m) update(2 * v, l, m, pos, val);
        //lệch bên phải
        else update(2 * v + 1, m + 1, r, pos, val);
        //giá trị của cây con bên trái hoặc bên phải có thể thay đổi -> cập nhật giá trị cho cha
        SEG[v] = SEG[2 * v] + SEG[2 * v + 1];
    }
}
```
### Fenwick
```cpp
//Cây Fenwick (Binary Indexed Tree) -> cập nhật giá tr và truy vấn tổng của i phần tử trong mảngị 
int const maxN = 1e6 + 5;
int a[maxN], BIT[maxN], n;

//2 thao tác -> tạo mảng bit -> update và query
//update
void update(int pos, int val){
    //từ chỉ số pos -> chỉ số n
    for(; pos <= n; pos += pos & (-pos)) BIT[pos] += val;
    //pos & (-pos) -> lấy ra bit 1 cuối cùng của pos hiện tại và cộng số đó vào pos theo pp bù 2
}

//query -> sum các phần tử từ chỉ số 1 -> pos
int query(int pos){
    int sum = 0;
    for(; pos > 0; pos -= pos & (-pos)) sum += BIT[pos];
    return sum;
}
```
### Test
```cpp
int main(){
    cin >> n;
    //tạo mảng BIT
    for(int i = 1; i <= n; i++){
        cin >> a[i];
        update(i, a[i]);
    }
    //sum từ chỉ số 1 đến số 2
    cout << query(5) - query(1) << endl;
   
   //tạo cây Segment
    build(1, 0, n - 1);
    for(int i = 1; i <= 20; i++){
        if(SEG[i] != 0)
            cout << SEG[i] << ' ';
    }
    cout << endl;
    int l, r;
    cin >> l >> r;
    cout << getSum(1, 0, n - 1, l, r) << endl;
    update(1, 0, 3, 2, 10);
    for(int i = 1; i <= 20; i++){
        if(SEG[i] != 0)
            cout << SEG[i] << ' ';
    }
}
```