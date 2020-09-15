## Round E 2020 - Kick Start 2020

### High Buildings
### Problem
In an unspecified country, Google has an office campus consisting of N office buildings in a line, numbered from 1 to N from left to right. 
When represented in meters, the height of each building is an integer between 1 to N, inclusive.

Andre and Sule are two Google employees working in this campus. 
On their lunch break, they wanted to see the skyline of the campus they are working in. 
Therefore, Andre went to the leftmost point of the campus (to the left of building 1), looking towards the rightmost point of the campus (to the right of building N). Similarly, Sule went to the rightmost point of the campus, looking towards the leftmost point of the campus.
To Andre, a building x is visible if and only if there is no building to the left of building x that is strictly higher than building x. 
Similarly, to Sule, a building x is visible if and only if there is no building to the right of building x that is strictly higher than building x.
Andre learned that there are A buildings that are visible to him, while Sule learned that there are B buildings that are visible to him. 
After they regrouped and exchanged information, they also learned that there are C buildings that are visible to both of them.
They are wondering about the height of each building. 
They are giving you the value of N, A, B, and C for your information. As their friend, you would like to construct a possible height for each building such that the information learned on the previous paragraph is correct, or indicate that there is no possible height construction that matches the information learned (thus at least one of them must have been mistaken).
### Input
The first line of the input gives the number of test cases, T. T test cases follow. Each consists of a single line with four integers N, A, B, and C: the information given by Andre and Sule.
### Output
For each test case, output one line containing ``Case #x: y``, where x is the test case number (starting from 1) and y is IMPOSSIBLE if there is no possible height for each building according to the above information, or N space-separated integers otherwise. The i-th integer in y must be the height of the i-th building (in meters) between 1 to N.
### Limits
Time limit: 20 seconds per test set.
Memory limit: 1GB.
1 ≤ T ≤ 100.
1 ≤ C ≤ N.
C ≤ A ≤ N.
C ≤ B ≤ N.
Test Set 1
1 ≤ N ≤ 5.
Test Set 2
1 ≤ N ≤ 100.

### Sample
```
Input 
 	
Output 
 
3
4 1 3 1
4 4 4 3
5 3 3 2

  
Case #1: 4 1 3 2
Case #2: IMPOSSIBLE
Case #3: 2 1 5 5 3
```
  
In Sample Case #1, the sample output sets the height of each building such that only the first building is visible to Andre, while the first, third, and fourth buildings are visible to Sule. Therefore, only the first building is visible to both Andre and Sule. Note that there exist other correct solutions, such as 4 3 1 2.
In Sample Case #2, all N = 4 buildings are visible to Andre and Sule. Therefore, it is impossible to have C ≠ N in this case.
In Sample Case #3, the sample output sets the height of each building such that the first, third, and fourth buildings are visible to Andre, while the third, fourth, and fifth buildings are visible to Sule. Therefore, the third and fourth buildings are visible to both Andre and Sule. Note that there exist other correct solutions.

### Solution try 1

```python
def isValidCombination(buildings, N, A, B, C):
    a, b, c = 0, 0, 0

    # check from left to right
    tallest_building = buildings[0]
    for building in buildings:
        if tallest_building <= building:
            tallest_building = building
            a += 1

    # check from right to left
    tallest_building = buildings[-1]
    for building in reversed(buildings):
        if tallest_building <= building:
            tallest_building = building
            b += 1

    # count number of buildings with maximum height
    c = buildings.count(tallest_building)

    return a == A and b == B and c == C


def findValidBuildingCombinations(N, A, B, C):
    # generate next permutation
    buildings = [1] * N
    stack = []
    stack.append(buildings)
    visited = []

    while len(stack) != 0:
        permutation = stack.pop()
        visited.append(permutation)

        if isValidCombination(permutation, N, A, B, C):
            return permutation

        for i in range(N):
            for j in range(permutation[i], N):
                next_permutation = permutation.copy()
                next_permutation[i] = j

                if next_permutation not in visited:
                    stack.append(next_permutation)


    return []
    
if __name__ == '__main__':
    nd = input().split()
    
    T = int(nd[0])
    
    for i in range(T):
            [N,A,B,C] = list(map(int, input().rstrip().split()))
            ans = findValidBuildingCombinations(N,A,B,C)
            print('Case #{}: '.format(i), end='')
            if ans == []:
                print('IMPOSSIBLE')
            else:
                print(*ans, sep=',')

```
