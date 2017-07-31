---
layout: post
title: 拜占庭将军问题
categories:
    - 分布式计算
tags:
    - 分布式理论
    - 一致性
    - Paper
    - Work in Progress
---

工作之余，花了一周的几乎每个晚上把Lamport神的那篇有关拜占庭将军问题的文章看完了。在这里做下笔记。

个人觉得最难懂的就是OM算法，我将它理解成一个动态规划：

    // @c is the commander id;
    // @l is one of the lieutenant id, which belongs to the current lieutenant set;
    // @m is the parameter m mentioned in the paper;
    // OM(c, l, m) means the final value lieutenant @l obeys from commander @c.
    
    - if m = 0:
    OM(c, l, m) = msg(c->l), where l in set{lieutenant};
    
    - if m > 0:
    OM(c, l, m) = majority(
      msg(c->l),
      the list of OM(l', l, m-1) where l' in (set{lieutenant} - {l})
    ).  // majority

实际上，准确来说上面的状态表示是不完全的，OM的第一个参数应该是一个commander的id列表，表示从算法开始的第一个commander通过发消息沿途产生的所有commander。这样说起来还是很抽象，具体还是看下文代码吧。

为了让自己有更深的理解，我用C++实现了一个模拟器，输入general的个数，指定commander是否loyal，模拟算法运行，打印出每条消息以及消息路径，最后得出每个忠诚的lieutenant遵守的命令。目测+用小数据测试没有发现错误，也贴在这里供以后回顾使用。

{% highlight c++ linenos %}
#include <cstdlib>
#include <cstdio>
#include <iostream>
#include <vector>
#include <set>
#include <map>
#include <string>
 
using std::cin;
using std::cout;
using std::endl;
using std::map;
using std::set;
using std::vector;
using std::string;
 
int num_generals;
bool commander_is_loyal;
bool is_loyal[64];
 
typedef int ValueType;
const ValueType kDefaultValue = -999999999;
 
// Classic algorithm to find the majority: maintain a value and
// a counter, when see a same value, ++counter; otherwise, if
// counter is 0, set the value to the one we see and let counter=1,
// or if counter>0, --counter.
struct MajorityCount {
  ValueType majority;
  int count;
  int seen;
 
  MajorityCount() : majority(kDefaultValue), count(0), seen(0) {}
  MajorityCount(const ValueType& v, const int c, const int s)
      : majority(v), count(c), seen(s) {}
 
  MajorityCount& Merge(const MajorityCount &other) {
    ++seen;
    if (count == 0) {
      majority = other.majority;
      count = 1;
    } else {
      if (majority == other.majority) {
        ++count;
      } else {
        --count;
      }
    }
    return *this;
  }
 
  void Print() const {
    cout << ": val=" << majority
         << ", cnt=" << count
         << ", seen=" << seen << endl;
  }
};
typedef map<string, MajorityCount> MapType;
MapType val_storage;
 
void RandomShuffle(bool *begin, int n) {
  for (int i = n; i > 1; --i) {
    int idx = rand() % i;
    std::swap(begin[idx], begin[i - 1]);
  }
  for (int i = 0; i < n; cout << begin[i++] << " ");
  cout << endl;
}
 
string Itoa(int i) {
  char res[32];
  std::sprintf(res, "%d", i);
  return res;
}
 
string Join(
    const vector<int>::iterator& begin,
    const vector<int>::iterator& end,
    const string& delim) {
  string res;
  for (vector<int>::iterator it = begin; it != end; ++it) {
    if (!res.empty()) res.append(delim);
    res.append(Itoa(*it));
  }
  return res;
}
 
ValueType RandomValue() {
  // return std::rand() % 97;
  return 250;
}
 
// Updates the value of OM(c, l, m) using OM(l', l, m-1).
void MergeUp(
    const vector<int> &passed_ids,
    const set<int> &remaining_ids) {
  vector<int> key_vec = passed_ids;
  int back = key_vec.back();
  string src_key = Join(key_vec.begin(), key_vec.end(), "-");
  int required_num_seen = remaining_ids.size() + 2;
 
  while (key_vec.size() > 2) {
    key_vec.pop_back();
    key_vec.back() = back;
    string dst_key = Join(key_vec.begin(), key_vec.end(), "-");
 
    MajorityCount &mc = val_storage[dst_key].Merge(val_storage[src_key]);
    if (!is_loyal[back]) mc.majority = RandomValue();
    const int seen = mc.seen;
    cout << "    " << src_key << " => " << dst_key;
    mc.Print();
 
    val_storage.erase(src_key);
 
    if (seen == required_num_seen) {
      src_key = dst_key;
      ++required_num_seen;
      continue;
    }
    break;
  }
}
 
// Sequential algorithm: for simplicity, assume that each lieutenant
// will send messages to others when needed.
void OM(const int m,
        const int commander_id,
        const ValueType &received_val,
        vector<int> *passed_ids,  // the last one is commander_id.
        set<int> *remaining_ids) {  // {1, 2, ..., n-1} - passed_ids
  const string prefix_path =
      Join(passed_ids->begin(), passed_ids->end(), "-");
  if (m >= 0) {
    set<int> remaining_ids_copy(*remaining_ids);
    for (set<int>::iterator it = remaining_ids_copy.begin();
        it != remaining_ids_copy.end(); ++it) {
      const int i = *it;
      ValueType val_to_send =
        (is_loyal[commander_id] ? received_val : RandomValue());
 
      // OM(m), step (1)
      string path = prefix_path + "-" + Itoa(i);
      const MajorityCount &mc =
        val_storage[path].Merge(
            MajorityCount(is_loyal[i] ? val_to_send : RandomValue(), 1, 1));
      cout << path;
      mc.Print();
 
      // OM(m), step (2)
      passed_ids->push_back(i);
      remaining_ids->erase(i);
      if (mc.seen == remaining_ids->size() + 1) {
        MergeUp(*passed_ids, *remaining_ids);
      }
      OM(m - 1, i, val_to_send, passed_ids, remaining_ids);
      remaining_ids->insert(i);
      passed_ids->pop_back();
    }
  } else {
    MergeUp(*passed_ids, *remaining_ids);
  }
}
 
int main() {
  string tmp;
  while (cout << "+++++++++++++++++++++++++++++++++" << endl,
         cout << "num_generals: ",
         cin >> num_generals) {
    int m = (num_generals - 1) / 3;
    cout << "m: " << m << endl;
 
    cout << "commander_is_loyal? ";
    if (m > 0) {
      cin >> tmp;
      commander_is_loyal = (tmp[0] == 'y');
    } else {
      commander_is_loyal = true;
      cout << "y" << endl;
    }
 
    val_storage.clear();
    memset(is_loyal, 1, sizeof(is_loyal));
    if (commander_is_loyal) {
      for (int i = 1; i <= m; is_loyal[i++] = false);
    } else {
      for (int i = 0; i < m; is_loyal[i++] = false);
    }
    RandomShuffle(is_loyal + 1, num_generals - 1);
    cout << "---------------------------------" << endl;
 
    vector<int> passed_ids(1, 0);
    set<int> remaining_ids;
    for (int i = 1; i < num_generals; ++i) {
      remaining_ids.insert(i);
    }
 
    // commander_id=0, received_val=618
    OM(m, 0, 618, &passed_ids, &remaining_ids);
 
    cout << "---------------------------------" << endl;
    for (MapType::iterator it = val_storage.begin();
        it != val_storage.end(); ++it) {
      cout << it->first;
      it->second.Print();
    }
  }
  return 0;
}
{% endhighlight %}
