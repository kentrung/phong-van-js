## 1. Hoisting trong Javascript xảy ra như thế nào?

```js
function sayHi() {
  console.log(name);
  console.log(age);
  var name = "Lydia";
  let age = 21;
}

sayHi();
```

- `Lydia and undefined`
- `Lydia and ReferenceError`
- `ReferenceError and 21`
- `undefined and ReferenceError`

<details><summary>Đáp án</summary>

- **Đáp án: D**
- Bên trong Function này, trước tiên chúng ta khai báo biến name bằng từ khóa `var`. Điều này có nghĩa là hoisting đã xảy ra (không gian bộ nhớ được thiết lập trong giai đoạn tạo, nhưng chưa thực hiện phép gán giá trị) với giá trị mặc định là `undefined`, tiếp sau đó chúng ta mới thực sự định nghĩa biến name.
- Trước khi cố gắng log biến name thì chúng ta chưa hề định nghĩa biến name nào, vì hoisting xảy ra và biến name giữ giá trị là `undefined`
- Các biến với từ khóa `let` - `const` cũng được hoisting, nhưng không giống như từ khóa `var`, chúng không thể truy cập trước khi chúng thực sự được khởi tạo.
- Đây được gọi là **Temporal Dead Zone** (Vùng chết tạm thời). Do đó, khi cố gắng truy cập các biến này trước khi được khai báo. Javascript sẽ ném ra `ReferenceError`
</details>

## 2. Vòng lặp sử dụng var hoặc let trong Javascript có gì khác nhau?

```js
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1);
}

for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1);
}
```

- `0 1 2 and 0 1 2`
- `0 1 2 and 3 3 3`
- `3 3 3 and 0 1 2`

<details><summary>Đáp án</summary>

- **Đáp án: C**
- Bởi vì hàng đợi sự kiện trong JavaScript, hàm callback `setTimeout()` được gọi sau khi vòng lặp được thực thi.
- Do biến i trong vòng lặp đầu tiên được khai báo bằng từ khóa var, nên giá trị này là toàn cục.
- Trong vòng lặp, chúng ta đã tăng giá trị của i lên 1 lần, bằng cách sử dụng toán tử `++`. Vào thời điểm hàm callback `setTimeout()` được gọi, i đã có giá trị bằng 3 ở trong vòng lặp for đầu tiên.
- Trong vòng lặp for thứ hai, biến i được khai báo bằng từ khóa let: biến được khai báo với từ khóa `let` (và `const`) có phạm vi dạng block hay còn gọi là `block-scoped` (Phạm vi trong dấu ngoặc `{ }`).
- Trong mỗi lần lặp, ta sẽ có một giá trị mới và mỗi giá trị nằm trong phạm vi của vòng lặp.
</details>

## 3. Khi sử dụng từ khóa this trong arrow function nên lưu ý điều gì?

```js
const shape = {
  radius: 10,
  diameter() {
    return this.radius * 2;
  },
  perimeter: () => 2 * Math.PI * this.radius,
};

shape.diameter();
shape.perimeter();
```

- `20 and 62.83185307179586`
- `20 and NaN`
- `20 and 63`
- `NaN and 63`

<details><summary>Đáp án</summary>

- **Đáp án: B**
- Lưu ý rằng giá trị của `diameter()` là một hàm thông thường, trong khi giá trị của `perimeter()` là một arrow function.
- Với các arrow function, không giống như các hàm thông thường, từ khóa this ở đây là đề cập đến phạm vi hiện tại của nó (nơi nó được gọi).
- Điều này có nghĩa là khi chúng ta gọi đến `permeter()`, nó không đề cập đến đối tượng shape, mà là phạm vi xung quanh của nó (ví dụ như đề cập đến window)
- Kết quả là không có giá trị radius trên đối tượng đó, nó trả về `undefined`
</details>

## 4. Chuyển đổi số bằng toán tử +, tính chất truthy value

```js
+true;
!"ICT Hà Nội";
```

- `1 and false`
- `false and NaN`
- `false and false`

<details><summary>Đáp án</summary>

- **Đáp án: A**
- Toán tử `+` ở đây đã chuyển đổi một toán hạng thành một số. true là 1 và false là 0.
- Các giá trị falsy:
  - `false`
  - `0` và `NaN`
  - `''` (empty string)
  - `null`
  - `undefined`
- Vậy chúng ta có tổng cộng là 6 falsy value. Các giá trị còn lại đều là truthy value.
- Chính vì thế chuỗi `'ICT Hà Nội'` là một truthy value. Do đó phủ định của truthy là false.
</details>

## 5. Sự khác nhau khi sử dụng dấu ngoặc [ ] và toán tử dot để truy cập Object là gì?

```js
const bird = {
  size: "small",
};

const mouse = {
  name: "Mickey",
  small: true,
};
```

- `mouse.bird.size không hợp lệ`
- `mouse[bird.size] không hợp lệ`
- `mouse[bird["size"]] không hợp lệ`
- `Tất cả đều hợp lệ`

