# Algorithm

## 2024

### [206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/)

```C++
//方法一   
ListNode* reverseList(ListNode* head) {
        if(head == nullptr || head->next == nullptr) return head;

        ListNode * newHead = reverseList(head->next);

        ListNode * curr = newHead;

        while(curr->next != nullptr){
            curr = curr->next;
        }
        curr->next = head;
        head->next = nullptr;
        return newHead;
    }

//方法二
ListNode* reverseList(ListNode* head) {
        if(head == nullptr || head->next == nullptr) return head;
        ListNode * newHead = head;
        ListNode * temp;

        for(ListNode * curr = head->next;curr != nullptr;){
            temp = curr;
            curr = curr->next;
            temp->next = newHead;
            newHead = temp;
        }
        head->next = nullptr;
        return newHead;
    }
```

### [24. 两两交换链表中的节点](https://leetcode.cn/problems/swap-nodes-in-pairs/)

```C++
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if(head == nullptr || head->next == nullptr) return head;
        
        ListNode * dummy = new ListNode();
        dummy->next = head;
        ListNode * pre = dummy;
        ListNode * curr = pre->next;
        
        while(curr != nullptr && curr->next != nullptr){
            pre->next = curr->next;
			curr->next = curr->next->next;
            pre->next->next = curr;
            pre = curr;
            curr = pre->next;

        }
        head = dummy->next;
        delete dummy;
        return head;
    }
};
```

### [19. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

```C++
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode * dummy = new ListNode();
        dummy->next = head;
        
        int i = 1;
        
        ListNode * curr = dummy->next;
        while(curr != nullptr){
            if(i == n){
                break;
            }
            i++;
            curr = curr->next;
        }
        
        ListNode * pre = dummy;
        
        while(curr->next != nullptr){
            pre = pre->next;
            curr = curr->next;
        }
        pre->next = pre->next->next;
        
        head = dummy->next;
        delete dummy;
        return head;   
    }
};
```

### [面试题 02.07. 链表相交](https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/)

```C++
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode * p = headA;
        ListNode * q = headB;
        
        while(p != q){
            if(p == nullptr) p = headB;
            else  p = p->next;

            if(q == nullptr) q = headA;
            else q = q->next;
        }
        
        if(p == nullptr) return nullptr;
        
        return p;
    }
};
```

### [142. 环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/)

```C++
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        if(head == nullptr || head->next == nullptr) return nullptr;
        
        ListNode * slow = head;
        ListNode * fast = head;
        
        while(fast != nullptr && fast->next != nullptr){
            slow = slow->next;
            fast = fast->next->next;
            
            if(slow == fast) break;
        }
        
        if(fast == nullptr || fast->next == nullptr) return nullptr;
        
        
        slow = head;
    
        while(slow != fast){
            fast = fast->next;
            slow = slow->next;
        }
        return slow;
    }
};
```

### [242. 有效的字母异位词](https://leetcode.cn/problems/valid-anagram/)

```C++
class Solution {
public:
    bool isAnagram(string s, string t) {
        int sHash[26] = {0};
        int tHash[26] = {0};
        
        for(const char & c : s){
            sHash[c - 'a']++;
        }
        
        for(const char & c : t){
            tHash[c - 'a']++;
        }
        
        for(int i = 0;i < 26;i++){
            if(sHash[i] != tHash[i]) return false;
        }
        
        return true;
    }
};
```

### [383. 赎金信](https://leetcode.cn/problems/ransom-note/)

```C++
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        int myHash[26] = {0};
        
        for(char & c : magazine){
            myHash[c - 'a']++;
        }
        
        for(char & c : ransomNote){
            myHash[c - 'a']--;
        }
        
        for(int i = 0;i < 26;i++){
            if(myHash[i] < 0) return false;
        }
        return true;
    }
};
```

### [49. 字母异位词分组](https://leetcode.cn/problems/group-anagrams/)

```C++
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        vector<vector<string>> result;
        
        unordered_map<string,unordered_set<int>> myMap;
        
        for(int i = 0;i < strs.size();i++){
            string s = strs[i];
            sort(s.begin(),s.end());
            myMap[s].insert(i);
        }
        
        for(const pair<string,unordered_set<int>> & p : myMap){
            vector<string> myV;
            
            for(const int & id : p.second){
                myV.push_back(strs[id]);
            }
            result.push_back(myV);  
        }
        return result;
    }
};
```

### [438. 找到字符串中所有字母异位词](https://leetcode.cn/problems/find-all-anagrams-in-a-string/)

```C++
class Solution {
public:  
    vector<int> findAnagrams(string s, string p) {
        if(s.size() < p.size()) return vector<int>();
        
        vector<int> result;
        
        int pHash[26] = {0};
        
        for(const char & c : p){
            pHash[c - 'a']++;
        }
        
        int i = 0;
        
        int sLen = s.size(),pLen = p.size();
        
        for(int j = 0;j < s.size();j++){
            int id = s[j] - 'a';
            pHash[id]--;
            
            while(pHash[id] < 0){
                pHash[s[i] - 'a']++;
                i++;
            }
            
            if(j - i + 1 == pLen){
                result.push_back(i);
            }
            
        }
        return result;
    }
};
```

### [349. 两个数组的交集](https://leetcode.cn/problems/intersection-of-two-arrays/)

```C++
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_map<int,int> myHash;
        
        for(int i = 0;i < nums1.size();i++){
            myHash[nums1[i]]++;
        }
        
        vector<int> result;
        
        for(int i = 0;i < nums2.size();i++){
            if(myHash[nums2[i]] > 0){
                result.push_back(nums2[i]);
                myHash[nums2[i]] = 0;
            }
        }
        return result;
    }
};
```

### [350. 两个数组的交集 II](https://leetcode.cn/problems/intersection-of-two-arrays-ii/)

```C++
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        unordered_map<int,int> myHash;
        
        for(int i = 0;i < nums1.size();i++){
            myHash[nums1[i]]++;
        }
        
        vector<int> result;
        
        for(int i = 0;i < nums2.size();i++){
            if(myHash[nums2[i]] > 0){
                result.push_back(nums2[i]);
                myHash[nums2[i]]--;
            }
        }
        return result;
    }
};
```

### [202. 快乐数](https://leetcode.cn/problems/happy-number/)

```C++
class Solution {
public:
    int getSum(int n){
        int sum = 0;
        while(n > 0){
            int remainder = n % 10;
            sum = sum + remainder * remainder;
            n /= 10;
        }
        return sum;
    }
    
    bool isHappy(int n) {
        
        unordered_set<int> sums;
        
        while(true){
            int sum = getSum(n);
            n = sum;
            if(sum == 1) break;
            if(sums.find(sum) == sums.end()){
                sums.insert(sum);
            }else{
                return false;
            }
        }
        
        return true;
    }
};
```

### [454. 四数相加 II](https://leetcode.cn/problems/4sum-ii/)

```C++
class Solution {
public:
    int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
        
        int result = 0;
        unordered_map<int,int> uMap;
        
        for(const int & c : nums1){
            for(const int & d : nums2){
                uMap[c + d]++;
            }
        }
        
        for(const int & c : nums3){
            for(const int & d : nums4){
                if(uMap[0 - c - d] != 0){
                    result += uMap[0 - c - d];
                }
            }
        }
        return result;
    }
};
```

### [15. 三数之和](https://leetcode.cn/problems/3sum/)

```C++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        
        vector<vector<int>> result;
        
        for(int i = 0;i < nums.size();i++){
            
            if(nums[i] > 0) break;
            
            if( i > 0 && nums[i] == nums[i - 1]) continue;
            
            int j = i + 1,z = nums.size() - 1;
            
            while(j < z){
                
                if(nums[i] + nums[j] + nums[z] > 0) z--;
                else if(nums[i] + nums[j] + nums[z] < 0) j++;
                else{
                    result.push_back({nums[i],nums[j],nums[z]});
                    j++;
                    z--;
                    while(j < z && nums[z] == nums[z + 1]) z--;
                    while(j < z && nums[j] == nums[j - 1]) j++;
                }
              
            }  
        }
     	return result;
    }
};
```

### [18. 四数之和](https://leetcode.cn/problems/4sum/)

```C++
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        
        sort(nums.begin(),nums.end());
        
        vector<vector<int>> result;
        
        for(int i = 0;i < nums.size();i++){
            int a = nums[i];
            
            if(i > 0 && a == nums[i - 1]) continue;
            
            for(int j = i + 1;j < nums.size();j++){
                int b = nums[j];
                
                if(j > i + 1 && b == nums[j - 1]) continue;
                
                int l = j + 1,r = nums.size() - 1;
                
                while(l < r){
                    
                    if(a + b > target - nums[l] - nums[r]) r--;
                    else if(a + b < target - nums[l] - nums[r]) l++;
                    else{
                        result.push_back({a,b,nums[l],nums[r]});
                        l++;
                        r--;
                        
                        while(l < r && nums[l] == nums[l - 1]) l++;
                        while(l < r && nums[r] == nums[r + 1]) r--;
                    }
                }
            }
        }
        
        return result;
    }
};
```

### [344. 反转字符串](https://leetcode.cn/problems/reverse-string/)

```C++
class Solution {
public:
    
    void swapHelper(char & a,char & b){
        char temp = a;
        a = b;
        b = temp;
    }
    
    void reverseString(vector<char>& s) {
        int l = 0,r = s.size() - 1;
        
        while(l < r){
            swapHelper(s[l],s[r]);
            l++;
            r--;
        }
    }
};
```

### [541. 反转字符串 II](https://leetcode.cn/problems/reverse-string-ii/)

```C++
class Solution {
public:
    
    void customSwap(string & s,int l,int r){
        while(l < r){
                swap(s[l],s[r]);
                l++;
                r--;
        }
    }
    
    string reverseStr(string s, int k) {
        int sLen = s.size();
        int i = 0;
        for(;i + 2 * k < sLen;i+= 2 * k){
            customSwap(s,i,i + k - 1);  
        }
        
        if(i + k < sLen){
            customSwap(s,i,i + k - 1);
        }else if(i + k >= sLen){
            customSwap(s,i,s.size() - 1);
        }
        return s;
    }
};
```

### [151. 反转字符串中的单词](https://leetcode.cn/problems/reverse-words-in-a-string/)

```C++
class Solution {
public:
    string reverseWords(string s) {
        vector<string> myStrs;
        
        string word;
        bool spaceFirst = false;
        int sLen = s.size();
        
        for(int i = 0;i < sLen;i++){
            
            if(s[i] != ' '){
                word += s[i];
            }else if(word.size() != 0){
                myStrs.push_back(word);
                word = "";
            }
        }
        
        if(word.size() != 0){
            myStrs.push_back(word);
        }
        
        string result;
        
        for(int i = myStrs.size() - 1;i >= 0;i--){
            if(i == myStrs.size() - 1) result = myStrs[i];
            else result = result + " " + myStrs[i];
            
        }
        return  result;
    }
};


//方法二

class Solution {
public:
    
   	void removeSpaces(string & s){
        
        int slow = 0
        
        for(int i = 0;i < s.size();i++){
            if(s[i] != ' '){
                while(i < s.size() && s[i] != ' '){
                    s[slow++] = s[i++];
                }
                s[slow++] = ' ';
            }
        }
        s.resize(slow - 1);
    }
    
    void reverseString(string & s,int l,int r){
        while(l < r){
            swap(s[l],s[r]);
            l++;
            r--;
        }
    }
    
    string reverseWords(string s) {
        removeSpaces(s);
        
        reverseString(s);
        
        int start;
        
        for(int i = 0;i <= s.size();i++){
            if(s[i] != ' '){
                start = i;
                while(i < s.size() && s[i] != ' '){
                    i++;
                }
                reverseString(s,start,i - 1);
            }
        }
        
        return s;
    }
};
```

### [28. 找出字符串中第一个匹配项的下标(KMP)](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/)

```C++
class Solution {
public:
    int strStr(string haystack, string needle) {
       	vector<int> ne(needle.size(),-1);
        
        for(int i = 1,j = -1;i < needle.size();i++){
            while(j != -1 && needle[i] != needle[j + 1]) j = ne[j];
            if(needle[i] == needle[j + 1]) j++;
            ne[i] = j;
        }
        
        for(int i = 0,j = -1;i < haystack.size();i++){
            while(j != -1 && haystack[i] != needle[j + 1]) j = ne[j];
            if(haystack[i] == needle[j + 1])j++;
            if(j == needle.size() - 1){
                return i - needle.size();
            }
        }
        
        return -1;
    }
};
```



### [27. 移除元素](https://leetcode.cn/problems/remove-element/)

```C++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int l = 0;
        
        for(int i = 0;i < nums.size();i++){
            if(nums[i] != val){
                swap(nums[l],nums[i]);
                l++;
            }
        }
        return l;
    }
};
```

### [26. 删除有序数组中的重复项](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/)

```C++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int l = 0;
        
        for(int i = 0;i < nums.size();i++){
            if(nums[i] != nums[l]){
                l++;
                nums[l] = nums[i];
            }
        }
        return l + 1;
    }
};
```

### [283. 移动零](https://leetcode.cn/problems/move-zeroes/)

```C++
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int l = 0;
        
        for(int i = 0;i < nums.size();i++){
            if(nums[i] != 0){
                swap(nums[i],nums[l]);
                l++;
            }
        }
    }
};
```

### [844. 比较含退格的字符串](https://leetcode.cn/problems/backspace-string-compare/)

```C++
class Solution {
public:
    bool backspaceCompare(string s, string t) {
        int l = 0;
        
        for(int i = 0;i < s.size();i++){
            if(s[i] != '#'){
                s[l] = s[i];
                l++;
            }else{
                l--;
                if(l < 0) l = 0;
            }
        }
        
        int r = 0;
        
        for(int i = 0;i < t.size();i++){
            if(t[i] != '#'){
                t[r] = t[i];
                r++;
            }else{
                r--;
                if(r < 0) r = 0;
            }
        }
        
        return s.substr(0,l) == t.substr(0,r);
    }
};
```

### [232. 用栈实现队列](https://leetcode.cn/problems/implement-queue-using-stacks/)

```C++
class MyQueue {
public:
    
    stack<int> input;
    stack<int> output;
    
    MyQueue() {
        
    }
    
    void push(int x) {
        input.push(x);
    }
    
    int pop() {
        if(!output.empty()){
            int top = output.top();
            output.pop();
            return top;
        }
        
        while(!input.empty()){
            output.push(input.top());
            input.pop();
        }
        
        int top = output.top();
        output.pop();
        return top;
    }
    
    int peek() {
        if(!output.empty()){
            return output.top();
        }
        while(!input.empty()){
            output.push(input.top());
            input.pop();
        }
        return output.top();
    }
    
    bool empty() {
        return output.empty() && input.empty();
    }
};

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue* obj = new MyQueue();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->peek();
 * bool param_4 = obj->empty();
 */
```

### [225. 用队列实现栈](https://leetcode.cn/problems/implement-stack-using-queues/)

```C++
class MyStack {
public:
    
    queue<int> input;
    queue<int> output;
    
    MyStack() {
        
    }
    
    void push(int x) {
        input.push(x);
    }
    
    int pop() {
        if(input.empty()){
            while(!output.empty()){
                int temp = output.front();
                output.pop();
                if(output.empty()) return temp;
                input.push(temp);
            }
        }else{
            while(!input.empty()){
                int temp = input.front();
                input.pop();
                if(input.empty()) return temp;
                output.push(temp);
            }
        }
        return -1;
    }
    
    int top() {
        if(input.empty()){
            return output.back();
        }else{
            return input.back();
        }
    }
    
    bool empty() {
        return output.empty() && input.empty();
    }
};

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack* obj = new MyStack();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->top();
 * bool param_4 = obj->empty();
 */
```

### [20. 有效的括号](https://leetcode.cn/problems/valid-parentheses/)

```C++
class Solution {
public:
    bool isValid(string s) {
        stack<char> myS;
        
        for(const char & c:s){
            if(c == '(') myS.push(')');
            else if(c == '{') myS.push('}');
            else if(c == '[') myS.push(']');
            else{
                if(myS.empty() || myS.top() != c) return false;
                else myS.pop();
            }
        }
       	
        return myS.empty();
    }
};
```

### [1047. 删除字符串中的所有相邻重复项](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/)

```C++
class Solution {
public:
    string removeDuplicates(string s) {
        string result = ""; 
        for(const char & c:s){
            if(result.empty()){
                result.push_back(c);
            }else{
                if(result.back() == c){
                    result.pop_back();
                }else{
                    result.push_back(c);
                }
            }
        }
        return result;
    }
};
```

### [150. 逆波兰表达式求值](https://leetcode.cn/problems/evaluate-reverse-polish-notation/)

```C++
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        stack<int> myStack;
        
        for(int i = 0;i < tokens.size();i++){
            if(tokens[i] == "*"){
                int a = myStack.top();
                myStack.pop();
                int b = myStack.top();
                myStack.pop();
                myStack.push(a * b);
            }else if (tokens[i] == "/"){
                int a = myStack.top();
                myStack.pop();
                int b = myStack.top();
                myStack.pop();
                myStack.push(b / a);
            }else if(tokens[i] == "+"){
                int a = myStack.top();
                myStack.pop();
                int b = myStack.top();
                myStack.pop();
                myStack.push(a + b);
            }else if(tokens[i] == "-"){
                int a = myStack.top();
                myStack.pop();
                int b = myStack.top();
                myStack.pop();
                myStack.push(b - a);
            }else{
                myStack.push(atoi(tokens[i].c_str()));
            }
        }
        return myStack.top();
    }
};
```

### [239. 滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/)

```C++
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        deque<pair<int,int>> myQ;
        vector<int> result;
        
        for(int i = 0;i < nums.size();i++){
                        
            while(!myQ.empty() && myQ.front().first < i - k + 1){
                myQ.pop_front();
            }
                            
            if(myQ.empty() || myQ.back().second >= nums[i]){
                myQ.push_back({i,nums[i]});
            }else{
                while(!myQ.empty() && myQ.back().second < nums[i]) myQ.pop_back();
                myQ.push_back({i,nums[i]});
            }
            
            if(i >= k - 1) result.push_back(myQ.front().second);
        }
        
        if(result.size() == 0) return {myQ.front().second};
        
        return result;
    }
};
```

### [347. 前 K 个高频元素](https://leetcode.cn/problems/top-k-frequent-elements/)

```C++
class Solution {
public:
    class myComp{
        public:
        bool operator()(const pair<int,int> lhs,const pair<int,int> rhs){
            return lhs.second > rhs.second;
        }
    };
    
    
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int,int> umap;
        
        
        vector<int> result;
        
        for(const int & i : nums){
            umap[i]++;
        }
        
        priority_queue<pair<int,int>,vector<pair<int,int>>,myComp> pri_queue;
        for(const pair<int,int> & p : umap){
            pri_queue.push(p);
            
            while(pri_queue.size() > k){
                pri_queue.pop();
            }
        }
        
        for(int i = 0;i < k;i++){
            result.push_back(pri_queue.top().first);
            pri_queue.pop();
        }
        return result;
    }
};
```

### [144. 二叉树的前序遍历](https://leetcode.cn/problems/binary-tree-preorder-traversal/)

```C++
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        stack<TreeNode *> myS;
        if(root == null) return {};
        
        myS.push(root);
        vector<int> result;
        
        while(!myS.empty()){
            TreeNode * temp = myS.top();
            result.push_back(temp->val);
            myS.pop(); 
            if(temp->right != nullptr) myS.push(temp->right);
            if(temp->left != nullptr) myS.push(temp->left);
        }
        return result;
        
    }
};
```

### [94. 二叉树的中序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/)

