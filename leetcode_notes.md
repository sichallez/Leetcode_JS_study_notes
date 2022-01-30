#leetcode #算法 #数据结构 #思路

### Array

#### 1. Two Sum

###### 题⽬
给一个数组和目标数字，找出数组中两个数之和等于目标数字。

**My solution**: 遍历整个array，求target与当前元素之差，然后搜索此差是否存在于array中。如Y，返回；如N，继续遍历直至结束。

**Solution 1**: 两个for循环遍历, **O(n²) time, O(1) space**.

**Solution 2**: 建立一个空hash table{}, 遍历数组，求diff=target - currentNum ，如果diff不存在于hash table中，放入currentNum到hash table中{currentNum: true}. 依次类推，如果diff存在于hash table中，找到了，返回[currentNum, diff].
**O(n) time, O(n) space**.

**Solution 3**: 首先对数组排序，然后定义两个指针，左右，各指向min, max. 首先min, max相加，对比它俩之和与target的大小，如果大，右指针向左移动一位，然后进行相同的运算。如果小，左指针向右移动一位，如此反复直到左右指针相遇。**O(nlogn) time, O(1) space**.

**最优解**：
target - num1 = num2, assign an object {num2: index1} to find num2 if this key exists, then [index1, index2] returns.


#### Validate Subsequence

**问题**：给两个数组，判断第二个数组是不是第一个数组的subset.
重点：Subsequence

**My solution**: 遍历第二个数组，用 indexOf 确认每一个元素是否存在于第一个数组中，如果不存在，返回false. 如果存在，记下index，查找下一个元素，但查找时从 index + 1 开始。接下来继续前述操作。
**O(n) time, O(1) space**.

**Solution 1**: while loop两数组长度均未到，如果第二个数组元素等于第一个数组元素，第二个数组 index + 1. 反之第一个数组 index + 1. 最后返回，看第二个数组index是否等于自身数组长度。

**Solution 2**: 基本思路一致，用 for loop 来实现。遍历第一个数组，如果第二个数组的元素等于第一个数组，第二个数组 index + 1. 最后返回，看第二个数组index是否等于自身数组长度。


#### 977. Squares of a Sorted Array

###### 题⽬
Given an array of integers A sorted in non-decreasing order, return an array of the squares of
each number, also in sorted non-decreasing order.

**Example 1**:
Input: [-4,-1,0,3,10]
Output: [0,1,9,16,100]
**Example 2**:
Input: [-7,-3,2,3,11]
Output: [4,9,9,49,121]
Note:
1. 1 <= A.length <= 10000
2. -10000 <= A[i] <= 10000
3. A is sorted in non-decreasing order.

###### 题⽬⼤意
给⼀个已经有序的数组，返回的数组也必须是有序的，且数组中的每个元素是由原数组中每个数字的平⽅得到的。

**My solution** or **Solution 1**: using built-in map, and sort functions. **O(nlogn) time, O(n) space**.

###### 解题思路 **Solution 2**

这⼀题由于原数组是有序的，所以要尽量利⽤这⼀特点来减少时间复杂度。
最终返回的数组，最后⼀位，是最⼤值，这个值应该是由原数组最⼤值，或者最⼩值得来的，所以可以从数组的最后⼀位开始排列最终数组。⽤ 2 个指针分别指向原数组的⾸尾，分别计算平⽅值，然后⽐较两者⼤⼩，⼤的放在最终数组的后⾯。然后⼤的⼀个指针移动。直⾄两个指针相撞，最终数组就排列完成了。
**O(n) time, O(n) space**.


#### Tournament Winner
###### 题⽬
Given an array of pairs representing the teams that have competed against each other and an array containing the results of each competition, write a function that returns the winner of the tournament. The input arrays are named competitions and results, respectively. The competitions array has elements in the form of [homeTeam, awayTeam], where each team is a string of at most 30 characters representing the name of the team. The results array contains information about the winner of each corresponding competition in the competitions array. Specifically, results[i] denotes the winner of competitions[i], where a 1 in the results array means that the home team in the corresponding competition won and a 0 means that the away team won.

It's guaranteed that exactly one team will win the tournament and that each team will compete against all other teams exactly once. It's also guaranteed that the tournament will always have at least two teams.

**Example 1**:
Input:
competitions = [
  ["HTML", "C#"],
  ["C#", "Python"],
  ["Python", "HTML"],
]
results = [0, 0, 1]

Output
"Python"
// C# beats HTML, Python Beats C#, and Python Beats HTML.
// HTML - 0 points 
// C# -  3 points
// Python -  6 points