<details><summary>Đáp án</summary>

- **Đáp án: A**
- Trong JavaScript, tất cả các object key là chuỗi (trừ khi đó là Symbol). Mặc dù chúng ta có thể không gõ chúng dưới dạng chuỗi, chúng luôn được chuyển đổi thành chuỗi.
- JavaScript thông dịch (hoặc unboxes) các statement. Khi chúng ta sử dụng ký hiệu ngoặc vuông, nó sẽ thấy dấu ngoặc vuông mở đầu tiên `[` và tiếp tục cho đến khi tìm thấy dấu đóng `]`. Sau đó, nó sẽ đánh giá statement.
- Đối với `mouse[bird.size]`: Đầu tiên nó sẽ đánh giá `bird.size`, có giá trị là `small`. Do đó `mouse["small"]` sẽ trả về giá trị true.
- Tuy nhiên, đối với việc sử dụng dấu chấm . thì hành động này lại không xảy ra. Object mouse không có key nào gọi là bird , điều này có nghĩa là `mouse.bird` sẽ trả về `undefined`
- Sau đó, chúng ta gọi đến size cũng bằng cách sử dụng toán tử dot `.` là: `mouse.bird.size`. Từ `mouse.bird` đã là `undefined` thì chúng ta đang thực sự gọi là `undefined.size`
- Dẫn tới, điều này là không hợp lệ, lúc này bạn sẽ nhận được một lỗi như là: `Cannot read property "size" of undefined`
</details>

## 6. Tương tác bằng tham chiếu trong Javascript là thế nào?

```js
let c = { greeting: "Hey!" };
let d;

d = c;
c.greeting = "Hello";
console.log(d.greeting);
```

- `Hello`
- `Hey`
- `undefined`
- `ReferenceError`
- `TypeError`

<details><summary>Đáp án</summary>

- **Đáp án: A**
- Trong JavaScript, tất cả các object tương tác bằng tham chiếu (reference) khi thiết lập chúng bằng nhau.
- Đầu tiên, biến c giữ một object. Sau đó, chúng ta gán d với tham chiếu c (đang giữ một object)
- Khi chúng ta thay đổi object đồng nghĩa với việc thay đổi tất cả chúng.
</details>

## 7. Toán tử == và === trong Javascript khác nhau như thế nào?

```js
let a = 3;
let b = new Number(3);
let c = 3;

console.log(a == b);
console.log(a === b);
console.log(b === c);
```

- `true false true`
- `false false true`
- `true false false`
- `false true true`

<details><summary>Đáp án</summary>

- **Đáp án: C**
- `new Number()` là một constructor function được xây dựng sẵn. Mặc dù nó trông giống như một số, nhưng nó không thực sự là một số: Nó có một loạt các tính năng bổ sung và là một đối tượng.
- Khi chúng ta sử dụng toán tử ==, nó chỉ kiểm tra xem nó có cùng giá trị hay không. Cả hai đều có giá trị là 3, vì vậy nó trả về true.
- Tuy nhiên, khi chúng ta sử dụng toán tử ===, thì sẽ phải so sánh cả giá trị và cả kiểu của nó. Do `new Number()` không phải là một số, nó là một object.
- Dẫn tới cả hai phép so sánh đều trả lại false.
</details>

## 8. Hiểu hơn về hàm Static trong Javascript?

```js
class Chameleon {
  static colorChange(newColor) {
    this.newColor = newColor;
    return this.newColor;
  }

  constructor({ newColor = "green" } = {}) {
    this.newColor = newColor;
  }
}

const freddie = new Chameleon({ newColor: "purple" });
freddie.colorChange("orange");
```

- `orange`
- `purple`
- `green`
- `TypeError`

<details><summary>Đáp án</summary>

- **Đáp án: D**
- Hàm `colorChange` là ở dạng static. Các phương thức static được thiết kế để chỉ ở trên hàm constructor mà chúng được tạo và không thể truyền lại cho bất kỳ child nào.
- Vì `freddie` là một child, do đó hàm không thể truyền lại, và không có sẵn trên freddie nên TypeError sẽ được ném ra.
</details>

## 9. Về cơ bản thì sử dụng 'use strict' để làm gì?

```js
let greeting;
greetign = {}; // Giả sử bị lỗi đánh máy!
console.log(greetign);
```

- `{}`
- `ReferenceError: greetign is not defined`
- `undefined`

<details><summary>Đáp án</summary>

- **Đáp án: A**
- Nó log ra một object, bởi vì chúng ta vừa tạo một object trên global object. Khi gõ lỗi `greeting` thành `greetign`, trình thông dịch JS thực sự đã hiểu tình huống này như là `global.greetign = {}` (hoặc `window.greetign = {}` nếu ở trên trình duyệt)
- Để tránh điều này, chúng ta có thể sử dụng `use strict`. Cách này đảm bảo rằng bạn đã khai báo một biến trước khi gián nó cho bất cứ thứ gì.
</details>

