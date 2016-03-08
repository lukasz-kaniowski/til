# Data driven tests hints

## Table with one column

```groovy
@Unroll
def '#expressionEqual2 should equal 2'() {
    expect:
    Eval.me(expressionEqual2) == 2
    where:
    expressionEqual2 | _
    '2 * 2 / 2'      | _
    '10 - 8'         | _
}
```

generated scenario titles:

```bash
2 * 2 / 2 should equal 2
10 - 8 should equal 2
```

## Unroll with custom text

```groovy
@Unroll
def '#expression #unrollDescription'() {

    expect:
    Eval.me(expression) == shouldWork
    where:
    expression      || shouldWork
    'true == true'  || true
    'true == false' || false

    unrollDescription = shouldWork ? "should work" : "should not work"
}
```

generated scenario titles:

```bash
true == true should work
true == false should not work
```
