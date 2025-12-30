# Звіт з лабораторної роботи 6. Робота з СУБД MongoDB та реалізація операцій

**Вознюк Ілля**
**Група: ІПЗ-31**
**Дата виконання:** 30 грудня 2025 року
**Варіант:** 1

## Мета роботи

Освоїти принципи роботи з NoSQL документо-орієнтованою базою даних MongoDB, навчитися виконувати базові операції створення, читання, оновлення та видалення документів, опанувати механізми запитів та індексування, розвинути розуміння відмінностей між реляційним та документним підходами до зберігання даних.

## Виконання роботи

### Рівень 1. 


#### Крок 3. Створення бази даних та колекцій



**Список студентів з курсами та оцінками:**

// Перемикання на нову базу даних (створюється автоматично)
use library

// Створення колекцій з валідацією
db.createCollection("books", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["title", "author", "publication_year"],
      properties: {
        title: { bsonType: "string" },
        author: { bsonType: "string" },
        publication_year: { bsonType: "int", minimum: 1000 },
        pages: { bsonType: "int", minimum: 1 },
        genres: { bsonType: "array", items: { bsonType: "string" } },
        available: { bsonType: "bool" }
      }
    }
  }
});

db.createCollection("authors");
db.createCollection("users");


<img width="577" height="573" alt="image" src="https://github.com/user-attachments/assets/93b3e334-5358-4f1c-bb5c-dfaad75f0eda" />




#### Крок 4. Заповнення колекцій даними


**1. Вставка авторів:**
db.authors.insertMany([
  {
    name: "Тарас Шевченко",
    birth_year: 1814,
    death_year: 1861,
    nationality: "український",
	biography: "Найвідоміший український поет, письменник, художник"
  },
  {
    name: "Леся Українка",
    birth_year: 1871,
    death_year: 1913,
    nationality: "український",
    biography: "Видатна українська письменниця, поетеса"
  },
  {
    name: "Іван Франко",
    birth_year: 1856,
    death_year: 1916,
    nationality: "український",
    biography: "Письменник, поет, публіцист, перекладач"
  },
  {
    name: "Михайло Коцюбинський",
    birth_year: 1864,
    death_year: 1913,
    nationality: "український",
    biography: "Український письменник-модерніст"
  },
  {
    name: "Панас Мирний",
    birth_year: 1849,
    death_year: 1920,
    nationality: "український",
    biography: "Український письменник-реаліст"
  }
]);

<img width="792" height="735" alt="image" src="https://github.com/user-attachments/assets/62c4b6bd-3a27-4d29-bb5f-4fa9949cadf3" />




**2. Вставка книг:**
db.books.insertMany([
  {
    title: "Кобзар",
    author: "Тарас Шевченко",
    publication_year: 1840,
    pages: 238,
    isbn: "978-966-03-4421-6",
    genres: ["поезія", "романтизм"],
    available: true,
    description: "Збірка віршів Тараса Шевченка"
  },
  {
    title: "Лісова пісня",
    author: "Леся Українка",
    publication_year: 1911,
    pages: 128,
    isbn: "978-966-03-3918-2",
    genres: ["драма", "феєрія", "романтизм"],
    available: true,
    description: "Драма-феєрія за мотивами українського фольклору"
  },
  {
    title: "Захар Беркут",
    author: "Іван Франко",
    publication_year: 1883,
    pages: 256,
    isbn: "978-966-03-6127-5",
    genres: ["роман", "історична проза"],
    available: false,
    description: "Історичний роман про боротьбу карпатських горян"
  },
  {
    title: "Тіні забутих предків",
    author: "Михайло Коцюбинський",
    publication_year: 1911,
    pages: 96,
    isbn: "978-966-03-5214-3",
    genres: ["повість", "романтизм", "модернізм"],
    available: true,
    description: "Поетична повість про кохання в Карпатах"
  },
  {
    title: "Хіба ревуть воли, як ясла повні",
    author: "Панас Мирний",
    publication_year: 1880,
    pages: 384,
    isbn: "978-966-03-4856-6",
    genres: ["роман", "реалізм"],
    available: true,
    description: "Соціально-побутовий роман про життя селян"
  },
  {
    title: "Каменярі",
    author: "Іван Франко",
    publication_year: 1878,
    pages: 64,
    isbn: "978-966-03-7841-9",
    genres: ["поезія", "реалізм"],
    available: true,
    description: "Збірка соціальної поезії"
  },
  {
    title: "Конотопська відьма",
    author: "Григорій Квітка-Основ'яненко",
    publication_year: 1837,
    pages: 112,
    isbn: "978-966-03-3762-1",
    genres: ["повість", "містика"],
    available: true,
    description: "Перша українська романтична повість"
  },
  {
    title: "Маруся",
    author: "Григорій Квітка-Основ'яненко",
    publication_year: 1834,
    pages: 96,
    isbn: "978-966-03-4125-3",
    genres: ["повість", "сентименталізм"],
    available: false,
    description: "Сентиментальна повість про нещасливе кохання"
  },
  {
    title: "Камінний хрест",
    author: "Василь Стефаник",
    publication_year: 1900,
    pages: 48,
    isbn: "978-966-03-5427-7",
    genres: ["новела", "реалізм"],
    available: true,
    description: "Збірка новел про важке життя селян"
  },
  {
    title: "Intermezzo",
    author: "Михайло Коцюбинський",
    publication_year: 1909,
    pages: 72,
    isbn: "978-966-03-6843-4",
    genres: ["повість", "імпресіонізм"],
    available: true,
    description: "Імпресіоністична повість про кохання"
  }
]);