## 10. Điều gì xảy ra khi tạo thuộc tính cho hàm giống như object?

```js
function bark() {
  console.log("Woof!");
}

bark.animal = "dog";
```

- `Không có gì. Thế này hoàn toàn ok`
- `SyntaxError. You cannot add properties to a function this way`
- `undefined`
- `ReferenceError`

<details><summary>Đáp án</summary>

- **Đáp án: A**
- Điều này là hoàn toàn có thể trong JavaScript, bởi vì các hàm là các object! (Tất cả mọi thứ ngoài các kiểu nguyên thủy đều là các object)
- Một hàm là một loại object đặc biệt. Code bạn tự viết không phải là hàm thực tế. Hàm này là một object có thuộc tính. Thuộc tính này là bất khả xâm phạm.
</details>

## 11. Thêm thuộc tính vào constructor thì xảy ra điều gì?

```js
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}

const member = new Person("Lydia", "Hallie");
Person.getFullName = function () {
  return `${this.firstName} ${this.lastName}`;
};

console.log(member.getFullName());
```

- `TypeError`
- `SyntaxError`
- `Lydia Hallie`
- `undefined undefined`

<details><summary>Đáp án</summary>

- **Đáp án: A**
- Bạn không thể thêm thuộc tính vào hàm constructor như bạn có thể với các object thông thường.
- Nếu bạn muốn thêm một hàm cho tất cả các object cùng một lúc, bạn phải sử dụng prototype để thay thế. Vì vậy, trong trường hợp này bạn phải làm thế này:

```js
Person.prototype.getFullName = function () {
  return `${this.firstName} ${this.lastName}`;
};
```

- Bằng cách này thì `member.getFullname()` sẽ làm việc. Tại sao lại thế?
- Giả sử nếu chúng ta đã thêm phương thức này vào chính hàm `constructor`. Có lẽ không phải mọi `Person` đều cần phương thức này.
- Điều này sẽ lãng phí rất nhiều dung lượng bộ nhớ, vì chúng vẫn có thuộc tính đó, chiếm không gian bộ nhớ cho mỗi phiên bản Person.
- Thay vào đó, nếu chúng ta chỉ thêm nó vào prototype, lúc đó chúng ta chỉ có một vị trí trong bộ nhớ, nhưng tất cả Person đều có quyền truy cập vào nó!
</details>

## 12. Khởi tạo đối tượng có từ khóa new và không có từ khóa new khác gì nhau?

```js
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}

const lydia = new Person("Lydia", "Hallie");
const sarah = Person("Sarah", "Smith");

console.log(lydia);
console.log(sarah);
```

- `Person {firstName: "Lydia", lastName: "Hallie"} and undefined`
- `Person {firstName: "Lydia", lastName: "Hallie"} and Person {firstName: "Sarah", lastName: "Smith"}`
- `Person {firstName: "Lydia", lastName: "Hallie"} and {}`
- `Person {firstName: "Lydia", lastName: "Hallie"} and ReferenceError`

<details><summary>Đáp án</summary>

- **Đáp án: A**
- Đối với sarah, chúng ta đã không sử dụng từ khóa new. Khi sử dụng từ khóa new, nó đề cập đến empty object mới mà chúng ta tạo. Tuy nhiên, nếu bạn không thêm new, nó sẽ tìm đến global object!
- Khi nói rằng this.firstName sẽ tương đương với "Sarah" và this.lastName sẽ bằng "Smith".
- Bản chất thực sự của hành động trên là định nghĩa global.firstName = 'Sarah' và global.lastName = 'Smith'
- Chính sarah thì là undefined
</details>

## 13. 3 Giai đoạn của event propagation là gì?

```js
A: Target > Capturing > Bubbling;
B: Bubbling > Target > Capturing;
C: Target > Bubbling > Capturing;
D: Capturing > Target > Bubbling;
```

<details><summary>Đáp án</summary>

- **Đán án: D**
- Trong giai đoạn Capturing, các sự kiện đi qua phần tử đầu tiên xuống phần tử mục tiêu. Sau đó nó target vào phần tử mục tiêu, và Bubbling bắt đầu
</details>

## 14. Tất cả các object đều có prototype?

```js
A: true;
B: false;
```

<details><summary>Đáp án</summary>

- **Đáp án: B**
- Tất cả các object đều có prototype, ngoại trừ base object. Base object có quyền truy cập vào một số phương thức và thuộc tính, chẳng hạn như .toString
- Đây là lý do tại sao bạn có thể sử dụng các phương thức JavaScript được tích hợp sẵn! Tất cả các phương thức như vậy có sẵn trên prototype.
- Mặc dù JavaScript không thể tìm thấy nó trực tiếp trên object của bạn, nhưng nó đi xuống prototype chain và tìm thấy nó ở đó, điều này giúp bạn có thể truy cập nó.
</details>

