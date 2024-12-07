# Algorithm

## 2024.11.6

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

## 2024.11.7

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

## 2024.11.8

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

## 2024.11.12

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

## 2024.11.14

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

## 2024.11.15

### [28. 找出字符串中第一个匹配项的下标(KMP)](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/)



## 2024.11.18

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

## 2024.11.19

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

## 2024.11.20

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

## 2024.11.21

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

## 2024.11.22

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

## 2024.11.23

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

## 2024.11.25

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

## 2024.11.29

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

## 2024.12.5

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

## 2024.12.6

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

## 2024.12.7

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

