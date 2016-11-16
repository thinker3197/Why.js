# Why.js

JavaScript is not a perfect language, but it's powerful and important. In the past couple of years there have been a lot of infamous talks like [this](https://www.destroyallsoftware.com/talks/wat) & [this](https://www.youtube.com/watch?v=et8xNAc2ic8) that exposed the weird ways in which JavaScript behaves at certain instances. Being the single most popular language on the web right now , it's really common for new developers to fall for it's bizarre behaviour. This repositry aims to highlight those behaviours and demystify _why_ these behaviours occur.

# Preface

Many of these WATs occur in JavaScript due to properties such as type coercion and evaluation methodology of `==` & `===` operator. The `==` and `===` operators evaluate an expression using the abstract equality comparsion algorithms and the strict equality comparison algorithm. JavaScript is a weekly typed language. This means that varibales can automatically be changed from one type to another while evaluating an expression. Although this is a very powerful feature of the language, it might give rise to some unconventional situtaions.

# WATs

1[](). **Equality of empty arrays**

```javascript
> [] == []
false
```
On first look, it sounds ridiculous. An empty array is not equal to itself? But this is not what the above statement actually means. Arrays are stored by references in JavaScript and JavaScript double equal operator returns `true` only when you're comparing same instances of same type. The comparison above actually asks that, "Is an instance of empty array equal to instace of another empty array?", which is defenitely `false`. The above statement is similar to

```javascript
> var a = [];
undefined
> var b = [];
undefined
> a == b
false
```
`a` and `b` are references to two different locations in memory, hence the result is false. However, if the both the array instances have been same, like the one below then answer below would have been `true`.

```javascript
> var a = b = [];
undefined
> a == b
true
```

2[](). **Equlaity of empty array and `NOT` empty array**

```javascript
> [] == ![]
true
```

Before understanding what is happening above, we need to understand the concept of *truthy* and *falsy* in JavaScript and how the `!` (logical NOT) operator works. Values such as `false`, `null`, `undefined`, `NaN`, `0`, `''` and `""` are considered as falsy. Other values like `true`, `{}`, `[]`, `"foo"` etc. are considered truthy. The `!` operator on the other hand is defined for *boolean* values only. So in the above context JavaScript automatically do the type coercion of the empty array and converts it into it's coresponding boolean value i.e. `true`.
`![]` evalutaes to `false` and the conversion now actually becomes

```javascript
> [] == false
true
```

Isn't that supposed to be false, since empty arrays are truthy? That's right but the double equal operator evalutes expression on certain rules. We're trying to compare an object with a boolean value and JavaScript will implicitly convert the operands to *Number* type. `Number([])` is `0` and `Number(false)` is also `0`, which evaluates to `true` since zero is equal to zero.

3[](). **Empty array plus empty array**

```javascript
> [] + []
""
```

It might look like the sum of two arrays will concatinate them and hence on adding two empty arrays one might get another empty array. But that's not the case in JavaScript. As stated in the ECMA language specification - The addition operator either performs string concatenation or numeric addition. Hence, the `+` is not defined for arrays and JavaScript will implicitly convert arrays into their _string_ eqvuilent and concatinate them. The above expression will become similar to

```javascript
> [].toString() + [].toString()
""
```

Concatination of two empty strings yeilds another empty string and hence the above statement is valid.

4[](). **Empty array minus empty array**

```javascript
> [] - []
0
```

This case is similar to the previous one. The `-` operator is not defined for arrays or _strings_. Hence JavaScript will implicitly convert the arrays into their corresponding _number_ type. Coercion of empty array into _number_ type yeilds `0`. The above expression is same as

```javascript
> Number([]) - Number([])
0
```

Zero minus zero is obviously zero, which makes sense.

5[](). **Weird array equal weird string**

```javascript
> [null, undefined, []] == ",,"
true
```

This is quite weird. When we try to compare an non-empty array with a _string_, Javascript coerces each array element into string and then joins them by commas. Stringification of `null`, `undefined` & `[]` gives an empty string. Hence the expression `[null, undefined, []].toString()` yeilds `",,"`. 

6[](). **Plus empty array**

```javascript
> + []
0
```

That's actually not WAT. In Javascript and unary plus operator is used to expicitly convert the object into _number_ type. So the above statement is similar to

```javascript
> Number([])
0
```

7[](). **Empty array plus empty object**

```javascript
> [] + {} 
"[object Object]"
```

You might have guessed this one. As seen in 3, JavaScript will coerce the array and the object into _string_ and then concatenate them. The default conversion for object type to _string_ is `[object type]`, where `type` is the type of object. There are different type of objects in JavaScript like `Object`, `Function`, `Array`, `RegExp` etc. In this case it's `Object`. 

```javascript
> "" + "[object Object]"
"[object Object]"
```

8[](). **Empty object plus empty array**

```javascript
> {} + []
0
```
 
 Wait! Didn't we just said that JavaScript coerces object and array to _string_ on plus operator? This is actally a bizzare edge case of javascript. The JavaScript compiler understands this statement a bit diffrently. It considers `{}` as an empty code block and `+ []` as another statement. We saw `+ []` equates to `0` which is why the above expression evaluates to `0`. 

9[](). **Empty object plus empty object**

```javascript
> {} + {}
NaN
```

This one is similar to the example above. Explicit conversion of an empty object into _number_ type yeilds `NaN`. 

10[](). The **_null_ and zero**

```javascript
> 0 < null
false
> 0 >= null 
true
> 0 == null 
false
```

Well, that's frustrating. JavaScript double equal operator and the relational operator are based on the abstract equality comparison algorithm and the abstract relational comparison algorithm. It's just a fancy set of rules to evaluate an expression. 
When we compare `0` and `null` using relational operators, JavaScript coerces `null` into it's primitive _number_ type. When using relational operators the comparison is similar to

```javascript
> 0 < Number(null)
false
> 0 >= Number(null)
true
```

`null` when converted to _number_ type yeilds `0` which explains evaluation of the above statements. However with the `==` opeartor, such a comparison dosen't fall into any category of rules defined for evaluation of expression by `==` operator. Neither `0` nor `null` gets coerced and by rule if such a case occurs where the comparison dosen't fall into any category, `false` is returned.

# Glossary

* **coercion** - *Also type-conversion* Type conversion (or typecasting) means transfer of data from one data type to another.
* **falsy** - In JavaScript, a falsy value is a value that translates to false when evaluated in a Boolean context.
* **truthy** - A truthy value is a value that translates to true when evaluated in a Boolean context.