## 15. Cộng 1 số với một chuỗi thì ra kết quả gì?

```js
function sum(a, b) {
  return a + b;
}

sum(1, "2");
```

- `NaN`
- `TypeError`
- `"12"`
- `3`

<details><summary>Đáp án</summary>

- **Đáp án: C**
- Javascript là một dynamic language: Chúng ta không chỉ định kiểu biến nhất định là gì.
- Giá trị có thể tự động được chuyển thành một loại mà bạn chưa biết, đó là tiềm ẩn được gọi là loại implicit type coercion (ép buộc). Coercion được chuyển đổi từ một kiểu này sang kiểu khác.
- Trong ví dụ này, JavaScript đã chuyển đổi số 1 thành một string để hàm có ý nghĩa và trả về một giá trị. Trong quá trình cộng kiểu number (1) và kiểu string ('2'), number đã được coi là một string.
- Vì vậy khi sử dụng toán tử + để cộng hai chuỗi lại với nhau như '1' + '2' sẽ trả về kết quả là '12'.
</details>

## 16. Sự khác biệt của number++ và ++number là gì?

```js
let number = 0;
console.log(number++);
console.log(++number);
console.log(number);
```

- `1 1 2`
- `1 2 2`
- `0 2 2`
- `0 1 2`

<details><summary>Đáp án</summary>

- **Đáp án: C**
- Toán tử ++ được thêm làm hậu tố hoạt động như sau:
- Đầu tiên nó trả về giá trị
- Sau đó mới tăng lên một đơn vị
- Toán tử ++ được thêm làm tiền tố hoạt động như sau:
- Đầu tiên nó tăng lên một đơn vị
- Sau đó trả về giá trị
- Kết quả như sau:

```js
let number = 0;
console.log(number++); //Log ra number sau đó mới tăng number lên 1 đơn vị
console.log(++number); //Tăng number lên 1 đơn vị, number = 2 và log ra console
console.log(number); // Log ra giá trị number hiện tại là 2
```

</details>

## 17. Nội suy chuỗi với Tag Template Literal trong ES6 là gì?

```js
function getPersonInfo(one, two, three) {
  console.log(one);
  console.log(two);
  console.log(three);
}

const person = "Lydia";
const age = 21;

getPersonInfo`${person} is ${age} years old`;
```

- `"Lydia" 21 ["", " is ", " years old"]`
- `["", " is ", " years old"] "Lydia" 21`
- `"Lydia" ["", " is ", " years old"] 21`

<details><summary>Đáp án</summary>

- **Đáp án: B**
- Nếu bạn sử dụng tagg template literal, giá trị của đối số đầu tiên luôn là một mảng của các giá trị chuỗi. Các đối số còn lại nhận được các giá trị của các biểu thức được truyền qua!
</details>

## 18. Truyền object như tham số để so sánh các object với nhau thì xảy ra vấn đề gì?

```js
function checkAge(data) {
  if (data === { age: 18 }) {
    console.log("You are an adult!");
  } else if (data == { age: 18 }) {
    console.log("You are still an adult.");
  } else {
    console.log(`Hmm.. You don't have an age I guess`);
  }
}

checkAge({ age: 18 });
```

- `You are an adult!`
- `You are still an adult.`
- `Hmm.. You don't have an age I guess`

<details><summary>Đáp án</summary>

- **Đáp án: C**
- Khi kiểm tra sự bằng nhau, các dữ liệu nguyên thủy được so sánh bởi giá trị của chúng, trong khi các object được so sánh bằng tham chiếu của chúng.
- JavaScript kiểm tra xem các object có tham chiếu đến cùng một vị trí trong bộ nhớ không.
- Hai object mà chúng ta đang so sánh không có điều đó: object chúng ta truyền dưới dạng tham số đề cập đến một vị trí khác trong bộ nhớ so với đối tượng chúng ta đã sử dụng để kiểm tra sự bằng nhau.
- Đây là lý do cả hai { age: 18 } === { age: 18 } và { age: 18 } == { age: 18 } trả về giá trị false.
</details>

## 19. Hiểu thêm một chút về toán tử Spread hay còn gọi là toán tử ...

```js
function getAge(...args) {
  console.log(typeof args);
}

getAge(21);
```

- `"number"`
- `"array"`
- `"object"`
- `"NaN"`

<details><summary>Đáp án</summary>

- **Đáp án: C**
- Toán tử spread ... (... args) trả về một mảng các đối số. Mảng là một object, vì thế typeof args sẽ trả về object
- Bạn có thể tìm hiểu rõ hơn về Spread Opeartor tại đây.
</details>

## 20. "use strict" có tác dụng gì?

```js
function getAge() {
  "use strict";
  age = 21;
  console.log(age);
}

getAge();
```

- `21`
- `undefined`
- `ReferenceError`
- `TypeError`

<details><summary>Đáp án</summary>