<img width="661" height="732" alt="image" src="https://github.com/user-attachments/assets/ef08f302-87cd-4b73-a547-7fced0149418" />




**3. Вставка користувачів:**
db.users.insertMany([
  {
    username: "ivan_petrov",
    email: "ivan.petrov@email.com",
    registration_date: new Date("2024-01-15"),
    borrowed_books: ["Кобзар", "Тіні забутих предків"],
    status: "active"
  },
  {
    username: "maria_kovalenko",
    email: "maria.kovalenko@email.com",
    registration_date: new Date("2024-02-20"),
    borrowed_books: ["Лісова пісня"],
    status: "active"
  },
  {
    username: "oleksandr_sydorov",
    email: "alex.sydorov@email.com",
    registration_date: new Date("2024-03-10"),
    borrowed_books: [],
    status: "active"
  },
  {
    username: "anna_melnyk",
    email: "anna.melnyk@email.com",
    registration_date: new Date("2023-11-05"),
    borrowed_books: ["Хіба ревуть воли, як ясла повні"],
    status: "active"
  },
  {
    username: "dmytro_bondar",
    email: "dmytro.bondar@email.com",
    registration_date: new Date("2024-01-28"),
    borrowed_books: ["Захар Беркут", "Каменярі"],
    status: "suspended"
  }
]);


<img width="766" height="745" alt="image" src="https://github.com/user-attachments/assets/844b0961-5e61-41bb-b305-a76667b09f7b" />


#### Крок 5. Виконання базових CRUD операцій

** Операції читання з різними умовами: **

// Пошук всіх доступних книг
db.books.find({ available: true });

// Пошук книг конкретного автора
db.books.find({ author: "Іван Франко" });

// Пошук книг з кількістю сторінок більше 100
db.books.find({ pages: { $gt: 100 } });

// Пошук книг певного жанру
db.books.find({ genres: "поезія" });

// Пошук з логічними операторами
db.books.find({
  $or: [
    { author: "Тарас Шевченко" },
    { genres: "романтизм" }
  ]
});

// Пошук за шаблоном
db.books.find({ title: { $regex: /Лісова/i } });

// Пошук з проєкцією конкретних полів
db.books.find(
  { available: true },
  { title: 1, author: 1, pages: 1, _id: 0 }
);

// Пошук з сортуванням
db.books.find().sort({ publication_year: -1 });

// Пошук з обмеженням результатів
db.books.find({ genres: "роман" }).limit(3);


<img width="739" height="819" alt="image" src="https://github.com/user-attachments/assets/81dd1476-7422-4e60-8590-a67bfa5bcfb3" />


** Операції оновлення: **

// Оновлення доступності книги
db.books.updateOne(
  { title: "Кобзар" },
  { $set: { available: false } }
);

// Додавання рейтингу до книги
db.books.updateOne(
  { title: "Лісова пісня" },
  {
    $push: {
      ratings: {
        user: "ivan_petrov",
        score: 5,
        date: new Date()
      }
    }
  }
);

// Оновлення багатьох документів
db.books.updateMany(
  { publication_year: { $lt: 1900 } },
  { $set: { category: "класика XIX століття" } }
);

// Інкрементування лічильника
db.books.updateOne(
  { title: "Тіні забутих предків" },
  { $inc: { borrowed_count: 1 } }
);

// Видалення поля з документа
db.books.updateOne(
  { title: "Захар Беркут" },
  { $unset: { temporary_note: "" } }
);


<img width="471" height="793" alt="image" src="https://github.com/user-attachments/assets/c59ea0cd-979e-4a76-8c6f-1f118c00e255" />


** Операції видалення: **

