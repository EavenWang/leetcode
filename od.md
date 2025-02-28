# od

### [AI处理器组合](https://hydro.ac/d/HWOD2023/p/OD001)

```C++
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

vector<int> link1;
vector<int> link2;

vector<vector<int>> result;
vector<int> path;

void addTolink(int x){
    switch(x){
        case 0:
        case 1:
        case 2:
        case 3:
            link1.push_back(x);
            break;
        case 4:case 5:case 6:case 7:
            link2.push_back(x);
            break;
    }
}

void mutation(vector<int> & link,int start,int number){
    if(path.size() == number){
        result.push_back(path);
        return;
    }

    for(int i = start;i < link.size();i++){
        path.push_back(link[i]);
        mutation(link,i + 1,number);
        path.pop_back();
    }
}

void dealOne(){
    vector<int> priSize = {1,3,2,4};

    for(int i = 0;i < priSize.size();i++){
        if(link1.size() == priSize[i] || link2.size() == priSize[i]){
            if(link1.size() == priSize[i]){
                mutation(link1,0,1);
            }

            if(link2.size() == priSize[i]){
                mutation(link2,0,1);
            }
            break;
        }
    }

}

void dealTwo(){
    vector<int> priSize = {2,4,3};

    for(int i = 0;i < priSize.size();i++){
        if(link1.size() == priSize[i] || link2.size() == priSize[i]){
            if(link1.size() == priSize[i]){
                mutation(link1,0,2);
            }

            if(link2.size() == priSize[i]){
                mutation(link2,0,2);
            }
            break;

            break;
        }
    }

}

int main(){
     
    string input;  

    getline(cin,input);

    for(char & c : input){
        if(isdigit(c)){
            addTolink(c - '0');
        }
    }

    sort(link1.begin(),link1.end());
    sort(link2.begin(),link2.end());

    int number;

    cin >> number;

    if(number == 8){
        if(link1.size() == 4 && link2.size() == 4){
            for(int i = 0;i < 4;i++){
                path.push_back(link1[i]);
            }

            for(int i = 0;i < 4;i++){
                path.push_back(link2[i]);
            }
            result.push_back(path);
        }
    }else if(number == 4){
        if(link1.size() == 4){
            result.push_back(link1);
        }
        if(link2.size() == 4){
            result.push_back(link2);
        }
    }else if(number == 2){
        dealTwo();
    }else{
        dealOne();
    }

    cout << "[";

    for(int j = 0;j < result.size();j++){
        vector<int> pa = result[j];
        cout << "[";
        for(int i = 0;i < pa.size();i++){
            cout << pa[i];
            if(i != pa.size() - 1){
                cout << ", ";
            }
        }
        cout << "]";

        if(j != result.size() - 1){
            cout << ", ";
        }
    }
    cout << "]";
    
    return 0;
}
```

### [寻找链表的中间结点](https://hydro.ac/d/HWOD2023/p/OD002)

```C++
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

const int N = 100010;

int link[N];
int ne[N];

int main(){

    int start,num;

    cin >> start >> num;

    for(int i = 0;i < num;i++){
        int addr,value,next;

        cin >> addr >> value >> next;

        link[addr] = value;
        ne[addr] = next;
    }

    int l,s;

    l = s = start;


    if(num == 2){
        cout << link[ne[start]];
        return 0;
    }

    while(s != -1 && ne[s] != -1){
        s = ne[ne[s]];
        l = ne[l];
    }

    cout << link[l];

    return 0;
}
```

### [ 字符串重新排序](https://hydro.ac/d/HWOD2023/p/OD003)

```C++
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
#include <unordered_map>
#include <sstream>

using namespace std;

unordered_map<string,int> nums;

struct MyCompare{
    bool operator()(string s1,string s2){
        if(nums[s1] != nums[s2]){
            return nums[s1] > nums[s2];
        }else if(s1.size() != s2.size()){
            return s1.size() < s2.size();
        }else{
            return s1 < s2;
        }

    }
};


int main(){

    string input;
    getline(cin,input);

    vector<string> words;

    istringstream iss(input);
    string word;

    while(iss >> word){
        sort(word.begin(),word.end());
        nums[word]++;
        words.push_back(word);
    }

    sort(words.begin(),words.end(),MyCompare());
    
    for(int i = 0;i < words.size();i++){
        cout << words[i];
        if(i != words.size() - 1){
            cout << " ";
        }
    }

    return 0;
}
```

### [找出通过车辆最多颜色](https://hydro.ac/d/HWOD2023/p/OD005)

```C++
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
#include <unordered_map>
#include <unordered_set>
#include <sstream>

using namespace std;

const int N = 100010;

int umap[N];

int main(){

    string input;

    getline(cin,input);

    int num;

    cin >> num;

    int maxId = 0;

    vector<int> colors;
    for(int i = 0;i < input.size();i++){
        if(isdigit(input[i])){
            colors.push_back(input[i] - '0');

            if(input[i] - '0' > maxId){
                maxId = input[i] - '0';
            }
        }
    }
    
    int i;
    for(i = 0;i < colors.size() && i < num;i++){
        umap[colors[i]]++;
    }

    int result = 0;

    for(int i = 0;i <= maxId;i++){
        result = max(result,umap[i]);
    }
    
    int s = 0;
    
    for(int k = i;k < colors.size();k++){
        umap[colors[k]]++;
        umap[colors[s++]]--;

        result = max(result,umap[colors[k]]);
    }

    cout << result;
    return 0;
}
```