- **Đáp án: C**
- Với việc sử dụng `use strict` hay còn gọi là "sử dụng nghiêm ngặt", bạn có thể đảm bảo rằng bạn không vô tình khai báo biến toàn cục (Global variable).
- Chúng ta chưa bao giờ khai báo biến age, và vì chúng ta sử dụng `use strict` nên chương trình sẽ ném ra một lỗi reference error.
- Nếu chúng tôi không sử dụng `use strict`, chương trình sẽ hoạt động và trả về giá trị 21 vì biến age lúc này là biến toàn cục.
</details>

## 21. Tác dụng của eval là gì trong Javascript?

```js
const sum = eval("10*10+5");
```

- `105`
- `"105"`
- `TypeError`
- `"10*10+5"`

<details><summary>Đáp án</summary>

- **Đáp án: A**
- eval đánh giá các mã được truyền dưới dạng một chuỗi. Nếu đó là một biểu thức, như trong trường hợp này, nó sẽ đánh giá biểu thức.
- Biểu thức là 10 \* 10 + 5 vì thế kết quả sẽ trả về số 105.
</details>

## 22. Dữ liệu được lưu trữ trong sessionStorage bị xóa khi nào?

```js
sessionStorage.setItem("cool_secret", 123);
```

- `Mãi Mãi, các dữ liệu không bị mất`
- `Khi người dùng đóng tab`
- `Khi người đóng lại toàn bộ trình duyệt, không chỉ là đóng tab`
- `Khi người dùng dùng tắt máy tính của họ`

<details><summary>Đáp án</summary>

- **Đáp án: B**
- Dữ liệu được lưu trữ trong `sessionStorage` sẽ bị xóa sau khi đóng tab.
- Nếu bạn sử dụng `localStorage`, dữ liệu sẽ ở đó mãi mãi, trừ khi `localStorage.clear()` được gọi.
</details>

## 23. Khai báo nhiều biến cùng tên trong cùng phạm vi có được không?

```js
var num = 8;
var num = 10;

console.log(num);
```

- `8`
- `10`
- `SyntaxError`
- `ReferenceError`

<details><summary>Đáp án</summary>

- **Đáp án: B**
- Với từ khóa `var`, bạn có thể khai báo nhiều biến có cùng tên. Biến được khai báo sau cùng sẽ giữ giá trị mới nhất.
- Nhưng bạn không thể làm thế với `let` hoặc `const` vì chúng nằm trong block-scoped.
</details>

## 24. Oject key được coi là kiểu gì?

```js
const obj = { 1: "a", 2: "b", 3: "c" };
const set = new Set([1, 2, 3, 4, 5]);

obj.hasOwnProperty("1");
obj.hasOwnProperty(1);
set.has("1");
set.has(1);
```

- `false true false true`
- `false true true true`
- `true true false true`
- `true true true true`

<details><summary>Đáp án</summary>

- **Đáp án: C**
- Tất cả các object key (trừ symbol) đều được coi là chuỗi, ngay cả khi bạn không tự gõ nó như là một chuỗi.
- Đây là lý do tại sao obj.hasOwnProperty ('1') cũng trả về true
- Tuy nhiên, nó không hoạt động với set, không có "1" trong set dẫn đến set.has('1') trả về false.
- Nó có kiểu số là 1, vì thế set.has(1) trả về true
</details>

## 25. 2 key cùng tên trong object sẽ gây ra vấn đề gì?

```js
const obj = { a: "one", b: "two", a: "three" };
console.log(obj);
```

- `{ a: "one", b: "two" }`
- `{ b: "two", a: "three" }`
- `{ a: "three", b: "two" }`
- `SyntaxError`

<details><summary>Đáp án</summary>

- **Đáp án: C**
- Nếu bạn có hai key có cùng tên, key sẽ được thay thế. Nó vẫn sẽ ở vị trí đầu tiên, nhưng với giá trị được chỉ định cuối cùng.
</details>

## 26. Có phải Javascript global execution context tạo ra 2 thứ: global object và từ khóa "this"?

<details><summary>Đáp án</summary>

- **Đáp án: Đúng**
- Về cơ bản thì bối cảnh thực thi toàn cầu (Global execution context) giúp bạn có thể truy cập ở mọi nơi trong code của bạn.
</details>

## 27. Tác dụng của continue là gì?

```js
for (let i = 1; i < 5; i++) {
  if (i === 3) continue;
  console.log(i);
}
```

- `1 2`
- `1 2 3`
- `1 2 4`
- `1 3 4`

<details><summary>Đáp án</summary>

- **Đáp án: C**
- Câu lệnh continue bỏ qua một lần lặp nếu một điều kiện nhất định trả về true.
</details>

## 28. Có thể thêm thuộc tính vào một string hay không?

```js
String.prototype.giveLydiaPizza = () => {
  return "Just give Lydia pizza already!";
};

const name = "Lydia";

name.giveLydiaPizza();
```

