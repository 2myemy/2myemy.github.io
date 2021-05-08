---
title: "Programmers - 스택/큐 기능개발"
date: 2020-05-08 08:26:28 -0400
categories: algorithm
---


{% highlight cpp %}
#include <string>
#include <vector>
#include <iostream> 
#include <queue>

using namespace std;

vector<int> solution(vector<int> progresses, vector<int> speeds) {
    vector<int> answer;
    queue<int> q;
    
    for (int i; i < progresses.size(); i++) {
        int result = 100 - progresses[i];
        int days = result / speeds[i];
        if (100 % speeds[i] != 0) days += 1;
        q.push(days);
    }
    
    int day = q.front();
    q.pop();
    int cnt = 1;
    
    while (!q.empty()) {
        if (day < q.front()) {
            answer.push_back(cnt);
            day = q.front();
            cnt = 0;
        } else {
            cnt++;
            q.pop();
        }
    }
    if(cnt !=0) answer.push_back(cnt);
    
    return answer;
}
{% endhighlight %}
