---
title: "Programmers algorithm - stack/queue"
date: 2020-05-16 08:26:28 -0400
categories: algorithm
---


{% highlight cpp %}
#include <string>
#include <vector>
#include <queue>

using namespace std;

int solution(int bridge_length, int weight, vector<int> truck_weights) {
    int answer = 0;
    int total_weight = 0;
    int truck_idx = 0;
    queue<int> q;
    
    for(int i = 0; i < bridge_length; i++) {
        q.push(0);
    }
    // for first input -> q = [0, 0]
    
    while (!q.empty()) {
        answer++; // increse time
        total_weight -= q.front();
        q.pop();
        
        // if there's more truck
        if (truck_idx < truck_weights.size()) {
            // if more truck can get on the bridge
            if (total_weight + truck_weights[truck_idx] <= weight) {
                total_weight += truck_weights[truck_idx];
                q.push(truck_weights[truck_idx]);
                truck_idx++;
            }
            // if not push 0 in qeueu
            else {
                q.push(0);
            }
        }
    }
    
    return answer;
}
{% endhighlight %}
