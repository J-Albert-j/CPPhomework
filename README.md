# c++ 11月份作业
#### P301	9.1

```C++
vector<int> ivec;  //0
vector<int> ivec(10); //0 
vector<int> ivec(10,1); //1
vector<int> ivec{0,1,2,3,4}; //0,1,2,3,4
vector<int> ivec(other_vec); //same as other vec
vector<int> ivec(other_vec.begin(),other_vec.end()); //same as other_vec
```

#### P302	9.20 

```C++
#include<iostream>
#include<list>
#include<deque>
 
using std::deque;using std::list;using std::endl;using std::cin;using std::cout;
 
int main()
{
	list<int> sde{1,2,3,4,5,6};
	deque<int> d1,d2;
	for(const auto &s:sde)
	{
		if(s%2==0)
			d1.push_back(s);	
		else
			d2.push_back(s);
	}
	for(auto &i1:d1)
		cout<<i1<<" ";
	cout<<endl;
	for(auto &i2:d2)
		cout<<i2<<" ";
	cout<<endl;
 
	return 0;
}
```

#### P315	9.29
***问：假定vec包含25个元素，那么vec.resize(100)会做什么？如果接下来调用vec.resize(10)会做什么？*** 

答：vec.resize(100)将75个值为0的元素添加到vec的末尾，vec.reszie(10)从vec末尾删除90个元素。

#### P324	9.43

```C++
#include<iostream>
#include<string>
 
using std::string;using std::cout;using std::endl;
 
void func(string& s, const string& oldVal,const string& newVal)
{
	for(string::size_type i=0; i!=s.size(); ++i)	
	{
		if(s.substr(i,oldVal.size()) == oldVal)
		{
			s.erase(i,oldVal.size());
			s.insert(i,newVal);
		}
	}
}
 
int main()
{
	string str{"drive straight thru is a foolish, tho courageous act."};
	func(str,"thru","through");
	func(str,"tho","though");
	cout<<str<<endl;
 
	return 0;
}
```

#### P331	9.52

```C++
//使用stack处理括号化的表达式。当你看到一个左括号，将其记录下来。当你在一个左括号之后看到一个右括号，从stack中pop对象，直到遇到左括号，将左括号也一起弹出栈。然后将一个值(括号内 的运算结果)push到栈中，表示一个括号化的(子)表达式已经处理完毕，被其运算结果所替代。

#include<iostream>
#include<string>
#include<stack>
 
using std::string;using std::endl;using std::cout;using std::stack;
 
int main()
{
	stack<char> stk;
	string str{"I am (Groot)"};
	bool find  = false;
 
	for(const auto &s:str)
	{
		if(s == '(')
		{
			find = true;
			continue;
		}
		else if(s == ')')
			find = false;
 
		if(find) stk.push(s);
	}
 
	string repstr;
	while(!stk.empty())
	{
		repstr += stk.top();
		stk.pop();
	}
 
	str.replace(str.find("(")+1,repstr.size(),repstr);
	cout<<str<<endl;
 
	return 0;
}
```

#### P339	10.3	

```C++
// accumulate的使用
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main()
{
    int value;
    vector<int> vec;
    cout << "please input numbers..." << endl;
    while(cin >> value)
        vec.push_back(value);

    int sum = accumulate(vec.cbegin(),vec.cend(),0);
    cout << "Sum of the vector is: " << sum << endl;
    return 0;
}
```

#### P349    10.15 

```C++
#include <iostream>

using namespace std;

int sum(const int &a, const int &b)
{
    auto f = [a](int b){return a + b;};
    return f(b);
}

int main()
{
   int a = 0, b = 0;
   cout << "please input two numbers..." << endl;
   while(cin >> a >> b)
   {
    cout << "sum of a and b is: " << sum(a,b) << endl;
   }
   return 0;
 } 
```

#### P365	10.34

```C++
//使用reverse_iterator逆序打印一个vector
#include <iostream>
#include <algorithm>
#include <vector>
#include <iterator>
using namespace std;

int main()
{
    vector<int> vec = {1,2,3,4,5,6,7,8,9,10};
    ostream_iterator<int> out_iter(cout," ");//打印正序 
    copy(vec.cbegin(),vec.cend(),out_iter);
    cout << endl;

    copy(vec.crbegin(),vec.crend(),out_iter);//打印逆序 
    cout << endl; 
    return 0;
} 
```

#### P370	10.42

```C++
//使用list代替vector重新实现10.2.3节（第343页）中的去除重复单词的程序。

#include <iostream>
#include <string>
#include <list>
#include <algorithm>
#include <iterator>
 
using namespace std;
 
void elimDups(list<string> &words)
{
	words.sort();
	words.unique();  
}
 
int main()
{
	list<string> slist = { "I", "C++", "Primer", "Primer", "C++", "is", "I", "list", "like" };
	elimDups(slist);
	ostream_iterator<string> out_iter(cout, " ");
	copy(slist.begin(), slist.end(), out_iter);
	return 0;
}
```