- `"Just give Lydia pizza already!"`
- `TypeError: not a function`
- `SyntaxError`
- `undefined`

<details><summary>Đáp án</summary>

- **Đáp án: A**
- String là một buitl-in constructor, vì thế chúng ta có thể thêm các thuộc tính vào. Mình chỉ thêm một phương thức vào nguyên mẫu của nó.
- Các chuỗi nguyên thủy được tự động chuyển đổi thành một string object, được tạo bởi prototype function.
- Vì vậy, tất cả các string (string object) đều có quyền truy cập vào phương thức đó!
</details>

## 29. Đặt object làm key thì chúng sẽ hoạt động như thế nào?

```js
const a = {};
const b = { key: "b" };
const c = { key: "c" };

a[b] = 123;
a[c] = 456;

console.log(a[b]);
```

- `123`
- `456`
- `undefined`
- `ReferenceError`

<details><summary>Đáp án</summary>

- **Đáp án: B**
- Các Object key được tự động chuyển thành chuỗi. Ví chúng ta đang cố gắng đặt một object làm key cho object a, với giá trị 123.
- Tuy nhiên, khi chúng ta xâu chuỗi một object, nó sẽ trở thành "[Object object]". Vì vậy, những gì chúng ta đang nói với Javascript ở đây đó là a["Object object"] = 123.
- Sau đó chúng ta lại làm lại tương tự một lần nữa.
- c là một object khác mà chúng ta đang ngầm xâu chuỗi. Vì vậy, a["Object object"] = 456
- Sau đó chúng ta log a[b], thực sự là a["Object object"]. Và chúng ta đã đặt nó bằng 456, vậy kết quả nhận được là 456.
</details>

## 30. Tìm hiểu một chút về stack trong Javascript.

```js
const foo = () => console.log("First");
const bar = () => setTimeout(() => console.log("Second"));
const baz = () => console.log("Third");

bar();
foo();
baz();
```

- `First Second Third`
- `First Third Second`
- `Second First Third`
- `Second Third First`

<details><summary>Đáp án</summary>

- **Đáp án: B**
- Chúng ta có một hàm setTimeout và gọi nó trước. Tuy nhiên, nó đã được log cuối cùng.
- Điều này là do trong các trình duyệt, chúng ta không chỉ có runtime engine, chúng ta cũng có một thứ gọi là WebAPI. WebAPI cung cấp cho chúng ta hàm setTimeout để bắt đầu, và ví dụ như DOM.
- Sau khi callback được đẩy lên WebAPI, chính hàm setTimeout (nhưng không phải là callback!) Được bật ra khỏi stack.
- Bây giờ, foo được gọi và 'First' đang được log.
- foo được bật ra khỏi stack, và baz được gọi. 'Third' được log
- WebAPI không thể chờ đẩy nhiệm vụ vào vào stack bất cứ khi nào nó sẵn sàng. Thay vào đó, nó đẩy hàm callback đến một thứ gọi là hàng đợi (queue).
- Đây là nơi một vòng lặp sự kiện bắt đầu hoạt động. Một vòng lặp sự kiện nhìn vào stack và nhiệm vụ trong hàng đợi. Nếu stack trống, nó sẽ lấy nhiệm vụ đầu tiên trên hàng đợi và đẩy nó lên stack.
- Lúc này bar được gọi, 'Second' được log và nó bật ra khỏi stack.
</details>

## 31. Envet.target gì xảy ra khi click vào button?

```js
<div onclick="console.log('first div')">
  <div onclick="console.log('second div')">
    <button onclick="console.log('button')">Click!</button>
  </div>
</div>
```

- `first div`
- `second div`
- `button`
- `Một mảng của tất cả các div lồng nhau`

<details><summary>Đáp án</summary>

- **Đáp án: C**
- Phần tử lồng nhau sâu nhất gây ra sự kiện là mục tiêu của sự kiện. Bạn có thể ngừng bubbling bằng event.stopPropagation.
</details>

## 32. Khi bấm vào đoạn văn bản thì log ra cái gì?

```js
<div onclick="console.log('div')">
  <p onclick="console.log('p')">Click here!</p>
</div>
```

- `p div`
- `div p`
- `p`
- `div`

<details><summary>Đáp án</summary>

- **Đáp án: A**
- Nếu chúng ta click vào p, chúng ta sẽ thấy hai bản ghi: p và div.
- Trong quá trình truyền bá sự kiện, có 3 giai đoạn: bắt giữ (capturing), nhắm mục tiêu (target) và sủi bọt (bubbling).
- Theo mặc định, các trình xử lý sự kiện (event handlers) được thực thi trong giai đoạn sủi bọt (trừ khi bạn đặt useCapture thành true). Nó đi từ phần tử lồng sâu nhất ra bên ngoài.
</details>

## 33. Sự khác nhau của hàm call và hàm bind là gì?

