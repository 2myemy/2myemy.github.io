---
title: "Programmers algorithm - hash"
date: 2020-05-16 08:26:28 -0400
categories: algorithm
---


{% highlight cpp %}
#include <string>
#include <vector>
#include <map>

using namespace std;

vector<int> solution(vector<string> genres, vector<int> plays) {
    vector<int> answer;
    map<string, int> music; // 각 장르 별로
    map<string, map<int, int>> musiclist; // 같은 장르 내에 재생된 곡
    
    for (int i = 0; i < genres.size(); i++) {
        music[genres[i]] += plays[i];
        musiclist[genres[i]][i] = plays[i]; // key = genres, 인덱스와 재생횟수 저장
    }
    
    while (!music.empty()) {
        int max = 0;
        string genre = genres[0];
        
        // 재생횟수가 많은 장르 찾기
        for (auto iter : music) {
            if (max < iter.second) {
                max = iter.second;
                genre = iter.first;
            }
        }
        
        // for (int i = 0; i < genres.size(); i++) {
        //     if (max < music[genres[i]]) {
        //         max = music[genres[i]];
        //         genre = genres[i];
        //     }
        // }
        
        // 2곡을 넣어야하므로 2번반복
        for (int k = 0; k < 2; k++) {
            int max_val = 0, max_idx = -1;
            
            for (auto iter : musiclist[genre]) {
                if (max_val < iter.second) {
                    max_val = iter.second;
                    max_idx = iter.first;
                }
            }
            
            // for (int j = 0; j < musiclist[genre].size(); j++) {
            //     if (max_val < musiclist[genre].second) {
            //         max_val = musiclist[genre].second;
            //         max_idx = musiclist[genre];
            //     }
            // }
            
            // 만약 노래가 0~1곡밖에없다면 반복문 탈출
            if (max_idx == -1)    break;
            
            // 리턴할 리스트에 노래번호 추가 장르에서 삭제
            answer.push_back(max_idx);
            musiclist[genre].erase(max_idx);
        }
        music.erase(genre);
    }
    
    return answer;
}
{% endhighlight %}
