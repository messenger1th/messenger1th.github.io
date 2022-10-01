# Binary Tree Traversal

## Recursion

```cpp
void preOrder(vector<int>& res,TreeNode* root){
    if(!root) {
        return;
    }
    
    res.emplace_back(root->val);
    preOrder(res, root->left);
    preOrder(res, root->right);
}

void inOrder(vector<int>& res,TreeNode* root){
    if(!root) {
        return;
    }
    
    inOrder(res, root->left);
    res.emplace_back(root->val);
    inOrder(res, root->right);
}

void postOrder(vector<int>& res,TreeNode* root){
    if(!root) {
        return;
    }
    
    postOrder(res, root->left);
    postOrder(res, root->right);
    res.emplace_back(root->val);
}
```

## Iteration: Recommend

```cpp
vector<int> preorderTraversal(TreeNode* root) {
    vector<int> res;
    stack<TreeNode*> stk;
    TreeNode* node = root;
    while (node || !stk.empty()) {
        while (node) {
            res.emplace_back(node->val); //收集答案和DFS顺序一致
            stk.emplace(node);
            node = node->left;
        }
        node = stk.top(); stk.pop();	//回到子树根节点
        node = node->right;	//子树右子节点
    }
    return res;
}

vector<int> inorderTraversal(TreeNode* root) {
    vector<int> res;
    stack<TreeNode*> stk;
    TreeNode* node = root;
    while (node || !stk.empty()) {
        while (node) {
            stk.emplace(node);
            node = node->left;
        }
        node = stk.top(); stk.pop(); //回到子树根节点
        res.emplace_back(node->val); //收集根节点
        node = node->right; //子树右子节点
    }
    return res;
}

vector<int> postorderTraversal(TreeNode* root) {
    vector<int> res;
    stack<TreeNode*> stk;
    TreeNode* node = root;
    TreeNode* lastFinished = nullptr;
    while (node || !stk.empty()) {
        while (node) {
            stk.emplace(node);
            node = node->left;
        }
        node = stk.top(); stk.pop(); //回到子树根节点, 因为第一次回到时没遍历右子树，故需要一个变量判断是第一次还是第二次
        if (node->right == nullptr || node->right == lastFinished) { //第二次或者没有右节点。
            lastFinished = node; //将该子树根节点设置为已完成。
            res.emplace_back(node->val);
            node = nullptr; //也算是一种标记（即该树已经遍历完成）， 向上移动
        } else { //第一次到子树根节点
            stk.emplace(node); //入栈
            node = node->right; //到右子树
        }
    } 
    return res;
}
```

## Iteration2: From Carl

```cpp
vector<int> preorderTraversal(TreeNode* root) {
    vector<int> res;
    stack<TreeNode*> stk;
    if (root != NULL) stk.push(root);
    while (!stk.empty()) {
        TreeNode* node = stk.top();stk.pop();
        if (node != NULL) {
            if (node->right) stk.push(node->right);  // 右
            if (node->left) stk.push(node->left);    // 左
            
            stk.push(node);                          // 中
            stk.push(NULL); 	//标志位， 告诉根节点已经遍历过，需要收集答案
        } else {
            node = stk.top(); stk.pop();
            res.push_back(node->val);
        }
    }
    return res;
}

vector<int> inorderTraversal(TreeNode* root) {
    vector<int> res;
    stack<TreeNode*> stk;
    if (root != NULL) stk.push(root);
    while (!stk.empty()) {
        TreeNode* node = stk.top();stk.pop();
        if (node != NULL) {
            if (node->right) stk.push(node->right);  // 右

            stk.push(node);                          // 中
            stk.push(NULL); //标志位， 告诉根节点已经遍历过，需要收集答案

            if (node->left) stk.push(node->left);    // 左
        } else {
            node = stk.top(); stk.pop();
            res.push_back(node->val);
        }
    }
    return res;
}

vector<int> postorderTraversal(TreeNode* root) {
    vector<int> res;
    stack<TreeNode*> stk;
    if (root != NULL) stk.push(root);
    while (!stk.empty()) {
        TreeNode* node = stk.top();stk.pop();
        if (node != NULL) {
            stk.push(node);                          // 中
            stk.push(NULL);	//标志位， 告诉根节点已经遍历过，需要收集答案

            if (node->right) stk.push(node->right);  // 右                
            if (node->left) stk.push(node->left);    // 左
        } else {
            node = stk.top(); stk.pop();
            res.push_back(node->val);
        }
    }
    return res;
}
```

## Morris

```cpp
vector<int> preorderTraversal(TreeNode* root) {
    vector<int> res;
    TreeNode* node = root;
    while (node) {
        if (node->left) {
            TreeNode* predecessor = node->left;
            while (predecessor->right != nullptr && predecessor->right != node) {
                predecessor = predecessor->right;
            } 
            if (predecessor->right == node) {
                predecessor->right = nullptr;
                node = node->right;
            } else {
                res.emplace_back(node->val); //向下的过程中需要收集， 也就是前序
                predecessor->right = node;
                node = node->left;
            }
        } else {
            res.emplace_back(node->val); //特殊处理， 没有左节点。
            node = node->right;
        }
    }
    return res;
}

vector<int> inorderTraversal(TreeNode* root) {
    vector<int> res;
    TreeNode* node = root;
    while (node) {
        if (node->left) {
            TreeNode* predecessor = node->left;
            while (predecessor->right != nullptr && predecessor->right != node) {
                predecessor = predecessor->right;
            } 
            if (predecessor->right == node) {
                res.emplace_back(node->val); //返回的过程中收集， 中序遍历
                predecessor->right = nullptr;
                node = node->right;
            } else {
                predecessor->right = node;
                node = node->left;
            }
        } else {
            res.emplace_back(node->val); //没有左节点，也算是从nullptr返回，特判收集。
            node = node->right;
        }
    }
    return res;
}


void addResult(vector<int>& res, TreeNode* root) {
    int cnt = 0;
    while (root) {
        res.emplace_back(root->val);
        ++cnt;
        root = root->right;
    }
    reverse(res.end() - cnt, res.end());
}

vector<int> postorderTraversal(TreeNode* root) {
    vector<int> res;
    TreeNode* node = root;
    while (node) {
        if (node->left) {
            TreeNode* predecessor = node->left;
            while (predecessor->right != nullptr && predecessor->right != node) {
                predecessor = predecessor->right;
            } 
            if (predecessor->right == node) { //第二次回到子树的根节点，代表左子树已经遍历完成
                predecessor->right = nullptr;
                addResult(res, node->left); //收集左子树的右节点信息和根节点信息。
                node = node->right;
            } else {
                predecessor->right = node;
                node = node->left;
            }
        } else {
            node = node->right;
        }
    }
    addResult(res, root); //遍历完成， 整树右节点也需要遍历。
    return res;
}
```

