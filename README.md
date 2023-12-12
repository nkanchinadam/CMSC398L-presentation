# Problem
[Codeforces 1904B - Collecting Game](https://codeforces.com/problemset/problem/1904/B)

# Solution
Since an element $a_{i}$ can only be removed from $a$ when $score > a_{i}$, it makes sense to first sort $a$. This ensures that for any element $a_{i}$ in the sorted array, if $a_{i}$ is able to removed, then all $a_{j}$ such that $j < i$ can also be removed from the array. Additionally, we will create a prefix array $p$ where $p_{i}$ is equal to the sum from $a_{0}$ to $a_{i}$, inclusive. Once we remove an initial element $a_{i}$, we are guaranteed to have a score $s$ of at least $p_{i}$. We can then use this new score to iteratively increase the score by finding the location in $a$ where $p_{i}$ can be inserted and then finding the corresponding $p_{i}$ and so on until no new elements are added to the score.

# Code
First, read in the data into two vectors, ```orig``` and ```a```. ```orig``` will be kept untouched to ensure that when we print the output, we are able to do so in the correct order while ```a``` will be sorted to calculate the answer.
```
int n;
cin >> n;

vector<long long> orig(n);
vector<long long> a(n);
for (int i = 0; i < n; i++) {
    int num;
    cin >> num;
    orig[i] = a[i] = num;
}
```

Next, sort ```a```
```
sort(a.begin(), a.end());
```

Create the prefix array ```p```
```
long long prefix[n];
prefix[0] = a[0];
for (int i = 1; i < n; i++) {
    prefix[i] = prefix[i - 1] + a[i];
}
```

Create a map to keep track of the answer for each $a_{i}$
```
map<int, int> numElems;
```

Iterate through ```p```. At each step, use binary search to figure out where in ```a``` $p_{i}$ maps to. If $p_{i}$ maps to index $i + 1$, then set the answers for all the elements of a until that point to i. Set the start of the next segment to i + 1 and continue this process until the end of ```p```.
```
int start = 0;
for (int i = 0; i < n; i++) {
    if (index == i + 1) {
        for (int j = start; j <= i; j++) {
            numElems[a[j]] = i;
            start = i + 1;
        }
    }
}
```

Finally, output the answer using ```orig``` and ```numElems```.
```
cout << numElems[orig[0]];
for (int i = 1; i < n; i++) {
    out << " " << numElems[orig[i]];
}
cout << '\n';
```

# Reflection
I thought this problem was pretty cool since the solution is graph-like in that each element in ```p``` points to a location in ```a``` that can be used to determine the answer for that element. Overall, it's a really elegant $O(nlog(n))$ solution to a problem that has a very inefficient brute force solution.