```C++
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        stack<TreeNode * > myS;
        vector<int> result;
        TreeNode * curr = root;
        
        while(curr != nullptr || !myS.empty()){
            if(curr != nullptr){
                myS.push(curr);
                curr = curr->left;
            }else{
                TreeNode * temp = myS.top();
                myS.pop();
                
                result.push_back(temp->val);
                
                curr = temp->right;
            }
        }
        return result;
    }
};
```

### [145. 二叉树的后序遍历](https://leetcode.cn/problems/binary-tree-postorder-traversal/)

```C++
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        stack<TreeNode *>myS;
        
        vector<int> result;
        
        if(root == nullptr) return result;

        myS.push(root);
        
        while(!myS.empty()){     
            TreeNode * temp = myS.top();
            result.push_back(temp->val);
            myS.pop();
            
            if(temp->left != nullptr) myS.push(temp->left);
            if(temp->right != nullptr) myS.push(temp->right);
        }
        
        reverse(result.begin(),result.end());
        
        return result;
    }
};
```

### [102. 二叉树的层序遍历](https://leetcode.cn/problems/binary-tree-level-order-traversal/)

```C++
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        queue<TreeNode *> myQ;
        
        vector<vector<int>> result;
        
        if(root == nullptr) return result;
        
        myQ.push(root);
        
        while(!myQ.empty()){
            
            int size = myQ.size();
            vector<int> myV;
            
            for(int i = 0;i < size;i++){
                TreeNode * temp = myQ.front();
            	myQ.pop();
            	myV.push_back(temp->val);
                
                if(temp->left != nullptr) myQ.push(temp->left);
                if(temp->right != nullptr) myQ.push(temp->right);
            }
			result.push_back(myV);
        }
        
        return result;
    }
};
```

### [107. 二叉树的层序遍历 II](https://leetcode.cn/problems/binary-tree-level-order-traversal-ii/)

```C++
class Solution {
public:
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        queue<TreeNode * > myQ;
        vector<vector<int>> result;
        
        if(root == nullptr) return result;
        myQ.push(root);
        
        while(!myQ.empty()){
            int size = myQ.size();
            
            vector<int> myV;
            for(int i = 0;i < size;i++){
                TreeNode * temp = myQ.front();
                myQ.pop();
                myV.push_back(temp->val);
                if(temp->left) myQ.push(temp->left);
                if(temp->right) myQ.push(temp->right);
            }
            result.push(myV);
        }
        
        reverse(result.begin(),result.end());
        return result;
    }
};
```

### [199. 二叉树的右视图](https://leetcode.cn/problems/binary-tree-right-side-view/)

```C++
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        vector<int> result;
        queue<TreeNode*> myQ;
       	
        if(root == nullptr) return result;
        
        myQ.push(root);
        
        while(!myQ.empty()){
            int size = myQ.size();
            
            for(int i = 0;i < size;i++){
                TreeNode * temp = myQ.top();
                myQ.pop();
                if(i == size - 1) result.push_back(temp->val);
                if(temp->left) myQ.push(temp->left);
                if(temp->right) myQ.push(temp->right);
            }
        }
       
        return result;
    }
};
```

### [429. N 叉树的层序遍历](https://leetcode.cn/problems/n-ary-tree-level-order-traversal/)

```C++
class Solution {
public:
    vector<vector<int>> levelOrder(Node* root) {
        vector<vector<int>> result;
        queue<Node*> myQ;
        
        if(root == nullptr) return result;
        
        myQ.push(root);
        
        while(!myQ.empty()){
            int size = myQ.size();
            
            vector<int> myV;
            
            for(int i = 0;i < size;i++){
                Node * temp = myQ.front();
                myQ.pop();
                myV.push_back(temp->val);
                for(int i = 0;i < temp->children.size();i++){
                    if(children[i]) myQ.push(children[i]);
                }
            }
            
            result.push_back(myV);
        }
        return result;
        
    }
};
```

### [226. 翻转二叉树](https://leetcode.cn/problems/invert-binary-tree/)

```C++
class Solution {
public:
    
    void swapTree(TreeNode *& lhs,TreeNode *& rhs){
        TreeNode * temp = lhs;
        
        lhs = rhs;
        
        rhs = temp;
        
    }
    
    TreeNode* invertTree(TreeNode* root) {
        if(root == nullptr) return root;
        
        swapTree(root->left,root->right);
        
        invertTree(root->left);
        invertTree(root->right);
        
        return root;
    }
};
```

### [101. 对称二叉树](https://leetcode.cn/problems/symmetric-tree/)

```C++
class Solution {
public:
    
    bool cmp(vector<TreeNode*> & myV){
        int i = 0,j = myV.size() - 1;
        
        while(i < j){
            TreeNode * lhs = myV[i];
            TreeNode * rhs = myV[j];
            
            if(lhs && rhs){
                if(lhs->val != rhs->val) return false;
            }else if(lhs || rhs) return false;
            
            i++;
            j--;
        }
        return true;
    }
    
    
    bool isSymmetric(TreeNode* root) {
        queue<TreeNode*> myQ;
        
        if(root == nullptr) return true;
        
        myQ.push(root);
        
        while(!myQ.empty()){
            int size = myQ.size();
            
            vector<TreeNode*> myV;
            for(int i = 0;i < size;i++){
                TreeNode * temp = myQ.front();
                myV.push_back(temp);
                myQ.pop();
                if(temp != nullptr){
                    if(temp->left) myQ.push(temp->left);
                    else myQ.push(nullptr);
                    
                    if(temp->right) myQ.push(temp->right);
                    else myQ.push(nullptr);
                }
            }
            if(!cmp(myV)){
                return false;
            }
        }
        return true;
    }
};
```

### [100. 相同的树](https://leetcode.cn/problems/same-tree/)

```C++
class Solution {
public:
    
    bool cmp(TreeNode * lhs,TreeNode * rhs){
        if(lhs && rhs){
            if(lhs->val != rhs->val) return false;
            
            bool result1 = cmp(lhs->left,rhs->left);
            bool result2 = cmp(lhs->right,rhs->right);
            
            return result1 && result2;
        }else if(lhs || rhs) return false;
        else return true;
    }
    
    bool cmp2(TreeNode * lhs,TreeNode * rhs){
        queue<TreeNode*> myQ;
        
        myQ.push(lhs);
        myQ.push(rhs);
        
        while(!myQ.empty()){
            TreeNode * tmp1 = myQ.front();myQ.pop();
            TreeNode * tmp2 = myQ.front();myQ.pop();
            
            if(!tmp1 && !tmp2) continue;
            
            if(tmp1 && tmp2){
                if(tmp1->val != tmp2->val) return false;
                
                myQ.push(tmp1->left);
                myQ.push(tmp2->left);
                myQ.push(tmp1->right);
                myQ.push(tmp2->right);
            }else return false;
        }
        return true;
    }
   
    bool isSameTree(TreeNode* p, TreeNode* q) {
        return cmp2(p,q);
    }
};
```

### [572. 另一棵树的子树](https://leetcode.cn/problems/subtree-of-another-tree/)

```C++
class Solution {
public:
    
    bool cmp(TreeNode * lhs,TreeNode * rhs){
        if(lhs && rhs){
            if(lhs->val != rhs->val) return false;
            
            bool result1 = cmp(lhs->left,rhs->left);
            bool result2 = cmp(lhs->right,rhs->right);
            
            return result1 && result2;
        }else if(lhs || rhs) return false;
        else return true;
    }
    
    bool isSubtree(TreeNode* root, TreeNode* subRoot) {
        if(cmp(root,subRoot)){
            return true;
        }
        if(root && subRoot){
            bool result1 = isSubtree(root->left,subRoot);
            bool result2 = isSubtree(root->right,subRoot);
    		return result1 || result2;
        }else if(root || subRoot) return false;
        else return true; 
    }
};
```

### [104. 二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)

```C++
class Solution {
public:
    int maxDepth(TreeNode* root) {
    	queue<TreeNode *> q;
        
        if(root == nullptr) return 0;
        
        q.push(root);
        int result = 0;
        while(!q.empty()){
            int size = q.size();
            result++;
            for(int i = 0;i < size;i++){
                TreeNode * temp = q.front();
                q.pop();
                if(temp->left) q.push(temp->left);
                if(temp->right) q.push(temp->right);
        
            }
        }
        return result;
    }
};

class Solution {
public:
    
    int getDepth(TreeNode * root){
        if(root == nullptr) return 0;
        
        int l = getDepth(root->left);
        int r = getDepth(root->right);
        
        return max(l,r) + 1;
    }
    
    int maxDepth(TreeNode* root) {
        return getDepth(root);
    }
}
```

### [559. N 叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-n-ary-tree/)

```C++
class Solution {
public:
    int maxDepth(Node* root) {
        queue<Node*> q;
        
        if(root == nullptr) return 0;
        
        q.push(root);
        int result = 0;
        
        while(!q.empty()){
            int size = q.size();
            
            result++;
            for(int i = 0;i < size;i++){
                Node * temp = q.front();
                q.pop();
                
                for(int i = 0;i < temp->children.size();i++){
                    if(temp->children[i]) q.push(temp->children[i]);
                }
            }
        }
        return result;
    }
};

class Solution {
public:
    
    int getDepth(Node * root){
        if(root == nullptr) return 0;

		int maxD = 0;
        for(int i = 0;i < root->children.size();i++){
            int d = getDepth(root->children[i]);
            maxD = max(d,maxD);
        }
        return maxD + 1;
    }
    
    int maxDepth(Node* root) {    
       return getDepth(root);
    }
};
```

### [222. 完全二叉树的节点个数](https://leetcode.cn/problems/count-complete-tree-nodes/)

```C++
class Solution {
public:
    int countNodes(TreeNode* root) {
        queue<TreeNode * > q;
        
        if(root == nullptr) return 0;
        
        q.push(root);
        
        int result = 0;
        
        while(!q.empty()){
            TreeNode * temp = q.front();
            q.pop();
            result++;
            
            if(temp->left) q.push(temp->left);
            if(temp->right) q.push(temp->right);
        }
        return result;
    }
};
```

### [257. 二叉树的所有路径](https://leetcode.cn/problems/binary-tree-paths/)

```C++
class Solution {
public:
    
    void helper(vector<string> & myV,string s,TreeNode * root){
        stringstream ss;
        ss << root->val;
        
        if(root->left == nullptr && root->right == nullptr){
            s = s + ss.str();
            myV.push_back(s);
            return;
        }
		s = s + ss.str(); 
        if(root->left){
            s = s + "->";
  			helper(myV,s,root->left);
            s.pop_back();
            s.pop_back();
        }

        if(root->right){
            s = s + "->";
  			helper(myV,s,root->right);
            s.pop_back();
            s.pop_back();
        }
    }

    vector<string> binaryTreePaths(TreeNode* root) {
        if(root == nullptr) return {};
		vector<string> result;
        string s = "";
        helper(myV,s,root);
    }
};
```

### [404. 左叶子之和](https://leetcode.cn/problems/sum-of-left-leaves/)

```C++
class Solution {
public:
     
    int sumOfLeftLeaves(TreeNode* root) {
    	if(root == nullptr) return 0;
        
    	if(root->left && (root->left->left == nullptr && root->left->right == nullptr)){
            return root->left->val +  sumOfLeftLeaves(root->right);
        }else{
            return sumOfLeftLeaves(root->left) + sumOfLeftLeaves(root->right);
        }    
        
    }
};

class Solution {
public:
    int sumOfLeftLeaves(TreeNode* root) {
        stack<TreeNode *> st;
        
        if(root == nullptr) return 0;
        
        st.push(root);
        int sum = 0;
        while(!st.empty()){
            TreeNode * temp = st.top();
            st.pop();
            
            if(temp->left && (temp->left->left == nullptr && temp->left->right == nullptr)){
                sum += temp->left->val;
            }
            
            if(temp->left) st.push(temp->left);
            if(temp->right) st.push(temp->right);
        }
        return sum;
    }
};
```

### [513. 找树左下角的值](https://leetcode.cn/problems/find-bottom-left-tree-value/)

```C++
class Solution {
public:
    int findBottomLeftValue(TreeNode* root) {
        queue<TreeNode * > q;
        
        if(root == nullptr) return 0;
        
        q.push(root);
        int result = 0;
        while(!q.empty()){
            int size = q.size();
            
            for(int i = 0;i < size;i++){
                TreeNode * temp = q.front();
                q.pop();
                
                if(i == 0) result = temp->val;
                
                if(temp->left) q.push(temp->left);
                if(temp->right) q.push(temp->right);
            }
        } 
        return result;
    }
};


class Solution {
public:
    
    int result;
    int maxDepth = 0;
    
    void helper(TreeNode * root,int depth){
        if(root->left == nullptr && root->right == nullptr){
            if(depth > maxDepth){
                result = result->val;
                maxDepth = depth;
            }
            return;
        }
        
        if(temp->left) helper(root->left,depth + 1);
        if(temp->right) helper(root->right,depth + 1);
    }
    
    int findBottomLeftValue(TreeNode* root) {
        if(root == nullptr) return 0;
        helper(root,1);
        return result;
    }
};
```

### [112. 路径总和](https://leetcode.cn/problems/path-sum/)

```C++
class Solution {
public:
    
    bool hasPathSumHelper(TreeNode * root,int targetSum){
        if(root->left == nullptr && root->right == nullptr && targetSum == root->val){
            return true;
        }
        
        bool result1 = false,result2 = false;
        if(root->left) result1 = hasPathSumHelper(root->left,targetSum - root->val);
        if(root->right) result2 = hasPathSumHelper(root->right,targetSum- root->val);
        
        return result1 || result2;
    }

    bool hasPathSum(TreeNode* root, int targetSum) {
        if(root == nullptr) return false;
        return hasPathSumHelper(root,targetSum);
    }
};
```

### [113. 路径总和 II](https://leetcode.cn/problems/path-sum-ii/)

```C++
class Solution {
public:
    
    vector<vector<int>> result;
    
    void helper(TreeNode * root,vector<int> & path,int targetSum){
        if(root->left == nullptr && root->right == nullptr && targetSum == root->val){
            path.push_back(root->val);
            result.push_back(path);
            path.pop_back();
            return;
        }
        
        if(root->left){
            path.push_back(root->val);
            helper(root->left,path,targetSum - root->val);
            path.pop_back();
        }
        
        if(root->right){
            path.push_back(root->val);
            helper(root->right,path,targetSum - root->val);
            path.pop_back();
        }
    }
    
    vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
        if(root == nullptr) return {{}};
        
        vector<int> path;
        
        helper(root,path,targetSum);
        
        return result;
    }
};


class Solution {
public:
    
    vector<vector<int>> result;
    
    void helper(TreeNode * root,vector<int> & path,int targetSum){
        if(root->left == nullptr && root->right == nullptr && targetSum == 0){
            result.push_back(path);
            return;
        }
        
        if(root->left){
            path.push_back(root->left->val);
            helper(root->left,path,targetSum - root->left->val);
            path.pop_back();
        }
        
        if(root->right){
            path.push_back(root->right->val);
            helper(root->right,path,targetSum - root->right->val);
            path.pop_back();
        }
    }
    
    vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
        if(root == nullptr) return {};
        
        vector<int> path;
        path->push_back(root->val);
        targetSum -= root->val;
        
        helper(root,path,targetSum);
        
        return result;
    }
};

```

### [106. 从中序与后序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

```C++
class Solution {
public:
    TreeNode * helper(vector<int>& inorder,int li,int ri,vector<int>& postorder,int lp,int rp){
        
        if(lp > rp) return nullptr;
        
        TreeNode * root = new TreeNode(postorder[rp]);
        
        int div;
        for(int i = li;i <= ri;i++){
            if(inorder[i] == root->val) {
                div = i;
                break;
            }
        }
        
        TreeNode * left = helper(inorder,li,div - 1,postorder,lp,lp + div - li - 1);
        TreeNode * right = helper(inorder,div + 1,ri,postorder,lp + div - li,rp - 1);
        
        root->left = left;
        root->right = right;
        return root;
    }
    
    
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) { 
		if(postorder.size() == 0) return nullptr;
        
        return helper(inorder,0,inorder.size() - 1,postorder,0,postorder.size() - 1);
    }
     
};
```

### [70. 爬楼梯](https://leetcode.cn/problems/climbing-stairs/)

```C++
class Solution {
public:
    int climbStairs(int n) {
        if(n == 1 || n == 2){
            return n;
        }
        
        vector<int> dp(n + 1);
        
        dp[1] = 1;
        dp[2] = 2;
        
        for(int i = 3;i <= n;i++){
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n];
    }
};

class Solution {
public:
    int climbStairs(int n) {
        if(n == 1 || n == 2){
            return n;
        }
        
        int pre = 1,post = 2;
        
        for(int i = 3;i <= n;i++){
            int temp = b;
 	        b = a + temp;      
            a = temp;
        }
        return b;
    }
};
```

### [746. 使用最小花费爬楼梯](https://leetcode.cn/problems/min-cost-climbing-stairs/)

```C++
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        int n = cost.size();
        if(n == 2){
            return min(cost[0],cost[1]);
        }
        
        vector<int> dp(n);
        dp[0] = cost[0];
        dp[1] = cost[1];
        
        for(int i = 2;i < cost.size();i++){
        	dp[i] = min(dp[i - 1],dp[i - 2]) +  cost[i];   
        }  
        return min(dp[n - 1],dp[n - 2]);
    }
};

class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        int n = cost.size();
        if(n == 2){
            return min(cost[0],cost[1]);
        } 
        vector<int> dp(n);
        int a = cost[0];
        int b = cost[1];
        
        for(int i = 2;i < cost.size();i++){
        	
            int temp = b; 
            b = min(a,b) +  cost[i];
            a = temp;
        }  
        return min(a,b);
    }
};
```

### [617. 合并二叉树](https://leetcode.cn/problems/merge-two-binary-trees/)

```C++
class Solution {
public:
    TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
        if(root1 == nullptr){
            return root2;
        }else if(root2 == nullptr){
            return root1;
        }
        
        TreeNode * root = new TreeNode(root1->val + root2->val);
        
        root->left = mergeTrees(root1->left,root2->left);
        root->right = mergeTrees(root1->right,root2->right);
        
        return root;
        
    }
};
```

### [700. 二叉搜索树中的搜索](https://leetcode.cn/problems/search-in-a-binary-search-tree/)

```C++
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        if(root == nullptr) return nullptr;
        
        if(root->val == val) return root;
        else if(root->val < val) return searchBST(root->right,val);
        else  return searchBST(root->left,val);
    }
};
```

### [98. 验证二叉搜索树](https://leetcode.cn/problems/validate-binary-search-tree/)

```C++
class Solution {
public:
       
    bool preOrder(TreeNode * root,long long & val){
        if(root == nullptr) return true;
        
        bool left = preOrder(root->left,val);
        
        if(root->val <= val){
            return false;
        }else{
            val = root->val;
        }
        
        bool right = preOrder(root->right,val);
        
        return left && right;
        
    }
    
    bool isValidBST(TreeNode* root) {
        long long val = LONG_MIN;
        if(root == nullptr) return true;
        return preOrder(root,val);
    }
};
```

### [530. 二叉搜索树的最小绝对差](https://leetcode.cn/problems/minimum-absolute-difference-in-bst/)

```C++
class Solution {
public:
    int diff = INT_MAX;
    
    int helper(TreeNode * root,int & val){
        if(root == nullptr) return diff;
        
        int left = helper(root->left,val);
        
        if(abs(root->val - val) < diff){
            diff = abs(root->val - val);
        }
        
        val = root->val;
        
        int right = helper(root->right,val);
        
        return min(left,right);
    }
    
    int getMinimumDifference(TreeNode* root) {
        int val = 0x3fff;
        return helper(root,val);
    }
};
```

### [501. 二叉搜索树中的众数](https://leetcode.cn/problems/find-mode-in-binary-search-tree/)