**My solution**: 建立一个空table，遍历results数组，如果result=0，添加awayTeam到table中并开始计数（在此之前先需要确认table中有无此key/property）。如果result=1，添加homeTeam，依次类推。最后用reduce查找table里最大值对应的key。

```js
function tournamentWinner(competitions, results) {
  // Write your code here.
	let table = {};
	for (let i = 0; i < results.length; i++) {
		if (results[i] === 0) {
			if (table.hasOwnProperty(competitions[i][1])) table[competitions[i][1]] += 1;
			else table[competitions[i][1]] = 1;	
		} else {
			if (table.hasOwnProperty(competitions[i][0])) table[competitions[i][0]] += 1;
			else table[competitions[i][0]] = 1;
		}
	}
	let winner = Object.keys(table).reduce((a, b) => table[a] > table[b] ? a : b);
  return winner;
}
```

###### 解题思路 **Solution**
遍历如上，但往table里放(key, value)的时候，同时记录每次放入的最大值及其所对应的key。
**O(n) time | O(k) space** - where n is the number of competitions and k is the number of teams.

```js
const HOME_TEAM_WON = 1;

function tournamentWinner(competitions, results) {

  let currentBestTeam = '';
  const scores = {[currentBestTeam]: 0};

  for (let idx = 0; idx < competitions.length; idx++) {

    const result = results[idx];
    const [homeTeam, awayTeam] = competitions[idx];

    const winningTeam = result === HOME_TEAM_WON ? homeTeam : awayTeam;

    updateScores(winningTeam, 3, scores);

    if (scores[winningTeam] > scores[currentBestTeam]) {
      currentBestTeam = winningTeam;
    }
  }
  return currentBestTeam;
}

function updateScores(team, points, scores) {
  if (!(team in scores)) scores[team] = 0;
  scores[team] += points;
}
```


#### Non-Constructible Change (322. Coin Change)
###### 题⽬
Given an array of positive integers representing the values of coins in your possession, write a function that returns the minimum amount of change (the minimum sum of money) that you **cannot** create. The given coins can have any positive integer value and aren't necessarily unique (i.e., you can have multiple coins of the same value).

For example, if you're given coins = [1, 2, 5], the minimum amount of change that you can't create is 4. If you're given no coins, the minimum amount of change that you can't create is 1.

**Example 1**:
Input: coins = [5, 7, 1, 1, 2, 3, 22]
Output: 20

###### 解题思路 **Solution**
先对数组进行升序排序，然后遍历，对比。
Start by sorting the input array. Since you're trying to find the **minimum** amount of change that you can't create, it makes sense to consider the smallest coins first.

To understand the trick to this problem, consider the following example: coins = [1, 2, 4]. With this set of coins, we can create 1, 2, 3, 4, 5, 6, 7 cents worth of change. Now, if we were to add a coin of value 9 to this set, we **would not** be able to create 8 cents. However, if we were to add a coin of value 7, we **would** be able to create 8 cents, and we would also be able to create all values of change from 1 to 15. Why is this the case?

Create a variable to store the amount of change that you can currently create up to. Sort all of your coins, and loop through them in ascending order. At every iteration, compare the current coin to the amount of change that you can currently create up to. Here are the two scenarios that you'll encounter:

-   The coin value is **greater** than the amount of change that you can currently create plus 1.
-   The coin value is **smaller than or equal to** the amount of change that you can currently create plus 1.

In the first scenario, you simply return the current amount of change that you can create plus 1, because you can't create that amount of change. In the second scenario, you add the value of the coin to the amount of change that you can currently create up to, and you continue iterating through the coins.

The reason for this is that, if you're in the second scenario, you can create all of the values of change that you can currently create plus the value of the coin that you just considered. If you're given coins [1, 2], then you can make 1, 2, 3 cents. So if you add a coin of value 4, then you can make 4 + 1 cents, 4 + 2 cents, and 4 + 3 cents. Thus, you can make up to 7 cents.

**O(nlogn) time | O(1) space** - where n is the number of coins
```python
def nonConstructibleChange(coins):
    coins.sort()
	
    currentChangeCreated = 0
    for coin in coins:
        if coin > currentChangeCreated + 1:
            return currentChangeCreated + 1
		
        currentChangeCreated += coin
		
    return currentChangeCreated + 1
```


#### Three Number Sum (15. 3Sum)
###### 题⽬
Write a function that takes in a non-empty array of distinct integers and an integer representing a target sum. The function should find all triplets in the array that sum up to the target sum and return a two-dimensional array of all these triplets. The numbers in each triplet should be ordered in ascending order, and the triplets themselves should be ordered in ascending order with respect to the numbers they hold.

If no three numbers sum up to the target sum, the function should return an empty array.

