# LeetCode Pattern: Arrays & Hashing

## Problem: Two Sum

### 1. What is the question asking?

You are given an array of integers `nums` and an integer `target`.

Your task is to find **exactly two numbers** in the array whose sum equals the target. Once you find them, return their **indices**, not the numbers themselves.

### Important constraints

* Exactly **one valid solution** exists.
* You **cannot use the same element twice**.
* The order of the returned indices does not matter.

Example:

```javascript
nums = [2, 7, 11, 15]
target = 9

Output: [0, 1]
```

Because:

```javascript
nums[0] + nums[1] === 9
2 + 7 === 9
```

---

## 2. The Brute Force Approach

The most straightforward solution is to compare every pair of numbers.

For each element:

* Check all the elements after it.
* If their sum equals the target, return their indices.

```javascript
for (let i = 0; i < nums.length; i++) {
    for (let j = i + 1; j < nums.length; j++) {
        if (nums[i] + nums[j] === target) {
            return [i, j];
        }
    }
}
```

### Time Complexity

```
O(n²)
```

Because for every element, we may examine almost every other element.

### Space Complexity

```
O(1)
```

No extra data structure is used.

The follow-up asks for a solution faster than `O(n²)`.

---

## 3. Recognizing the Pattern: Hashing

### Key interview clue

Whenever you hear yourself asking:

* "Have I seen this value before?"
* "Does this value already exist?"
* "Can I find this value quickly?"

think:

```
Hash Map
```

A Hash Map allows:

* Insertion → `O(1)`
* Lookup → `O(1)`

This makes it ideal for problems involving complements, frequencies, or previously seen values.

---

## 4. The Optimal Strategy: One-Pass Hash Map

Instead of looking ahead, we look backward.

We store numbers we have already processed so future numbers can use them.

### Hash Map structure

```javascript
{
    number: index
}
```

For every number in the array:

### Step 1

Calculate its complement.

```javascript
complement = target - currentNumber
```

This answers the question:

> "What number do I need to complete the target?"

---

### Step 2

Ask the Hash Map:

> "Have I already seen this complement?"

If **YES**:

Return the stored index and the current index.

```javascript
return [map.get(complement), currentIndex];
```

---

### Step 3

If **NO**:

Store the current number and its index.

```javascript
map.set(currentNumber, currentIndex);
```

This allows future elements to find it.

---

## Example Walkthrough

```javascript
nums = [2, 7, 11, 15]
target = 9
```

### Iteration 1

Current number:

```javascript
2
```

Complement:

```javascript
9 - 2 = 7
```

Map:

```javascript
{}
```

Have we seen `7`?

```
No
```

Store:

```javascript
{
    2: 0
}
```

---

### Iteration 2

Current number:

```javascript
7
```

Complement:

```javascript
9 - 7 = 2
```

Map:

```javascript
{
    2: 0
}
```

Have we seen `2`?

```
Yes
```

Return:

```javascript
[0, 1]
```

---

## Why do we check before storing?

We cannot use the same element twice.

By checking first, we ensure that the complement came from an earlier position in the array.

This also correctly handles cases such as:

```javascript
nums = [3, 3]
target = 6
```

---

## JavaScript Solution

```javascript
var twoSum = function(nums, target) {
    const map = new Map();

    for (let i = 0; i < nums.length; i++) {
        const complement = target - nums[i];

        if (map.has(complement)) {
            return [map.get(complement), i];
        }

        map.set(nums[i], i);
    }
};
```

---

## Complexity Analysis

### Time Complexity

```
O(n)
```

We traverse the array once.

### Space Complexity

```
O(n)
```

In the worst case, the Hash Map stores every element.

---

## Pattern Recognition Summary

When solving problems, ask yourself:

1. Am I trying to find a pair?
2. Am I repeatedly checking whether a value exists?
3. Would fast lookup improve the solution?

If the answer is **yes**, consider using a:

```
Hash Map
```

### Core Formula

```
currentNumber + complement = target

complement = target - currentNumber
```

### Mental Model

```
Need something?
↓
Have I seen it before?
↓
Yes → return answer
No → remember current value
```
