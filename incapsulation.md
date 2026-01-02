# 🔒 Incapsulyatsiya: Ma'lumotlarni Himoya Qilish San'ati

## Kirish

Incapsulyatsiya (Encapsulation) - bu ob'ektga yo'naltirilgan dasturlashning eng muhim tamoyillaridan biri bo'lib, ma'lumotlar va ularni boshqaruvchi metodlarni bitta modulda birlashtirishni anglatadi. Bu konsepsiya ma'lumotlarni tashqi ta'sirlardan himoya qilish va dastur kodining xavfsizligini oshirish uchun ishlatiladi.

---

## 📚 Incapsulyatsiya Nima?

Incapsulyatsiya so'zining o'zi "kapsula" (qobiq) so'zidan kelib chiqqan. Xuddi dori kapsulasi ichidagi moddani tashqi muhitdan himoya qilganidek, dasturlashdagi incapsulyatsiya ham ma'lumotlarni keraksiz o'zgarishlardan saqlaydi.

### Asosiy Maqsadlar:

- **Ma'lumotlarni yashirish** - Ichki tuzilmani tashqi dunyodan yashirish
- **Xavfsizlik** - Ma'lumotlarga nazorat ostida kirish
- **Modullilik** - Kodning mustaqil qismlarini yaratish
- **O'zgarishlarga chidamlilik** - Ichki o'zgarishlar tashqi kodga ta'sir qilmaydi

---

## 🎯 Nima Uchun Incapsulyatsiya Muhim?

### 1. **Ma'lumotlar Xavfsizligi**

```python
# NOTO'G'RI - Ma'lumotlarga to'g'ridan-to'g'ri kirish
class BankAccount:
    def __init__(self):
        self.balance = 0  # Hamma o'zgartirishi mumkin!

account = BankAccount()
account.balance = -1000  # 😱 Muammo!

# TO'G'RI - Incapsulyatsiya bilan
class BankAccount:
    def __init__(self):
        self.__balance = 0  # Private o'zgaruvchi
    
    def deposit(self, amount):
        if amount > 0:
            self.__balance += amount
            return True
        return False
    
    def get_balance(self):
        return self.__balance

account = BankAccount()
account.deposit(1000)  # ✅ Faqat to'g'ri yo'l bilan
```

### 2. **Kodning Tushunarli va Boshqariluvchan Bo'lishi**

Incapsulyatsiya yordamida kod ancha tartibli va tushunarliroq bo'ladi:

```java
// TO'G'RI incapsulyatsiya
public class User {
    private String email;
    private String password;
    
    // Emailni validatsiya bilan o'rnatish
    public void setEmail(String email) {
        if (isValidEmail(email)) {
            this.email = email;
        } else {
            throw new IllegalArgumentException("Noto'g'ri email!");
        }
    }
    
    public String getEmail() {
        return email;
    }
    
    private boolean isValidEmail(String email) {
        return email.contains("@");
    }
}
```

### 3. **Kodni O'zgartirishda Qulaylik**

Ichki implementatsiyani o'zgartirish tashqi kodga ta'sir qilmaydi:

```javascript
class Temperature {
    constructor(celsius) {
        this._celsius = celsius;
    }
    
    // Ichki saqlash usuli o'zgardi, lekin API o'zgarmadi
    getFahrenheit() {
        return (this._celsius * 9/5) + 32;
    }
    
    getKelvin() {
        return this._celsius + 273.15;
    }
}
```

---

## 🔑 Kirish Modifikatorlari

### Public (Ochiq)
- Hamma joydan kirish mumkin
- Interfeys qismi

### Private (Yopiq)
- Faqat class ichidan kirish mumkin
- Ichki implementatsiya

### Protected (Himoyalangan)
- Class va uning merosxo'rlaridan kirish mumkin

### Turli Tillarda Misollar:

```cpp
// C++
class Example {
public:
    int publicVar;      // Hamma joydan
    
protected:
    int protectedVar;   // Meros orqali
    
private:
    int privateVar;     // Faqat ichkarida
};
```

```python
# Python (konventsiya asosida)
class Example:
    def __init__(self):
        self.public = 10           # Public
        self._protected = 20       # Protected (konventsiya)
        self.__private = 30        # Private (name mangling)
```

```csharp
// C#
public class Example {
    public int PublicVar { get; set; }
    protected int ProtectedVar { get; set; }
    private int PrivateVar { get; set; }
}
```

---

## 💡 Real Hayotdan Misollar

### Misol 1: Avtomobil