```C++
class Solution {
public:
    
    vector<int> result;
    
    int maxNums = 1;
    int count = 0;
    TreeNode * pre = nullptr;
    
    void preOrder(TreeNode * root){
        if(root == nullptr) {
            return;
        }
            
        preOrder(root->left);   
        
        if(pre == nullptr){
       		count++;
        }else{
            if(root->val == pre->val){
                count++;
            }else{
                if(count > maxNums){
                	result.clear();
                	result.push_back(pre->val);
                    maxNums = count;
           		}else if(count == maxNums){
                	result.push_back(pre->val);
                }
                count = 1;
            }
        }
        pre = root;
        preOrder(root->right);
    }
    
    vector<int> findMode(TreeNode* root) {
        preOrder(root);
        if(count > maxNums){
            result.clear();
            result.push_back(pre->val);
            maxNums = count;
        }else if(count == maxNums){
            result.push_back(pre->val);
        }
        
        return result;
    }
};
```

### [236. 二叉树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)

```C++
class Solution {
public:
        
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        
        if(root == p || root == q || root == nullptr) return root;
		
        TreeNode * left =  lowestCommonAncestor(root->left,p,q);
        TreeNode * right =  lowestCommonAncestor(root->right,p,q);
        if(left != nullptr && right != nullptr) return root;
        
        else if(left == nullptr) return right;
        
        else return left;
             
    }
};
```

### [235. 二叉搜索树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-search-tree/)

```C++
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root == nullptr) return root;
        
        if(root->val < p->val && root->val < q->val) {
            TreeNode * right =  lowestCommonAncestor(root->right,p,q);
            if(right != nullptr){
                return right;
            }
        }
        
        if(root->val > p->val && root->val > q->val){
            TreeNode * left = lowestCommonAncestor(root->left,p,q);
            if(left != nullptr){
                return left;
            }
        }
        return root;
    }
};
```

### [701. 二叉搜索树中的插入操作](https://leetcode.cn/problems/insert-into-a-binary-search-tree/)

```C++
class Solution {
public:
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        TreeNode * curr = root;
        
        while(curr){
            if(curr->val < val) {
                if(curr->right)
                	curr = curr->right;
                else{
                    curr->right = new TreeNode(val);
                    break;
                } 
            }else {
                if(curr->left)
                	curr = curr->left;
                else {
                    curr->left = new TreeNode(val);
                    break;
                }
            }
        }

        return root;
    }
};
```

### [450. 删除二叉搜索树中的节点](https://leetcode.cn/problems/delete-node-in-a-bst/)

```C++
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {
        if(root == nullptr) return nullptr;
        
        if(root->val == key){
            if(root->left == nullptr && root->right == nullptr){
                delete root;
                return nullptr
            }
            
            if(root->left == nullptr){
                TreeNode * temp = root->right;
                delete root;
                return temp;
            }else if(root->right == nullptr){
                TreeNode * temp = root->left;
                delete root;
                return temp;
            }
            
            TreeNode * curr = root->right;
            
            while(curr->left != nullptr){
                curr = curr->left;
            }
            
            curr->left = root->left;
            
            curr = root->right;
            
            delete root;
            
            return curr;
        }
        
        if(root->val < key) root->right = deleteNode(root->right,key);
        if(root->val > key) root->left = deleteNode(root->left,key);
        
        return root;
    }
};
```

### [669. 修剪二叉搜索树](https://leetcode.cn/problems/trim-a-binary-search-tree/)

```C++
class Solution {
public:
    TreeNode* trimBST(TreeNode* root, int low, int high) {
        if(root == nullptr) return nullptr;
        
        if(root->val < low) {
            return trimBST(root->right,low,high)
        }
        if(root->val > high){
            return trimBST(root->left,low,high)
        }
        
        root->left = trimBST(root->left,low,high);
        root->right = trimBST(root->right,low,high);
        return root;
            
    }
};
```

### [108. 将有序数组转换为二叉搜索树](https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/)

```C++
class Solution {
public:
    
    TreeNode * helper(vector<int> & nums,int l,int r){
        if(l > r) return nullptr;
        int mid = (l + r) / 2;
        
        TreeNode * root = new TreeNode(nums[mid]);
        
        root->left = helper(nums,l,mid - 1);
        root->right = helper(nums,mid + 1,r);
        return root;
    }
    
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return helper(nums,0,nums.size() - 1);
    }
};
```

### [538. 把二叉搜索树转换为累加树](https://leetcode.cn/problems/convert-bst-to-greater-tree/)

```C++
class Solution {
public:
    
    int pre = 0;
    
    TreeNode* convertBST(TreeNode* root) {
        if(root == nullptr) return nullptr;
    
        root->right = convertBST(root->right);
        root->val = root->val + pre;
        pre = root->val;
       	root->left = convertBST(root->left);
        
        return root;
    }
};
```

### [77. 组合](https://leetcode.cn/problems/combinations/)

```C++
class Solution {
public:
    
    vector<vector<int>> result;
    vector<int> path;
    
    void helper(int n,int k,int start){
        if(path.size() == k){
            result.push_back(path);
            return;
        }
        
        for(int i = start;i <= n - (k - path.size()) + 1;i++){
            path.push_back(i);
            helper(n,k,i + 1);
            path.pop_back();
        }
    }
    
    vector<vector<int>> combine(int n, int k) {
        helper(n,k,1);
        return result;
    }
};
```

### [216. 组合总和 III](https://leetcode.cn/problems/combination-sum-iii/)

```C++
class Solution {
public:
    vector<vector<int>> result;
    vector<int> path;
    
    void helper(int k,int n,int start){
		
        if(n < 0) return;
        
        if(path.size() == k){
            if(n == 0) result.push_back(path);
            return;
        }
        
        for(int i = start;i <= 9 - (k - path.size()) + 1 && n > 0;i++){
            path.push_back(i);
            helper(k,n - i,i + 1);
            path.pop_back();
        }
    }
    
    vector<vector<int>> combinationSum3(int k, int n) {
        helper(k,n,1);
        return result;
    }
};
```

### [17. 电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/)

```C++
class Solution {
public:
    const string letterMap[10] = {
    "", // 0
    "", // 1
    "abc", // 2
    "def", // 3
    "ghi", // 4
    "jkl", // 5
    "mno", // 6
    "pqrs", // 7
    "tuv", // 8
    "wxyz", // 9
    };
    
    vector<string> result;
    string path;
	
    void helper(string digits,int start){
        if(start == digits.size()){
            result.push_back(path);
            return;
        }
        
        int number = digits[start] - '0';
        string letter = letterMap[number];
        for(int j = 0;j < letter.size();j++){
            path.push_back(letter[j]);
            helper(digits,start + 1);
            path.pop_back();
        }  
        
    }
    
    vector<string> letterCombinations(string digits) {
        if(digits.size() == 0) return {};
        helper(digits,0);
        return result;
    }
};
```

### [39. 组合总和](https://leetcode.cn/problems/combination-sum/)

```C++
class Solution {
public:
    vector<vector<int>> result;
    
    vector<int> path;
    
    void helper(vector<int>& candidates, int target,int start){
        
        if(target < 0) return;
        
        if(target == 0){
            result.push_back(path);
            return;
        }
        
        for(int i = start;i < candidates.size();i++){
            if(candidates[i] > target) continue;
            path.push_back(candidates[i]);
            helper(candidates,target - candidates[i],i);
            path.pop_back();
        }
    }
    
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        helper(candidates,target,0);
        return result;
    }
};
```

### [40. 组合总和 II](https://leetcode.cn/problems/combination-sum-ii/)

```C++
class Solution {
public:
    
    vector<vector<int>> result;
    vector<int> path;
    
    void helper(vector<int> & candidates,int target,int start){
        if(target == 0){
            result.push_back(path);
            return;
        }
        if(target < 0) return;
        
        for(int i = start;i < candidates.size();i++){
            if(i > start && candidates[i] == candidates[i - 1]){
                continue;
            }
            
            if(candidates[i] > target) break;
            
            path.push_back(candidates[i]);
            
            helper(candidates,target - candidates[i],i + 1);
            
            path.pop_back();
        }
    }
    
    
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        sort(candidates.begin(),candidates.end());
        
        helper(candidates,target,0);
        
        return result;
    }
};
```

### [131. 分割回文串](https://leetcode.cn/problems/palindrome-partitioning/)

```C++
class Solution {
public:
    
    bool isPalin(const string & s,int l,int r){
        while(l < r){
            if(s[l] != s[r]) return false;
            l++;
            r--;
        }
        return true;
    }
    
    vector<vector<string>> result;
    vector<string> path;
    
    void helper(string& s,int start){
        if(start == s.size()){
            result.push_back(path);
            return;
        }
        
        for(int i = start;i < s.size();i++){
            string sub = s.substr(start,i - start + 1);
            
            if(isPalin(sub,0,sub.size() - 1)){
                path.push_back(sub);
            }else{
                continue;
            }
            helper(s,i + 1);
            path.pop_back();
        }
    }
    
    vector<vector<string>> partition(string s) {
        helper(s,0);
        return result;
    }
};
```

### [93. 复原 IP 地址](https://leetcode.cn/problems/restore-ip-addresses/)

```C++
class Solution {
public:
    
    vector<string> result;
    vector<string> path;
    
    void helper(string & s,int start){
        if(path.size() == 4){
            if(start == s.size()){
                string t = "";
                for(int i = 0;i < path.size();i++){
                    if(i == 0) t = path[i];
                    else t = t + "." + path[i];
                }
                result.push_back(t);
            }
            return;
        }
        
        for(int i = start;i < s.size();i++){
            string ipStr = s.substr(start,i - start + 1);
            if(ipStr.size() > 1 && ipStr[0] - '0' == 0) continue;
            if(htoi(ipStr.c_str()) < 0 || htoi(ipStr.c_str()) > 255) continue;
            path.push_back(ipStr);
            helper(s,i + 1);
            path.pop_back();
        }
    }
    
    vector<string> restoreIpAddresses(string s) {
        helper(s,0);
        return result;
    }
};
```

### [78. 子集](https://leetcode.cn/problems/subsets/)

```C++
class Solution {
public:
    vector<vector<int>> result;
    vector<int> path;
    
    void helper(vector<int> & nums,int start){
        result.push_back(path);
        
        for(int i = start;i < nums.size();i++){
            path.push_back(nums[i]);
            helper(nums,i + 1);
            path.pop_back();
        }
    }
    
    vector<vector<int>> subsets(vector<int>& nums) {
        helper(nums,0);
        return result;
    }
};
```

### [90. 子集 II](https://leetcode.cn/problems/subsets-ii/)

```C++
class Solution {
public:
    
    vector<vector<int>> result;
    vector<int> path;
    
    void helper(vector<int> & nums,int start){
    	result.push_back(path);
        
        for(int i = start;i < nums.size();i++){
            if(i > start && nums[i] == nums[i - 1]) continue;
            path.push_back(nums[i]);
            helper(nums,i + 1);
            path.pop_back();
        }
    }
    
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        helper(nums,0);
        return result;
    }
};
```

### [491. 非递减子序列](https://leetcode.cn/problems/non-decreasing-subsequences/)

```C++
class Solution {
public:
    vector<vector<int>> result;
    vector<int> path;
    
    void helper(vector<int> & nums,int start){
        if(path.size() >= 2){
             result.push_back(path);
        }
        
        unordered_set<int> uset;
        for(int i = start;i < nums.size();i++){
            if(uset.find(nums[i]) != uset.end()) continue;
            if(path.size() > 0  && nums[i] < path[path.size() - 1]) continue;
            uset.insert(nums[i]);
            path.push_back(nums[i]);
            helper(nums,i + 1);
            path.pop_back();
        }
    }
    vector<vector<int>> findSubsequences(vector<int>& nums) {
        helper(nums,0);
        return result;
    }
};
```

### [46. 全排列](https://leetcode.cn/problems/permutations/)

```C++
class Solution {
public:
    
    bool used[21] = {false};
    vector<vector<int>> result;
    vector<int> path;
    
    void helper(vector<int> & nums){
        if(path.size() == nums.size()){
            result.push_back(path);
            return;
        }
        
        for(int i = 0;i < nums.size();i++){
            if(used[nums[i] + 10]) continue;
            
            used[nums[i] + 10] = true;
            path.push_back(nums[i]);
            helper(nums);
            path.pop_back();
            used[nums[i] + 10] = false;
        }
    }
    
    vector<vector<int>> permute(vector<int>& nums) {
        helper(nums);
        return result;
    }
};
```

### [47. 全排列 II](https://leetcode.cn/problems/permutations-ii/)

```C++
class Solution {
public:
    vector<vector<int>> result;
    vector<int> path;
    bool used[21] = {false};
    
    void helper(vector<int> & nums){
        if(path.size() == nums.size()){
            result.push_back(path);
            return;
        }
        
        for(int i = 0;i < nums.size();i++){
            if(used[i]) continue;
            if(i > 0 && nums[i] == nums[i - 1] && used[i - 1] == false) continue;
            path.push_back(nums[i]);
            used[i] = true;
            helper(nums);
            used[i] = false;
            path.pop_back();
        }
    }
    
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        helper(nums);
        return result;
    }
};
```

### [332. 重新安排行程](https://leetcode.cn/problems/reconstruct-itinerary/)

```C++
class Solution {
public:
    unordered_map<string,map<string,int>> planes;
    vector<string> result;
    
    bool helper(int nums){
        if(result.size() == nums + 1){
            return true;
        }
        
        for(pair<const string,int> & target : targets[result[result.size() - 1]]){
            if(target.second > 0){
                result.push_back(target.first);
                target.second--;
                if(helper(nums)) return true;
                result.pop_back();
                target.second++;
            }
        }
        return false;
    }
    
    vector<string> findItinerary(vector<vector<string>>& tickets) {
        for(const vector<string> & t : tickets){
            planes[t[0]][t[1]]++;
        }
        result.push_back("JFK");
        helper(tickets.size());
        return result; 
    }
};
```

### [51. N 皇后](https://leetcode.cn/problems/n-queens/)

```C++
class Solution {
public:
    bool dg[20],udg[20],col[20];
    vector<vector<string>> result;
    void helper(int row,int n,vector<string> & path){
        if(row == n){ 
            result.push_back(path);
            return;
        }
        
        for(int i = 0;i < n;i++){
            if(!col[i] && !dg[i + row] && !udg[n + i - row]){
                path[row][i] = 'Q';
                col[i] = dg[i + row] = udg[n + i - row] = true;
                helper(row + 1,n,path);
                path[row][i] = '.';
                col[i] = dg[i + row] = udg[n + i - row] = false;
            }
        }
    }
    vector<vector<string>> solveNQueens(int n) {
        vector<string> path(n,string(n,'.'));
        helper(0,n,path);
        return result;
    }
};
```

### [37. 解数独](https://leetcode.cn/problems/sudoku-solver/)

```C++
class Solution {
public:
    bool row[10][10];
    bool col[10][10];
    bool grid[3][3][10];
    
    bool helper(vector<vector<char>>& board){
        for(int i = 0;i < board.size();i++){
            for(int j = 0;j < board[i].size();j++){
                if(board[i][j] == '.'){
                    for(char k = '1';k < '9';k++){
                        if(row[i][k - '0'] || col[j][k - '0'] || grid[i / 3][j / 3][k - '0']) continue;
                        board[i][j] = k;
                        row[i][board[i][j] - '0'] = true;
                    	col[j][board[i][j] - '0'] = true;
                    	grid[i / 3][j / 3][board[i][j] - '0'] = true;
                        if(helper(board)) return true;
                        board[i][j] = '.';
                        row[i][board[i][j] - '0'] = false;
                    	col[j][board[i][j] - '0'] = false;
                    	grid[i / 3][j / 3][board[i][j] - '0'] = false;   
                    }
                    return false;
                }
            }
        }
        return true;
    }
    
    void solveSudoku(vector<vector<char>>& board) {
        for(int i = 0;i < board.size();i++){
            for(int j = 0;j < board[i].size();j++){
                if(board[i][j] != '.'){
                    row[i][board[i][j] - '0'] = true;
                    col[j][board[i][j] - '0'] = true;
                    grid[i / 3][j / 3][board[i][j] - '0'] = true;
                }
            }
        }
        helper(board);
    }
};
```

### [455. 分发饼干](https://leetcode.cn/problems/assign-cookies/)

```C++
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        sort(g.begin(),g.end());
        sort(s.begin(),s.end());
        
        int index = s.size() - 1;
        
        int result = 0;
        
        for(int i = g.size() - 1;i >= 0;i--){
            if(index >= 0 && s[index] >= g[i]){
                result++;
                index--;
            }
        }
        return result;
    }
};

class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        sort(g.begin(),g.end());
        sort(s.begin(),s.end());
        
        int index = 0;
        
        for(int i = 0;i < s.size();i++){
            if(index < g.size() && g[index] <= s[i]){
                index++;
            }
        }
        return result;
    }
};

```

### [376. 摆动序列](https://leetcode.cn/problems/wiggle-subsequence/)

```C++
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
       	int len = nums.size();
        if(len == 0) return 0;
        if(len < 2) return 1;
        
        int sum = 1;
        int dircetion = 0;
        
        for(int i = 1;i < len;i++){
            if(nums[i] == nums[i - 1]) continue;
            
            if(nums[i] > nums[i - 1]){
            	if(direction != 0 && direction == 1) continue;
                direction = 1;
            }else{
                if(direction != 0 && direction == -1) continue;
                direction = -1;
            }
            sum++;
        }
        return sum;
    }
};
```

### [53. 最大子数组和](https://leetcode.cn/problems/maximum-subarray/)

```C++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int result = INT32_MIN;
        int count = 0;
        
        for(int i = 0;i < nums.size();i++){
            count += nums[i];
            if(count > result){
                result = count;
            }
            
            if(count <= 0){
                count = 0;
            }
        }
        return result;
    }
};
```

### [122. 买卖股票的最佳时机 II](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/)

```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int result = 0;
        
        for(int i = 1;i < prices.size();i++){
            if(prices[i] > prices[i - 1]){
                result += (prices[i] - prices[i - 1]);
            }
        }
        return result;
    }
};
```

### [55. 跳跃游戏](https://leetcode.cn/problems/jump-game/)

```C++
class Solution {
public:
    bool canJump(vector<int>& nums) {
       if(nums.size() <= 1) return true;
        int cover = 0;
        for(int i = 0;i <= cover;i++){
            cover = max(i + nums[i],cover);
            if(cover >= nums.size() - 1) return true;
        }
        return false;
    }
};
```

### [45. 跳跃游戏 II](https://leetcode.cn/problems/jump-game-ii/)

```C++
class Solution {
public:
    int jump(vector<int>& nums) {
        int next = 0;
        int cur = 0;
        int steps = 0;
        if(nums.size() <= 1) return 0;
        
        for(int i = 0;i < nums.size();i++){
            next = max(i + nums[i],next);
            if(i == curr){
                steps++;
                curr = next;
                if(curr >= nums.size() - 1) return steps;
            }
        }
        return steps;
    }
};
```

### [1005. K 次取反后最大化的数组和](https://leetcode.cn/problems/maximize-sum-of-array-after-k-negations/)

```C++
class Solution {
public:
    
    static bool cmp(int a,int b){
        return abs(a) > abs(b);
    }
    
    int largestSumAfterKNegations(vector<int>& nums, int k) {
        sort(nums.begin(),nums.end(),cmp);
        
        for(int i = 0;i < nums.size();i++){
            if(nums[i] < 0 && k > 0){
                k--;
                nums[i] = -nums[i];
            }
        }
        while(k > 0){
            k--;
            nums[nums.size() - 1] *= -1;
        }
        int result = 0;
        for(int & e:nums)result += e;
        return result;
    }
};
```