```js
const person = { name: "Lydia" };

function sayHi(age) {
  console.log(`${this.name} is ${age}`);
}

sayHi.call(person, 21);
sayHi.bind(person, 21);
```

- `undefined is 21 Lydia is 21`
- `function function`
- `Lydia is 21 Lydia is 21`
- `Lydia is 21 function`

<details><summary>Đáp án</summary>

- **Đáp án: D**
- Đối với với cả hai trường hợp, chúng ta có thể truyền object mà chúng ta muốn this tham chiếu đến. Tuy nhiên, .call được thực thi ngay lập tức!
- Bạn có thể hiểu hàm call là: Gọi hàm và cho phép bạn truyền vào một object và các đối số phân cách nhau bởi dấu phẩy.
- VD: function.call(thisArg, arg1, arg2, ...)
- Còn hàm bind là: Trả về một hàm số mới, cho phép bạn truyền vào một object và các đối số phân cách nhau bởi dấu phẩy.
- VD: var newFunction = fun.bind(thisArg[, arg1[, arg2[, ...]]])
- Vì .bind trả về một copy của function, nhưng với một bối cảnh bị ràng buộc (bound context)! Nó không được thực thi ngay lập tức.
</details>

## 34. typeof sayHi() là gì?

```js
function sayHi() {
  return (() => 0)();
}

typeof sayHi();
```

- `"object"`
- `"number"`
- `"function"`
- `"undefined"`

<details><summary>Đáp án</summary>

- **Đáp án: B**
- Hàm sayHi trả về giá trị trả về của hàm được gọi ngay lập tức (IIFE). Hàm này trả về 0, là loại 'number'.
- FYI: Có 7 built-in types: null, undefined, boolean, number, string, object and symbol. "function" không phải là một kiểu, nhưng vì các hàm là các object nên nó thuộc loại object.
</details>

## 35. Giá trị nào dưới đây là falsy?

```js
0;
new Number(0);
("");
(" ");
new Boolean(false);
undefined;
```

- `0, '', undefined`
- `0, new Number(0), '', new Boolean(false), undefined`
- `0, '', new Boolean(false), undefined`
- `All of them are falsy`

<details><summary>Đáp án</summary>

- **Đáp án: A**
- Như đã liệt kê 6 falsy ở câu hỏi số 4. Ngoài 6 falsy value này thì các loại khác đều là truthy value.
- Hàm constructor giống như new Number và new Boolean đều là truthy value.
</details>

## 36. Kiểu của Kiểu của nummber trả về kết quả gì?

```js
console.log(typeof typeof 1);
```

- `"number"`
- `"string"`
- `"object"`
- `"undefined"`

<details><summary>Đáp án</summary>

- **Đáp án: B**
- typeof 1 trả về "number". Do đó, typeof "number" trả về "string"
</details>

## 37. Set giá trị vượt quá độ dài của mảng trong Javascript thì xảy ra vấn đề gì?

```js
const numbers = [1, 2, 3];
numbers[10] = 11;
console.log(numbers);
```

- `[1, 2, 3, 7 x null, 11]`
- `[1, 2, 3, 11]`
- `[1, 2, 3, 7 x empty, 11]`
- `SyntaxError`

<details><summary>Đáp án</summary>

- **Đáp án: C**
- Trong Java thì điều này không được phép. Nhưng trong Javascript thì khi bạn set một giá trị có index vượt quá độ dài của mảng, JavaScript sẽ tạo ra một thứ gọi là 'empty'.
- Chúng thực sự có giá trị undefined, nhưng bạn sẽ thấy như là: [1, 2, 3, 7 x empty, 11]
- tùy thuộc vào nơi bạn chạy nó (nó khác nhau đối với các trình duyệt, node, v.v.)
</details>

## 38. Chương trình Javascript sau trả về kết quả gì?

```js
(() => {
  let x, y;
  try {
    throw new Error();
  } catch (x) {
    (x = 1), (y = 2);
    console.log(x);
  }
  console.log(x);
  console.log(y);
})();
```

- `1 undefined 2`
- `undefined undefined undefined`
- `1 1 2`
- `1 undefined undefined`

<details><summary>Đáp án</summary>

- **Đáp án: A**
- Khối catch nhận đối số x. Biến này không giống như x khi chúng ta truyền đối số. Biến x này là block-scoped.
- Sau đó, chúng ta đặt biến block-scoped này bằng 1 và đặt giá trị của biến y. Bây giờ, chúng ta log biến block-scoped x, kết quả bằng 1.
- Bên ngoài khối catch, x vẫn là undefined và y là 2. Khi chúng ta muốn console.log(x) bên ngoài khối catch, nó trả về undefined và y trả về 2.
</details>

## 39. Mọi thứ trong JavaScript đều là gì?

```js
A: primitive or object
B: function or object
C: only objects
D: number or object
```

<details><summary>Đáp án</summary>