// Видалення одного документа
db.books.deleteOne({ title: "Застарілий довідник" });

// Видалення за умовою
db.books.deleteMany({ available: false, publication_year: { $lt: 1850 } });

// Видалення користувача з призупиненим статусом
db.users.deleteOne({ status: "suspended", username: "old_user" });


<img width="699" height="324" alt="image" src="https://github.com/user-attachments/assets/c2c0514a-79f0-4653-a239-2cb302aacbad" />


#### Крок 6. Створення індексів

// Простий індекс для швидкого пошуку за назвою
db.books.createIndex({ title: 1 });

// Складений індекс для пошуку за автором та роком
db.books.createIndex({ author: 1, publication_year: -1 });

// Унікальний індекс для ISBN
db.books.createIndex({ isbn: 1 }, { unique: true });

// Перевірка списку індексів
db.books.getIndexes();

// Аналіз використання індексів
db.books.find({ author: "Тарас Шевченко" }).explain("executionStats");


<img width="685" height="976" alt="image" src="https://github.com/user-attachments/assets/0c3de0f9-b9b2-44f9-9b15-a3005b947371" />
<img width="663" height="951" alt="image" src="https://github.com/user-attachments/assets/d7a8f626-6a4b-46cc-9dc9-22c1cddf8992" />
<img width="669" height="938" alt="image" src="https://github.com/user-attachments/assets/563469c5-fa7a-48f5-8764-a3848e81efca" />
<img width="680" height="700" alt="image" src="https://github.com/user-attachments/assets/148bcfb0-0e75-4fa4-aa5a-39f12c0e9467" />





** Порівняйте час виконання запиту до та після створення індексів: **

// Без індексу
db.books.find({ author: "Іван Франко" }).explain("executionStats");

// Створення індексу
db.books.createIndex({ author: 1 });

// З індексом
db.books.find({ author: "Іван Франко" }).explain("executionStats");

<img width="672" height="886" alt="image" src="https://github.com/user-attachments/assets/00bd086d-9928-4a29-84b8-bbef1e2d3d45" />
<img width="676" height="915" alt="image" src="https://github.com/user-attachments/assets/3ac0c226-94de-4e80-8aeb-a0e8bdaad7d8" />
<img width="618" height="957" alt="image" src="https://github.com/user-attachments/assets/34793487-b51c-4212-acc9-a411f89c6e04" />
<img width="681" height="961" alt="image" src="https://github.com/user-attachments/assets/318fe3fe-071a-461c-bb85-c46ccc858521" />
<img width="615" height="202" alt="image" src="https://github.com/user-attachments/assets/3ef4a5ca-c363-46d9-a235-1246f8a77cd4" />




### Рівень 2. 
#### Крок 7. Агрегаційні запити

// Підрахунок книг за жанрами
db.books.aggregate([
  { $unwind: "$genres" },
  { $group: { _id: "$genres", count: { $sum: 1 } } },
  { $sort: { count: -1 } }
]);

// Середня кількість сторінок за автором
db.books.aggregate([
  {
    $group: {
      _id: "$author",
      avg_pages: { $avg: "$pages" },
      total_books: { $sum: 1 }
    }
  },
  { $sort: { avg_pages: -1 } }
]);

// Статистика по століттях
db.books.aggregate([
  {
    $project: {
      title: 1,
      author: 1,
      century: { $ceil: { $divide: ["$publication_year", 100] } }
    }
  },
  {
    $group: {
      _id: "$century",
      books_count: { $sum: 1 },
      authors: { $addToSet: "$author" }
    }
  }
]);

// Топ користувачів за кількістю взятих книг
db.users.aggregate([
  {
    $project: {
      username: 1,
      email: 1,
      borrowed_count: { $size: "$borrowed_books" }
    }
  },
  { $sort: { borrowed_count: -1 } },
  { $limit: 5 }
]);


<img width="637" height="755" alt="image" src="https://github.com/user-attachments/assets/476b9925-7d67-4a48-80ce-eabebc9d0a68" />



#### Крок 8. Об'єднання колекцій з $lookup

// Спочатку додамо author_id до книг
db.authors.find().forEach(function(author) {
  db.books.updateMany(
    { author: author.name },
    { $set: { author_id: author._id } }
  );
});

// Тепер виконаємо lookup
db.books.aggregate([
  {
    $lookup: {
      from: "authors",
      localField: "author_id",
      foreignField: "_id",
      as: "author_details"
    }
  },
  { $unwind: "$author_details" },
  {
    $project: {
      title: 1,
      "author_details.name": 1,
      "author_details.birth_year": 1,
      publication_year: 1,
      pages: 1
    }
  }
]);