### [134. 加油站](https://leetcode.cn/problems/gas-station/)

```C++
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int s = 0;
        int curSum = 0;
        int totalSum = 0;
        
        for(int i = 0;i < gas.size();i++){
            curSum += gas[i] - cost[i];
            total += gas[i] - cost[i];
            if(curSum < 0){
                start = i + 1;
                curSum = 0;
            }
        }
        if(totalSum < 0) return -1;
        return start;
    }
};

class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int min = INT32_MAX;
        int curSum = 0;
        
        for(int i = 0;i < gas.size();i++){
            curSum += gas[i] - cost[i];
            if(curSum < min){
                min = curSum;
            }
        }
        
        if(curSum < 0) return -1;
        
        if(min >= 0) return 0;
        
        for(int i = gas.size() - 1;i >= 0;i++){
            min += (gas[i] - cost[i]);
            if(min >= 0) return i;
        }
        return -1;
    }
};
```

### [135. 分发糖果](https://leetcode.cn/problems/candy/)

```C++
class Solution {
public:
    int candy(vector<int>& ratings) {
        int result = ratings.size();
        vector<int> ratingsHelper(ratings.size(),1);
        
       for(int i = 0;i < ratings.size();i++){
            if(i > 0 && ratings[i] > ratings[i - 1]) {
                result += (ratingsHelper[i-1] - ratingsHelper[i] + 1);
                ratingsHelper[i] = ratingsHelper[i - 1] + 1;
            }
        }
        
        for(int i = ratings.size() - 1;i >= 0;i--){
            if(i < ratings.size() - 1 && ratings[i] > ratings[i + 1]){
                if(ratingsHelper[i] <= ratingsHelper[i + 1]){
                    result += (ratingsHelper[i + 1] - ratingsHelper[i] + 1);
                    ratingsHelper[i] = ratingsHelper[i + 1] + 1;
                }
            }
        }
        return result;
    }
};
```

### [860. 柠檬水找零](https://leetcode.cn/problems/lemonade-change/)

```C++
class Solution {
public:
    bool lemonadeChange(vector<int>& bills) {
        int numsf = 0;
        int numst = 0;
        
        for(int i = 0;i < bills.size();i++){
            if(bills[i] == 5) numsf++;
            
            if(bills[i] == 10){
                numsf--;
                numst++;
            }
            
            if(bills[i] == 20){
                if(numst > 0){
                    numst--;
                    numsf--;
                }else{
                    numsf -= 3;
                }
            }
            if(numsf < 0) return false;
        }
        return true;
    }
};
```

### [406. 根据身高重建队列](https://leetcode.cn/problems/queue-reconstruction-by-height/)

```C++
class Solution {
public:
    
    static bool cmp(const vector<int> & lhs,const vector<int> & rhs){
        if(lhs[0] == rhs[0]) return lhs[1] < rhs[1];
        return lhs[0] > rhs[0];
    }
    
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        sort(people.begin(),people.end(),cmp);
        
        list<vector<int>> q;
        
        for(int i = 0;i < people.size();i++){
            int pos = people[i][1];
            list<vector<int>>::iterator it = q.begin();
            while(q > 0){
                it++;
                q--;
            }
            q.insert(it,people[i]);
        }
        return vector<vector<int>>(q.begin(),q.end());
    }
};
```

### [452. 用最少数量的箭引爆气球](https://leetcode.cn/problems/minimum-number-of-arrows-to-burst-balloons/)

```C++
class Solution {
public:
    
    static bool cmp(vector<int> & lhs,vector<int> & rhs){
        if(lhs[0] == rhs[0]) return lhs[1] < rhs[1];
        return lhs[0] < rhs[0];
    }
    
    int findMinArrowShots(vector<vector<int>>& points) {
        sort(points.begin(),points.end(),cmp);
        
        int result = 0;
        int end;
        int start;
        for(int i = 0;i < points.size();i++){
            if(i == 0 || points[i][0] > end){
                end = points[i][1];
                start = points[i][0];
                result++;
            }else{
                start = points[i][0];
                end = min(end,points[i][1]);
            }
        }
        return result;
    }
};
```

###  [435. 无重叠区间](https://leetcode.cn/problems/non-overlapping-intervals/)

```C++
class Solution {
public:
    
    static bool cmp(vector<int> & lhs,vector<int> & rhs){
        if(lhs[0] == rhs[0]) return lhs[1] < rhs[1];
        return lhs[0] < rhs[0];
    }
    
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        
        sort(intervals.begin(),intervals.end(),cmp);
        int result = 0;
        int end;
        
        for(int i = 0;i < intervals.size();i++){
            if(i == 0 || end <= intervals[i][0]){
                end = intervals[i][1];
            }else{
                result++;
                end = min(end,intervals[i][1]);
            }
        }
        
        return result;
    }
};
```

### [763. 划分字母区间](https://leetcode.cn/problems/partition-labels/)

```C++
class Solution {
public:
    vector<int> partitionLabels(string s) {
        int uhash[26] = {0};
        
        for(int i = 0;i < s.size();i++){
            uhash[s[i] - 'a'] = i;
        }
        
        int l = 0;
        int r = 0;
        vector<int> result;
        
        for(int i = 0;i < s.size();i++){
            r = max(r,uhash[s[i] - 'a']);
            
            if(i == r){
                result.push_back(r - l + 1);
                r = i + 1;
                l = i + 1;
            }
        }
        return result;
    }
};
```

### [56. 合并区间](https://leetcode.cn/problems/merge-intervals/)

```C++
class Solution {
public:
    static bool cmp(vector<int> & lhs,vector<int> & rhs){
        if(lhs[0] == rhs[0]) return lhs[1] < rhs[1];
        return lhs[0] < rhs[0];
    }
    
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(),intervals.end(),cmp);
        
        int start = intervals[0][0];
        int end = intervals[0][1];
        
        if(intervals.size() == 1) return vector({start,end});
        
        vector<vector<int>> result;
        for(int i = 1;i < intervals.size();i++){
            if(intervals[i][0] > end){
                result.push_back({start,end});
                start = intervals[i][0];
                end = intervals[i][1];
            }else{
                end = max(end,intervals[i][1]);
            }
        }
        result.push_back({start,end});
        return result;
    }
};
```

### [738. 单调递增的数字](https://leetcode.cn/problems/monotone-increasing-digits/)

```C++
class Solution {
public:
    int monotoneIncreasingDigits(int n) {
        string num = to_string(n);
        
        int flag = num.size();
        
        for(int i = flag - 1;i > 0;i--){
            if(num[i - 1] > num[i]){
                num[i - 1]--;
                flag = i;
            }
        }
        
        for(int i = flag;i < num.size();i++){
            num[i] = '9';
        }
        return stoi(num);
    }
};
```

### [968. 监控二叉树](https://leetcode.cn/problems/binary-tree-cameras/)

```C++
class Solution {
public:
    
    int result = 0;
    
    int helper(TreeNode * root){
        if(root == nullptr) return 2;
        
        int left = helper(root->left);
        int right = helper(root->right);
        
        if(left == 2 && right == 2) return 0;
        else if(left == 0 || right == 0){
            result++;
            return 1;
        }else return 2;
        
    }
    
    int minCameraCover(TreeNode* root) {
        if(helper(root) == 0){
            result++;
        }
        return result;
    }
};
```

### [509. 斐波那契数](https://leetcode.cn/problems/fibonacci-number/)

```C++
class Solution {
public:
    int fib(int n) {
        if(n == 0) return 0;
        if(n == 1) return 1;
        
        int a = 0,b = 1;
        
        for(int i = 2;i <= n;i++){
            int tmp = a + b;
            a = b;
			b = tmp;
        }
        return b;
    }
};
```

### [70. 爬楼梯](https://leetcode.cn/problems/climbing-stairs/)

```C++
class Solution {
public:
    int climbStairs(int n) {
        if(n == 1 || n == 2) return n;
        
        int a = 1;
        int b = 2;
        
        for(int i = 2;i <= n;i++){
            int tmp = a + b;
            a = b;
            b = tmp;
        }
        return b;
    }
};
```

### [746. 使用最小花费爬楼梯](https://leetcode.cn/problems/min-cost-climbing-stairs/)

```C++
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        if(cost.size() == 1 || cost.size() == 0) return 0;
        
        
        int a = cost[0];
        int b = cost[1];
        
        for(int i = 2;i < cost.size();i++){
            int tmp = min(a,b) + cost[i];
            a = b;
            b = tmp;
        }
        return min(a,b);
    }
};
```

### [62. 不同路径](https://leetcode.cn/problems/unique-paths/)

```C++
class Solution {
public:
    int uniquePaths(int m, int n) {
        if(m == 1 || n == 1) return 1;
        
        int dp[m][n] = {};
        
        for(int i = 0;i < m;i++){
            dp[i][0] = 1;
        }
        
        for(int j = 0;j < n;j++){
            dp[0][j] = 1;
        }
        
        for(int i = 1;i < m;i++){
            for(int j = 1;j < n;j++){
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            }
        }
        return dp[m - 1][n - 1];
    }
};

class Solution {
public:
    int uniquePaths(int m, int n) {
        if(m == 1 || n == 1) return 1;
        int dp[n] = {0};
        
        for(int i = 0;i < n;i++) dp[i] = 1;
        
        for(int i = 1;i < m;i++){
            for(int j = 1;j < n;j++){
                dp[j] = dp[j] + dp[j - 1];
            }
        }
        return dp[n - 1];
    }
};

```

### [63. 不同路径 II](https://leetcode.cn/problems/unique-paths-ii/)

```C++
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();
        
        vector<int> dp(n,0);
        for(int i = 0;i < n;i++){
            if(obstacleGrid[0][i] == 0) dp[i] = 1;
            else break;
        }
        
        for(int i = 1;i < m;i++){
            for(int j = 1;j < n;j++){
                if(obstacleGrid[i][j] == 0)
                	dp[j] = dp[j] + dp[j - 1];
                else dp[j] = 0;
            }
        }
        return dp[j - 1];
    }
};
```

## 2025.1

### [343. 整数拆分](https://leetcode.cn/problems/integer-break/)

```C++
class Solution {
public:
    int integerBreak(int n) {
        vector<int> dp(n + 1,0);
        
        dp[2] = 1;
        dp[1] = 1;
        for(int i = 3;i <= n;i++){
            for(int j = 1;j <= i / 2;j++){
                dp[i] = max(dp[i],max((i - j) * j,dp[i - j] * j));
            }
        }
        return dp[n];
    }
};
```

### [96. 不同的二叉搜索树](https://leetcode.cn/problems/unique-binary-search-trees/)

```C++
class Solution {
public:
    int numTrees(int n) {
        vector<int> dp(n + 1,0);
        
        if(n == 1) return 1;
        dp[0] = 1;
        dp[1] = 1;
        
        for(int i = 2;i <= n;i++){
            for(int j = 1;j <= i;j++){
                dp[i] += (dp[j - 1] * dp[i - j]);
            }
        }
        return dp[n];
    }
};
```

### [416. 分割等和子集](https://leetcode.cn/problems/partition-equal-subset-sum/)

```C++
class Solution {
public:
    bool canPartition(vector<int>& nums) {
		int sum = 0;
        for(int i = 0;i < nums.size();i++){
            sum += nums[i];
        }
        
        if(sum % 2 == 1) return false;
        
        int target = sum / 2;
        
        vector<int> dp(target + 1,0);
        
        for(int i = 0;i < nums.size();i++){
            for(int j = target;j >= nums[i];j--){
                dp[j] = max(dp[j],dp[j - nums[i]] + nums[i]);
            }
        }
        return dp[target] == target;
    }
};
```

### [1049. 最后一块石头的重量 II](https://leetcode.cn/problems/last-stone-weight-ii/)

```C++
class Solution {
public:
    int lastStoneWeightII(vector<int>& stones) {
        int sum = 0;
        
        for(int i = 0;i < stones.size();i++) sum += stones[i];
        
        int target = sum / 2;
        
        vector<int> dp(target + 1,0);
        
        for(int i = 0;i < stones.size();i++){
            for(int j = target;j >= stones[i];j--){
                dp[j] = max(dp[j],dp[j - stones[i]] + stones[i]);
            }
        }
        
        return sum - dp[target] - dp[target];
    }
};
```

### [494. 目标和](https://leetcode.cn/problems/target-sum/)

```C++
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        
        int sum = 0;
        for(int i = 0;i < nums.size();i++){
            sum += nums[i];
        }
        
        if(abs(sum) < abs(target)) return 0;
        
        if((sum + target) % 2 == 1) return 0;
        
        int result = (sum + target) / 2;
        
        vector<vector<int>> dp(nums.size(),vector<int>(result + 1,0));
        
        if(nums[0] <= result) dp[0][nums[0]] = 1;
        
        dp[0][0] = 1;
        
        int numsZero = 0;
        
        for(int i = 0;i < nums.size();i++){
            if(nums[i] == 0) numsZero++;
            dp[i][0] = (int)pow(2.0,numsZero);
        }
        
        
        for(int i = 1;i < nums.size();i++){
            for(int j = 0;j <= result;j++){
                if(nums[i] > j) dp[i][j] = dp[i - 1][j];
                else dp[i][j] = dp[i - 1][j] + dp[i - 1][j - nums[i]];
            }
        }
        
        return dp[nums.size() - 1][result];
    }
};


class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        
        int sum = 0;
        for(int i = 0;i < nums.size();i++){
            sum += nums[i];
        }
        
        if(abs(sum) < abs(target)) return 0;
        
        if((sum + target) % 2 == 1) return 0;
        
        int result = (sum + target) / 2;
        
        vector<int> dp(result + 1);
        
        dp[0] = 1;
        
        for(int i = 0; < nums.size();i++){
            for(int j = result;j >= nums[i];j--){
                dp[j] = dp[j] + dp[j - nums[i]];
            }
        }
        
        return dp[result];
    }
};
```

### [474. 一和零](https://leetcode.cn/problems/ones-and-zeroes/)

```C++
class Solution {
public:
    int findMaxForm(vector<string>& strs, int m, int n) {
        vector<int> numsZero(strs.size(),0);
        vector<int> numsOne(strs.size(),0);
        
        for(int i = 0;i < strs.size();i++){
            for(const char & c:strs[i]){
            	if(c == '0') numsZero[i]++;
                if(c == '1') numsOne[i]++;
        	}
        }
        
        vector<vector<int>> dp(m + 1,vector<int>(n + 1,0));
        
        if(numsZero[0] <= m && numsOne[0] <= n){
            for(int j = numsZero[0];j <= m;j++){
                for(int k = numsOne[0];k <= n;k++){
                    dp[j][k] = 1;
                }
            }
        }
        
        for(int i = 1;i < strs.size();i++){
            int zeros = numsZero[i];
            int ones = numsOne[i];
            
            for(int j = m;j >= zeros;j--){
                for(int k = n;k >= ones;k--){
                    dp[j][k] = max(dp[j][k],dp[j - zeros][k - ones] + 1);
                }
            }
        }
        return dp[m][n];
    }
};
```

### [518. 零钱兑换 II](https://leetcode.cn/problems/coin-change-ii/)

```C++
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        vector<vector<int>> dp(coins.size(),vector<int>(amount + 1,0));
        
        for(int i = 0;i < coints.size();i++){
            dp[i][0] = 1;
        }
        
        for(int i = coins[0];i <= amount;i++){
            dp[0][i] = 1;
        }
        
        for(int i = 1;i < coints.size();i++){
            for(int j = 0;j <= amount;j++){
                if(j < coins[i]) dp[i][j] = dp[i - 1][j];
                else dp[i][j] = dp[i - 1][j] + dp[i][j - coints[i]];
            }
        }
        return dp[coins.size() - 1][amount];
    }
};


class Solution {
public:
    int change(int amount, vector<int>& coins) {
        vector<unsigned long long> dp(amount + 1,0);
        
        
        for(int i = coins[0];i <= amount;i++){
            if(i % coins[0] == 0)
                dp[i] = 1;
        }
        dp[0] = 1;
        for(int i = 1;i < coins.size();i++){
            for(int j = coins[i];j <= amount;j++){
                dp[j] = dp[j] + dp[j - coins[i]];
            }
        }
        return dp[amount];
    }
};
```

### [377. 组合总和 Ⅳ](https://leetcode.cn/problems/combination-sum-iv/)

```C++
class Solution {
public:
    int combinationSum4(vector<int>& nums, int target) {
        vector<int> dp(target + 1,0);
        
        dp[0] = 1;
        for(int j = 0;j <= target;j++){
            for(int i = 0;i < nums.size();i++){
                if(j < nums[i] && dp[j] < INT_MAX - dp[j - nums[i]]) dp[j] = dp[j] + dp[j - nums[i]];
            }
        }
        return dp[target];
    }
};
```

### [322. 零钱兑换](https://leetcode.cn/problems/coin-change/)

```C++
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        vector<vector<int>> dp(coins.size(),vector<int>(amount + 1,0x3fff));
        
        for(int i = 0;i < coins.size();i++){
            dp[i][0] = 0;
        }   
        for(int i = 1;i <= amount;i++){
            if(i % coins[0] == 0) dp[0][i] = i / coins[0];
            else dp[0][i] = -1;
        }
        
        for(int i = 1;i < coins.size();i++){
            for(int j = 1;j <= amount;j++){
                if(j < coins[i] || dp[i][j - coins[i]] == -1) dp[i][j] = dp[i - 1][j];
                else{
                    if(dp[i - 1][j] == -1) dp[i][j] = dp[i][j - coins[i]] + 1;
                    else dp[i][j] = min(dp[i - 1][j],dp[i][j - coins[i]] + 1);
                }
            }
        }
        return dp[coins.size() - 1][amount];
    }
};

class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        vector<int> dp(amount + 1,0x3fff);
        
        dp[0] = 0;
        
        for(int i = 0;i < coins.size();i++){
            for(int j = coins[i];j <= amount;j++){
                dp[j] = min(dp[j],dp[j - coins[i]] + 1);
            }
        }
        if(dp[amount] == 0x3fff) return -1;
        return dp[amount];
    }
};

```

### [279. 完全平方数](https://leetcode.cn/problems/perfect-squares/)

```C++
class Solution {
public:
    int numSquares(int n) {        
        vector<int> dp(n + 1,0x3fff);
        
        dp[0] = 0;
        
        for(int i = 0;i <= n;i++){
            for(int j = 1;j * j <= n;j++){
               	dp[i] = min(dp[i],dp[i - j * j] + 1);
            }
        }
        
        return dp[n];
    }
};
```

###  [139. 单词拆分](https://leetcode.cn/problems/word-break/)

```C++
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> wordSet(wordDict.begin(),wordDict.end());
        vector<bool> dp(s.size() + 1,false);
        dp[0] = true;
        
        for(int i = 0;i <= s.size();i++){
            for(int j = 0;j < i;j++){
                string word = s.substr(j,i - j);
                if(wordSet.find(word) != wordSet.end() && dp[j]){
                    dp[i] = true;
                }
            }
        }
        return dp[s.size()];
    }
};
```

### [198. 打家劫舍](https://leetcode.cn/problems/house-robber/)

```C++
class Solution {
public:
    int rob(vector<int>& nums) {
        vector<int> dp(nums.size(),0);
        
        if(nums.size() == 1) return nums[0];
        if(nums.size() == 2) return max(nums[0],nums[1]);

        dp[0] = nums[0];
        dp[1] = max(nums[0],nums[1]);
        
        for(int i = 2;i < nums.size();i++){
            dp[i] = max(dp[i - 1],dp[i - 2] + nums[i]);
        }
        
        return dp[nums.size() - 1];
    }
};
```

### [213. 打家劫舍 II](https://leetcode.cn/problems/house-robber-ii/)

