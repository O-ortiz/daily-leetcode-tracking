Given an array of meeting time interval objects consisting of start and end times `[[start_1,end_1],[start_2,end_2],...] (start_i < end_i)`, determine if a person could add all meetings to their schedule without any conflicts.

**Example 1:**

```java
Input: intervals = [(0,30),(5,10),(15,20)]

Output: false
```

Explanation:

- `(0,30)` and `(5,10)` will conflict
- `(0,30)` and `(15,20)` will conflict

**Example 2:**

```java
Input: intervals = [(5,8),(9,15)]

Output: true
```

**Note:**

- (0,8),(8,10) is not considered a conflict at 8

**Constraints:**

- `0 <= intervals.length <= 500`
- `0 <= intervals[i].start < intervals[i].end <= 1,000,000`


---

So we need to determine if any of the time blocks overlap with each other. We can do this by first sorting the vector by the first value, and then going down the vector and returning false if the end time of a block is greater than the start time of the next one. 

```
/**
 * Definition of Interval:
 * class Interval {
 * public:
 *     int start, end;
 *     Interval(int start, int end) {
 *         this->start = start;
 *         this->end = end;
 *     }
 * }
 */

class Solution {
public:
    bool canAttendMeetings(vector<Interval>& intervals) {
        if (intervals.empty()){
            return true;
        }
        std::sort(intervals.begin(), intervals.end(), [](const Interval& a, const Interval& b) {
        return a.start < b.start;
        });

        for (int i = 0; i < intervals.size() -1; i++) {
            if (intervals[i].end > intervals[i+1].start){
                return false;
            }
        }
        return true;
    }
};

```

I had to look up how to define a custom lambda for sort that would let us sort by a member variable of the item the container (vector) has. This solution passes all test cases and is O(n + n log n) due to sorting. So O(nlogn). I cannot think of a solution that would be O(n) or better if the time blocks are given to us unsorted. 

LeetCode is greedy so we used NeetCode for this specific problem. 