# [150. Evaluate Reverse Polish Notation (Medium)](https://leetcode.com/problems/evaluate-reverse-polish-notation/)

<p>Evaluate the value of an arithmetic expression in <a href="http://en.wikipedia.org/wiki/Reverse_Polish_notation" target="_blank">Reverse Polish Notation</a>.</p>

<p>Valid operators are <code>+</code>, <code>-</code>, <code>*</code>, and <code>/</code>. Each operand may be an integer or another expression.</p>

<p><strong>Note</strong> that division between two integers should truncate toward zero.</p>

<p>It is guaranteed that the given RPN expression is always valid. That means the expression would always evaluate to a result, and there will not be any division by zero operation.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> tokens = ["2","1","+","3","*"]
<strong>Output:</strong> 9
<strong>Explanation:</strong> ((2 + 1) * 3) = 9
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> tokens = ["4","13","5","/","+"]
<strong>Output:</strong> 6
<strong>Explanation:</strong> (4 + (13 / 5)) = 6
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]
<strong>Output:</strong> 22
<strong>Explanation:</strong> ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= tokens.length &lt;= 10<sup>4</sup></code></li>
	<li><code>tokens[i]</code> is either an operator: <code>"+"</code>, <code>"-"</code>, <code>"*"</code>, or <code>"/"</code>, or an integer in the range <code>[-200, 200]</code>.</li>
</ul>


**Companies**:  
[Amazon](https://leetcode.com/company/amazon), [Google](https://leetcode.com/company/google), [LinkedIn](https://leetcode.com/company/linkedin), [Microsoft](https://leetcode.com/company/microsoft)

**Related Topics**:  
[Stack](https://leetcode.com/tag/stack/)

**Similar Questions**:
* [Basic Calculator (Hard)](https://leetcode.com/problems/basic-calculator/)
* [Expression Add Operators (Hard)](https://leetcode.com/problems/expression-add-operators/)

## Solution 1.

```cpp
class Solution 
{
public:
    int evalRPN(vector<string>& tokens) 
    {
        // Create the stack 
        stack<int> s;
        
        // Iterate on the token array
        for(auto x: tokens)
        {
            // If '+' encountered then perform the following
            if(x == "+")
            {
                // Extract the first and pop it
                int first= s.top(); s.pop();
                
                // Extract the second and pop it
                int second= s.top(); s.pop();
                
                s.push(first+second);
            }
            
            // If '*' encountered then perform the following
            else if(x == "*")
            {
                // Extract the first and pop it
                long long first= s.top(); s.pop();
                
                // Extract the second and pop it
                long long second= s.top(); s.pop();
                
                s.push(first*second);
            }
            
            // If '+' encountered then perform the following
            else if(x == "-")
            {
                // Extract the first and pop it
                int first= s.top(); s.pop();
                
                // Extract the second and pop it
                int second= s.top(); s.pop();
                
                s.push(second-first);
            }
            
            // If '+' encountered then perform the following
            else if(x == "/")
            {
                // Extract the first and pop it
                int first= s.top(); s.pop();
                
                // Extract the second and pop it
                int second= s.top(); s.pop();
                
                s.push(second/first);
            }
            
            // Push the elements into the stack
            else s.push(stoi(x));
        }
        
        // Finally return the result
        return s.top();
    }
};
```

```cpp
class Solution 
{
public:
    int evalRPN(vector<string>& tokens) 
    {
        unordered_map<string, function<int (int, int) > > map = 
	{
            { "+" , [] (int a, int b) { return a + b; } },
            { "-" , [] (int a, int b) { return a - b; } },
            { "*" , [] (int a, int b) { return a * b; } },
            { "/" , [] (int a, int b) { return a / b; } }
        };
        
	std::stack<int> stack;
        
	for (string& s : tokens) 
	{
            if (!map.count(s)) 
	    {
                stack.push(stoi(s));
            } 
	    
	    else 
	    {
                int op1 = stack.top();
                stack.pop();
                int op2 = stack.top();
                stack.pop();
                stack.push(map[s](op2, op1));
            }
        }
        return stack.top();
    }
};
```

```cpp
// OJ: https://leetcode.com/problems/evaluate-reverse-polish-notation/
// Time: O(N)
// Space: O(N)

class Solution 
{
public:
    int evalRPN(vector<string>& A) 
    {
        stack<int> s;
        for (auto &t : A) 
	{
            if (isdigit(t.back())) 
	    {
                s.push(stoi(t));
            }
	    
	    else 
	    {
                int n = s.top();
                s.pop();
                switch (t[0]) 
		{
                    case '+': s.top() += n; break;
                    case '-': s.top() -= n; break;
                    case '*': s.top() *= n; break;
                    case '/': s.top() /= n; break;
                }
            }
        }
	
        return s.top();
    }
};
```
