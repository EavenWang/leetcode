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

