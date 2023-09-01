---
title: "Programmers algorithm - stack/queue"
date: 2020-05-16 08:26:28 -0400
categories: algorithm
---


{% highlight cpp %}
#include <string>
#include <vector>
#include <stack>

using namespace std;

vector<int> solution(vector<int> prices) {
    vector<int> answer(prices.size());
    stack<int> s;
    
    for (int i = 0; i < prices.size(); i++) {
        // if stack is not empty and price is not going down
        while (!s.empty() && prices[s.top()] > prices[i]) {
            answer[s.top()] = i - s.top(); // current time - top of stack
            s.pop();
        }
        s.push(i);
    }
    
    // while문으로 stack에 남아있는 값 처리 
    while (!s.empty()) {
        answer[s.top()] = prices.size() - 1 - s.top();
        // prices.size() - 1 = end time 
        s.pop();
    }
    
    return answer;
}
{% endhighlight %}