```C++
class Solution {
public:
    
    int helper(vector<int> & nums,int l,int r){
        vector<int> dp(nums.size(),0);
        dp[l] = nums[l];
        dp[l + 1] = max(nums[l],nums[l + 1]);
        
        for(int i = l + 2;i <= r;i++){
            dp[i] = max(dp[i - 2] + nums[i],dp[i - 1]);
        }
        return dp[r];
    }
    
    int rob(vector<int>& nums) {
        vector<int> dp(nums.size() + 1,0);
        
        if(nums.size() == 1) return nums[0];
        if(nums.size() == 2) return max(nums[0],nums[1]);

        int case1 = helper(nums,0,nums.size() - 2);
        int case2 = helper(nums,1,nums.size() - 1);
        
        return max(case1,case2);
    }
};
```

### [337. 打家劫舍 III](https://leetcode.cn/problems/house-robber-iii/)

```C++
class Solution {
public:
    
    vector<int> helper(TreeNode * root){
        if(root == nullptr) return {0,0};
        
        vector<int> left = helper(root->left);
        vector<int> right = helper(root->right);
        
        int case1 = root->val + left[0] + right[0];
        int case2 = max(left[0],left[1]) + max(right[0],right[1]);
        
        return {case2,case1};
    }
    
    int rob(TreeNode* root) {
        vector<int> result = helper(root);
        return max(result[0],result[1]);
    }
};
```

### [121. 买卖股票的最佳时机](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/)

```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int low = 0x3fff;
        int result = 0;
        
        for(int i = 1;i < prices.size();i++){
            low = min(low,prices[i]);
            result = max(result,prices[i] - low);
        }
        return result;
    }
};
```

### [300. 最长递增子序列](https://leetcode.cn/problems/longest-increasing-subsequence/)

```C++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        vector<int> dp(nums.size(),1);
        
        dp[0] = 1;
        int result = 0;
        for(int i = 1;i < nums.size();i++){
            for(int j = i - 1;j >= 0;j--){
                if(nums[j] < nums[i]){
                    dp[i] = max(dp[i],dp[j] + 1);
                }
            }
            if(dp[i] > result) result = dp[i];
        }
        return result;
    }
};
```

### [123. 买卖股票的最佳时机 III](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iii/)

```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        vector<int> dp(5,0);
        
        dp[1] = -prices[0];
        dp[3] = -prices[0];
        
        for(int i = 1;i < prices.size();i++){
            dp[1] = max(dp[1],dp[0] - prices[i]);
            dp[2] = max(dp[2],dp[1] + prices[i]);
            dp[3] = max(dp[3],dp[2] - prices[i]);
            dp[4] = max(dp[4],dp[3] + prices[i]);
        }
        return dp[4];
    }
};
```

### [188. 买卖股票的最佳时机 IV](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iv/)

```C++
class Solution {
public:
    int maxProfit(int k, vector<int>& prices) {
        vector<int> dp(2 * k + 1,0);
        
        for(int j = 1;j < 2 * k + 1;j += 2){
            dp[j] = -prices[0];
        }
        
        for(int i = 1;i < prices.size();i++){
            for(int j = 0;j < 2 * k - 1;j += 2){
                dp[j + 1] = max(dp[j + 1],dp[j] - prices[i]);
                dp[j + 2] = max(dp[j + 2],dp[j + 1] + prices[i]);
            }
        }
        
        return dp[2 * k];
    }
};
```

### [309. 买卖股票的最佳时机含冷冻期](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if(prices.size() == 1) return 0;
        if(prices.size() == 2){
            if(prices[0] < prices[1]) return prices[1] - prices[0];
            else return 0;
        }
        
        vector<int> dp(prices.size(),vector<int>(2,0));
        
        dp[0][0] = -prices[0];
        dp[1][0] = -prices[1];
        dp[1][1] = max(dp[0][1], dp[0][0] + prices[1]);
        
        for(int i = 2;i < prices.size();i++){
            dp[i][0] = max(dp[i - 1][0],dp[i - 2][1] - prices[i]);
            dp[i][1] = max(dp[i - 1][1],dp[i - 1][0] + prices[i]);
        }
        
        return dp[prices.size() - 1][1];
    }
};
```

### [714. 买卖股票的最佳时机含手续费](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)

```C++
class Solution {
public:
    int maxProfit(vector<int>& prices, int fee) {
        vector<int> dp(2,0);
        dp[0] = -prices[0];
        for(int i = 1;i < prices.size();i++){
            dp[0] = max(dp[0],dp[1] - prices[i]);
            dp[1] = max(dp[1],dp[0] + prices[i] - fee);
        }
        return dp[1];
    }
};
```

### [718. 最长重复子数组](https://leetcode.cn/problems/maximum-length-of-repeated-subarray/)

```C++
class Solution {
public:
    int findLength(vector<int>& nums1, vector<int>& nums2) {
        vector<vector<int>> dp(nums1.size() + 1,vector<int>(nums2.size() + 1,0));
        int result = 0;
        
        for(int i = 1;i <= nums1.size();i++){
            for(int j = 1;j <= nums2.size();j++){
                if(nums1[i - 1] == nums2[j - 1]){
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                }
                
                if(dp[i][j] > result) result = dp[i][j];
            }
        }
        return result;
    }
};

class Solution {
public:
    int findLength(vector<int>& nums1, vector<int>& nums2) {
        vector<int> dp(nums2.size() + 1,0);
        int result = 0;
        
        for(int i = 1;i <= nums1.size();i++){
            for(int j = nums2.size();j > 0;j--){
                if(nums1[i - 1] == nums2[j - 1]){
                    dp[j] = dp[j - 1] + 1;
                }else dp[j] = 0;
                
                if(dp[j] > result) result = dp[j];
            }
        }
        return result;
    }
};
```

### [1143. 最长公共子序列](https://leetcode.cn/problems/longest-common-subsequence/)

```C++
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        vector<vector<int>> dp(text1.size() + 1,vector<int>(text2.size() + 1,0));
        
        for(int i = 1;i <= text1.size();i++){
            for(int j = 1;j <= text2.size();j++){
                if(text1[i - 1] == text2[j - 1]){
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                }else{
                    dp[i][j] = max(dp[i - 1][j],dp[i][j - 1]); 
                }
            }
        }
        return dp[text1.size()][text2.size()];
    }
};
```

### [1035. 不相交的线](https://leetcode.cn/problems/uncrossed-lines/)

```C++
class Solution {
public:
    int maxUncrossedLines(vector<int>& nums1, vector<int>& nums2) {
        vector<vector<int>> dp(nums1.size() + 1,vector<int>(nums2.size() + 1,0));
        
        for(int i = 1;i <= nums1.size();i++){
            for(int j = 1;j <= nums2.size();j++){
                if(nums1[i - 1] == nums2[j - 1]){
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                }else{
                    dp[i][j] = max(dp[i - 1][j],dp[i][j - 1]); 
                }
            }
        }
        return dp[nums1.size()][nums2.size()];
    }
};
```

### [53. 最大子数组和](https://leetcode.cn/problems/maximum-subarray/)

```C++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int sum = nums[0];
        int result = sum;
        for(int i = 1;i < nums.size();i++){
            sum = max(nums[i],sum + nums[i]);
            if(sum > result){
                result = sum;
            }
        }
        return result;
    }
};
```

### [392. 判断子序列](https://leetcode.cn/problems/is-subsequence/)

```C++
class Solution {
public:
    bool isSubsequence(string s, string t) {
  		vector<vector<int>> dp(t.size() + 1,vector<int>(s.size() + 1,0));
        
        for(int i = 1;i <= t.size();i++){
            for(int j = 1;j <= s.size();j++){
                if(t[i - 1] == s[j - 1]){
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                }else{
                    dp[i][j] = dp[i - ][j];
                }
            }
        }
        
        return dp[t.size()][s.size()] == s.size();
    }
};
```

### [115. 不同的子序列](https://leetcode.cn/problems/distinct-subsequences/)

```C++
class Solution {
public:
    int numDistinct(string s, string t) {
        vector<vector<int>> dp(s.size() + 1,vector<int>(t.size() + 1,0));
        
        for(int i = 0;i < t.size();i++){
            dp[0][i] = 0;
        }
        
        for(int j = 0;j < s.size();j++){
            dp[j][0] = 1;
        }
        
        for(int i = 1;i <= s.size();i++){
            for(int j = 1;j <= t.size();j++){
                if(s[i - 1] == t[j - 1]){
                    dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j];
                }else{
                    dp[i][j] = dp[i - 1][j];
                }
            }
        }
        return dp[s.size()][t.size()];
    }
};
```

### [583. 两个字符串的删除操作](https://leetcode.cn/problems/delete-operation-for-two-strings/)

```C++
class Solution {
public:
    int minDistance(string word1, string word2) {
        vector<vector<int>> dp(word1.size() + 1,vector<int>(word2.size() + 1,0));
        
        for(int i = 0;i <= word2.size();i++){
            dp[0][i] = i;
        }
        
        for(int j = 0;j <= word1.size();j++){
            dp[j][0] = j;
        }
        
        for(int i = 1;i <= word1.size();i++){
            for(int j = 1;j <= word2.size();j++){
                if(word1[i - 1] == word2[j - 1]){
                    dp[i][j] = dp[i - 1][j - 1];
                }else{
                    dp[i][j] = min(dp[i - 1][j],dp[i][j - 1]) + 1;
                }
            }
        }
        
        return dp[word1.size()][word2.size()];
    }
};
```

### [72. 编辑距离](https://leetcode.cn/problems/edit-distance/)

```C++
class Solution {
public:
    int minDistance(string word1, string word2) {
        vector<vector<int>> dp(word1.size() + 1,vector<int>(word2.size() + 1,0));
        
        for(int i = 0;i <= word2.size();i++){
            dp[0][i] = i;
        }
        
        for(int i = 0;i <= word1.size();i++){
            dp[i][0] = i;
        }
        
        for(int i = 1;i <= word1.size();i++){
            for(int j = 1;j <= word2.size();j++){
                if(word1[i - 1] == word2[j - 1]){
                    dp[i][j] = dp[i - 1][j - 1];
                }else{
                    dp[i][j] = min(dp[i - 1][j],min(dp[i - 1][j - 1],dp[i][j - 1])) + 1;
                }
            }
        }
        
        return dp[word1.size()][word2.size()];
    }
};
```

### [647. 回文子串](https://leetcode.cn/problems/palindromic-substrings/)

```C++
class Solution {
public:
    int countSubstrings(string s) {
        vector<vector<bool>> dp(s.size(),vector<bool>(s.size(),false));
        int result = 0;
        for(int i = s.size() - 1;i >= 0;i--){
            for(int j = i;j < s.size();j++){
                if(s[i] == s[j]){
                    if(i +1 >= j){
                        result++;
                        dp[i][j] = true;
                    }else if(dp[i + 1][j - 1]){
                        dp[i][j] = true;
                        result++;
                    }
                }
            }
        }
        return result;
    }
};
```

### [516. 最长回文子序列](https://leetcode.cn/problems/longest-palindromic-subsequence/)

```C++
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        vector<vector<int>> dp(s.size(),vector<int>(s.size(),0));
        int result = 0;
        
        for(int i = s.size() - 1;i >= 0;i--){
            for(int j = i;j < s.size();j++){
                if(s[i] == s[j]){
                    if(i + 1 >= j){
                        dp[i][j] = j - i + 1;
                    }else{
                        dp[i][j] = dp[i + 1][j - 1] + 2;
                    }
                }else{
                    dp[i][j] = max(dp[i + 1][j],dp[i][j - 1]);
                }
                
                if(dp[i][j] > result) result = dp[i][j];
            }
        }
        return result;
    }
};
```

### [797. 所有可能的路径](https://leetcode.cn/problems/all-paths-from-source-to-target/)

```C++
class Solution {
public:
    vector<vector<int>> result;
    
    vector<int> path;
    
    void dfs(vector<vector<int>>& graph,int x,int n){
        if(x == n){
            result.push_back(path);
            return;
        }
        
        for(int i = 0;i < graph[x].size();i++){
            path.push_back(graph[x][i]);
            dfs(graph,graph[x][i],n);
            path.pop_back();
        }
    }
    
    vector<vector<int>> allPathsSourceTarget(vector<vector<int>>& graph) {
        path.push_back(0);
        dfs(graph,0,graph.size() - 1);
        return result;
    }
};
```

## 2025.2

### 所有可达路径

```C++
#include <iostream>
#include <vector>

using namespace std;

const int N = 110;
const int M = 510;

int e[N][N];

int n,m;
vector<int> path;
vector<vector<int>> result;
void dfs(int start){
    if(start == n){
        result.push_back(path);
        return;
    }

    for(int i = 1;i <= n;i++){
        if(e[start][i] == 1){
            path.push_back(i);
            dfs(i);
            path.pop_back();
        }
    }
}

int main(){
 
    cin >> n >> m;
    for(int i = 0;i < m;i++){
        int a,b;
        cin >> a >> b;
        e[a][b] = 1;
    }
    
    path.push_back(1);
    dfs(1);
    if(result.size() == 0) cout << -1 << "\n";
    for(const vector<int> & pa : result){
        for(int i = 0;i < pa.size() - 1;i++){
            cout << pa[i] << " ";
        }
        cout << pa[pa.size() - 1] << "\n";
    }
    return 0;
}

//邻接表写法
#include <iostream>
#include <vector>
#include <list>

using namespace std;

const int N = 110;
const int M = 510;


int n,m;
vector<int> path;
vector<vector<int>> result;

void dfs(vector<list<int>> & graph,int start){
    if(start == n){
        result.push_back(path);
        return;
    }

    for(int i : graph[start]){
        path.push_back(i);
        dfs(graph,i);
        path.pop_back();
    }
}

int main(){
     
    cin >> n >> m;

    vector<list<int>> graph(n + 1);

    for(int i = 0;i < m;i++){
        int a,b;
        cin >> a >> b;
        graph[a].push_back(b);
    }
    
    path.push_back(1);
    dfs(graph,1);

    if(result.size() == 0) cout << -1 << "\n";
    
    for(const vector<int> & pa : result){
        for(int i = 0;i < pa.size() - 1;i++){
            cout << pa[i] << " ";
        }
        cout << pa[pa.size() - 1] << "\n";
    }
    return 0;
}
```

### 岛屿数量(深搜)

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

int dt[4][2] = {0, 1, 1, 0, -1, 0, 0, -1};

void dfs(vector<vector<int>> & grid,vector<vector<int>> & visited,int x,int y){
    for(int i = 0;i < 4;i++){
        int a = x + dt[i][0];
        int b = y + dt[i][1];

        if(a >= 0 && a < grid.size() && b >= 0 && b < grid[0].size() && grid[a][b] == 1 && !visited[a][b]){
            visited[a][b] = true;
            dfs(grid,visited,a,b);
        }
    }
}

int main(){

    int n,m;

    cin >> n >> m;

    vector<vector<int>> grid(n,vector<int>(m,0));

    for(int i = 0;i < n;i++){
        for(int j = 0;j < m;j++){
            cin >> grid[i][j];
        }
    }

    vector<vector<int>> visited(n,vector<int>(m,false));

    int result = 0;
    for(int i = 0;i < n;i++){
        for(int j = 0;j < m;j++){
            if(!visited[i][j] && grid[i][j] == 1){
                result++;
                visited[i][j] = true;
                dfs(grid,visited,i,j);
            }
        }
    }
    
    cout << result;
    return 0;
}
```

### 岛屿数量(广搜)

```C++
#include <iostream>
#include <string>
#include <vector>
#include <queue>
#include <algorithm>
#include <unordered_map>
#include <unordered_set>
#include <sstream>
#include <set>

using namespace std;

int dt[4][2] = {0, 1, 1, 0, -1, 0, 0, -1};

void bfs(vector<vector<int>> & grid,vector<vector<int>> & visited,int x,int y){
    queue<pair<int,int>> q;
    visited[x][y] = true;
    q.push({x,y});

    while(!q.empty()){
        pair<int,int> pos = q.front(); q.pop();

        int curX = pos.first;
        int curY = pos.second;

        for(int i = 0;i < 4;i++){
            int a = curX + dt[i][0];
            int b = curY + dt[i][1];
            if(a >= 0 && a < grid.size() && b >= 0 && b < grid[0].size() && !visited[a][b] && grid[a][b] == 1){
                visited[a][b] = true;
                q.push({a,b});
            }
        }
    }
}

int main(){

    int n,m;

    cin >> n >> m;

    vector<vector<int>> grid(n,vector<int>(m,0));

    for(int i = 0;i < n;i++){
        for(int j = 0;j < m;j++){
            cin >> grid[i][j];
        }
    }

    vector<vector<int>> visited(n,vector<int>(m,false));

    int result = 0;
    for(int i = 0;i < n;i++){
        for(int j = 0;j < m;j++){
            if(!visited[i][j] && grid[i][j] == 1){
                result++;
                bfs(grid,visited,i,j);
            }
        }
    }
    
    cout << result;
    return 0;
}
```

### 岛屿的最大面积

```C++
#include <iostream>
#include <string>
#include <vector>
#include <queue>
#include <algorithm>
#include <unordered_map>
#include <unordered_set>
#include <sstream>
#include <set>

using namespace std;

int dt[4][2] = {0, 1, 1, 0, -1, 0, 0, -1};

int cnt = 0;

void bfs(vector<vector<int>> & grid,vector<vector<int>> & visited,int x,int y){
    queue<pair<int,int>> q;
    visited[x][y] = true;
    q.push({x,y});
    cnt++;
    while(!q.empty()){
        pair<int,int> pos = q.front(); q.pop();

        int curX = pos.first;
        int curY = pos.second;

        for(int i = 0;i < 4;i++){
            int a = curX + dt[i][0];
            int b = curY + dt[i][1];
            if(a >= 0 && a < grid.size() && b >= 0 && b < grid[0].size() && !visited[a][b] && grid[a][b] == 1){
                visited[a][b] = true;
                cnt++;
                q.push({a,b});
            }
        }
    }
}

int main(){

    int n,m;

    cin >> n >> m;

    vector<vector<int>> grid(n,vector<int>(m,0));

    for(int i = 0;i < n;i++){
        for(int j = 0;j < m;j++){
            cin >> grid[i][j];
        }
    }

    vector<vector<int>> visited(n,vector<int>(m,false));

    int result = 0;
    for(int i = 0;i < n;i++){
        for(int j = 0;j < m;j++){
            if(!visited[i][j] && grid[i][j] == 1){
                cnt = 0;
                bfs(grid,visited,i,j);
                result = max(cnt,result);
            }
        }
    }
    
    cout << result;
    return 0;
}
```

## 2025.3

###  孤岛的总面积