#### P381	11.12

```C++
copy(v.begin(),v.end(),inserter(c,c.end()));//正确 
copy(v.begin(),v.end(),back_inserter(c)); //错误 multiset没有push_back这个操作，尾插法不适合 
copy(c.begin(),c.end(),inserter(v,v.end()));//正确 
copy(c.begin(),c.end(),back_inserter(v));//正确

//编写程序，读入string和int的序列，将每个string和int存入一个pair中，pair保存在一个vector中

#include <iostream>
#include <string>
#include <vector>
#include <utility>

using namespace std;

int main()
{
    pair<string,int> p;//定义一个空的pair类型 
    vector<pair<string,int>> vec; 
    string word;
    int ival;
    while(cin >> word >> ival )
    {
        p={word,ival};//采用列表初始化方式 
        vec.push_back(p);
    }

    for(int i = 0; i != vec.size();++i)//输出vector里面内容 
    {
        cout << (vec[i]).first << " " << (vec[i]).second << " " << endl;
    }
    return 0;
} 

```

#### P387	11.17

```C++
#include <iostream>
#include <string>
#include <set>
#include <map>
#include <algorithm>

using namespace std;

int main()
{
    multiset<string> c = {"good","good","best","never","let","it","rest"};
    vector<string> v = {"good","good","best","never","let","it","rest"};
     copy(v.begin(),v.end(),inserter(c,c.end()));//正确 
    //copy(v.begin(),v.end(),back_inserter(c)); //错误 multiset没有push_back这个操作，尾插法不适合 
    //copy(c.begin(),c.end(),inserter(v,v.end()));//正确 
     //copy(c.begin(),c.end(),back_inserter(v));//正确 

    for(vector<string>::iterator it = v.begin(); it != v.end(); ++it)
        cout << *it << " ";
        cout << endl;

    for(multiset<string>::iterator it = c.begin(); it != c.end(); ++it)
        cout << *it << " ";
        cout << endl;
    return 0;
} 
```

#### P396	11.38

```C++
#include <iostream>
#include <vector>
#include <map>
#include <fstream>
#include <string>
#include<sstream>
#include <tr1/unordered_map>

using namespace std;

//建立转换映射 
std::tr1::unordered_map<string, string> buildMap(ifstream &map_file)
{
    std::tr1::unordered_map<string, string> trans_map; //保存转换规则
    string key; //要转换的单词
    string value; //替换后的内容
    //读取第一个单词存入key中，行中剩余内容存入value
    while (map_file >> key && getline(map_file, value)) 
        if(value.size() > 1) //检查是否有转换规则
            trans_map[key] = value.substr(1); //跳过前导空格
        else
            throw runtime_error("no rule for " + key);
    return trans_map; 
 } 

//生成转换文本
const string & transform(const string &s, const std::tr1::unordered_map<string,string> &m)
{
    //实际的转换工作；此部分是程序的核心
    auto map_it = m.find(s);
    //如果单词在转换规则map中
    if (map_it != m.end()) //cend改为end 
        return map_it -> second; //使用替换语句
    else
        return s; //否则返回原string 
} 


/*单词转换程序*/
void word_transform(ifstream &map_file, ifstream &input)
{
    auto trans_map = buildMap(map_file); //保存转换规则
    string text;
    while (getline(input, text)) 
    {
        istringstream stream(text);  //读取每个单词
        string word;
        bool firstword = true;      //控制是否打印空格 
        while (stream >> word){
            if (firstword)
                firstword = false;
            else
                cout << " ";      //在单词间打印一个空格
            //transform 返回它的第一个参数或其他转换之后的形式
            cout << transform(word, trans_map); //打印输出 
        } 
        cout << endl;  //完成一行的转换 
     } 
}



int main()
{

    ifstream file1("map_file.txt"); //转换的规则 
    ifstream file2("data_input.txt"); //输入的文件 
    word_transform(file1, file2);
    return 0;
}
```

#### P446	13.12
***3次。item1,item2,accum,退出作用域时，对item1和item2调用析构函数。***

#### P452	13.18

```C++
#include <iostream>
#include <string>

class Employee
{
friend void print(const Employee&);
public:
	Employee() { id = n; ++n; };
	Employee(const std::string &s) { id = n; ++n; name = s; };
private:
	std::string name;
	int id;
	static int n;
};

void print(const Employee &e)
{
	std::cout << e.name << " " << e.id << std::endl;
}

int Employee::n = 0;

int main()
{
	Employee a;
	Employee b("bbb");

	print(a);
	print(b);

	return 0;
}
```

#### P472	13.46

```C++
vector<int> vi(100);
int&& r1 = f();
int& r2 = vi[0];
int& r3 = r1;
int&& r4 = vi[0] * f();
```

#### P481	13.49