```python
class Car:
    def __init__(self, brand, model):
        self.__brand = brand
        self.__model = model
        self.__engine_status = False
        self.__speed = 0
        self.__fuel = 100
    
    def start_engine(self):
        """Dvigatelni ishga tushirish"""
        if self.__fuel > 0:
            self.__engine_status = True
            return "Dvigatel ishga tushdi ✅"
        return "Yoqilg'i yetarli emas ❌"
    
    def accelerate(self, amount):
        """Tezlikni oshirish"""
        if not self.__engine_status:
            return "Avval dvigatelni ishga tushiring!"
        
        if self.__fuel > 0:
            self.__speed += amount
            self.__fuel -= amount * 0.1
            return f"Tezlik: {self.__speed} km/soat"
        return "Yoqilg'i tugadi!"
    
    def get_info(self):
        """Avtomobil ma'lumotlari"""
        return {
            'brand': self.__brand,
            'model': self.__model,
            'speed': self.__speed,
            'fuel': round(self.__fuel, 2),
            'engine': 'Ishlamoqda' if self.__engine_status else 'O\'chiq'
        }

# Foydalanish
car = Car("Tesla", "Model 3")
print(car.start_engine())
print(car.accelerate(50))
print(car.get_info())
```

### Misol 2: Bank Hisobi

```java
public class BankAccount {
    private String accountNumber;
    private String ownerName;
    private double balance;
    private List<String> transactionHistory;
    
    public BankAccount(String accountNumber, String ownerName) {
        this.accountNumber = accountNumber;
        this.ownerName = ownerName;
        this.balance = 0.0;
        this.transactionHistory = new ArrayList<>();
    }
    
    public boolean deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            addTransaction("Kirim: +" + amount);
            return true;
        }
        return false;
    }
    
    public boolean withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            addTransaction("Chiqim: -" + amount);
            return true;
        }
        return false;
    }
    
    public double getBalance() {
        return balance;
    }
    
    private void addTransaction(String transaction) {
        String timestamp = LocalDateTime.now().toString();
        transactionHistory.add(timestamp + " - " + transaction);
    }
    
    public List<String> getTransactionHistory() {
        return new ArrayList<>(transactionHistory); // Copy qaytarish
    }
}
```

### Misol 3: Foydalanuvchi Profili

```javascript
class UserProfile {
    #email;
    #passwordHash;
    #loginAttempts = 0;
    #isLocked = false;
    
    constructor(email, password) {
        this.#email = email;
        this.#passwordHash = this.#hashPassword(password);
    }
    
    #hashPassword(password) {
        // Parolni shifrlash (soddalashtirilgan)
        return password.split('').reverse().join('');
    }
    
    login(email, password) {
        if (this.#isLocked) {
            return "Hisob bloklangan! Administrator bilan bog'laning.";
        }
        
        if (email === this.#email && 
            this.#hashPassword(password) === this.#passwordHash) {
            this.#loginAttempts = 0;
            return "Kirish muvaffaqiyatli! ✅";
        }
        
        this.#loginAttempts++;
        if (this.#loginAttempts >= 3) {
            this.#isLocked = true;
            return "Hisob bloklandi! ⛔";
        }
        
        return `Noto'g'ri ma'lumotlar. Qolgan urinishlar: ${3 - this.#loginAttempts}`;
    }
    
    getEmail() {
        return this.#email;
    }
    
    changePassword(oldPassword, newPassword) {
        if (this.#hashPassword(oldPassword) === this.#passwordHash) {
            this.#passwordHash = this.#hashPassword(newPassword);
            return "Parol o'zgartirildi ✅";
        }
        return "Noto'g'ri eski parol ❌";
    }
}

// Foydalanish
const user = new UserProfile("user@example.com", "secret123");
console.log(user.login("user@example.com", "wrong"));
console.log(user.login("user@example.com", "secret123"));
```

---

## ⚠️ Incapsulyatsiyaning Afzalliklari va Kamchiliklari

### ✅ Afzalliklari:

1. **Xavfsizlik**: Ma'lumotlar himoyalangan
2. **Moslashuvchanlik**: Ichki o'zgarishlar oson amalga oshiriladi
3. **Boshqaruvchanlik**: Kod tartibli va tushunarli
4. **Qayta foydalanish**: Modullar mustaqil ishlaydi
5. **Testing**: Har bir qism alohida test qilinadi
6. **Validatsiya**: Ma'lumotlar to'g'riligini tekshirish oson

### ❌ Kamchiliklari:

1. **Qo'shimcha kod**: Getter/Setter metodlar kerak
2. **Murakkablik**: Yangi dasturchilar uchun qiyin
3. **Performance**: Ba'zi hollarda sekinroq (juda kam)

---

## 🎓 Best Practices (Eng Yaxshi Amaliyotlar)

### 1. Barcha maydonlarni private qiling

```java
// YAXSHI
public class Product {
    private String name;
    private double price;
    