```C++
#include <iostream>
#include <string>
#include <vector>
#include <queue>
#include <algorithm>
#include <unordered_map>
#include <unordered_set>
#include <sstream>
#include <set>

using namespace std;

int dt[4][2] = {0, 1, 1, 0, -1, 0, 0, -1};

int cnt = 0;

void dfs(vector<vector<int>> & grid,vector<vector<bool>> & visited,int x,int y){
    visited[x][y] = true;
    cnt++;
    for(int i = 0;i < 4;i++){
        int a = x + dt[i][0];
        int b = y + dt[i][1];

         if(a >= 0 && b >= 0 && a < grid.size() && b < grid[0].size() && grid[a][b] == 1 && !visited[a][b]){
            dfs(grid,visited,a,b);    
        }
        
    }
}


void bfs(vector<vector<int>> & grid,vector<vector<bool>> & visited,int x,int y){
    queue<pair<int,int>> q;
    visited[x][y] = true;

    q.push({x,y});
    cnt++;
    while(!q.empty()){
        pair<int,int> pos = q.front();q.pop();

        int curX = pos.first;
        int curY = pos.second;

        for(int i = 0;i < 4;i++){
            int a = dt[i][0] + curX;
            int b = dt[i][1] + curY;

            if(a >= 0 && b >= 0 && a < grid.size() && b < grid[0].size() && grid[a][b] == 1 && !visited[a][b]){
                visited[a][b] = true;
                cnt++;
                q.push({a,b});
            }
        }
    }
}


int main(){

    int n,m;

    cin >> n >> m;

    vector<vector<int>> grid(n,vector<int>(m,0));

    for(int i = 0;i < n;i++){
        for(int j = 0;j < m;j++){
            cin >> grid[i][j];
        }
    }

    vector<vector<bool>> visited(n,vector<bool>(m,false));
    
    for(int i = 0;i < m;i++){
        if(grid[0][i] == 1 && !visited[0][i]) dfs(grid,visited,0,i);
        if(grid[n - 1][i] == 1 && !visited[n - 1][i]) dfs(grid,visited,n - 1,i);
    }

    for(int i = 0;i < n;i++){
        if(grid[i][0] == 1 && !visited[i][0]) dfs(grid,visited,i,0);
        if(grid[i][m - 1] == 1 && !visited[i][m - 1]) dfs(grid,visited,i,m - 1);
    }
    cnt = 0;
    for(int i = 0;i < n;i++){
        for(int j = 0;j < m;j++){
            if(grid[i][j] == 1 && !visited[i][j]){
                dfs(grid,visited,i,j);
            }
        }
    }


    cout << cnt;
    return 0;
}
```

### 沉没孤岛

```C++
#include <iostream>
#include <string>
#include <vector>
#include <queue>
#include <algorithm>
#include <unordered_map>
#include <unordered_set>
#include <sstream>
#include <set>

using namespace std;

int dt[4][2] = {0, 1, 1, 0, -1, 0, 0, -1};

int cnt = 0;

void dfs(vector<vector<int>> & grid,vector<vector<bool>> & visited,int x,int y){
    visited[x][y] = true;
    grid[x][y] = 2;
    cnt++;
    for(int i = 0;i < 4;i++){
        int a = x + dt[i][0];
        int b = y + dt[i][1];

         if(a >= 0 && b >= 0 && a < grid.size() && b < grid[0].size() && grid[a][b] == 1 && !visited[a][b]){
            dfs(grid,visited,a,b);    
        }
        
    }
}


void bfs(vector<vector<int>> & grid,vector<vector<bool>> & visited,int x,int y){
    queue<pair<int,int>> q;
    visited[x][y] = true;

    q.push({x,y});
    cnt++;
    while(!q.empty()){
        pair<int,int> pos = q.front();q.pop();

        int curX = pos.first;
        int curY = pos.second;

        for(int i = 0;i < 4;i++){
            int a = dt[i][0] + curX;
            int b = dt[i][1] + curY;

            if(a >= 0 && b >= 0 && a < grid.size() && b < grid[0].size() && grid[a][b] == 1 && !visited[a][b]){
                visited[a][b] = true;
                cnt++;
                q.push({a,b});
            }
        }
    }
}


int main(){

    int n,m;

    cin >> n >> m;

    vector<vector<int>> grid(n,vector<int>(m,0));

    for(int i = 0;i < n;i++){
        for(int j = 0;j < m;j++){
            cin >> grid[i][j];
        }
    }

    vector<vector<bool>> visited(n,vector<bool>(m,false));
    
    for(int i = 0;i < m;i++){
        if(grid[0][i] == 1 && !visited[0][i]) dfs(grid,visited,0,i);
        if(grid[n - 1][i] == 1 && !visited[n - 1][i]) dfs(grid,visited,n - 1,i);
    }

    for(int i = 0;i < n;i++){
        if(grid[i][0] == 1 && !visited[i][0]) dfs(grid,visited,i,0);
        if(grid[i][m - 1] == 1 && !visited[i][m - 1]) dfs(grid,visited,i,m - 1);
    }

    for(int i = 0;i < n;i++){
        for(int j = 0;j < m;j++){
            if(grid[i][j] == 1) grid[i][j] = 0;
            if(grid[i][j] == 2) grid[i][j] = 1;
        }
    }

    for(int i = 0;i < n;i++){
        for(int j = 0;j < m;j++){
            cout << grid[i][j] << " ";
        }
        cout << "\n";
    }

    return 0;
}
```

### 水流问题

```C++
#include <iostream>
#include <string>
#include <vector>
#include <queue>
#include <algorithm>
#include <unordered_map>
#include <unordered_set>
#include <sstream>
#include <set>

using namespace std;

int dt[4][2] = {0,1,1,0,0,-1,-1,0};

int cnt = 0;

void dfs(vector<vector<int>> & grid,vector<vector<bool>> & border,int x,int y){
    border[x][y] = true;

    for(int i = 0;i < 4;i++){
        int a = x + dt[i][0];
        int b = y + dt[i][1];

        if(a >= 0 && b >= 0 && a < grid.size() && b < grid[0].size() && border[a][b] == false){
            if(grid[x][y] <= grid[a][b]){
                dfs(grid,border,a,b);
            }
        }
    }
}


int main(){

    int n,m;

    cin >> n >> m;

    vector<vector<int>> grid(n,vector<int>(m,0));
    vector<vector<bool>> firstBorder(n,vector<bool>(m,false));
    vector<vector<bool>> secondBorder(n,vector<bool>(m,false));


    for(int i = 0;i < n;i++){
        for(int j = 0;j < m;j++){
            cin >> grid[i][j];
        }
    }

    for(int i = 0; i < m;i++){
        dfs(grid,firstBorder,0,i);
        dfs(grid,secondBorder,n - 1,i);
    }

    for(int i = 0;i < n;i++){
        dfs(grid,firstBorder,i,0);
        dfs(grid,secondBorder,i,m - 1);
    }

    for(int i = 0;i < n;i++){
        for(int j = 0;j < m;j++){
            if(firstBorder[i][j] && secondBorder[i][j]){
                cout << i << " " << j << "\n";
            }
        }
    }

    return 0;
}
```

### 建造最大岛屿

```C++
#include <iostream>
#include <string>
#include <vector>
#include <queue>
#include <algorithm>
#include <unordered_map>
#include <unordered_set>
#include <sstream>
#include <set>

using namespace std;

int dt[4][2] = {0,1,1,0,0,-1,-1,0};

int cnt = 0;

void dfs(vector<vector<int>> & grid,vector<vector<bool>> & visited,int x,int y,int mark){
    visited[x][y] = true;
    grid[x][y] = mark;
    cnt++;
    for(int i = 0;i < 4;i++){
        int a = x + dt[i][0];
        int b = y + dt[i][1];
        if(a >= 0 && b >= 0 && a < grid.size() && b < grid[0].size() && grid[a][b] == 1 && !visited[a][b]){
            dfs(grid,visited,a,b,mark);
        }
    }
}


int main(){

    int n,m;

    cin >> n >> m;

    vector<vector<int>> grid(n,vector<int>(m,0));

    vector<vector<bool>> visited(n,vector<bool>(m,false));

    for(int i = 0;i < n;i++){
        for(int j = 0;j < m;j++){
            cin >> grid[i][j];
        }
    }
    
    unordered_map<int,int> gridNum;

    int mark = 2;
    bool isAllGrid = true;

    for(int i = 0;i < n;i++){
        for(int j = 0;j < m;j++){
            if(grid[i][j] == 0) isAllGrid = false;
            if(!visited[i][j] && grid[i][j] == 1){
                cnt = 0;
                dfs(grid,visited,i,j,mark);
                gridNum[mark] = cnt;
                mark++;
            }
        }
    }

    if(isAllGrid){
        cout << n * m << "\n";
        return 0;
    }

    int result = 0;
    unordered_set<int> visitedGrid;

    for(int i = 0;i < n;i++){
        for(int j = 0;j < m;j++){

            if(grid[i][j] != 0) continue;
            cnt = 1;
            visitedGrid.clear();
            for(int k = 0;k < 4;k++){
                int a = i + dt[k][0];
                int b = j + dt[k][1];
                if(a >= 0 && a < grid.size() && b >= 0 && b < grid[0].size()){
                    if(visitedGrid.find(grid[a][b]) == visitedGrid.end()){
                        visitedGrid.insert(grid[a][b]);
                        cnt += gridNum[grid[a][b]];
                    }
                }
            }
            result = max(result,cnt);
        }
    }

    cout << result << "\n";

    return 0;
}
```

### 字符串接龙

```C++
#include <iostream>
#include <string>
#include <vector>
#include <queue>
#include <algorithm>
#include <unordered_map>
#include <unordered_set>
#include <sstream>
#include <set>

using namespace std;

int dt[4][2] = {0,1,1,0,0,-1,-1,0};

int cnt = 0;


int main(){

    int n;

    cin >> n;

    string beginStr,endStr;

    cin >> beginStr >> endStr;

    unordered_set<string> strSet;

    for(int i = 0;i < n;i++){
        string w;
        cin >> w;
        strSet.insert(w);
    }

    queue<string> q;

    unordered_map<string,int> umap;

    q.push(beginStr);

    umap.insert({beginStr,1});

    while(!q.empty()){
        string word = q.front();q.pop();
        int path = umap[word];

        for(int i = 0;i < word.size();i++){
            string newWord = word;

            for(int j = 0;j < 26;j++){
                newWord[i] = j + 'a';

                if(newWord == endStr){
                    cout << path + 1 << "\n";
                    return 0;
                }

                if(strSet.find(newWord) != strSet.end() && umap.find(newWord) == umap.end()){
                    q.push(newWord);
                    umap.insert({newWord,path + 1});
                }

            }
        }
    }

    cout << 0 << "\n";

    return 0;
}
```

### 有向图的完全可达性

```C++
#include <iostream>
#include <string>
#include <vector>
#include <queue>
#include <algorithm>
#include <unordered_map>
#include <unordered_set>
#include <sstream>
#include <set>
#include <list>

using namespace std;

int dt[4][2] = {0,1,1,0,0,-1,-1,0};

void dfs(vector<list<int>> & graph,vector<bool> & visited,int x){
    
    list<int> l = graph[x];
    for(const int & p:l){
        if(visited[p] == false){
            visited[p] = true;
            dfs(graph,visited,p);
        }
    }
}


int main(){

    int n,m;

    cin >> n >> m;

    vector<list<int>> graph(n + 1);

    for(int i = 0;i < m;i++){
        int a,b;
        cin >> a >> b;
        graph[a].push_back(b);
    }

    vector<bool> visited(n + 1,false);

    visited[1] = true;

    dfs(graph,visited,1);

    for(int i = 1;i <= n;i++){
        if(visited[i] == false){
            cout << -1 << "\n";
            return 0;
        }
    }

    cout << 1 << "\n";


    return 0;
}
```

### 岛屿的周长

```C++
#include <iostream>
#include <string>
#include <vector>
#include <queue>
#include <algorithm>
#include <unordered_map>
#include <unordered_set>
#include <sstream>
#include <set>
#include <list>

using namespace std;

int dt[4][2] = {0,1,1,0,0,-1,-1,0};


int main(){

    int n,m;

    cin >> n >> m;

    vector<vector<int>> graph(n,vector<int>(m,0));

    for(int i = 0;i < n;i++){
        for(int j = 0;j < m;j++){
            cin >> graph[i][j];
        }
    }

    int result = 0;

    for(int i = 0;i < n;i++){
        for(int j = 0;j < m;j++){
            if(graph[i][j] == 1){
                for(int k = 0;k < 4;k++){
                    int x = i + dt[k][0];
                    int y = j + dt[k][1];
                    if(x < 0 || y < 0 || x >= graph.size() || y >= graph[0].size() || graph[x][y] == 0){
                        result++;
                    }
                }
            }
        }
    }

    cout << result;

    return 0;
}
```

### 寻找存在的路径

```C++
#include <iostream>
#include <string>
#include <vector>
#include <queue>
#include <algorithm>
#include <unordered_map>
#include <unordered_set>
#include <sstream>
#include <set>
#include <list>

using namespace std;

int dt[4][2] = {0,1,1,0,0,-1,-1,0};

const int N = 10010;

vector<int> father(N,0);

void init(){
    for(int i = 0;i < N;i++){
        father[i] = i;
    }
}

int find(int u){
    if(father[u] == u) return u;

    else return father[u] = find(father[u]);
}

bool isSame(int u,int v){
    u = find(u);
    v = find(v);

    return u == v;
}

void joint(int u,int v){
    u = find(u);
    v = find(v);
    if(u == v) return ;

    father[v] = u;
}


int main(){

    int n,m;

    init();

    cin >> n >> m;

    for(int i = 0;i < m;i++){
        int a,b;
        cin >> a >> b;
        joint(a,b);
    }

    int s,d;

    cin >> s >> d;

    if(isSame(s,d)){
        cout << 1 << "\n";
    }else{
        cout << 0 << "\n";
    }


    return 0;
}
```

### 冗余连接

```C++
#include <iostream>
#include <string>
#include <vector>
#include <queue>
#include <algorithm>
#include <unordered_map>
#include <unordered_set>
#include <sstream>
#include <set>
#include <list>

using namespace std;

int dt[4][2] = {0,1,1,0,0,-1,-1,0};

const int N = 10010;

vector<int> father(N,0);

void init(){
    for(int i = 0;i < N;i++){
        father[i] = i;
    }
}

int find(int u){
    if(father[u] == u) return u;
    else return father[u] = find(father[u]);
}

int isSame(int u,int v){
    u = find(u);
    v = find(v);
    return u == v;
}

void joint(int u,int v){
    u = find(u);
    v = find(v);
    if(u == v) return ;
    father[v] = u;
}

int main(){

    int n;

    init();

    cin >> n;

    for(int i = 0;i < n;i++){
        int a,b;
        cin >> a >> b;

        if(isSame(a,b)){
            cout << a << " " << b;
            return 0;
        }

        joint(a,b);
    }

    return 0;
}
```

### 冗余连接II

```C++
#include <iostream>
#include <string>
#include <vector>
#include <queue>
#include <algorithm>
#include <unordered_map>
#include <unordered_set>
#include <sstream>
#include <set>
#include <list>

using namespace std;

int dt[4][2] = {0,1,1,0,0,-1,-1,0};

const int N = 10010;
int n;
vector<int> father(N,0);

void init(){
    for(int i = 0;i < N;i++){
        father[i] = i;
    }
}

int find(int u){
    if(father[u] == u) return u;
    else return father[u] = find(father[u]);
}

int isSame(int u,int v){
    u = find(u);
    v = find(v);
    return u == v;
}

void joint(int u,int v){
    u = find(u);
    v = find(v);
    if(u == v) return ;
    father[v] = u;
}

void getRemoveEdge(const vector<vector<int>> & edges){
    init();
    for(int i = 0;i < n;i++){
        if(isSame(edges[i][0],edges[i][1])){
            cout << edges[i][0] << " " << edges[i][1];
            return;
        }else{
            joint(edges[i][0],edges[i][1]);
        }
    }
}

bool isTreeAfterRemoveEdge(const vector<vector<int>> & edges,int deleteEdge){
    init();
    for(int i = 0;i < n;i++){
        if(i == deleteEdge) continue;
        if(isSame(edges[i][0],edges[i][1])){
            return false;
        }
        joint(edges[i][0],edges[i][1]);
    }
    return true;
}

int main(){

    cin >> n;
    vector<vector<int>> edges;
    vector<int> inDegree(n + 1,0);
    for(int i = 0;i < n;i++){
        int s,t;
        cin >> s >> t;
        inDegree[t]++;
        edges.push_back({s,t});
    }

    vector<int> vec;

    for(int i = n - 1;i >= 0;i--){
        if(inDegree[edges[i][1]] == 2){
            vec.push_back(i);
        }
    }

    if(vec.size() > 0){
        if(isTreeAfterRemoveEdge(edges,vec[0])){
            cout << edges[vec[0]][0] << " " << edges[vec[0]][1];
        }else{
            cout << edges[vec[1]][0] << " " << edges[vec[1]][1];
        }
        return 0;
    }

    getRemoveEdge(edges);

    return 0;
}
```

### 寻宝（prim算法）

```C++
#include <iostream>
#include <string>
#include <vector>
#include <queue>
#include <algorithm>
#include <unordered_map>
#include <unordered_set>
#include <sstream>
#include <set>
#include <list>
#include <climits>

using namespace std;

int main(){

    int v,e;
    cin >> v >> e;

    vector<vector<int>> grid(v + 1,vector<int>(v + 1,0x3fff));

    for(int i = 0;i < e;i++){
        int x,y,k;

        cin >> x >> y >> k;

        grid[x][y] = k;
        grid[y][x] = k;
    }

    vector<int> minDst(v + 1,0x3fff);
    vector<int> isInTree(v + 1,false);

    for(int i = 1;i < v;i++){
        int cur = 01;
        int minVal = INT_MAX;
        for(int j = 1;j <= v;j++){
            if(!isInTree[j] &&minDst[j] < minVal){
                minVal = minDst[j];
                cur = j;
            }
        }

        isInTree[cur] = true;

        for(int j = 1;j <= v;j++){
            if(!isInTree[j] && grid[cur][j] < minDst[j]){
                minDst[j] = grid[cur][j];
            }
        }
    }

    int result = 0;

    for(int i = 2;i <= v;i++){
        result += minDst[i];
    }

    cout << result << "\n";

    return 0;
}
```

### 寻宝（kruskal算法）

```C++
#include <iostream>
#include <string>
#include <vector>
#include <queue>
#include <algorithm>
#include <unordered_map>
#include <unordered_set>
#include <sstream>
#include <set>
#include <list>
#include <climits>

using namespace std;

struct Edge{
    int l,r,val;
};

const int N = 10010;

vector<int> father(N,-1);

void init(){

    for(int i = 0;i < N;i++){
        father[i] = i;
    }
}

int find(int u){
    if(father[u] == u) return u;
    else return father[u] = find(father[u]);
}

bool isSame(int u,int v){
    u = find(u);
    v = find(v);
    return u == v;
}

void joint(int u,int v){
    u = find(u);
    v = find(v);
    if(u == v) return;
    father[v] = u;
}

int main(){

    int v,e;
    cin >> v >> e;
    vector<Edge> edges;

    for(int i = 0;i < e;i++){
        int v1,v2,val;
        cin >> v1 >> v2 >> val;
        edges.push_back({v1,v2,val});
    }

    sort(edges.begin(),edges.end(),[](const Edge & a,const Edge & b){
        return a.val < b.val;
    });

    init();

    int result = 0;

    for(const Edge & edge : edges){
        int x = find(edge.l);
        int y = find(edge.r);

        if(x != y){
            result += edge.val;
            joint(x,y);
        }
    }
    
    cout << result << "\n";

    return 0;
}
```

### 软件构建

```C++
#include <iostream>
#include <string>
#include <vector>
#include <queue>
#include <algorithm>
#include <unordered_map>
#include <unordered_set>
#include <sstream>
#include <set>
#include <list>
#include <climits>

using namespace std;


int main(){

    int n,m;

    cin >> n >> m;

    vector<int> inDegree(n,0);
    vector<int> result;
    unordered_map<int,vector<int>> umap;

    while(m--){
        int s,t;
        cin >> s >> t;
        inDegree[t]++;
        umap[s].push_back(t);
    }

    queue<int> q;

    for(int i = 0;i < n;i++){
        if(inDegree[i] == 0) q.push(i);
    }

    while(!q.empty()){
        int cur = q.front();q.pop();
        result.push_back(cur);
        vector<int> files = umap[cur];
        for(int i = 0;i < files.size();i++){
            inDegree[files[i]]--;
            if(inDegree[files[i]] == 0) q.push(files[i]); 
        }
    }

    if(result.size() == n){
        for(int i = 0;i < n - 1;i++) cout << result[i] << " ";
        cout << result[n - 1] << "\n";
    }else{
        cout << -1 << "\n";
    }

    return 0;
}
```