### [**OD004**  获得完美走位](https://hydro.ac/d/HWOD2023/p/OD004)

```C++
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
#include <unordered_map>
#include <unordered_set>
#include <sstream>

using namespace std;

const int N = 26;

int umap[N];
int more[N];
int temp[N];

int main(){

    string input;

    getline(cin,input);

    int num = input.size() / 4;

    for(int i = 0;i < input.size();i++){
        umap[input[i] - 'A']++;
    }

    int length = 0;

    for(int i = 0;i < 26;i++){
        if(umap[i] > num){
            more[i] = umap[i] - num;
            length++;
        }
    }

    if(length == 0){
        cout << 0;
        return 0;
    }

    int l = 0;
    int e = 0;
    int result = 0x3fff;
    
    for(int r = 0;r < input.size();r++){
        int id = input[r] - 'A';

        if(more[id] > 0){
            temp[id]++;
            if(temp[id] == more[id]){
                e++;
            }
        }

        while(e == length){
            int ld = input[l] - 'A';
            result = min(result,r - l + 1);
            if(more[ld] > 0){
                temp[ld]--;
                if(temp[ld] < more[ld]) e--;
            }

            l++;
        }
    }

    cout << result;

    return 0;
}
```

### [**OD006**  不含101的数](https://hydro.ac/d/HWOD2023/p/OD006)

```C++
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
#include <unordered_map>
#include <unordered_set>
#include <sstream>

using namespace std;

bool f1,f2;

bool hasIt(int x){
    int flag = 0x1;

    if(x < 5) return false;
    f1 = f2 = false;
    while(x > 0){
        if((x & flag) == 1 && f1 && f2){
            return true;
        }

        if((x & flag) == 1){
            f1 = true;
        }else if( (x & flag) == 0 && f1 == true && f2 == false){
            f2 = true;
        }else{
            f2 = false;
            f1 = false;
        }
        x >>= 1;

    }
    return false;
}

int main(){

    int a,b;

    cin >> a >> b;

    int result = 0;


    for(int i = a;i <= b;i++){
        if(hasIt(i) == false){
            result++;
        }
    }
    
    cout << result;

    return 0;
}
```

### [**OD007**  租车骑绿道](https://hydro.ac/d/HWOD2023/p/OD007)

```C++
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
#include <unordered_map>
#include <unordered_set>
#include <sstream>

using namespace std;


int main(){

    vector<int> people;
    int m,n;

    cin >> m >> n;

    for(int i = 0;i < n;i++){
        int a;
        cin >> a;
        people.push_back(a);
    }

    sort(people.begin(),people.end());

    int result = 0;

    int i = 0,j = people.size() - 1;

    while(i < j){
        if(people[i] + people[j] <= m){
            result++;
            i++;
            j--;
        }else{
            result++;
            j--;
        }
    }
    if(i == j) result++;
    cout << result;

    return 0;
}
```

### [**OD009**  字母组合](https://hydro.ac/d/HWOD2023/p/OD009)

```C++
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
#include <unordered_map>
#include <unordered_set>
#include <sstream>

using namespace std;

string num,word;

string q[10] = {"abc","def","ghi","jkl","mno","pqr","st","uv","wx","yz"};

bool hasIt(const string & s){
    int result = 0;
    for(int i = 0;i < s.size();i++){
        for(int j = 0;j < word.size();j++){
            if(s[i] == word[j]) result++;
        }
    }

    return result == word.size();
}

void helper(int start,string s){
    if(s.size() == num.size()){
        cout << s << ",";
        return;
    }

    int id = num[start] - '0';
    string temp = q[id];
    for(int j = 0;j < temp.size();j++){
        if(hasIt(s + temp[j])){
            continue;
        }
        s.push_back(temp[j]);
        helper(start + 1,s);
        s.pop_back();
    }
    
}

int main(){

    cin >> num >> word;

    helper(0,"");

    return 0;
}
```

### [**OD011**  最小的调整次数](https://hydro.ac/d/HWOD2023/p/OD011)

```C++
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
#include <unordered_map>
#include <unordered_set>
#include <sstream>
#include <set>

using namespace std;

unordered_set<string> words;

struct MyCompare{
    bool operator()(const string & a,const string & b) const {
        if(a.size() == b.size()){
            return a > b;
        }else{
            return a.size() > b.size();
        }
    }
};

int main(){

    string s;
      
    getline(cin,s);
    istringstream iss(s);
    string word;
    while(iss >> word){
        words.insert(word);
    }
    

    set<string,MyCompare> result;

    for(const string & s:words){
        bool flag = true;
        for(int i = 1;i < s.size();i++){
            if(words.find(s.substr(0,i)) == words.end()){
                flag = false;
                break;
            }
        }
        if(flag) result.insert(s);
    }
    
    cout << *(result.begin());

    return 0;
}
```



