# OOP-Characteristic


there 4 characteristic of OOP :

- Abstraction
- Encapsulation
- Inheritance
- Polymorphism

## Abstraction > Specifying what to do but not know how to do it.

contoh:
pada tombol remote kita tau kegunaan dari masing - masing tombolnya tapi kita tak tau bagaimana proses bekerjanya.

Abstraction example :

```js
"use strict";

class Car {
  constructor(model, price) {
    this.model = model;
    this.price = price;
  }

  applyDiscount() {
    let today = new Date();
    let date = today.getDate();
    if (date % 8 === 0) {
      let discount = this.price * 0.1;
      this.price -= discount;
      console.log(`Discount applied!`);
    } else {
      console.log(`Failed apply discount`);
    }
  }
}

const myCar = new Car("BMW", 1000_000_000);

console.log(myCar);
myCar.applyDiscount();

//disini pemanggilan applyDiscount dianggap abstraction, karena kita tidak perlu tau bagaimana proses didalam method applyDiscountnya hanya perlu memanggil methodnya saja
//setelah object di instantiate

//other example

class Employee {
  constructor(name, position, salary) {
    this.name = name;
    this.position = position;
    this.salary = salary;
  }

  finalSalary() {
    const date = new Date();
    const bonus = date.getMonth() === 11 ? this.salary : 0; // Jan start from 0
    const tunjangan = this.position === "Manajer" ? 200_000 : 150_000;

    return this.salary + bonus + tunjangan;
  }
}

const emp1 = new Employee("Budi", "Manajer", 50_000_000);
const finalSalary = emp1.finalSalary(); // diluar sini tidak bisa merubah-rubah method finalSalary secara langsung
console.log(finalSalary);
```

## Encapsulation > Hiding the internals or implementation details of the class from the user through an object's methods (membungkus)

example of encapsulation :

```js
"use strict";

class BankAccount {
  #accountNumber; // this way to define a private property
  #balance;
  constructor(name, accountNumber, balance, gender) {
    this.name = name;
    this.#accountNumber = accountNumber; // then assign the private property
    this.#balance = balance;
    this.gender = gender;
  }

  showInfo() {
    let info = `Name: ${this.name}, Acc No : ${this.#accountNumber}, balance: ${
      this.#balance
    }, gender: ${this.gender} `;
    return info;
  }

  //this special method call accessor to get private property (getter)
  get account() {
    let value = this.#accountNumber.toString().split("").slice(0, -3);
    return `${value.join("")}XXX`;
  }

  get nameWithSalutation() {
    let tittle = "";
    if (this.gender === "male") {
      tittle = "Mr.";
    } else if (this.gender === "female") {
      tittle = "Mrs.";
    } else {
      tittle = "Mr. / Mrs.";
    }
    return `${tittle} ${this.name}`;
  }

  get balance() {
    return this.#balance.toLocaleString("id-ID", {
      currency: "IDR",
      style: "currency",
    }); // return value of balance with style IDR
  }

  // this special method call mutator to set private property with new value (setter)
  set balance(value) {
    if (value < 0) {
      throw new Error(`balance is insufficient`);
    }
    this.#balance = value;
  }
}

const myAcc = new BankAccount("Shiro", 66652142166, 500_000_000, "male");
console.log(myAcc.showInfo());

// u cant access the private property directly outside the class BankAccount, but if u wanna get the value from private property
// u can use getter to access it

// to call getter like this why
console.log(myAcc.account);
// console.log(myAcc.balance)

// you can use getter to call modified property value too in class ex :
console.log(myAcc.nameWithSalutation); // this will calling accessor / getter from class

// to change value private property you can use setter
console.log(myAcc.balance, "<<< before setter"); // 500_000_000
myAcc.balance = 9_000_000_000;
console.log(myAcc.balance, "<<< after setter"); // 9_000_000_000

// you can use setter to build validation
myAcc.balance -= 500_000_000_000;
console.log(myAcc.balance);
```

## Inheritance > a Class can inherit traits form that Class to child Class

for the example we can see :

```js
"use strict";

class Smartphone {
  constructor(brand, cpu, memory) {
    this.brand = brand;
    this.cpu = cpu;
    this.memory = memory;
  }

  takePicture() {
    console.log(`Muehehehe`);
  }
}

//using extends mean Android will get every property and method from the Smartphone this is we call inheritance
class Android extends Smartphone {}

class Iphone extends Smartphone {}

const myPhone = new Android(550, 8, 12);
console.log(myPhone.takePicture()); // we can call the Smartphone method whenever in we instance from Android cause Android inheritance every property and method from Smartphone.
```

## Polymorphism > after we know bout inheritance, sometimes we need to change the value of property or we need to add new method in the child class from parents class..

we can use this way to change it for the example :

```js
"use strict";

class Smartphone {
  #noImei
  constructor(brand, cpu, memory, noImei) {
    this.brand = brand;
    this.cpu = cpu;
    this.memory = memory;
    this.noImei = noImei;
  }

  get noImei(){
    return this.#noImei
  }
  set noImei(value){
    this.#noImei = value
  }

  takePicture() {
    console.log(`Muehehehe`);
  }
}

//using extends mean Android will get every property and method from the Smartphone this is we call inheritance
class Android extends Smartphone {
  constructor(cpu, memory, camera) {
    super("Android", cpu, memory);
    this.camera = camera;
  }
  get noImei(){
    return super.noImei.slice(0, 4) + '*******'
  }
  set noImei(value){
    return super.noImei = value
  }
  // the different here called the polymorphism cause we change the value of brand in super with 'Android' and we add trackGps
  trackGps() {
    console.log(`looking for the map`);
  }
}

class Iphone extends Smartphone {
  constructor(cpu, memory, camera) {
    super("Iphone", cpu, memory);
    this.camera = camera;
  }
}

const myPhone = new Android(550, 8, 12);
console.log(myPhone.takePicture());
```