### 参加科学大会（dijkstra朴素版）

```C++
#include <iostream>
#include <string>
#include <vector>
#include <queue>
#include <algorithm>
#include <unordered_map>
#include <unordered_set>
#include <sstream>
#include <set>
#include <list>
#include <stdint.h>

using namespace std;


int main(){

    int n,m;
    cin >> n >> m;

    vector<vector<int>> grid(n + 1,vector<int>(n + 1,INT32_MAX));

    for(int i = 0;i < m;i++){
        int p1,p2,val;
        cin >> p1 >> p2 >> val;
        grid[p1][p2] = val;
    }

    vector<int> minDst(n + 1,INT32_MAX);
    vector<bool> visited(n + 1,false);

    minDst[1] = 0;

    for(int i = 1;i <= n;i++){
        int minVal = INT32_MAX;
        int cur = 1;

        for(int j = 1;j <= n;j++){
            if(!visited[j] && minDst[j] < minVal){
                minVal = minDst[j];
                cur = j;
            }
        }

        visited[cur] = true;

        for(int j = 1;j <= n;j++){
            if(!visited[j] && grid[cur][j] != INT32_MAX && minDst[cur] + grid[cur][j] < minDst[j]){
                minDst[j] = minDst[cur] + grid[cur][j];
            }
        }
    }

    if(minDst[n] == INT32_MAX) cout << -1 << "\n";
    else cout << minDst[n] << "\n";
    
    return 0;
}
```

### 参加科学大会（dijkstra堆优化）

```C++
#include <iostream>
#include <string>
#include <vector>
#include <queue>
#include <algorithm>
#include <unordered_map>
#include <unordered_set>
#include <sstream>
#include <set>
#include <list>
#include <stdint.h>

using namespace std;

struct Edge{
    int to;
    int val;
    Edge(int a,int b):to(a),val(b){}
};

struct MyCompare{
    bool operator()(const pair<int,int> & lhs,const pair<int,int> & rhs){
        return lhs.second > rhs.second;
    }
};

int main(){

    int n,m;
    cin >> n >> m;

    vector<list<Edge>> grid(n + 1);

    for(int i = 0;i < m;i++){
        int p1,p2,val;
        cin >> p1 >> p2 >> val;
        grid[p1].push_back(Edge(p2,val));
    }


    vector<int> minDst(n + 1,INT32_MAX);
    vector<bool> visited(n + 1,false);

    priority_queue<pair<int,int>,vector<pair<int,int>>,MyCompare> pq;

    pq.push({1,0});

    minDst[1] = 0;

    while(!pq.empty()){
        pair<int,int> cur = pq.top();pq.pop();

        if(visited[cur.first]) continue;

        visited[cur.first] = true;

        for(Edge & edge: grid[cur.first]){
            if(!visited[edge.to] && minDst[cur.first] + edge.val < minDst[edge.to]){
                minDst[edge.to] = minDst[cur.first] + edge.val;
                pq.push({edge.to,minDst[edge.to]});
            }
        }
    }

    if(minDst[n] == INT32_MAX) cout << -1 << "\n";
    else cout << minDst[n] << "\n";
    
    return 0;
}
```

###  城市间货物运输 I(Bellman_ford )

```C++
#include <iostream>
#include <string>
#include <vector>
#include <queue>
#include <algorithm>
#include <unordered_map>
#include <unordered_set>
#include <sstream>
#include <set>
#include <list>
#include <stdint.h>

using namespace std;

int main(){

    int n,m;
    cin >> n >> m;

    vector<vector<int>> grid;

    for(int i = 0;i < m;i++){
        int p1,p2,val;
        cin >> p1 >> p2 >> val;
        grid.push_back({p1,p2,val});
    }
    
    vector<int> minDst(n + 1,INT32_MAX);
    minDst[1] = 0;
    for(int i = 1;i < n;i++){
        for(vector<int> & side : grid){
            int from = side[0];
            int to = side[1];
            int val = side[2];

            if(minDst[from] != INT32_MAX && minDst[to] > minDst[from] + val){
                minDst[to] = minDst[from] + val;
            }
        }
    }
    
    if(minDst[n] == INT32_MAX) cout << "unconnected" << "\n";
    else cout << minDst[n] << "\n";
    
    return 0;
}
```

### 城市间货物运输 I(Bellman_ford 队列优化)

```C++
#include <iostream>
#include <string>
#include <vector>
#include <queue>
#include <algorithm>
#include <unordered_map>
#include <unordered_set>
#include <sstream>
#include <set>
#include <list>
#include <stdint.h>

using namespace std;

struct Edge{
    int to;
    int val;
    Edge(int a,int b):to(a),val(b){}
};


int main(){

    int n,m;
    cin >> n >> m;

    vector<list<Edge>> grid(n + 1);

    for(int i = 0;i < m;i++){
        int p1,p2,val;
        cin >> p1 >> p2 >> val;
        grid[p1].push_back(Edge(p2,val));
    }

    vector<bool> inQ(n + 1,false);
    
    vector<int> minDst(n + 1,INT32_MAX);
    
    queue<int> q;

    minDst[1] = 0;
    q.push(1);

    while(!q.empty()){
        int p = q.front();q.pop();
        inQ[p] = false;
        for(Edge & edge : grid[p]){
            if(minDst[edge.to] > minDst[p] + edge.val){
                minDst[edge.to] = minDst[p] + edge.val;
                if(inQ[edge.to] == false){
                    q.push(edge.to);
                    inQ[edge.to] = true;
                }
            }
        }
    }
    
    if(minDst[n] == INT32_MAX) cout << "unconnected" << "\n";
    else cout << minDst[n] << "\n";
    
    return 0;
}
```

### 城市间货物运输 I(Bellman_ford 队列优化 判断负权回路)

```C++
#include <iostream>
#include <string>
#include <vector>
#include <queue>
#include <algorithm>
#include <unordered_map>
#include <unordered_set>
#include <sstream>
#include <set>
#include <list>
#include <stdint.h>

using namespace std;

struct Edge{
    int to;
    int val;
    Edge(int a,int b):to(a),val(b){}
};


int main(){

    int n,m;
    cin >> n >> m;

    vector<list<Edge>> grid(n + 1);

    for(int i = 0;i < m;i++){
        int p1,p2,val;
        cin >> p1 >> p2 >> val;
        grid[p1].push_back(Edge(p2,val));
    }

    vector<int> inQ(n + 1,false);
    
    vector<int> minDst(n + 1,INT32_MAX);
    
    queue<int> q;
    bool flag = false;
    minDst[1] = 0;
    q.push(1);
    inQ[1]++;
    while(!q.empty()){
        int p = q.front();q.pop();

        for(Edge & edge : grid[p]){
            if(minDst[edge.to] > minDst[p] + edge.val){
                minDst[edge.to] = minDst[p] + edge.val;
                q.push(edge.to);
                inQ[edge.to]++;
                if(inQ[edge.to] == n){
                    flag = true;
                    while(!q.empty()) q.pop();
                    break;
                }
            }
        }
    }
    
    if(flag) cout << "circle" << "\n";
    else if(minDst[n] == INT32_MAX) cout << "unconnected" << "\n";
    else cout << minDst[n] << "\n";
    
    return 0;
}
```

### bellman_ford之单源有限最短路

```C++
#include <iostream>
#include <string>
#include <vector>
#include <queue>
#include <algorithm>
#include <unordered_map>
#include <unordered_set>
#include <sstream>
#include <set>
#include <list>
#include <stdint.h>

using namespace std;


int main(){

    int n,m;
    cin >> n >> m;

    vector<vector<int>> grid;

    for(int i = 0;i < m;i++){
        int p1,p2,val;
        cin >> p1 >> p2 >> val;
        grid.push_back({p1,p2,val});
    }

    int src,dst,k;
    cin >> src >> dst >> k;

    vector<int> midDst(n + 1,INT32_MAX);

    midDst[src] = 0;

    for(int i = 0;i <= k;i++){
        vector<int> midDst_copy = midDst;
        for(vector<int> & side : grid){
            int from = side[0];
            int to = side[1];
            int val = side[2];

            if(midDst_copy[from] != INT32_MAX && midDst[to] > midDst_copy[from] + val){
                midDst[to] = midDst_copy[from] + val;
            }
        }
    }    
    
    if(midDst[dst] == INT32_MAX) cout << "unreachable" << endl;
    else cout << midDst[dst] << endl;
    
    return 0;
}
```

### Floyd 算法

```C++
#include <iostream>
#include <string>
#include <vector>
#include <queue>
#include <algorithm>
#include <unordered_map>
#include <unordered_set>
#include <sstream>
#include <set>
#include <list>
#include <stdint.h>

using namespace std;


int main(){

    int n,m;
    cin >> n >> m;

    vector<vector<vector<int>>> grid(n + 1,vector<vector<int>> (n + 1,vector<int>(n + 1,0x3fff)));
    
    for(int i = 0;i < m;i++){
        int p1,p2,val;
        cin >> p1 >> p2 >> val;
        grid[p1][p2][0] = val;
        grid[p2][p1][0] = val;
    }

    for(int k = 1;k <= n;k++){
        for(int i = 1;i <= n;i++){
            for(int j = 1;j <= n;j++){
                grid[i][j][k] = min(grid[i][j][k - 1],grid[i][k][k - 1] + grid[k][j][k - 1]);
            }
        }
    }


    int q;

    cin >> q;

    while(q--){
        int s,e;
        cin >> s >> e;

        if(grid[s][e][n] == 0x3fff) cout << -1 << "\n";
        else cout << grid[s][e][n] << "\n";
    }


    return 0;
}
```

### A*算法

```C++
#include <iostream>
#include <string>
#include <vector>
#include <queue>
#include <algorithm>
#include <unordered_map>
#include <unordered_set>
#include <sstream>
#include <set>
#include <list>
#include <stdint.h>
#include <string.h>

using namespace std;

int maps[1001][1001];

int dt[8][2] = {-2,1,-2,-1,-1,2,-1,-2,1,2,1,-2,2,1,2,-1};

int b1,b2;

struct Knight{
    int x,y;
    int g,h,f;

    bool operator < (const Knight & rhs) const{
        return rhs.f < f;
    }
};

int Heuristic(const Knight & k){
    return (k.x - b1) * (k.x - b1) + (k.y - b2) * (k.y - b2);
}

priority_queue<Knight> q;

void start(const Knight & k){
    Knight cur,next;
    q.push(k);

    while(!q.empty()){
        cur = q.top();q.pop();

        if(cur.x == b1 && cur.y == b2) break;

        for(int i = 0;i < 8;i++){
            next.x = cur.x + dt[i][0];
            next.y = cur.y + dt[i][1];

            if(next.x < 1 || next.x > 1000 || next.y < 1 || next.y > 1000) continue;

            if(!maps[next.x][next.y]){
                maps[next.x][next.y] = maps[cur.x][cur.y] + 1;

                next.g = cur.g + 5;
                next.h = Heuristic(next);
                next.f = next.g + next.h;
                q.push(next);
            }
        }
    }
}


int main(){
    int n,a1,a2;

    cin >> n;

    for(int i = 0;i < n;i++){
        cin >> a1 >> a2 >> b1 >> b2;
        memset(maps,0,sizeof(maps));
        Knight s;
        s.x = a1;
        s.y = a2;
        s.g = 0;
        s.h = Heuristic(s);
        s.f = s.g + s.h;
        start(s);
        while(!q.empty()) q.pop();
        cout << maps[b1][b2] << "\n";
    }

    return 0;
}
```

### 快排

```C++
#include <iostream>
#include <cstdio>

using namespace std;

const int N = 1e6 + 10;

int q[N];

void quick_sort(int q[],int l,int r){
    if(l >= r) return;

    int x = q[(l + r) / 2],i = l - 1,j = r + 1;

    while(i < j){
        do i++; while(q[i] < x);
        do j--; while(q[j] > x);
        if(i < j) swap(q[i],q[j]);
    }

    quick_sort(q,l,j);
    quick_sort(q,j + 1,r);
}

int main(){

    int n;
    cin >> n;

    for(int i = 0;i < n;i++) scanf("%d",&q[i]);
    quick_sort(q,0,n - 1);
    for(int i = 0;i < n;i++) printf("%d ",q[i]);
    return 0;

}
```

### 第k个数

```C++
#include <iostream>
#include <cstdio>

using namespace std;

const int N = 1e6 + 10;

int q[N];

int k;

int quick_sort(int l,int r,int k){
    if(l >= r) return q[l];

    int x = q[(l + r) / 2],i = l - 1,j = r + 1;

    while(i < j){
        do i++; while(q[i] < x);
        do j--; while(q[j] > x);
        if(i < j) swap(q[i],q[j]);
    }

    int len = j - l + 1;
    if(len >= k) return quick_sort(l,j,k);
    else return quick_sort(j + 1,r,k - len);
    
}

int main(){

    int n;
    cin >> n;
    cin >> k;

    for(int i = 0;i < n;i++) scanf("%d",&q[i]);
    cout << quick_sort(0,n - 1,k);

    return 0;

}
```

### 归并排序

```C++
#include <iostream>
#include <cstdio>

using namespace std;

const int N = 1e6 + 10;

int q[N],tmp[N];


void merge_sort(int l,int r){
    if(l >= r) return;
    
    int mid = (l + r) >> 1;

    merge_sort(l,mid);
    merge_sort(mid + 1,r);

    int i = l,j = mid + 1;
    int k = 0;
    while(i <= mid && j <= r){
        if(q[i] < q[j]) tmp[k++] = q[i++];
        else tmp[k++] = q[j++];
    }

    while(i <= mid) tmp[k++] = q[i++];
    while(j <= r) tmp[k++] = q[j++];

    for(int i = l,k = 0;i <= r;i++){
        q[i] = tmp[k++];
    }
}

int main(){

    int n;
    cin >> n;
    for(int i = 0;i < n;i++) scanf("%d",&q[i]);

    merge_sort(0,n - 1);

    for(int i = 0;i < n;i++) printf("%d ",q[i]);

    return 0;

}
```

### 数的范围

```C++
#include <iostream>
#include <cstdio>

using namespace std;

typedef long long LL;

const int N = 1e6 + 10;

int tp[N];


int main(){

    int n,q;
    cin >> n >> q;
    for(int i = 0;i < n;i++) scanf("%d",&tp[i]);

    while(q--){
        int x;
        cin >> x;

        int l = 0,r = n - 1;

    
        while(l < r){
            int mid = l + r >> 1;
            if(tp[mid] >= x) r = mid;
            else l = mid + 1;
        }

        if(tp[l] != x){
            cout << "-1 -1" << "\n";

        } else{
            cout << l << " ";

            l = 0,r = n - 1;

            while(l < r){
                int mid = l + r + 1 >> 1;
                if(tp[mid] <= x) l = mid;
                else r = mid - 1;
            }

            cout << r << "\n";
        }   
    }
    return 0;
}
```

### 数的三次方根

```C++
#include <iostream>
#include <cstdio>
#include <cmath>

using namespace std;

typedef long long LL;

const int N = 1e6 + 10;

int tp[N];


int main(){

    double n;
    cin >> n;
    
    double l = -1000,r = 1000;

    while(abs(r - l) > 1e-8){
        double mid = (l + r) / 2;
        if(mid * mid * mid < n) l = mid;
        else r = mid;
    }

    printf("%.6lf",l);    

    return 0;

}
```

### 高精度加法

```C++
#include <iostream>
#include <cstdio>
#include <cmath>
#include <vector>
#include <string>

using namespace std;

vector<int> add(vector<int> & A,vector<int> & B){
    vector<int> C;
    int t = 0;

    for(int i = 0;i < A.size() || i < B.size();i++){
        if(i < A.size()) t += A[i];
        if(i < B.size()) t += B[i];

        C.push_back(t % 10);
        t /= 10;
    }

    if(t) C.push_back(t);

    while(C.back() == 0 && C.size() > 1) C.pop_back();

    return C;
}

int main(){

    string a,b;
    cin >> a >> b;

    vector<int> A,B;

    for(int i = a.size() - 1;i >= 0;i--) A.push_back(a[i] - '0');
    for(int i = b.size() - 1;i >= 0;i--) B.push_back(b[i] - '0');

    vector<int> C = add(A,B);

    for(int i = C.size() - 1;i >= 0;i--){
        cout << C[i];
    }

    return 0;

}
```

### 高精度减法

```C++
#include <iostream>
#include <cstdio>
#include <cmath>
#include <vector>
#include <string>

using namespace std;

bool cmp(vector<int> & A,vector<int> & B){
    if(A.size() != B.size()) return (A.size() > B.size());

    for(int i = A.size() - 1;i >= 0;i--){
        if(A[i] != B[i]) return (A[i] > B[i]);
    }

    return true;
}

vector<int> sub(vector<int> & A,vector<int> & B){
    vector<int> C;
    int t = 0;

    for(int i = 0;i < A.size();i++){
        if(i < B.size()) t = A[i] - B[i] - t;
        else t = A[i] - t;
        C.push_back((t + 10) % 10);

        if(t >= 0) t = 0;
        else t = 1;
    }

    while(C.back() == 0 && C.size() > 1) C.pop_back();
    return C;
}


int main(){

    string a,b;
    cin >> a >> b;

    vector<int> A,B;

    for(int i = a.size() - 1;i >= 0;i--) A.push_back(a[i] - '0');
    for(int i = b.size() - 1;i >= 0;i--) B.push_back(b[i] - '0');

    vector<int> C;

    if(cmp(A,B)){
        C = sub(A,B);
    }else{
        C = sub(B,A);
        cout << "-";
    }

    for(int i = C.size() - 1;i >= 0;i--){
        cout << C[i];
    }

    return 0;

}
```

### 高精度乘法

```C++
#include <iostream>
#include <cstdio>
#include <cmath>
#include <vector>
#include <string>

using namespace std;


vector<int> mul(vector<int> & A,int b){
    vector<int> C;
    int t = 0;

    for(int i = 0;i < A.size() || t;i++){
        if(i < A.size()) t = A[i] * b + t;
        C.push_back(t % 10);    
        t /= 10;
    }

    while(C.back() == 0 && C.size() > 1) C.pop_back();
    return C;
}


int main(){

    string a;
    int b;
    cin >> a >> b;

    vector<int> A,B;

    for(int i = a.size() - 1;i >= 0;i--) A.push_back(a[i] - '0');

    vector<int> C = mul(A,b);

    for(int i = C.size() - 1;i >= 0;i--){
        cout << C[i];
    }

    return 0;

}
```

### 高精度除法

```C++
#include <iostream>
#include <cstdio>
#include <cmath>
#include <vector>
#include <string>
#include <algorithm>

using namespace std;


vector<int> div(vector<int> & A,int b,int & r){
    vector<int> C;
    r = 0;
    for(int i = A.size() - 1;i >= 0;i--){
        C.push_back((r * 10 + A[i]) / b);
        r = ((r * 10) + A[i]) % b;
    }

    reverse(C.begin(),C.end());

    while(C.back() == 0 && C.size() > 1) C.pop_back();

    return C;
}


int main(){

    string a;
    int b;
    cin >> a >> b;

    vector<int> A,B;

    for(int i = a.size() - 1;i >= 0;i--) A.push_back(a[i] - '0');

    int r;
    vector<int> C = div(A,b,r);

    for(int i = C.size() - 1;i >= 0;i--){
        cout << C[i];
    }

    cout << "\n" << r;

    return 0;

}
```

### 前缀和

