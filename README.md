# Ed Pegg's problem.

How many of the nine-digit permutations of 123456789 can be represented by 4 digits and two arithmetic symbols?  
For example, 9*14^7 = 948721536.

[Link to Facebook Post](https://www.facebook.com/ed.pegg/posts/pfbid02muocAwL2V8VD9y5fjxCgFssD6PnSWuRs9LeqcgjBKL5Arwxc9qLfJx1h4vKjYF82l)

GPT-4 provided the following solution after a few rounds:

```python
import itertools

def get_combinations(n):
    if n == 0:
        return [[]]

    combinations = []
    for i in range(1, n + 1):
        for combo in get_combinations(n - i):
            combinations.append([i] + combo)

    return combinations

def get_integer_combinations(combination):
    return itertools.product(*[range(10 ** p) for p in combination])

def get_operator_combinations(combination):
    ops = ['+', '-', '*', '**']
    for r in range(len(combination)):
        for op_comb in itertools.combinations_with_replacement(ops, r):
            if op_comb.count('**') <= 1:  # Limit '**' to only once
                for perm in itertools.permutations(op_comb):
                    yield perm

def is_permutation_of_9876543210(number):
    str_num = str(number)
    return sorted(str_num) == sorted('9876543210') or sorted(str_num) == sorted('987654321')

combinations = get_combinations(4)

for combination in combinations:
    for integers in get_integer_combinations(combination):
        for operators in get_operator_combinations(combination):
            expression = ''
            for i in range(len(integers)):
                expression += str(integers[i])
                if i < len(operators):
                    if operators[i] == '**':
                        expression += '**'
                    else:
                        expression += ' ' + operators[i] + ' '
            try:
                result = eval(expression)
                if is_permutation_of_9876543210(result):
                    print(f'{expression} = {result}')
            except Exception:
                pass
```