# Why.js

JavaScript is not a perfect language, but it's powerful and important. In the past couple of years there have been a lot of infamous talks like [this](https://www.destroyallsoftware.com/talks/wat) & [this](https://www.youtube.com/watch?v=et8xNAc2ic8) that exposed the weird ways in which JavaScript behaves at certain instances. Being the single most popular language on the web right now , it's really common for new developers to fall for it's bizarre behaviour. This repositry aims to highlight those behaviours and demystify _why_ these behaviours occur.

# Preface

Many of these WATs occur in JavaScript due to properties such as type coercion and evaluation methodology of `==` & `===` operator. JavaScript is a weekly typed language. This means that varibales can automatically be changed from one type to another while evaluating an expression. Although, this is a very powerful feature of the language it might give rise to some unconventional situtaions.

# WATs

1. **Equality of empty arrays**
```javascript
> [] == []
false
```
On first look, it sounds ridiculous. An empty array is not equal to itself? But this is not what the above statement actually means. Arrays are stored by references in JavaScript and JavaScript double equal operator returns `true` only when you're comparing same instances of same type. Instead the comparison above actually asks that, "Is an instance of empty array equal to instace of another empty array?", which is defenitely `false`. The above statement is similar to

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