<img width="597" height="720" alt="image" src="https://github.com/user-attachments/assets/2beb8a69-3361-47b3-9b35-5615d18b7be6" />
<img width="584" height="748" alt="image" src="https://github.com/user-attachments/assets/dc973b56-8141-43ff-8da1-a6d1f7e94da0" />




#### Крок 9. Текстовий пошук

// Створення текстового індексу
db.books.createIndex({ title: "text", description: "text" });

<img width="521" height="51" alt="image" src="https://github.com/user-attachments/assets/5120ef26-8ab2-4d6d-9ec0-e9a303ce4dba" />


// Пошук за ключовими словами
db.books.find({ $text: { $search: "кохання Карпати" } });

<img width="603" height="826" alt="image" src="https://github.com/user-attachments/assets/3b0daddc-e408-4b25-a3d2-9dc5a6efa61f" />


// Пошук з оцінкою релевантності
db.books.find(
  { $text: { $search: "український письменник" } },
  { score: { $meta: "textScore" } }
).sort({ score: { $meta: "textScore" } });

<img width="458" height="132" alt="image" src="https://github.com/user-attachments/assets/cfc22ff9-8065-4790-ab0b-f1ccefadd16c" />





## Висновки

У ході роботи було успішно встановлено MongoDB  та налаштовано підключення через mongosh і MongoDB Compass. Створено базу даних library з колекціями, наповненими даними, та застосовано валідацію схем. Продемонстровано повний цикл роботи з MongoDB: CRUD-операції, створення й аналіз індексів, агрегаційні запити, об’єднання колекцій через $lookup і текстовий пошук. Результати показують, що MongoDB ефективно підтримує роботу з напівструктурованими даними, забезпечує гнучкий аналіз інформації та суттєве підвищення продуктивності завдяки індексації.






## Контрольні запитання

	1. Відмінності між реляційними та документо-орієнтованими БД

Реляційні БД (MySQL, PostgreSQL):
Дані зберігаються у таблицях з чіткою схемою
Зв’язки реалізуються через JOIN
Підходять для складних транзакцій і строгих вимог до цілісності
Документо-орієнтовані БД (MongoDB):
Дані зберігаються у вигляді документів (JSON/BSON)
Гнучка або відсутня жорстка схема
Висока масштабованість і швидка робота з вкладеними даними
Коли використовувати:
Реляційні — фінансові системи, бухгалтерія, ERP
Документо-орієнтовані — веб-застосунки, мікросервіси, Big Data

	2. BSON і відмінність від JSON

BSON (Binary JSON) — бінарний формат зберігання документів у MongoDB.
Відмінності та переваги:
Підтримує більше типів даних (Date, ObjectId, Binary)
Швидше парситься
Зберігає типи даних без втрат
Ефективніший за розміром для обчислень

	3. Денормалізація в NoSQL

Денормалізація — збереження пов’язаних даних разом в одному документі.
Переваги:
Менше запитів до БД
Вища продуктивність читання
Недоліки:
Дублювання даних
Складність оновлення

	4. Агрегаційний pipeline в MongoDB

Pipeline — це послідовність етапів обробки даних.
Основні етапи:
$match — фільтрація
$project — вибір/перетворення полів
$group — агрегація
$sort — сортування
$lookup — об’єднання колекцій

	5. Індекси в MongoDB

Індекси прискорюють пошук і сортування даних.
Вплив:
Значно зменшують час виконання запитів
Збільшують використання пам’яті
Типи індексів:
Однопольові
Складені
Унікальні
Текстові
Геопросторові
TTL

	6. updateOne, updateMany, replaceOne

updateOne — оновлює один документ
updateMany — оновлює всі документи, що відповідають умові
replaceOne — повністю замінює документ
Використання:
update — часткові зміни
replace — повна заміна структури

	7. Валідація схеми в MongoDB

Валідація схеми — перевірка структури документів при записі.
Реалізація:
Через $jsonSchema
Визначає обов’язкові поля та типи даних

	8. Зв’язки між документами

Підходи:
Вбудовані документи — швидше читання, менше зв’язків
Посилання (reference) — менше дублювання, гнучкі зв’язки
Вибір залежить від частоти доступу та розміру даних.

	9. Текстовий індекс

Текстовий індекс дозволяє виконувати повнотекстовий пошук.
Можливості:
Пошук за ключовими словами
Ранжування результатів за релевантністю

	10. Масштабування MongoDB

Реплікація:
Кілька копій даних
Висока доступність і відмовостійкість
Sharding:
Розподіл даних між серверами
Горизонтальне масштабування
Підсумок: MongoDB легко масштабується та підходить для високонавантажених систем.