```C++
#include <iostream>
#include <cstdio>
#include <cmath>
#include <vector>
#include <string>
#include <algorithm>

using namespace std;

const int N = 1e5 + 10;

int a[N],s[N];


int main(){

    int n,m;

    cin >> n >> m;
    for(int i = 1;i <= n;i++){
        cin >> a[i];
        s[i] = s[i - 1] + a[i];
    }

    while(m--){
        int l,r;
        scanf("%d%d",&l,&r);

        printf("%d\n",s[r] - s[l - 1]);
    }

    return 0;

}
```

### 子矩阵的和

```C++
#include <iostream>
#include <cstdio>
#include <cmath>
#include <vector>
#include <string>
#include <algorithm>

using namespace std;

const int N = 1010;

int a[N][N],s[N][N];

int main(){

    int n,m,q;

    cin >> n >> m >> q;
    for(int i = 1;i <= n;i++){
        for(int j = 1;j <= m;j++){
            cin >> a[i][j];
        }
    }

    for(int i = 1;i <= n;i++){
        for(int j = 1;j <= m;j++){
            s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + a[i][j];
        }
    }

    while(q--){
        int x1,y1,x2,y2;
        scanf("%d%d%d%d",&x1,&y1,&x2,&y2);
        printf("%d\n",s[x2][y2] - s[x2][y1 - 1] - s[x1 - 1][y2] + s[x1 - 1][y1 - 1]);
    }

    return 0;

}
```

### 差分

```C++
#include <iostream>
#include <cstdio>
#include <cmath>
#include <vector>
#include <string>
#include <algorithm>

using namespace std;

const int N = 1e5 + 10;

int a[N],b[N];

int main(){

    int n,m;

    cin >> n >> m;
    for(int i = 1;i <= n;i++){
        cin >> a[i];
        b[i] += a[i];
        b[i + 1] -= a[i];
    }

    while(m--){
        int l,r,c;
        cin >> l >> r >> c;
        b[l] += c;
        b[r + 1] -= c;
    }

    for(int i = 1;i <= n;i++){
        a[i] = a[i - 1] + b[i];
        cout << a[i] << " ";
    }


    return 0;

}
```

### 差分矩阵

```C++
#include <iostream>
#include <cstdio>
#include <cmath>
#include <vector>
#include <string>
#include <algorithm>

using namespace std;

const int N = 1010;

int a[N][N],b[N][N];

int main(){

    int n,m,q;

    cin >> n >> m >> q;
    for(int i = 1;i <= n;i++){
        for(int j = 1;j <= m;j++){
            cin >> a[i][j];
            b[i][j] += a[i][j];
            b[i + 1][j] -= a[i][j];
            b[i][j + 1] -= a[i][j];
            b[i + 1][j + 1] += a[i][j];
        }
    }
    
    while(q--){
        int x1,y1,x2,y2,c;
        scanf("%d%d%d%d%d",&x1,&y1,&x2,&y2,&c);

        b[x1][y1] += c;
        b[x2 + 1][y1] -= c;
        b[x1][y2 + 1] -= c;
        b[x2 + 1][y2 + 1] += c;
    }

    for(int i = 1;i <= n;i++){
        for(int j = 1;j <= m;j++){
            a[i][j] = a[i - 1][j] + a[i][j - 1] - a[i - 1][j - 1] + b[i][j];
            cout << a[i][j] << " ";
        }
        cout << "\n";
    }

    return 0;

}
```

### 最长连续不重复子串

```C++
#include <iostream>
#include <cstdio>
#include <cmath>
#include <vector>
#include <string>
#include <algorithm>

using namespace std;

const int N = 1e5 + 10;

int a[N],s[N];

int main(){
    int n;
    cin >> n;

    int l = 0;
    int j = 0;
    for(int i = 0;i < n;i++){
        scanf("%d",&a[i]);
        s[a[i]]++;
        while(s[a[i]] > 1) s[a[j++]]--;
        l = max(l,i - j + 1);
    }
    
    cout << l;
    return 0;

}
```

### 数组元素的目标和

```C++
#include <iostream>
#include <cstdio>
#include <cmath>
#include <vector>
#include <string>
#include <algorithm>

using namespace std;

const int N = 1e5 + 10;

int a[N],b[N];

int main(){
    int n,m,x;
    cin >> n >> m >> x;
    for(int i = 0;i < n;i++) cin >> a[i];
    for(int i = 0;i < m;i++) cin >> b[i];

    
    for(int i = 0,j = m - 1;i < n;i++){
        while(j >= 0 && a[i] + b[j] > x) j--;
        if(a[i] + b[j] == x){
            cout << i << " " << j;
            break;
        }
        
    }

    return 0;

}
```

### 判断子序列

```C++
#include <iostream>
#include <cstdio>
#include <cmath>
#include <vector>
#include <string>
#include <algorithm>

using namespace std;

const int N = 1e5 + 10;

int a[N],b[N];

int main(){
    int n,m,x;
    cin >> n >> m;
    for(int i = 0;i < n;i++) cin >> a[i];
    for(int i = 0;i < m;i++) cin >> b[i];

    int j = 0;
    for(int i = 0;i < m;i++){
        if(b[i] == a[j]) j++;
        if(j == n) break;
    }

    if(j == n) cout << "Yes";
    else cout << "No";

    return 0;

}
```

### 二进制中1的个数

```C++
#include <iostream>
#include <cstdio>
#include <cmath>
#include <vector>
#include <string>
#include <algorithm>

using namespace std;

int lowbit(int x){
    return (x & -x);
}

int main(){
    int n;

    cin >> n;

    while(n--){
        int x;

        cin >> x;
        int res = 0;
        while(x > 0){
            x -= lowbit(x);
            res++;
        }
        cout << res << " ";
    }

    return 0;

}
```

### 区间和

```C++
#include <iostream>
#include <cstdio>
#include <cmath>
#include <vector>
#include <string>
#include <algorithm>

using namespace std;

typedef pair<int,int> PII;

const int N = 300010;

int a[N];
int s[N];

vector<int> alls;
vector<PII> add;
vector<PII> query;

int find(int x){

    int l = 0,r = alls.size() - 1;

    while(l < r){
        int mid = l + r >> 1;

        if(alls[mid] >= x) r = mid;
        else l = mid + 1;
    }

    return r + 1;
}

int main(){
    int n,m;

    cin >> n >> m;

    for(int i = 0;i < n;i++){
        int x,c;
        cin >> x >> c;
        alls.push_back(x);
        add.push_back({x,c});
    }

    for(int i = 0;i < m;i++){
        int l,r;
        cin >> l >> r;
        alls.push_back(l);
        alls.push_back(r);
        query.push_back({l,r});
    }

    sort(alls.begin(),alls.end());

    alls.erase(unique(alls.begin(),alls.end()),alls.end());

    for(auto & it : add){
        int x = find(it.first);
        a[x] += it.second;
    }

    for(int i = 1;i <= alls.size();i++) s[i] = s[i - 1] + a[i];

    for(auto & it : query){
        int l = find(it.first);
        int r = find(it.second);
        cout << s[r] - s[l - 1] << "\n";
    }

    return 0;

}
```

### 区间合并

```C++
#include <iostream>
#include <cstdio>
#include <cmath>
#include <vector>
#include <string>
#include <algorithm>

using namespace std;

typedef pair<int,int> PII;

vector<PII> alls;

int main(){
    int n,m;

    cin >> n;

    for(int i = 0;i < n;i++){
        int l,r;
        cin >> l >> r;

        alls.push_back({l,r});
    }

    sort(alls.begin(),alls.end());

    int st = -2e9,ed = -2e9;

    vector<PII> res;
    for(int i = 0;i < alls.size();i++){
        int l = alls[i].first;
        int r = alls[i].second;

        if(l > ed){
            if(st != -2e9) res.push_back({st,ed});
            st = l,ed = r; 
        }else{
            ed = max(ed,r);
        }
    }
   
    res.push_back({st,ed});
    cout << res.size();

    return 0;

}
```

### 单链表

```C++
#include <iostream>
#include <cstdio>
#include <cmath>
#include <vector>
#include <string>
#include <algorithm>

using namespace std;

const int N = 1e5 + 10;

int head,idx;

int e[N],ne[N];

void init(){
    head = -1;
    idx = 0;
}

void add_to_head(int x){
    e[idx] = x;
    ne[idx] = head;
    head = idx;
    idx++;
}

void add(int k,int x){
    e[idx] = x;
    ne[idx] = ne[k - 1];
    ne[k - 1] = idx;
    idx++;
}

void del(int k){
    if(k == 0){
        if(head != -1) head = ne[head];
    }else{
        ne[k - 1] = ne[ne[k - 1]];
    }
}


int main(){
    int m;

    init();

    cin >> m;

    while(m--){
        char c;
        cin >> c;
        if(c == 'H'){
            int x;
            cin >> x;
            add_to_head(x);
        }else if(c == 'D'){
            int k;
            cin >> k;
            del(k);
        }else{
            int k,x;
            cin >> k >> x;
            add(k,x);
        }
    }
    
    for(int i = head;i != -1;i = ne[i]){
        cout << e[i] << " ";
    }

    return 0;

}
```

### 双链表

```C++
#include <iostream>
#include <cstdio>
#include <cmath>
#include <vector>
#include <string>
#include <algorithm>

using namespace std;

const int N = 1e5 + 10;

int idx;

int l[N],r[N];

int e[N];

void init(){
    r[0] = 1;
    l[1] = 0;
    idx = 2;
}

void add(int k,int x){
    e[idx] = x;
    l[idx] = k;
    r[idx] = r[k];
    l[r[k]] = idx;
    r[k] = idx;
    idx++;
}

void del(int k){
    r[l[k]] = r[k];
    l[r[k]] = l[k];
}

int main(){
    int m;
    init();

    cin >> m;

    while(m--){
        string c;

        cin >> c;

        if(c == "L"){
            int x;
            cin >> x;

            add(0,x);
        }else if(c == "R"){
            int x;
            cin >> x;
            add(l[1],x);
        }else if(c == "D"){
            int k;
            cin >> k;
            del(k + 1);
        }else if(c == "IL"){
            int k,x;
            cin >> k >> x;
            add(l[k + 1],x);
        }else{
            int k,x;
            cin >> k >> x;
            add(k + 1,x);
        }
    }
    
    for(int i = r[0];i != 1;i = r[i]){
        cout << e[i] << " ";
    }

    return 0;

}
```

### 栈

```C++
#include <iostream>
#include <cstdio>
#include <cmath>
#include <vector>
#include <string>
#include <algorithm>

using namespace std;

const int N = 1e5 + 10;

int st[N],tt;

void push(int x){
    st[++tt] = x;
}

void pop(){
    tt--;
}

bool empty(){
    if(tt <= 0) return true;
    else return false;
}

int query(){
    return st[tt];
}


int main(){
    int m;
    
    cin >> m;

    while(m--){
        string c;

        cin >> c;

        if(c == "push"){
            int x;
            cin >> x;
            push(x);
        }else if(c == "query"){
            cout << query() << "\n";
        }else if(c == "pop"){
            pop();
        }else{
            if(empty()){
                cout << "YES" << "\n";
            }else{
                cout << "NO" << "\n";
            }
        }
    }
    
    return 0;
}
```

### 表达式求值

```C++
#include <iostream>
#include <cstdio>
#include <cmath>
#include <vector>
#include <string>
#include <algorithm>
#include <stack>
#include <unordered_map>

using namespace std;

stack<char> op;
stack<int> num;

void eval(){
    int b = num.top();num.pop();
    int a = num.top();num.pop();

    char c = op.top();op.pop();

    int x ;
    if(c == '+') x = a + b;
    else if(c == '-') x = a - b;
    else if(c == '*') x = a * b;
    else x = a / b;
    num.push(x);
}

int main(){
    string s;
    cin >> s;
    
    unordered_map<char,int> pr{{'+',1},{'-',1},{'*',2},{'/',2}};

    for(int i = 0;i < s.size();i++){
        char c = s[i];
        if(isdigit(c)){
            int x = 0,j = i;
            while(j < s.size() && isdigit(s[j])){
                x = x * 10 + (s[j++] - '0');
            }
            i = j - 1;
            num.push(x);
        }else if(c == '(') op.push(c);
        else if(c == ')'){
            while(op.top() != '(') eval();
            op.pop();
        }else{
            while(op.size() > 0 && op.top() != '(' && pr[op.top()] >= pr[c]) eval();
            op.push(c);
        }
    }

    while(op.size() > 0) eval();
    cout << num.top() << "\n";
    
    return 0;
}
```

### 模拟队列

```C++
#include <iostream>
#include <cstdio>
#include <cmath>
#include <vector>
#include <string>
#include <algorithm>
#include <stack>
#include <unordered_map>

using namespace std;

const int N = 1e5 + 10;

int q[N];

int hh = 0;
int tt = -1;

void push(int x){
    q[++tt] = x;
}

void pop(){
    hh++;
}

bool empty(){
    if(tt < hh) return true;
    else return false;
}

void query(){
    cout << q[hh] << "\n";
}

int main(){
    int m;
    cin >> m;
    while(m--){
        string s;
        cin >> s;
        if(s == "push"){
            int x;
            cin >> x;
            push(x);
        }else if(s == "empty"){
            if(empty()){
                cout << "YES" << "\n";
            }else{
                cout << "NO" << "\n";
            }
        }else if(s == "pop"){
            pop();
        }else{
            query();
        }
    }
    
    return 0;
}
```

### 单调栈

```C++
#include <iostream>
#include <cstdio>
#include <cmath>
#include <vector>
#include <string>
#include <algorithm>
#include <stack>
#include <unordered_map>

using namespace std;

const int N = 1e5 + 10;

int tt = 0;

int q[N];

int main(){
    int n;

    cin >> n;

    for(int i = 0;i < n;i++){
        int x;
        cin >> x;
        while(tt && q[tt] >=x) tt--;
        if(!tt) printf("-1 ");
        else printf("%d ",q[tt]);

        q[++tt] = x;
    }

    return 0;
}
```

### 单调队列

```C++
#include <iostream>
#include <cstdio>
#include <cmath>
#include <vector>
#include <string>
#include <algorithm>
#include <stack>
#include <unordered_map>

using namespace std;

const int N = 1e6 + 10;

int hh = 0,tt = -1;
int a[N],q[N];


int main(){
    int n,k;

    cin >> n >> k;
    for(int i = 0;i < n;i++) scanf("%d",&a[i]);

    for(int i = 0;i < n;i++){
        if(hh <= tt && i - k + 1 > q[hh]) hh++;
        while(hh <= tt && a[q[tt]] >= a[i]) tt--;

        q[++tt] = i;

        if(i >= k - 1) printf("%d ",a[q[hh]]);
    }

    cout << "\n";

    hh = 0,tt = -1;

    for(int i = 0;i < n;i++){
        if(hh <= tt && i - k + 1 > q[hh]) hh++;

        while(hh <= tt && a[q[tt]] <= a[i]) tt--;

        q[++tt] = i;

        if(i >= k - 1) printf("%d ",a[q[hh]]);
    }
    
    return 0;
}
```

### KMP

```C++
#include <iostream>
#include <cstdio>
#include <cmath>
#include <vector>
#include <string>
#include <algorithm>
#include <stack>
#include <unordered_map>

using namespace std;

const int N = 1e5 + 10;
const int M = 1e6 + 10;

int n,m;
char s[M],p[N];
int ne[N];

int main(){
    cin >> n >> p + 1 >> m >> s + 1;

    for(int i = 2,j = 0;i <= n;i++){
        while(j && p[i] != p[j + 1]) j = ne[j];
        if(p[i] == p[j + 1]) j++;
        ne[i] = j;
    }

    for(int i = 1,j = 0;i <= m;i++){
        while(j && s[i] != p[j + 1]) j = ne[j];
        if(s[i] == p[j + 1]) j++;
        if(j == n){
            printf("%d ",i - n);
            j = ne[j];
        }
    }
    return 0;
}
```

### Trie字符串统计

```C++
#include <iostream>
#include <cstdio>
#include <cmath>
#include <vector>
#include <string>
#include <algorithm>
#include <stack>
#include <unordered_map>

using namespace std;

const int N = 1e5 + 10;

int idx;
int son[N][26],cnt[N];
char str[N];

void insert(char * str){
    int p = 0;
    for(int i = 0;str[i];i++){
        int u = str[i] - 'a';
        if(!son[p][u]) son[p][u] = ++idx;
        p = son[p][u];
    }
    cnt[p]++;
}

int query(char * str){
    int p = 0;
    for(int i = 0;str[i];i++){
        int u = str[i] - 'a';
        if(!son[p][u]) return 0;
        p = son[p][u];
    }
    return cnt[p];
}

int main(){
    int n;
    cin >> n;

    while(n--){
        char op[2];
        scanf("%s%s",op,str);
        if(*op == 'I'){
            insert(str);
        }else{
            printf("%d\n",query(str));
        }
    }
    
    return 0;
}
```

### 最大异或对

```C++
#include <iostream>
#include <cstdio>
#include <cmath>
#include <vector>
#include <string>
#include <algorithm>
#include <stack>
#include <unordered_map>

using namespace std;

const int N = 1e5 + 10;

int idx;
int son[N * 31][2];
char str[N];

void insert(int x){
    int p = 0;

    for(int i = 30;i >= 0;i--){
        int u = x >> i & 1;
        if(!son[p][u]) son[p][u] = ++idx;
        p = son[p][u];
    }
}

int query(int x){
    int p = 0;
    int ret = 0;

    for(int i = 30;i >= 0;i--){
        int u = x >> i & 1;

        if(son[p][!u]){
            p = son[p][!u];
            ret = ret * 2 + !u;
        }else{
            p = son[p][u];
            ret = ret * 2 + u;
        }
    }

    ret = ret ^ x;
    return ret;
}

int main(){
    int n;
    cin >> n;
    int maxXorNum = 0;
    int x;
    for(int i = 0;i < n;i++){
        cin >> x;
        insert(x);
        maxXorNum = max(maxXorNum,query(x));
    }
    cout << maxXorNum;
    
    return 0;
}
```

### 并查集

```C++
#include <iostream>
#include <cstdio>
#include <cmath>
#include <vector>
#include <string>
#include <algorithm>
#include <stack>
#include <unordered_map>

using namespace std;

const int N = 1e5 + 10;

int p[N];

int find(int x){
    if(p[x] != x) p[x] = find(p[x]);
    return p[x];
}

int main(){
    int n,m;
    cin >> n >> m;

    for(int i = 1;i <= n;i++) p[i] = i;

    while(m--){
        char op[2];
        int a,b;

        scanf("%s%d%d",op,&a,&b);

        if(*op == 'M'){
            p[find(a)] = find(b);
        }else{
            if(find(a) == find(b)){
                cout << "Yes" << "\n";
            }else{
                cout << "No" << "\n";
            }
        }
    }

    return 0;
}
```

### 连通块中点的数量

```C++
#include <iostream>
#include <cstdio>
#include <cmath>
#include <vector>
#include <string>
#include <algorithm>
#include <stack>
#include <unordered_map>

using namespace std;

const int N = 1e5 + 10;

int p[N];
int cnt[N];

int find(int x){
    if(p[x] != x) p[x] = find(p[x]);
    return p[x];
}

int main(){
    int n,m;
    cin >> n >> m;

    for(int i = 1;i <= n;i++){
        p[i] = i;
        cnt[i] = 1;
    } 

    while(m--){
        char op[3];
        scanf("%s",op);

        if(*op == 'C'){
            int a,b;
            cin >> a >> b;
            if(find(a) == find(b)) continue;
            cnt[find(b)] += cnt[find(a)];
            p[find(a)] = find(b);
        }else if(op[1] == '1'){
            int a,b;
            cin >> a >> b;
            if(find(a) == find(b)){
                cout << "Yes\n";
            }else{
                cout << "No\n";
            }
        }else{
            int a;
            cin >> a;
            cout << cnt[find(a)] << "\n";
        }
    }


    return 0;
}
```

