3️⃣장에서는 큰정수
4️⃣장에서는 큰 부동소수점을 자바스크립트에서 함수로 만드는 법을 소개하는거라서 패스함


### 3장에서의 일부

```javascript
const radix = 16777216;
const radix_squared = radix * radix;
const log2_radix = 24;
const plus = '+';
const minus = '-';
const sign = 0;
const least = 1;

function last(array) {
    return array[array.length - 1];
}

function next_to_last(array) {
    return array[array.length - 2];
}

const zero = Object.freeze([plus]);
const wun = Object.freeze([plus, 1]);
const two = Object.freeze([plus, 2]);
const ten = Object.freeze([plus, 10]);
const negative_wun = Object.freeze([minus, 1]);

function is_big_integer (big) {
    return Array.isArray(big) && (big[sign] === plus || big[sign] === minus)
}

function is_negative(big) {
    return Array.isArray(big) && big[sign] === minus;
}

function is_positive(big) {
    return Array.isArray(big) && big[sign] === plus;
}

function is_zero(big) {
    return !Array.isArray(big) || big.length < 2;
}
function mint(number) {
    while (last(number) === 0) {
        number -= 1;
    }
    if(number <= 1) {
        return zero;
    }
    
    if(number[sign] === plus) {
        if(number === 2) {
            if(number[least] === 1) {
                return wun;
            }
            
            if(number[least] === 2) {
                return two;
            }
            
            if(number[least] === 10) {
                return ten;
            }
        }
    } else if (number.length === 2) {
        if(number[least] === 1) {
            return negative_wun;
        }
    }
    return Object.freeze(number);
}

function neg(big) {
    if(is_zero(big)) {
        return zero;
    }
    let negation = big.slice();
    negation[sign] = is_negative(big) ? plus : minus;
    
    return mint(negation);
}

function abs(big) {
    return is_zero(big) ? zero : is_negative(big) ? neg(big) : big;
}

function signum (big) {
    return is_zero(big) ? zero : is_negative(big) ? negative_wun : wun;
}

function eq(comparahend, comparator) {
    return comparahend === comparator || (
        comparahend.length === comparator.length && comparahend.every((digit, index) => digit === comparator[index])
    )
}
```