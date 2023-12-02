### Snow Operation Calibration
[Source: adventofcode.com](https://adventofcode.com/2023/day/1)

#### Problem Statement

You are given a list of strings representing a calibration document for a snow operation system. Your task is to calculate the sum of specific calibration values derived from each string.

#### Part One

**Objective:** For each string, identify the first and last digit present in the string and combine them to form a two-digit number. Sum these numbers across all strings.

**Input:** `List[str]` where each string contains alphanumeric characters.

**Output:** `int` representing the sum of the two-digit numbers found in all strings.

**Example:**
- Input: `["1abc2", "pqr3stu8vwx", "a1b2c3d4e5f", "treb7uchet"]`
- Output: `142` (Calibration values are 12, 38, 15, and 77)


```python
def findSum(inputs):
    total_sum = 0
    for input_string in inputs:
        num = findNum(input_string)
        print(num, input_string)
        total_sum += num
    return total_sum

def findNum(input_string):
    first_digit = None
    last_digit = None
    
    # Iterate through the string only once
    for char in input_string:
        if char.isdigit():
            # Update the first digit if it's not yet found
            if first_digit is None:
                first_digit = char
            # Always update the last digit
            last_digit = char

    # Combine the first and last digits to form a number, if found
    if first_digit is not None and last_digit is not None:
        return int(first_digit + last_digit)
    return 0

# Example usage
inputs = ["1abc2", "pqr3stu8vwx", "a1b2c3d4e5f", "treb7uchet"]
print(findSum(inputs))
```

#### Part Two

**Objective:** Modify the logic to also consider digits spelled out as words (one, two, three, etc.) as valid digits. Identify the real first and last 'digit' (number or spelled-out number) in each line and combine them as before.

**Input:** Same as part one.

**Output:** Same as part one.

**Example:**
- Input: `["two1nine", "eightwothree", "abcone2threexyz", "xtwone3four", "4nineeightseven2", "zoneight234", "7pqrstsixteen"]`
- Output: `281` (Calibration values are 29, 83, 13, 24, 42, 14, and 76)

```python
def findSum(inputs):
    sum1 = 0
    for input in inputs:
        num1 = findNum(input)
        print(num1, input)
        sum1 += num1
    return sum1
def findNum(input):

    nums = {
        "nine": "9",
        "eight": "8",
        "seven": "7",
        "six": "6",
        "five": "5",
        "four": "4",
        "three": "3",
        "two": "2",
        "one": "1",
        "zero": "0",
    }
    start, end  = float('inf'), float('-inf')
    left, right = '0','0'
    for key,value in nums.items():
        start,end,left,right = findboth(input,start,end,left,right,key,value)
        start,end,left,right = findboth(input,start,end,left,right,value,value)
    return int(left+right)
        
            
        
def findboth(input,start,end,left,right,key,value):
    index1 =input.find(key)
    if index1>=0 and index1<start:
        start = index1
        left = value
    index1 =input.rfind(key)
    if index1>=0 and index1>end:
        end = index1
        right = value
    return start,end,left,right
    
print(findSum(inputs))
```
#### Constraints

- The length of the input list is between 1 and 10,000.
- Each string will have a length between 1 and 100 characters.
- Strings may contain lowercase letters, uppercase letters, and digits.
- For part two, spelled-out numbers will be in lowercase and within the range of one to nine.

#### Notes

- Assume that each string will have at least one valid digit or spelled-out number at the beginning and end.
- For part two, if a spelled-out number appears at the beginning or end of a string, it should be treated as a single digit (e.g., "one" should be treated as 1).