OPERATORS = {'->': (1, lambda x, y: (x & y) | (x ^ 1)),
             '|': (2, lambda x, y: x | y),
             '&': (3, lambda x, y: x & y),
             '!': (4, lambda x: x ^ 1)
             }
variables = dict()

expression = input().replace('!!', '')
# ((PPP->PPP’)->PPP)->PPP  Output: Valid
# A&!A  Output: Unsatisfiable
# A->!B123  Output: Satisfiable and invalid, 3 true and 1 false cases


def plus_one(digits):
    i = len(digits)-1
    while True:
        if i == -1:
            return digits
        if digits[i] == 1:
            digits[i] = 0
        else:
            digits[i] += 1
            break
        i -= 1
    return digits


def calc2(lst2):
    stack = []
    for token in lst2:
        if token in OPERATORS:
            if token == '!':
                x = stack.pop()
                stack.append(OPERATORS[token][1](x))
            else:
                y, x = stack.pop(), stack.pop()
                stack.append(OPERATORS[token][1](x, y))
        else:
            stack.append(variables[token])
    return stack[0]


def parse(expr):
    variable = ''
    for s in expr:
        if s not in {'!', '&', '-', '|', '>', '(', ')'}:
            variable += s
            continue
        elif variable:
            variables[variable] = 0
            yield variable
            variable = ''
        if s in {'!', '&', '-', '|', '(', ')'}:
            if s in '-':
                yield '->'
            else:
                yield s
    if variable:
        variables[variable] = 0
        yield variable


def create_stack(parsed_expr):
    stack = []
    for token in parsed_expr:
        if token in OPERATORS:
            while stack and stack[-1] != "(" and OPERATORS[token][0] <= OPERATORS[stack[-1]][0]:
                yield stack.pop()
            stack.append(token)
        elif token == ")":
            while stack:
                x = stack.pop()
                if x == "(":
                    break
                yield x
        elif token == "(":
            stack.append(token)
        else:
            yield token
    while stack:
        yield stack.pop()


def calc(lst):
    true = 0
    false = 0
    lst2 = []
    for i in lst:
        lst2.append(i)
    if calc2(lst2) == 1:
        true += 1
    else:
        false += 1
    length = len(variables)
    digits = [0 for i in range(length)]
    for i in range(1, 2 ** length):
        digits = plus_one(digits)
        k = 0
        for key in variables.keys():
            variables[key] = digits[k]
            k += 1
        if calc2(lst2) == 1:
            true += 1
        else:
            false += 1

    if true == 2 ** length:
        print('Valid')
    elif false == 2 ** length:
        print('Unsatisfiable')
    else:
        print(f'Satisfiable and invalid, {true} true and {false} false cases')


calc(create_stack(parse(expression)))
