# my
CREATE DATABASE IF NOT EXISTS rayskiy_sad;
USE rayskiy_sad;

-- 1. Таблица категорий цветов
CREATE TABLE categories (
    category_id INT PRIMARY KEY AUTO_INCREMENT,
    category_name VARCHAR(100) NOT NULL,
    description TEXT,
    sort_order INT DEFAULT 0,
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- 2. Таблица товаров (цветов)
CREATE TABLE products (
    product_id INT PRIMARY KEY AUTO_INCREMENT,
    category_id INT NOT NULL,
    name VARCHAR(200) NOT NULL,
    price DECIMAL(10, 2) NOT NULL,
    stock_quantity INT DEFAULT 0,
    description TEXT,
    image_url VARCHAR(500),
    is_available BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (category_id) REFERENCES categories(category_id)
);

-- 3. Таблица клиентов
CREATE TABLE customers (
    customer_id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(100) NOT NULL,
    last_name VARCHAR(100) NOT NULL,
    email VARCHAR(150) UNIQUE NOT NULL,
    phone VARCHAR(20),
    address TEXT,
    registration_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    loyalty_points INT DEFAULT 0,
    is_active BOOLEAN DEFAULT TRUE
);

-- 4. Таблица заказов
CREATE TABLE orders (
    order_id INT PRIMARY KEY AUTO_INCREMENT,
    customer_id INT NOT NULL,
    order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    total_amount DECIMAL(10, 2) NOT NULL,
    status ENUM('pending', 'processing', 'delivered', 'cancelled') DEFAULT 'pending',
    delivery_address TEXT NOT NULL,
    payment_method ENUM('cash', 'card', 'online') DEFAULT 'card',
    delivery_date DATE,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);

-- 5. Таблица деталей заказов
CREATE TABLE order_details (
    detail_id INT PRIMARY KEY AUTO_INCREMENT,
    order_id INT NOT NULL,
    product_id INT NOT NULL,
    quantity INT NOT NULL,
    price_at_time DECIMAL(10, 2) NOT NULL,
    discount DECIMAL(5, 2) DEFAULT 0,
    total_price DECIMAL(10, 2) GENERATED ALWAYS AS ((price_at_time * quantity) - discount) STORED,
    FOREIGN KEY (order_id) REFERENCES orders(order_id),
    FOREIGN KEY (product_id) REFERENCES products(product_id)
);

-- Вставка данных (по 15 записей в каждую таблицу)

-- Категории цветов
INSERT INTO categories (category_name, description, sort_order) VALUES
('Розы', 'Классические розы всех оттенков', 1),
('Тюльпаны', 'Весенние тюльпаны различных цветов', 2),
('Лилии', 'Изящные лилии с нежным ароматом', 3),
('Хризантемы', 'Осенние хризантемы для долгого цветения', 4),
('Орхидеи', 'Экзотические орхидеи премиум-класса', 5),
('Пионы', 'Пышные пионы для романтических букетов', 6),
('Герберы', 'Яркие герберы для создания настроения', 7),
('Ирисы', 'Элегантные ирисы для стильных букетов', 8),
('Гвоздики', 'Аккуратные гвоздики с длительным сроком стояния', 9),
('Астры', 'Нежные астры для осенних композиций', 10),
('Ромашки', 'Простые и милые ромашки для полевых букетов', 11),
('Гортензии', 'Пышные гортензии для свадебных букетов', 12),
('Фрезии', 'Ароматные фрезии нежных оттенков', 13),
('Альстромерии', 'Экзотические альстромерии для оригинальных букетов', 14),
('Эустомы', 'Изысканные эустомы для элегантных композиций', 15);

-- Товары (цветы)
INSERT INTO products (category_id, name, price, stock_quantity, description) VALUES
(1, 'Роза красная', 250.00, 50, 'Красная роза, символ любви и страсти'),
(1, 'Роза белая', 270.00, 45, 'Белая роза, символ чистоты и невинности'),
(1, 'Роза розовая', 260.00, 48, 'Нежная розовая роза для романтического подарка'),
(2, 'Тюльпан красный', 120.00, 100, 'Красный тюльпан на длинной ножке'),
(2, 'Тюльпан желтый', 120.00, 95, 'Яркий желтый тюльпан для создания настроения'),
(3, 'Лилия белая', 300.00, 30, 'Крупная белая лилия с сильным ароматом'),
(3, 'Лилия оранжевая', 320.00, 28, 'Оранжевая лилия для ярких букетов'),
(4, 'Хризантема кустовая', 180.00, 60, 'Кустовая хризантема, цветет более 2 недель'),
(5, 'Орхидея фаленопсис', 800.00, 20, 'Экзотическая орхидея в горшке премиум-качества'),
(5, 'Орхидея цимбидиум', 950.00, 15, 'Крупноцветковая орхидея для особых случаев'),
(6, 'Пион розовый', 350.00, 25, 'Пышный розовый пион с нежным ароматом'),
(6, 'Пион белый', 370.00, 22, 'Снежно-белый пион для свадебных букетов'),
(7, 'Гербера красная', 150.00, 80, 'Яркая красная гербера'),
(7, 'Гербера желтая', 150.00, 78, 'Солнечная желтая гербера'),
(8, 'Ирис синий', 200.00, 40, 'Синий ирис элегантной формы');

-- Клиенты
INSERT INTO customers (first_name, last_name, email, phone, address, loyalty_points) VALUES
('Иван', 'Петров', 'ivan.petrov@email.com', '+7-901-123-45-67', 'г. Москва, ул. Тверская, д.15, кв.34', 150),
('Мария', 'Иванова', 'maria.ivanova@email.com', '+7-902-234-56-78', 'г. Москва, ул. Арбат, д.7, кв.12', 320),
('Алексей', 'Смирнов', 'alexey.smirnov@email.com', '+7-903-345-67-89', 'г. Санкт-Петербург, Невский пр., д.50, кв.8', 75),
('Елена', 'Кузнецова', 'elena.kuznetsova@email.com', '+7-904-456-78-90', 'г. Москва, Ленинский пр., д.30, кв.150', 500),
('Дмитрий', 'Попов', 'dmitry.popov@email.com', '+7-905-567-89-01', 'г. Казань, ул. Баумана, д.10, кв.45', 210),
('Анна', 'Васильева', 'anna.vasilieva@email.com', '+7-906-678-90-12', 'г. Новосибирск, ул. Ленина, д.25, кв.67', 90),
('Сергей', 'Михайлов', 'sergey.mikhailov@email.com', '+7-907-789-01-23', 'г. Екатеринбург, ул. Малышева, д.8, кв.23', 445),
('Татьяна', 'Федорова', 'tatiana.fedorova@email.com', '+7-908-890-12-34', 'г. Москва, ул. Покровка, д.5, кв.89', 180),
('Павел', 'Соколов', 'pavel.sokolov@email.com', '+7-909-901-23-45', 'г. Краснодар, ул. Красная, д.12, кв.56', 60),
('Ольга', 'Морозова', 'olga.morozova@email.com', '+7-910-012-34-56', 'г. Сочи, ул. Навагинская, д.3, кв.11', 310),
('Константин', 'Волков', 'konstantin.volkov@email.com', '+7-911-123-45-67', 'г. Ростов-на-Дону, пр. Буденновский, д.20, кв.44', 135),
('Наталья', 'Зайцева', 'natalia.zaitseva@email.com', '+7-912-234-56-78', 'г. Нижний Новгород, ул. Большая Покровская, д.15, кв.7', 250),
('Максим', 'Соловьев', 'maxim.soloviev@email.com', '+7-913-345-67-89', 'г. Самара, ул. Ленинская, д.8, кв.33', 95),
('Юлия', 'Козлова', 'yulia.kozlova@email.com', '+7-914-456-78-90', 'г. Уфа, пр. Октября, д.45, кв.78', 420),
('Владимир', 'Новиков', 'vladimir.novikov@email.com', '+7-915-567-89-01', 'г. Воронеж, ул. Плехановская, д.10, кв.99', 175);

-- Заказы
INSERT INTO orders (customer_id, total_amount, status, delivery_address, payment_method, delivery_date) VALUES
(1, 750.00, 'delivered', 'г. Москва, ул. Тверская, д.15, кв.34', 'card', '2024-01-15'),
(2, 1200.00, 'delivered', 'г. Москва, ул. Арбат, д.7, кв.12', 'online', '2024-01-18'),
(3, 390.00, 'processing', 'г. Санкт-Петербург, Невский пр., д.50, кв.8', 'card', '2024-01-20'),
(4, 2500.00, 'delivered', 'г. Москва, Ленинский пр., д.30, кв.150', 'card', '2024-01-10'),
(5, 600.00, 'cancelled', 'г. Казань, ул. Баумана, д.10, кв.45', 'online', '2024-01-12'),
(6, 890.00, 'delivered', 'г. Новосибирск, ул. Ленина, д.25, кв.67', 'cash', '2024-01-22'),
(7, 1450.00, 'pending', 'г. Екатеринбург, ул. Малышева, д.8, кв.23', 'card', '2024-01-25'),
(8, 520.00, 'delivered', 'г. Москва, ул. Покровка, д.5, кв.89', 'online', '2024-01-08'),
(9, 980.00, 'processing', 'г. Краснодар, ул. Красная, д.12, кв.56', 'card', '2024-01-19'),
(10, 2100.00, 'delivered', 'г. Сочи, ул. Навагинская, д.3, кв.11', 'card', '2024-01-14'),
(11, 430.00, 'delivered', 'г. Ростов-на-Дону, пр. Буденновский, д.20, кв.44', 'cash', '2024-01-17'),
(12, 1670.00, 'processing', 'г. Нижний Новгород, ул. Большая Покровская, д.15, кв.7', 'online', '2024-01-23'),
(13, 550.00, 'pending', 'г. Самара, ул. Ленинская, д.8, кв.33', 'card', '2024-01-24'),
(14, 1850.00, 'delivered', 'г. Уфа, пр. Октября, д.45, кв.78', 'card', '2024-01-09'),
(15, 720.00, 'delivered', 'г. Воронеж, ул. Плехановская, д.10, кв.99', 'online', '2024-01-16');

-- Детали заказов
INSERT INTO order_details (order_id, product_id, quantity, price_at_time, discount) VALUES
(1, 1, 3, 250.00, 0),
(1, 2, 2, 270.00, 15.00),
(2, 5, 10, 120.00, 0),
(2, 3, 1, 260.00, 10.00),
(3, 8, 2, 180.00, 0),
(4, 9, 2, 800.00, 100.00),
(4, 10, 1, 950.00, 50.00),
(5, 4, 5, 120.00, 0),
(6, 7, 2, 320.00, 0),
(6, 6, 1, 300.00, 30.00),
(7, 11, 3, 350.00, 0),
(7, 12, 1, 370.00, 10.00),
(8, 14, 3, 150.00, 0),
(8, 13, 1, 150.00, 20.00),
(9, 1, 2, 250.00, 0),
(9, 15, 2, 200.00, 20.00),
(10, 9, 2, 800.00, 0),
(10, 10, 1, 950.00, 200.00),
(11, 4, 3, 120.00, 0),
(11, 5, 1, 120.00, 0),
(12, 2, 4, 270.00, 0),
(12, 11, 2, 350.00, 30.00),
(13, 8, 3, 180.00, 20.00),
(14, 9, 1, 800.00, 50.00),
(14, 10, 1, 950.00, 0),
(15, 6, 2, 300.00, 0),
(15, 7, 1, 320.00, 20.00),
(1, 5, 2, 120.00, 0),
(2, 8, 3, 180.00, 15.00),
(3, 13, 2, 150.00, 0);

-- Дополнительные запросы для проверки
-- Показать все заказы с информацией о клиентах
SELECT o.order_id, c.first_name, c.last_name, o.order_date, o.total_amount, o.status
FROM orders o
JOIN customers c ON o.customer_id = c.id
ORDER BY o.order_date DESC;

-- Показать популярные товары
SELECT p.name, SUM(od.quantity) as total_sold
FROM order_details od
JOIN products p ON od.product_id = p.product_id
GROUP BY p.product_id
ORDER BY total_sold DESC
LIMIT 5;





 ЗАПРОС 1. Выборка всех доступных цветов с ценой меньше 300 рублей
-- =====================================================
-- Описание: Показывает все цветы в наличии дешевле 300 руб, отсортированные по цене
SELECT 
    product_id AS 'ID товара',
    name AS 'Название',
    price AS 'Цена',
    stock_quantity AS 'Количество на складе'
FROM Products
WHERE is_available = 1 AND price < 300
ORDER BY price ASC;


-- =====================================================
-- ЗАПРОС 2. Выборка активных клиентов с баллами лояльности более 200
-- =====================================================
-- Описание: Список постоянных клиентов с высокими баллами лояльности
SELECT 
    first_name AS 'Имя',
    last_name AS 'Фамилия',
    email AS 'Email',
    loyalty_points AS 'Баллы',
    registration_date AS 'Дата регистрации'
FROM Customers
WHERE is_active = 1 AND loyalty_points > 200
ORDER BY loyalty_points DESC;


-- =====================================================
-- ЗАПРОС 3. Выборка заказов со статусом "доставлен"
-- =====================================================
-- Описание: Получение списка всех выполненных заказов
SELECT 
    order_id AS 'Номер заказа',
    order_date AS 'Дата заказа',
    total_amount AS 'Сумма',
    delivery_date AS 'Дата доставки'
FROM Orders
WHERE status = 'delivered'
ORDER BY order_date DESC;


-- =====================================================
-- ЗАПРОС 4. Выборка популярных товаров (остаток менее 30 штук)
-- =====================================================
-- Описание: Товары, которые скоро закончатся на складе
SELECT 
    name AS 'Название',
    stock_quantity AS 'Остаток',
    price AS 'Цена'
FROM Products
WHERE stock_quantity < 30 AND is_available = 1
ORDER BY stock_quantity ASC;


-- =====================================================
-- ЗАПРОС 5. Выборка уникальных методов оплаты из заказов
-- =====================================================
-- Описание: Какие способы оплаты используют клиенты
SELECT DISTINCT 
    payment_method AS 'Способ оплаты',
    COUNT(*) AS 'Количество использований'
FROM Orders
GROUP BY payment_method
ORDER BY COUNT(*) DESC;


-- =====================================================
-- ЗАПРОС 6. Запрос с JOIN: Информация о заказах с данными клиентов
-- =====================================================
-- Описание: Детальная информация по каждому заказу с именем клиента
SELECT 
    o.order_id AS 'Номер заказа',
    c.first_name + ' ' + c.last_name AS 'Клиент',
    c.phone AS 'Телефон',
    o.order_date AS 'Дата заказа',
    o.total_amount AS 'Сумма',
    o.status AS 'Статус'
FROM Orders o
INNER JOIN Customers c ON o.customer_id = c.customer_id
WHERE o.order_date >= DATEADD(month, -1, GETDATE())
ORDER BY o.order_date DESC;


-- =====================================================
-- ЗАПРОС 7. Запрос с JOIN: Детали заказов с названиями товаров
-- =====================================================
-- Описание: Какие товары в каких количествах заказывали
SELECT 
    od.detail_id AS 'ID детали',
    o.order_id AS 'Заказ №',
    p.name AS 'Товар',
    od.quantity AS 'Количество',
    od.price_at_time AS 'Цена',
    od.discount AS 'Скидка',
    od.total_price AS 'Итого'
FROM OrderDetails od
INNER JOIN Orders o ON od.order_id = o.order_id
INNER JOIN Products p ON od.product_id = p.product_id
WHERE o.status = 'delivered'
ORDER BY o.order_date DESC;


-- =====================================================
-- ЗАПРОС 8. JOIN трех таблиц: Полная информация по заказам
-- =====================================================
-- Описание: Все заказы с клиентами, товарами и суммами
SELECT 
    c.last_name + ' ' + c.first_name AS 'Клиент',
    o.order_id AS 'Заказ №',
    o.order_date AS 'Дата',
    p.name AS 'Товар',
    od.quantity AS 'Кол-во',
    od.total_price AS 'Сумма за позицию',
    o.total_amount AS 'Общая сумма заказа'
FROM Customers c
INNER JOIN Orders o ON c.customer_id = o.customer_id
INNER JOIN OrderDetails od ON o.order_id = od.order_id
INNER JOIN Products p ON od.product_id = p.product_id
ORDER BY o.order_date DESC, o.order_id;


-- =====================================================
-- ЗАПРОС 9. Агрегация с GROUP BY: Статистика по категориям товаров
-- =====================================================
-- Описание: Количество товаров, мин/макс/средняя цена по категориям
SELECT 
    cat.category_name AS 'Категория',
    COUNT(p.product_id) AS 'Кол-во товаров',
    MIN(p.price) AS 'Мин. цена',
    MAX(p.price) AS 'Макс. цена',
    AVG(p.price) AS 'Средняя цена',
    SUM(p.stock_quantity) AS 'Всего на складе'
FROM Categories cat
LEFT JOIN Products p ON cat.category_id = p.category_id
WHERE cat.is_active = 1
GROUP BY cat.category_name
ORDER BY AVG(p.price) DESC;


-- =====================================================
-- ЗАПРОС 10. Агрегация с HAVING: Категории со средним чеком выше 250 руб
-- =====================================================
-- Описание: Категории, где средняя цена цветка превышает 250 рублей
SELECT 
    cat.category_name AS 'Категория',
    COUNT(p.product_id) AS 'Кол-во',
    AVG(p.price) AS 'Средняя цена'
FROM Categories cat
INNER JOIN Products p ON cat.category_id = p.category_id
GROUP BY cat.category_name
HAVING AVG(p.price) > 250
ORDER BY AVG(p.price) DESC;


-- =====================================================
-- ЗАПРОС 11. Сортировка с TOP и оконными функциями
-- =====================================================
-- Описание: Топ-5 самых дорогих товаров с рейтингом
SELECT TOP 5
    name AS 'Название',
    price AS 'Цена',
    RANK() OVER (ORDER BY price DESC) AS 'Место в рейтинге',
    NTILE(4) OVER (ORDER BY price) AS 'Квартиль'
FROM Products
WHERE is_available = 1
ORDER BY price DESC;


-- =====================================================
-- ЗАПРОС 12. Вложенный запрос: Клиенты с заказами выше среднего
-- =====================================================
-- Описание: Клиенты, чья сумма заказов превышает среднюю по всем заказам
SELECT DISTINCT
    c.first_name + ' ' + c.last_name AS 'Клиент',
    c.email AS 'Email',
    (SELECT SUM(total_amount) FROM Orders o2 WHERE o2.customer_id = c.customer_id) AS 'Общая сумма'
FROM Customers c
WHERE EXISTS (
    SELECT 1 FROM Orders o 
    WHERE o.customer_id = c.customer_id
    AND o.total_amount > (SELECT AVG(total_amount) FROM Orders WHERE status = 'delivered')
)
ORDER BY 'Общая сумма' DESC;


-- =====================================================
-- ЗАПРОС 13. Объединение запросов UNION: Сводка по активности
-- =====================================================
-- Описание: Объединение новых клиентов и новых заказов
SELECT 
    'Новый клиент' AS 'Тип события',
    first_name + ' ' + last_name AS 'Название',
    registration_date AS 'Дата',
    email AS 'Детали'
FROM Customers
WHERE registration_date >= DATEADD(month, -3, GETDATE())
UNION ALL
SELECT 
    'Новый заказ',
    'Заказ №' + CAST(order_id AS NVARCHAR),
    order_date,
    'Сумма: ' + CAST(total_amount AS NVARCHAR) + ' руб.'
FROM Orders
WHERE order_date >= DATEADD(month, -1, GETDATE())
ORDER BY 'Дата' DESC;


-- =====================================================
-- ЗАПРОС 14. Функции с датами: Заказы за последний месяц по дням
-- =====================================================
-- Описание: Анализ заказов по дням за последние 30 дней
SELECT 
    CAST(order_date AS DATE) AS 'Дата',
    COUNT(*) AS 'Кол-во заказов',
    SUM(total_amount) AS 'Выручка',
    AVG(total_amount) AS 'Средний чек',
    DATEPART(weekday, order_date) AS 'День недели',
    DATENAME(weekday, order_date) AS 'День недели (название)'
FROM Orders
WHERE order_date >= DATEADD(day, -30, GETDATE())
GROUP BY CAST(order_date AS DATE), DATEPART(weekday, order_date), DATENAME(weekday, order_date)
ORDER BY 'Дата' DESC;


-- =====================================================
-- ЗАПРОС 15. Функции с числами и строками: Форматированный вывод
-- =====================================================
-- Описание: Красивое форматирование информации о товарах
SELECT 
    UPPER(name) AS 'НАЗВАНИЕ',
    '★' + CAST(ROUND(price / 100, 0) AS NVARCHAR) + '★' AS 'Ценовой рейтинг',
    LEFT(description, 50) + '...' AS 'Краткое описание',
    FORMAT(price, 'N2', 'ru-RU') + ' руб.' AS 'Цена',
    REPLICATE('▪', stock_quantity / 10) AS 'Индикатор остатка'
FROM Products
WHERE description IS NOT NULL
ORDER BY price DESC;


-- =====================================================
-- ЗАПРОС 16. Переменные и управляющие конструкции: Динамический анализ
-- =====================================================
-- Описание: Анализ заказов с использованием переменных
DECLARE @MinAmount DECIMAL(10,2) = 500;
DECLARE @MaxAmount DECIMAL(10,2) = 1500;
DECLARE @ReportDate DATETIME = GETDATE();

SELECT 
    order_id AS 'Номер',
    total_amount AS 'Сумма',
    CASE 
        WHEN total_amount < @MinAmount THEN 'Маленький'
        WHEN total_amount BETWEEN @MinAmount AND @MaxAmount THEN 'Средний'
        ELSE 'Крупный'
    END AS 'Тип заказа',
    CASE 
        WHEN status = 'delivered' THEN '✅ Доставлен'
        WHEN status = 'pending' THEN '⏳ Ожидает'
        WHEN status = 'processing' THEN '🔄 В обработке'
        ELSE '❌ Отменен'
    END AS 'Статус',
    @ReportDate AS 'Дата отчета'
FROM Orders
WHERE order_date >= DATEADD(month, -6, @ReportDate)
ORDER BY total_amount DESC;


-- =====================================================
-- ЗАПРОС 17. Хранимая процедура: Добавление нового заказа
-- =====================================================
-- Описание: Процедура для создания нового заказа
CREATE OR ALTER PROCEDURE sp_CreateOrder
    @CustomerID INT,
    @DeliveryAddress NVARCHAR(MAX),
    @PaymentMethod NVARCHAR(20)
AS
BEGIN
    DECLARE @NewOrderID INT;
    
    BEGIN TRY
        BEGIN TRANSACTION;
        
        INSERT INTO Orders (customer_id, total_amount, status, delivery_address, payment_method)
        VALUES (@CustomerID, 0, 'pending', @DeliveryAddress, @PaymentMethod);
        
        SET @NewOrderID = SCOPE_IDENTITY();
        
        SELECT @NewOrderID AS 'Номер созданного заказа';
        
        COMMIT TRANSACTION;
    END TRY
    BEGIN CATCH
        ROLLBACK TRANSACTION;
        SELECT ERROR_MESSAGE() AS 'Ошибка';
    END CATCH
END;
GO

-- Пример вызова процедуры
EXEC sp_CreateOrder @CustomerID = 1, @DeliveryAddress = 'г. Москва, ул. Тверская 15', @PaymentMethod = 'card';


-- =====================================================
-- ЗАПРОС 18. Хранимая процедура с параметрами: Отчет по клиенту
-- =====================================================
-- Описание: Процедура для получения статистики по клиенту
CREATE OR ALTER PROCEDURE sp_GetCustomerStatistics
    @CustomerID INT,
    @StartDate DATE = NULL,
    @EndDate DATE = NULL
AS
BEGIN
    SET @StartDate = ISNULL(@StartDate, '2024-01-01');
    SET @EndDate = ISNULL(@EndDate, GETDATE());
    
    SELECT 
        c.customer_id,
        c.first_name + ' ' + c.last_name AS 'Клиент',
        COUNT(DISTINCT o.order_id) AS 'Всего заказов',
        ISNULL(SUM(o.total_amount), 0) AS 'Сумма всех заказов',
        ISNULL(AVG(o.total_amount), 0) AS 'Средний чек',
        MAX(o.order_date) AS 'Последний заказ',
        MIN(o.order_date) AS 'Первый заказ'
    FROM Customers c
    LEFT JOIN Orders o ON c.customer_id = o.customer_id 
        AND o.order_date BETWEEN @StartDate AND @EndDate
    WHERE c.customer_id = @CustomerID
    GROUP BY c.customer_id, c.first_name, c.last_name;
END;
GO

-- Пример вызова
EXEC sp_GetCustomerStatistics @CustomerID = 2, @StartDate = '2024-01-01';


-- =====================================================
-- ЗАПРОС 19. Триггер: Автообновление суммы заказа
-- =====================================================
-- Описание: Триггер пересчитывает общую сумму заказа при добавлении/изменении деталей
CREATE OR ALTER TRIGGER trg_UpdateOrderTotal
ON OrderDetails
AFTER INSERT, UPDATE, DELETE
AS
BEGIN
    SET NOCOUNT ON;
    
    UPDATE o
    SET o.total_amount = (
        SELECT ISNULL(SUM(od.total_price), 0)
        FROM OrderDetails od
        WHERE od.order_id = o.order_id
    )
    FROM Orders o
    WHERE o.order_id IN (
        SELECT order_id FROM inserted
        UNION
        SELECT order_id FROM deleted
    );
END;
GO

-- Тестирование триггера
INSERT INTO OrderDetails (order_id, product_id, quantity, price_at_time, discount)
VALUES (1, 3, 2, 260, 0);
-- Сумма в Orders обновится автоматически


-- =====================================================
-- ЗАПРОС 20. Триггер: Проверка остатков при заказе
-- =====================================================
-- Описание: Триггер проверяет наличие товаров на складе при добавлении в заказ
CREATE OR ALTER TRIGGER trg_CheckStockBeforeOrder
ON OrderDetails
INSTEAD OF INSERT
AS
BEGIN
    SET NOCOUNT ON;
    
    IF EXISTS (
        SELECT 1
        FROM inserted i
        INNER JOIN Products p ON i.product_id = p.product_id
        WHERE i.quantity > p.stock_quantity
    )
    BEGIN
        RAISERROR('Недостаточно товара на складе!', 16, 1);
        ROLLBACK;
        RETURN;
    END
    
    -- Если все проверки пройдены, вставляем данные
    INSERT INTO OrderDetails (order_id, product_id, quantity, price_at_time, discount)
    SELECT order_id, product_id, quantity, price_at_time, discount
    FROM inserted;
    
    -- Обновляем остатки
    UPDATE p
    SET p.stock_quantity = p.stock_quantity - i.quantity
    FROM Products p
    INNER JOIN inserted i ON p.product_id = i.product_id;
END;
GO


-- =====================================================
-- ЗАПРОС 21. Представление: Активные заказы с клиентами
-- =====================================================
-- Описание: Представление для активных заказов
CREATE OR ALTER VIEW v_ActiveOrders
AS
SELECT 
    o.order_id,
    c.first_name + ' ' + c.last_name AS 'customer_name',
    c.phone,
    o.order_date,
    o.total_amount,
    o.status,
    o.delivery_address,
    DATEDIFF(day, o.order_date, GETDATE()) AS 'days_since_order'
FROM Orders o
INNER JOIN Customers c ON o.customer_id = c.customer_id
WHERE o.status IN ('pending', 'processing')
AND o.order_date >= DATEADD(month, -3, GETDATE());
GO

-- Использование представления
SELECT * FROM v_ActiveOrders ORDER BY days_since_order DESC;


-- =====================================================
-- ЗАПРОС 22. Представление: Топ товаров по продажам
-- =====================================================
-- Описание: Рейтинг самых популярных товаров
CREATE OR ALTER VIEW v_TopProducts
AS
SELECT 
    p.product_id,
    p.name,
    cat.category_name,
    SUM(od.quantity) AS 'total_sold',
    COUNT(DISTINCT od.order_id) AS 'times_ordered',
    AVG(od.price_at_time) AS 'avg_price',
    RANK() OVER (ORDER BY SUM(od.quantity) DESC) AS 'sales_rank'
FROM Products p
INNER JOIN Categories cat ON p.category_id = cat.category_id
INNER JOIN OrderDetails od ON p.product_id = od.product_id
INNER JOIN Orders o ON od.order_id = o.order_id
WHERE o.status = 'delivered'
GROUP BY p.product_id, p.name, cat.category_name;
GO

SELECT TOP 10 * FROM v_TopProducts ORDER BY sales_rank;


-- =====================================================
-- ЗАПРОС 23. Оконная функция: Сравнение с предыдущим заказом (LAG)
-- =====================================================
-- Описание: Для каждого клиента показывает предыдущий заказ
SELECT 
    c.customer_id,
    c.first_name + ' ' + c.last_name AS 'Клиент',
    o.order_id,
    o.order_date,
    o.total_amount,
    LAG(o.total_amount, 1, 0) OVER (PARTITION BY c.customer_id ORDER BY o.order_date) AS 'Сумма предыдущего заказа',
    o.total_amount - LAG(o.total_amount, 1, 0) OVER (PARTITION BY c.customer_id ORDER BY o.order_date) AS 'Разница',
    LEAD(o.order_date, 1) OVER (PARTITION BY c.customer_id ORDER BY o.order_date) AS 'Дата следующего заказа',
    DATEDIFF(day, o.order_date, LEAD(o.order_date, 1) OVER (PARTITION BY c.customer_id ORDER BY o.order_date)) AS 'Дней до следующего'
FROM Orders o
INNER JOIN Customers c ON o.customer_id = c.customer_id
ORDER BY c.customer_id, o.order_date;


-- =====================================================
-- ЗАПРОС 24. Оконная функция: Кумулятивная сумма и скользящее среднее
-- =====================================================
-- Описание: Продажи с накоплением и скользящим средним за 3 дня
SELECT 
    CAST(order_date AS DATE) AS 'Дата',
    COUNT(*) AS 'Заказов',
    SUM(total_amount) AS 'Выручка',
    SUM(SUM(total_amount)) OVER (ORDER BY CAST(order_date AS DATE)) AS 'Кумулятивная выручка',
    AVG(SUM(total_amount)) OVER (ORDER BY CAST(order_date AS DATE) ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) AS 'Скользящее среднее (3 дня)'
FROM Orders
WHERE order_date >= DATEADD(month, -3, GETDATE())
GROUP BY CAST(order_date AS DATE)
ORDER BY 'Дата';


-- =====================================================
-- ЗАПРОС 25. Оконная функция: Ранжирование товаров по категориям
-- =====================================================
-- Описание: Топ-3 товара в каждой категории по продажам
WITH RankedProducts AS (
    SELECT 
        cat.category_name,
        p.name,
        SUM(od.quantity) AS 'total_sold',
        ROW_NUMBER() OVER (PARTITION BY cat.category_id ORDER BY SUM(od.quantity) DESC) AS 'rank_in_category',
        DENSE_RANK() OVER (ORDER BY SUM(od.quantity) DESC) AS 'global_rank'
    FROM Products p
    INNER JOIN Categories cat ON p.category_id = cat.category_id
    INNER JOIN OrderDetails od ON p.product_id = od.product_id
    GROUP BY cat.category_id, cat.category_name, p.name
)
SELECT *
FROM RankedProducts
WHERE rank_in_category <= 3
ORDER BY category_name, rank_in_category;


-- =====================================================
-- ЗАПРОС 26. Сложный запрос с CTE и оконными функциями
-- =====================================================
-- Описание: Анализ клиентов по активности с несколькими окнами
WITH CustomerOrders AS (
    SELECT 
        c.customer_id,
        c.first_name,
        c.last_name,
        COUNT(o.order_id) AS 'total_orders',
        SUM(o.total_amount) AS 'total_spent',
        AVG(o.total_amount) AS 'avg_order',
        MIN(o.order_date) AS 'first_order',
        MAX(o.order_date) AS 'last_order',
        DATEDIFF(day, MAX(o.order_date), GETDATE()) AS 'days_since_last_order'
    FROM Customers c
    LEFT JOIN Orders o ON c.customer_id = o.customer_id
    WHERE o.status = 'delivered' OR o.status IS NULL
    GROUP BY c.customer_id, c.first_name, c.last_name
)
SELECT 
    first_name + ' ' + last_name AS 'Клиент',
    total_orders,
    total_spent,
    avg_order,
    NTILE(4) OVER (ORDER BY total_spent DESC) AS 'сегмент_по_тратам',
    CASE 
        WHEN days_since_last_order <= 30 THEN 'Активный'
        WHEN days_since_last_order <= 90 THEN 'Спящий'
        ELSE 'Ушедший'
    END AS 'статус_активности',
    PERCENT_RANK() OVER (ORDER BY total_spent) * 100 AS 'процентиль_по_тратам'
FROM CustomerOrders
WHERE total_orders > 0
ORDER BY total_spent DESC;


-- =====================================================
-- ЗАПРОС 27. Запрос с PIVOT для анализа по месяцам
-- =====================================================
-- Описание: Выручка по месяцам и способам оплаты (сводная таблица)
SELECT *
FROM (
    SELECT 
        FORMAT(o.order_date, 'yyyy-MM') AS 'Месяц',
        o.payment_method AS 'Спопсоб оплаты',
        o.total_amount
    FROM Orders o
    WHERE o.status = 'delivered'
    AND o.order_date >= DATEADD(year, -1, GETDATE())
) AS SourceData
PIVOT (
    SUM(total_amount)
    FOR payment_method IN ([card], [cash], [online])
) AS PivotTable
ORDER BY Месяц DESC;


-- =====================================================
-- ЗАПРОС 28. Динамический SQL: Поиск по разным полям
-- =====================================================
-- Описание: Универсальный поиск товаров с динамическими условиями
DECLARE @SearchKeyword NVARCHAR(100) = 'роза';
DECLARE @SQL NVARCHAR(MAX);

SET @SQL = '
SELECT 
    product_id,
    name,
    price,
    CASE 
        WHEN name LIKE ''%@SearchKeyword%'' THEN ''Название''
        WHEN description LIKE ''%@SearchKeyword%'' THEN ''Описание''
        ELSE ''Категория''
    END AS ''Найдено в''
FROM Products
WHERE name LIKE ''%' + @SearchKeyword + '%''
   OR description LIKE ''%' + @SearchKeyword + '%''
   OR product_id IN (
        SELECT product_id FROM Products 
        WHERE category_id IN (
            SELECT category_id FROM Categories 
            WHERE category_name LIKE ''%' + @SearchKeyword + '%''
        )
   )
ORDER BY price';

SET @SQL = REPLACE(@SQL, '@SearchKeyword', @SearchKeyword);
EXEC sp_executesql @SQL;


-- =====================================================
-- ЗАПРОС 29. Запрос с рекурсивным CTE: Иерархия категорий
-- =====================================================
-- Описание: Создание древовидной структуры категорий (если бы была иерархия)
-- Добавим parent_category_id для примера
ALTER TABLE Categories ADD parent_category_id INT NULL;

WITH CategoryHierarchy AS (
    -- Anchor: корневые категории
    SELECT 
        category_id,
        category_name,
        0 AS level,
        CAST(category_name AS NVARCHAR(500)) AS path
    FROM Categories
    WHERE parent_category_id IS NULL
    
    UNION ALL
    
    -- Recursive: дочерние категории
    SELECT 
        c.category_id,
        c.category_name,
        ch.level + 1,
        CAST(ch.path + ' -> ' + c.category_name AS NVARCHAR(500))
    FROM Categories c
    INNER JOIN CategoryHierarchy ch ON c.parent_category_id = ch.category_id
)
SELECT 
    REPLICATE('  ', level) + category_name AS 'Дерево категорий',
    level AS 'Уровень',
    path AS 'Полный путь'
FROM CategoryHierarchy
ORDER BY path;


-- =====================================================
-- ЗАПРОС 30. Финальный отчет: Комплексная аналитика магазина
-- =====================================================
-- Описание: Полная сводка по работе магазина для руководства
WITH SalesStats AS (
    SELECT 
        COUNT(DISTINCT order_id) AS total_orders,
        COUNT(DISTINCT customer_id) AS unique_customers,
        SUM(total_amount) AS total_revenue,
        AVG(total_amount) AS avg_order_value,
        MIN(order_date) AS first_order_date,
        MAX(order_date) AS last_order_date
    FROM Orders
    WHERE status = 'delivered'
),
CustomerStats AS (
    SELECT 
        AVG(loyalty_points) AS avg_loyalty_points,
        SUM(CASE WHEN loyalty_points > 500 THEN 1 ELSE 0 END) AS vip_customers
    FROM Customers
    WHERE is_active = 1
),
ProductStats AS (
    SELECT 
        COUNT(*) AS total_products,
        SUM(stock_quantity) AS total_in_stock,
        AVG(price) AS avg_price,
        SUM(CASE WHEN stock_quantity < 10 THEN 1 ELSE 0 END) AS low_stock_products
    FROM Products
    WHERE is_available = 1
)
SELECT 
    '===== ОБЩАЯ СТАТИСТИКА =====' AS 'Показатель',
    '' AS 'Значение',
    GETDATE() AS 'Дата отчета'
UNION ALL
SELECT 
    'Всего заказов:', 
    CAST(total_orders AS NVARCHAR),
    GETDATE()
FROM SalesStats
UNION ALL
SELECT 
    'Выручка:', 
    FORMAT(total_revenue, 'N0', 'ru-RU') + ' руб.',
    GETDATE()
FROM SalesStats
UNION ALL
SELECT 
    'Средний чек:', 
    FORMAT(avg_order_value, 'N2', 'ru-RU') + ' руб.',
    GETDATE()
FROM SalesStats
UNION ALL
SELECT 
    'Уникальных клиентов:', 
    CAST(unique_customers AS NVARCHAR),
    GETDATE()
FROM SalesStats
UNION ALL
SELECT 
    'VIP клиентов (>500 баллов):', 
    CAST(vip_customers AS NVARCHAR),
    GETDATE()
FROM CustomerStats
UNION ALL
SELECT 
    'Средний балл лояльности:', 
    CAST(avg_loyalty_points AS NVARCHAR),
    GETDATE()
FROM CustomerStats
UNION ALL
SELECT 
    'Всего товаров в наличии:', 
    CAST(total_products AS NVARCHAR),
    GETDATE()
FROM ProductStats
UNION ALL
SELECT 
    'Товаров на складе (шт.):', 
    CAST(total_in_stock AS NVARCHAR),
    GETDATE()
FROM ProductStats
UNION ALL
SELECT 
    'Товаров с низким остатком:', 





    ghghg
    CREATE TABLE Clients (
    id_client INT IDENTITY(1,1) PRIMARY KEY,
   first_name VARCHAR(50) NOT NULL,
   last_name VARCHAR(50) NOT NULL,
    middle_name VARCHAR(50),
    passport_num VARCHAR(20) UNIQUE,
    phone VARCHAR(20),
    email VARCHAR(100),
   address VARCHAR(MAX)
);

CREATE TABLE Agents(
    id_agent INT IDENTITY(1,1) PRIMARY KEY,
   first_name VARCHAR(50) NOT NULL,
   last_name VARCHAR(50) NOT NULL,
    middle_name VARCHAR(50),
    phone VARCHAR(20),
);
CREATE TABLE Products(
    id_product INT IDENTITY(1,1) PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  base_rate DECIMAL(10,4) NOT NULL
);
CREATE TABLE Policies(
    id_policy INT IDENTITY(1,1) PRIMARY KEY,
  number VARCHAR(300) NOT NULL UNIQUE,
  start_date DATE NOT NULL,
  end_date DATE NOT NULL,
  premium_total DECIMAL(12,2) NOT NULL,
  status VARCHAR(20) NOT NULL DEFAULT 'active',
  id_client INT NOT NULL,
  id_agent INT NULL,
  id_product INT NOT NULL,
  FOREIGN KEY (id_client) REFERENCES
  Clients(id_client)ON DELETE NO ACTION,
  FOREIGN KEY (id_agent) REFERENCES
  Agents (id_agent)ON DELETE SET NULL,
  FOREIGN KEY (id_product) REFERENCES
  Products(id_product)ON DELETE NO ACTION,
  CHECK (status IN ('active','expired','cancelled'))
);
CREATE TABLE Claims (
id_claim INT IDENTITY(1,1)
PRIMARY KEY,
claim_date DATE NOT NULL,
amount_approved DECIMAL (12,2) NULL,
id_policy INT NOT NULL,
FOREIGN KEY (id_policy) REFERENCES 
Policies(id_policy) ON DELETE CASCADE
);
CREATE TABLE Payments (
id_payment INT IDENTITY(1,1)
PRIMARY KEY,
PAYMENT_DATE date not null,
amount DECIMAL(12,2) NOT NULL,
id_policy INT NOT NULL,
FOREIGN KEY(id_policy) REFERENCES 
Policies (id_policy) ON DELETE CASCADE
);
INSERT INTO Clients(first_name,last_name,middle_name,passport_num,phone,email,address)
VALUES
('Иван','Петров','Сергеевич','4510123456','+7916-123-45-67','ivanpetrov@mail.ru','г.Москва,ул.Ленина,д.1'),
('Марина','Сидоровна','Алексеевна','4578023456','+7916-456-45-77','marinasm@mail.ru','г.Вологда,ул.Заполярная,д.34'),
('Алексей','Кузнецов','Владимирович','2310123456','+7921-222-45-69','alekskz@mail.ru','г.Каллининград,ул.Кузнецова,д.5'),
('Елена','Смирнова','Дмитриевна','3410123456','+752-123-46-89','lenasm@mail.ru','г.Челябинск,ул.Карла Маркса,д.18'),
('Кирилл','Шаньгин','Игоревич','453423456','+7916-459-52-52','kirhan@mail.ru','г.Котлас,ул.Володарского,д.16'),
('Павел','Дьяков','Дмитриевич','4510678456','+7920-500-50-70','paveldyak@mail.ru','г.Коряжмa,ул.Гагарина,д.8'),
('Анна','Соколова','Александровна','4516793456','+7921-323-55-77','annask@mail.ru','г.Сыктывкар,ул.Победы,д.9'),
('Сергей','Куликов','Александрович','4516783456','+7916-555-45-80','sergeykul@mail.ru','г.Уфа,ул.Ленина,д.45'),
('Сергей','Кубраков','Петрович','4510093456','+7952-567-62-67','sergeykub@mail.ru','г.Пермь,ул.Луночарского,д.10'),
('Сергей','Скворцов','Евгениевич','4510116456','+7952-666-52-99','sergeyskv@mail.ru','г.Воронеж,ул.Плехановская,д.11'),
('Юлия','Амосова','Валерьевна','4567823456','+7916-222-77-80','ulam@mail.ru','г.Казань,ул.Баумана,д.7'),
('Наталья','Субботина','Игоревна','4510120056','+7999-666-88-45','nat22@mail.ru','г.Екатеринбург,ул.Малышева,д.22'),
('Александр','Петухов','Валерьевич','4510567856','+7916-100-50-70','aleksandrpet@mail.ru','г.Самара,ул.пр.Мира,д.20'),
('Максим','Худаков','Максимович','4556563456','+7916-200-45-34','maks354@mail.ru','г.Крым,ул.Великая,д.46'),
('Артем','Сухорев','Александрович','4517693456','+7935-678-60-50','suharev2656@mail.ru','г.Ухта,ул.Маяковского,д.1');

INSERT INTO Agents(first_name,last_name,middle_name,phone)
VALUES
('Анна','Ивановна','Сергеевна','+7916-125-48-97'),
('Надежда','Петровна','Павловна','+7916-145-12-87'),
('Виктория','Сидоровна','Владимировна','+7952-156-47-27'),
('Глеб','Смирнов','Игоревич','+7999-156-78-99'),
('Дарья','Кузнецова','Дмитриевна','+7915-007-77-07'),
('Анастасия','Коновалова','Александровна','+7921-333-60-98'),
('Ирина','Крылова','Кирилловна','+7930-200-57-70'),
('Алина','Куликова','Владимировна','+7908-456-34-78'),
('Полина','Куликова','Владимировна','+7903-465-75-23'),
('Михаил','Зайцев','Сергеевич','+7917-333-22-77'),
('Олег','Соколов','Павлович','+7995-111-55-63'),
('Евгений','Пушкин','Николаевич','+7944-789-11-88'),
('Александр','Новосельцев','Викторович','+7969-156-79-60'),
('Виктор','Шестак','Адександрович','+7916-123-45-67'),
('Людмила','Иванова','Ивановна','+7921-052-75-25');

INSERT INTO Products(name,base_rate)
VALUES
('ОСАГО',2.50),
('КАСКО (полное)',5.80),
('Страхование жизни',1.20),
('Страхование квартиры',1.50),
('Страхование дома и дачи',1.80),
('Страхование здоровья ',4.50),
('Страхование путешественников',2.00),
('Страхование грузов',3.20),
('Страхование ответственности',2.70),
('Страхование сельхозрисков',5.00),
('Страхование электроники',1.90),
('Страхование от несчастных случаев',1.10),
('Страхование бизнеса',4.20),
('Страхование водного транспорта',3.80),
('Зеленая карта',2.30);

INSERT INTO Policies(number,start_date,end_date,premium_total,status,id_client,id_agent,id_product)
VALUES
('POL-2024-0001','2024-01-15','2024-12-31',5432.00,'active',9,1,1),
('POL-2024-0002','2024-02-01','2024-01-31',12000.00,'active',10,2,2),
('POL-2024-0003','2024-03-10','2024-12-30',8900.00,'active',11,3,3),
('POL-2024-0004','2024-04-20','2024-01-19',3500.00,'active',12,4,4),
('POL-2024-0005','2024-10-10','2024-11-30',6700.00,'active',13,5,5),
('POL-2024-0006','2024-03-20','2024-05-17',3000.00,'active',14,6,6),
('POL-2024-0007','2024-01-01','2024-04-28',9540.00,'active',15,7,7),
('POL-2024-0008','2024-05-01','2024-06-22',6500.00,'active',16,8,8),
('POL-2024-0009','2024-06-01','2024-05-05',12500.00,'active',17,9,9),
('POL-2024-0010','2024-05-10','2024-02-04',2400.00,'active',18,10,10),
('POL-2024-0011','2024-06-15','2024-01-15',9800.00,'active',19,11,11),
('POL-2024-0012','2024-03-15','2024-09-30',4700.00,'active',20,12,12),
('POL-2024-0013','2024-08-01','2024-10-27',15600.00,'active',21,13,13),
('POL-2024-0014','2024-09-05','2024-12-11',3200.00,'active',22,14,14),
('POL-2024-0015','2024-02-01','2024-10-07',7800.00,'active',23,15,15);

INSERT INTO Claims(claim_date,amount_approved,id_policy)
VALUES
('2024-02-10',5000.00,6),
('2024-03-15',8700.00,7),
('2024-04-20',NULL,8),
('2024-02-10',0.00,9),
('2024-05-10',3200.00,10),
('2024-02-28',10500.00,11),
('2024-02-22',Null,12),
('2024-04-12',4500.00,13),
('2024-06-01',2000.00,14),
('2024-03-25',12500.00,15),
('2024-07-05',800.00,16),
('2024-04-18',6700.00,17),
('2024-06-10',9200.00,18),
('2024-05-15',3300.00,19),
('2024-07-12',0.00,20);

INSERT INTO Payments(payment_date,amount,id_policy)
VALUES
('2024-01-10',5000.00,6),
('2024-01-25',6000.00,7),
('2024-02-01',3000.00,8),
('2024-03-05',8900,9),
('2024-01-15',3500.00,10),
('2024-12-05',6500.00,11),
('2024-01-10',5000,12),
('2024-04-01',12300.00,13),
('2024-02-11',5000.00,14),
('2024-10-05',4700.00,15),
('2024-12-25',15600.00,16),
('2024-04-01',6200.00,17),
('2024-02-25',4300.00,18),
('2024-05-05',8300.00,19),
('2024-06-10',5200,20);
lllglk
    CAST(low_stock_products AS NVARCHAR),
    GETDATE()
FROM ProductStats;
код