    public double getPrice() { return price; }
    public void setPrice(double price) {
        if (price > 0) this.price = price;
    }
}
```

### 2. Getter/Setter mantiqiy bo'lsin

```python
class Rectangle:
    def __init__(self, width, height):
        self.__width = width
        self.__height = height
    
    @property
    def area(self):
        # Hisoblangan qiymat, setter kerak emas
        return self.__width * self.__height
    
    @property
    def width(self):
        return self.__width
    
    @width.setter
    def width(self, value):
        if value > 0:
            self.__width = value
```

### 3. Immutable ob'ektlar yarating

```javascript
class Point {
    #x;
    #y;
    
    constructor(x, y) {
        this.#x = x;
        this.#y = y;
    }
    
    getX() { return this.#x; }
    getY() { return this.#y; }
    
    // O'zgartirish o'rniga yangi ob'ekt qaytaring
    move(dx, dy) {
        return new Point(this.#x + dx, this.#y + dy);
    }
}
```

### 4. Copy-lardan foydalaning

```java
public class Team {
    private List<String> members;
    
    public Team() {
        members = new ArrayList<>();
    }
    
    // Nusxa qaytarish - original himoyalangan
    public List<String> getMembers() {
        return new ArrayList<>(members);
    }
}
```

---

## 🧪 Amaliy Mashq

Quyidagi vazifani incapsulyatsiya tamoyiliga rioya qilgan holda amalga oshiring:

**Vazifa**: Kitobxona tizimi uchun `Book` va `Library` classlarini yarating.

**Talablar**:
- Book: title, author, ISBN, available (mavjudligi)
- Library: kitoblar to'plami, qo'shish, olish, qaytarish metodlari
- Barcha maydonlar private bo'lsin
- Ma'lumotlar validatsiyasi bo'lsin
- ISBN formati tekshirilsin

```python
class Book:
    def __init__(self, title, author, isbn):
        self.__title = title
        self.__author = author
        self.__isbn = self.__validate_isbn(isbn)
        self.__available = True
    
    def __validate_isbn(self, isbn):
        # ISBN-13 formatini tekshirish (soddalashtirilgan)
        if len(isbn) == 13 and isbn.isdigit():
            return isbn
        raise ValueError("Noto'g'ri ISBN formati!")
    
    def checkout(self):
        if self.__available:
            self.__available = False
            return True
        return False
    
    def return_book(self):
        self.__available = True
    
    def is_available(self):
        return self.__available
    
    def get_info(self):
        return f"{self.__title} - {self.__author} ({'Mavjud' if self.__available else 'Olingan'})"


class Library:
    def __init__(self, name):
        self.__name = name
        self.__books = []
    
    def add_book(self, book):
        if isinstance(book, Book):
            self.__books.append(book)
            return True
        return False
    
    def find_book(self, title):
        for book in self.__books:
            if title.lower() in book.get_info().lower():
                return book
        return None
    
    def checkout_book(self, title):
        book = self.find_book(title)
        if book and book.checkout():
            return f"✅ Kitob olindi: {book.get_info()}"
        return "❌ Kitob mavjud emas"
    
    def list_available(self):
        available = [b.get_info() for b in self.__books if b.is_available()]
        return available if available else ["Mavjud kitoblar yo'q"]
```

---

## 🎯 Xulosa

Incapsulyatsiya - bu dasturlashning fundamental printsipi bo'lib:

- 🔒 **Ma'lumotlarni himoya qiladi**
- 🧩 **Kodni modulli qiladi**
- 🔧 **O'zgartirishni osonlashtiradi**
- 🎯 **Xatoliklarni kamaytiradi**
- 📚 **Kodni tushunarli qiladi**

Har doim ma'lumotlarni yashiring va ular bilan ishlash uchun aniq interfeys taqdim eting. Bu sizning kodingizni xavfsizroq, tushunarli va professional qiladi!

---

## 📖 Qo'shimcha O'rganish Uchun

- OOP ning boshqa prinsiplari: Inheritance, Polymorphism, Abstraction
- Design Patterns: Singleton, Factory, Observer
- SOLID prinsiplari
- Clean Code tamoyillari

**Esda tuting**: Yaxshi kod - bu boshqalar oson o'qiy oladigan va tushunadigan kod! 🚀