以上就是统一遍历法的写法了。



## 迭代法理解

**对于树的遍历，共有3种情况**

1. 第一次到根节点
2. 左子树遍历完到根节点
3. 右子树遍历完到根节点

对于三种遍历方式， 我们可以通过当前节点是否入栈来简化一些代码逻辑。 

如`preOrder`只需要入栈左右子树， `inOrder`需要入栈3种节点，`postOrder`入栈3种节点， 但有多种节点访问状态。我们来看具体分析。

### PreOrderTraversal

对于`preOrderTraversal`, 只用考虑一种情况

1. 第一次到根节点



因为， 对于`preOrderTraversal`， 收集答案的顺序和DFS遍历顺序是一致的。无需重复入栈。

不难写出如下代码, 每个节点都不需要重复入栈，取出便收集，有子树才入栈子树。

```cpp
vector<int> preorderTraversal(TreeNode* root) {
    vector<int> res;
    stack<TreeNode*> stk;
    if (root) {
        stk.emplace(root);
    }
    while (!stk.empty()) {
     	//没有对node进行状态判断， 因为只有一种情况。
        TreeNode* node = stk.top(); stk.pop();
        res.emplace_back(node->val);
        if (node->right) { 
            stk.emplace(node->right);
        }
        if (node->left) {
            stk.emplace(node->left);
        }
    }
    return res;
}
```

### InOrderTraversal


对于`inOrderTraversal`, 只用考虑两种状态

1. 第一次到根节点
2. 左子树遍历完到根节点



因为在收集完根节点答案后，无需再借用根节点访问子节点，因此不用再次入栈， 也就是右子树收集完后，栈就为空了。

因此， 相比于统一法，有如下一种更好理解的方式。

```cpp
vector<int> inorderTraversal(TreeNode* root) {
    vector<int> res;
    stack<TreeNode*> stk;
    TreeNode* node = root;
    while (node || !stk.empty()) {
        if (node == nullptr) { //状态2：说明左子树遍历完成。
            res.emplace_back(stk.top()->val); //收集在栈顶的根节点
            node = stk.top()->right; //右子树
            stk.pop(); //收集完就可以把根节点扔了
        } else { //状态1：第一次到子树根节点
            stk.emplace(node); //向左移动前先保存下， 因为需要二次访问。
            node =node->left; 
        }
    }
    return res;
}
```



### PostOrderTraversal

对于`posrOrderTraversal`，3种情况都需要考虑

1. 第一次到根节点
2. 左子树遍历完回到根节点
3. 右子树遍历完回到根节点



那它能否像`InOrderTraversal`一样，在`while (node || !stk.empty())`中用`if`直接判断状态呢？

答案是：可以。 但是，由于目前我们只知道`node`是否为`nullptr`， 这是不足以区分这3种状态的，因此，我们采用哈希集合`leftFinished`来区分左子树是否遍历完成。

因此， `posrOrderTraversal`也可以同理实现。

```cpp
vector<int> postorderTraversal(TreeNode* root) {
    unordered_set<TreeNode*> leftFinished;
    stack<TreeNode*> stk;
    TreeNode* node = root;
    vector<int> res;
    while (node || !stk.empty()) {
        if (node == nullptr && leftFinished.count(stk.top())) { //状态3: 左子树已经遍历，同时node为空， 也就是右子树也完成。
            res.emplace_back(stk.top()->val); stk.pop(); //
        } else if (node == nullptr) { //状态2：左子树完了，栈顶是根节点
            //左子树为空，也就是遍历完成， 将根节点（栈顶）加入集合
            leftFinished.insert(stk.top()); //不弹栈，因为需要二次遍历
            node = stk.top()->right;// 直接进入右子树。
        } else { //状态1
            stk.emplace(node);
            node = node->left;
        }
    }
    return res;
}
```



### 总结

实际上， 迭代法做不到递归那样优雅是因为需要我们自己显式地区分状态。 

在递归中，函数递归调用之前，会将当前信息压栈，以方便下一层递归结束，回到本层时，能够按照代码编写顺序执行。这个过程，就是状态区分的过程。

而迭代， 我们编码习惯导致， 我们仅仅知道`node`是否等于`nullptr`，`preOrder`和`inOrder`尚且可以用其区分状态。

但`postOrder`就不行了，**需要辅助空间（如哈希集合，如Carl的`stack`加入`nullptr`标志位）**，

或者，换一种编码思路,如`Leetcode官方解答`中使用的`while(node)`， 直接遍历到所有左节点，在这种情况下，一个**额外的`lastFinished`指针结合`node`**就可以区分这三种状态。