#### P485	13.58

```C++
#include <vector>
#include <iostream>
#include <algorithm>
 
using std::vector;
using std::sort;
 
class Foo {
public:
    Foo sorted()&&;
    Foo sorted() const&;
 
private:
    vector<int> data;
};
 
Foo Foo::sorted() &&
{
    sort(data.begin(), data.end());
    std::cout << "&&" << std::endl; // debug
    return *this;
}
 
Foo Foo::sorted() const &
{
    //    Foo ret(*this);
    //    sort(ret.data.begin(), ret.data.end());
    //    return ret;
 
    std::cout << "const &" << std::endl; // debug
 
    //    Foo ret(*this);
    //    ret.sorted();     // Exercise 13.56
    //    return ret;
 
    return Foo(*this).sorted(); // Exercise 13.57
}
 
int main()
{
    Foo().sorted(); // call "&&"
    Foo f;
    f.sorted(); // call "const &"
}
```

#### P493	14.3
"cobble"=="stone"     const char*

svec1[0]==svec2[0]    string版本

svec1==svec2          vector版本

svec1[0]=="stone"     string版本

#### P500	14.20

#### P509	14.38

```C++
#include <string>
#include <iostream>
#include <fstream>
#include <vector>
#include <algorithm>

class CompareString
{
public:
	CompareString(size_t n) : default_size(n) {};
	bool operator()(const std::string &s) const
	{
		return default_size == s.size();
	}
private:
	size_t default_size;
};

int main()
{
	std::ifstream ifs("../ch09_Sequential_Containers/letter.txt");
    if (!ifs) return -1;

    std::vector<std::string> vs;

    for(std::string curr; ifs >> curr; vs.push_back(curr));

    for(int i = 1, n = 0; i < 11; ++i)
    {
    	for(auto iter = vs.begin(); iter != vs.end(); )
    	{
    		iter = std::find_if(iter+1, vs.end(), CompareString(i));
    		if(iter != vs.end()) ++n;
    	}
    	std::cout << "length:" << i << "," << n << std::endl;
    	n = 0;
    }
    	

	return 0;
}
```

#### P522	14.52
```C++
operator+(int, double)
SmallInt->int,LongDouble->double
operator+(int, float)
SmallInt->int,LongDouble->float
SmallInt operator+(const SmallInt&, const SmallInt&)
编译器只能执行一个用户定义的类型转换。
上述加法表达式具有二义性。

LongDouble operator(const SmallInt&)
精确匹配
operator+(double, int)
LongDouble->double，SmallInt->int
operator+(float, int) 
LongDouble->float，SmallInt->int
```

#### P539	15.12

```C++
// Quote类
class Quote
{
public:
    Quote() = default;
    Quote(const std::string &book, double sales_price) : bookNo(book), price(sales_price) {}
    std::string isbn() const { return bookNo; }
    // return the total sales of given number books.
    // the derived class need overwrite the method of calculate the discount.
    virtual double net_price(std::size_t n) const { return n * price; }
    virtual ~Quote() = default;
private:
    std::string bookNo;     // isbn number
protected:
    double price = 0.0;     // origin price
};
// print_total函数
double print_total(ostream& os, const Quote& item, size_t n)
{
    double ret = item.net_price(n);
    os << "ISBN: " << item.isbn() << " # sold: " << n << " total due: " << ret << endl;
    return ret;
}
```

#### P542	15.16

```C++
//改写你在15.2.2节（第533页）练习中编写的数量受限的折扣策略，令其继承Disc_quote。
class Limit_quote : public Disc_quote
{
public:
    Limit_quote() = default;
    Limit_quote(const std::string& b, double p, std::size_t max, double disc):
        Disc_quote(b, p, max, disc)  {   }

    double net_price(std::size_t n) const override
    { return n * price * (n < quantity ? 1 - discount : 1 ); }
};
```

#### P562	15.30

```C++
#include <iostream>
#include <memory>
#include <set>
#include "Quote.h"

using namespace std;

class Basket
{
public:
    void add_item(const shared_ptr<Quote> &sales)
    {
        items.insert(sales);
    }
    double total_receipt (std::ostream&) const;     // 打印每本书的总价和购物篮中所有书的总价
private:
    static bool compare(const std::shared_ptr<Quote> &lhs, const std::shared_ptr<Quote> &rhs)
    {
        return lhs->isbn() < rhs->isbn();
    }
    // multiset保存多个报价，按照compare成员排序
    std::multiset<std::shared_ptr<Quote>, decltype(compare)*> items{compare};
};

double Basket::total_receipt(std::ostream &os) const
{
    double sum = 0.0;

    for (auto iter = items.cbegin(); iter != items.cend(); iter=items.upper_bound(*iter))
    {
        sum += print_total (os, **iter, items.count(*iter));
    }
    os << "Total Sale: " << sum << endl;
    return  sum;
}
```
