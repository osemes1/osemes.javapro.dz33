# osemes.javapro.dz33

Виконання DZ33

1. Реалізувати клас Order. Цей клас зберігатиме значення: id, date, cost, products
2. Реалізувати клас Product. Цей клас зберігатиме значення: id, name, cost
3. Замовлення зберігатимуться у спеціалізованому класі-репозиторії OrderRepository. 
3.1 Реалізувати метод отримання замовлення за id
3.2 Реалізувати метод отримання всіх замовлень
3.3 Реалізувати метод додавання замовлення
4. Налаштувати Spring-додаток через application.yml
4.1 Додаток повинен бути доступний за URL: http://localhost:8080
4.2 Налаштувати підключення до БД
4 . логування на рівні INFO для пакетів програми та для пакету org.springframework.web 
Процес логування включає виведення в консоль та запис у файл
5. Реалізувати контролер Ping для перевірки того, що програма працює. Цей контролер має лише один спосіб і повертає повідомлення “ОК”.
5.1 Контролер доступний за URL: http://localhost:8080/ping
6. Реалізувати контролер взаємодії з ресурсом Order.
6.1 Контролер доступний за URL: http://localhost:8080/orders
6.2 Отримання конкретного замовлення
6.3 Отримання всіх замовлень
ВАЖЛИВО: OrderRepository повертає дані з БД, для цього необхідно створити БД та відповідні таблиці

У процесі виконання завдання я створив пакет Spring Boot, створив класи з orders-operations, створив підключення до БД та перевірив доступність додатка за URLs:

Якщо програма стартувала без проблем, тоді за посиланням:
http://localhost:8080
буде:
Welcome to My App
This is the home page of my application.

...Також за посиланням:
http://localhost:8080/ping
буде:
OK

...Також за посиланням:
http://localhost:8080/orders
буде:
[]
Адаптер робить, але потрібно створити щось типу DataInitializer щоб додати ордера у БД:

import com.example.demo.model.Order;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.CommandLineRunner;
import org.springframework.stereotype.Component;
import com.example.demo.repository.OrderRepository;

import java.sql.Timestamp; // Змінимо імпорт на java.sql.Timestamp

@Component
public class DataInitializer implements CommandLineRunner {

    private final OrderRepository orderRepository;

    @Autowired
    public DataInitializer(OrderRepository orderRepository) {
        this.orderRepository = orderRepository;
    }

    @Override
    public void run(String... args) {
        // Перевірка, чи база даних порожня перед додаванням тестових даних
        if (orderRepository.count() == 0) {
            // Створення декількох тестових замовлень і збереження їх у базі даних
            Timestamp currentTimestamp = new Timestamp(System.currentTimeMillis());

            Order order1 = new Order(currentTimestamp, 100.0);
            Order order2 = new Order(currentTimestamp, 200.0);
            Order order3 = new Order(currentTimestamp, 300.0);

            orderRepository.save(order1);
            orderRepository.save(order2);
            orderRepository.save(order3);
        }
    }
}


