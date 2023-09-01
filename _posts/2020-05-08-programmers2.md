---
title: "Programmers algorithm - stack/queue"
date: 2020-05-08 08:26:28 -0400
categories: algorithm
---


{% highlight cpp %}
#include <string>
#include <vector>
#include <queue>

using namespace std;

int solution(vector<int> priorities, int location) {
    int answer = 0;
    int cnt = 0;
    queue<pair<int,int>> q;
    priority_queue<int> pq;
    
    for (int i = 0; i < priorities.size(); i++) {
        q.push(make_pair(i, priorities[i]));
        pq.push(priorities[i]);
    }
    
    while (!q.empty()) {
        int index = q.front().first;
        int priority = q.front().second;
        q.pop();
        
        if (pq.top() == priority) {
            pq.pop();
            cnt++;
            if (index == location) {
                answer = cnt;
                return answer;
            }
        }
        else {
            q.push(make_pair(index, priority));
        }
    }
}
{% endhighlight %}