**Sample Input**
array = [12, 3, 1, 2, -6, 5, -8, 6]
targetSum = 0

**Sample Output**
[[-8, 2, 6], [-8, 3, 5], [-6, 1, 5]]

###### 解题思路 **Solution**
先升序排序，遍历，当前元素，两个指针left, right分别遍历当前元素后第一个到最后一个数字。

**O(n^2) time | O(n) space**
```python
def threeNumberSum(array, targetSum):
    array.sort()
    triplets = []
    for i in range(len(array) - 2):
        left = i + 1
        right = len(array) - 1
        while left < right:
            currentSum = array[i] + array[left] + array[right]
            if currentSum == targetSum:
                triplets.append([array[i], array[left], array[right]])
                left += 1
                right -= 1
            elif currentSum < targetSum:
                left += 1
            elif currentSum > targetSum:
                right -= 1
    return triplets
```

#### Smallest Difference
###### 题⽬
Write a function that takes in two non-empty arrays of integers, finds the pair of numbers (one from each array) whose absolute difference is closest to zero, and returns an array containing these two numbers, with the number from the first array in the first position.

Note that the absolute difference of two integers is the distance between them on the real number line. For example, the absolute difference of -5 and 5 is 10, and the absolute difference of -5 and -4 is 1.

You can assume that there will only be one pair of numbers with the smallest difference.

**Sample Input**
arrayOne = [-1, 5, 10, 20, 28, 3]
arrayTwo = [26, 134, 135, 15, 17]

**Sample Output**
[28, 26]

###### 解题思路 **Solution**
两数组均先升序排序，建立两个指针，分别指向数组X和数组Y的第一个元素。比较x0, y0大小，并记录两者的差值。如果x0 < y0，数组X的指针往右移；如果x0 > y0，反之，数组Y的指针往右移；如果x0 = y0, return.

**O(nlog(n) + mlog(m)) time | O(1) space**
```python
def smallestDifference(arrayOne, arrayTwo):
    arrayOne.sort()
    arrayTwo.sort()
	
    idxOne = 0
    idxTwo = 0
    smallest = float("inf")
    current = float("inf")
    smallestPair = []
	
    while idxOne < len(arrayOne) and idxTwo < len(arrayTwo):
        firstNum = arrayOne[idxOne]
        secondNum = arrayTwo[idxTwo]
        if firstNum < secondNum:
            current = secondNum - firstNum
            idxOne += 1
        elif secondNum < firstNum:
            current = firstNum - secondNum
            idxTwo += 1
        else:
            return [firstNum, secondNum]
		
        if smallest > current:
            smallest = current
            smallestPair = [firstNum, secondNum]
			
    return smallestPair
```

#### Move Element To End
###### 题⽬
You're given an array of integers and an integer. Write a function that moves all instances of that integer in the array to the end of the array and returns the array.

The function should perform this in place (i.e., it should mutate the input array) and doesn't need to maintain the order of the other integers.

**Sample Input**
array = [2, 1, 2, 2, 2, 3, 4, 2]
toMove = 2
**Sample Output**
[1, 3, 4, 2, 2, 2, 2, 2] // the numbers 1, 3, and 4 could be ordered differently

###### My Solution
建立两个指针分别指向数组的首尾两端，记录原始数组长度。while(left < right), 如果array[left] === toMove, 删除该元素(array.splice), right -= 1 (因为数组总长度少了一个，所以右指针得减1)。else, 左指针往右移一位 left += 1.
遍历结束后会得到新数组(删除了所有的toMove元素)。比较新数组与原始数组长度差别，用array.fill将缺少的数量的toMove元素push到新数组尾部，返回新数组，完成。

```js
function moveElementToEnd(array, toMove) {

  let right = array.length - 1;
  let initLength = array.length;
  let left = 0;

  while (left < right) {
    if (array[left] === toMove) {
      array.splice(left, 1);
      right -= 1;
    } else left += 1;
  }

  let diff = initLength - array.length;
  let addArray = Array(diff).fill(toMove);
  array.push(...addArray);

  return array; 
}
```

###### 解题思路 **Solution**
set two pointers at the start and end of the array, respectively. Move the right pointer inwards so long as it points to the integer to move, and move the left pointer inwards so long as it doesn't point to the integer to move. When both pointers aren't moving, swap their values in place. Repeat this process until the pointers pass each other.

**O(n) time | O(1) space - where n is the length of the array**
```python
def moveElementToEnd(array, toMove):
    i = 0
    j = len(array) - 1

    while i < j:
        while i < j and array[j] == toMove:
            j -= 1
        if array[i] == toMove:
            array[i], array[j] = array[j], array[i]
        i += 1

    return array
```