- **Đáp án: A**
- JavaScript chỉ có các kiểu primitive (Kiểu nguyên thủy) và objects.
- Kiểu nguyên thủy bao gồm: boolean, null, undefined, bigint, number, string, and symbol
- Điều khác biệt giữa primitive với object là primitive không có bất kỳ thuộc tính hay phương thức nào. Tuy nhiên, bạn cần lưu ý rằng 'foo'.toUpperCase() ước tính là 'FOO' và không dẫn đến TypeError.
- Điều này là do khi bạn cố gắng truy cập một thuộc tính hoặc phương thức trên một kiểu primitive như string, JavaScript sẽ bao bọc đối tượng bằng một wrapper classes.
- Và sau đó ngay lập tức loại bỏ wrapper sau khi biểu thức được ước tính. Tất cả các primitive ngoại trừ null và undefined đều có hành vi này.
</details>

## 40. Bạn đã hiểu về reduce chưa?

```js
[
  [0, 1],
  [2, 3],
].reduce(
  (acc, cur) => {
    return acc.concat(cur);
  },
  [1, 2]
);
```

- `[0, 1, 2, 3, 1, 2]`
- `[6, 1, 2]`
- `[1, 2, 0, 1, 2, 3]`
- `[1, 2, 6]`

<details><summary>Đáp án</summary>

- **Đáp án: C**
- [1, 2] là giá trị ban đầu của chúng ta. Đây là giá trị chúng ta bắt đầu và giá trị đầu tiên của acc.
- Trong vòng đầu tiên, acc là [1,2] và cur là [0, 1]. Chúng ta ghép chúng lại, kết quả là [1, 2, 0, 1].
- Sau đó, [1, 2, 0, 1] là acc và [2, 3] là cur. Chúng ta nối chúng và nhận được kết quả [1, 2, 0, 1, 2, 3].
- Nếu bạn chưa hiểu rõ, mình kiến nghị bạn nên tìm hiểu thêm về reduce trong JavaScript.
- Nó là một phương thức rất thường sử dụng khi bạn học React.js
</details>

## 41. Câu hỏi về toán tử !! tính chất truthy và falsy của dữ liệu.

```js
!!null;
!!"";
!!1;
```

- `false true false`
- `false false true`
- `false true true`
- `true true false`

<details><summary>Đáp án</summary>

- **Đáp án: B**
- null là falsy. !null trả về true. !true trả về false.
- "" là falsy. !"" trả về true. !true trả về false.
- 1 là truthy. !1 trả về false. !false trả về true
</details>

## 42. Phương thức setInterval trả về cái gì?

```js
setInterval(() => console.log("Hi"), 1000);
```

- `Một id duy nhất`
- `Số mili giây được chỉ định`
- `passed function`
- `undefined`

<details><summary>Đáp án</summary>

- **Đáp án: A**
- Nó trả về một id duy nhất. Id này có thể được sử dụng để xóa khoảng nghỉ đó với hàm `clearInterval()`
</details>

## 43. Cách từng ký tự trong chuỗi đơn giản, ngắn gọn như thế nào?

```js
[..."Lydia"];
```

- `["L", "y", "d", "i", "a"]`
- `["Lydia"]`
- `[[], "Lydia"]`
- `[["L", "y", "d", "i", "a"]]`

<details><summary>Đáp án</summary>

- **Đáp án: A**
- Một chuỗi là được ghép từ các ký tự. Toán tử spread ánh xạ mỗi ký tự thành một phần tử
- Note: Nếu không sử dụng toán tử Spread. Bạn cũng có thể tách ký tự bằng phương thức split
</details>


## 44. Tìm hiểu new Promise

```js
console.log(1)

setTimeout(() => console.log(2), 0)

new Promise(resolve => {
  console.log(3)
  resolve()
}).then(() => console.log(4))

console.log(5)
```
Thứ tự log sẽ như nào?
<details><summary>Đáp án</summary>

```js
console.log(1)
console.log(3)
console.log(5)
console.log(4)
console.log(2)
```
- log 1 xuất ra đầu tiên
- log 2 là `setTimeout` với thời gian là `0ms` nên func callback sẽ được đẩy luôn vào queue đợi (macro queue) nên sẽ xuất hiện sau
- khi khởi tạo `new Promise` và truyền vào func thì bên trong func sẽ được gọi luôn nên log 3 sẽ ra tiếp theo sau đó run tham số `resolve()`
- kết quả của `new Promise` trả về 1 Promise, các func callback sẽ được đẩy vào queue đợi (micro queue) nên log 4 sẽ xuất hiện sau
- sau đó log 5
- sau khi quét qua toàn bộ file, call stack rỗng thì mới đến lượt `event loop` đẩy các func callback từ queue vào
- tại thời điểm này thì queue đã có 2 cái đợi ở micro và macro, micro được ưu tiên hơn log 4 sẽ xuất hiện
- tiếp theo là log 2
</details>
---

Tham khảo bài viết gốc: https://niithanoi.edu.vn/43-cau-hoi-javascript-nang-cao.html
