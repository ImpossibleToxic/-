<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Форма пользователя</title>
</head>
<body>
    <h1>Форма пользователя</h1>
    <form method="POST" action="">
        <div>
            <label for="username">Имя пользователя:</label>
            <input type="text" id="username" name="username" required>
        </div>
        
        <div>
            <label for="age">Возраст:</label>
            <select id="age" name="age" required>
                <?php for($i = 15; $i <= 55; $i++): ?>
                    <option value="<?php echo $i; ?>"><?php echo $i; ?></option>
                <?php endfor; ?>
            </select>
        </div>
        
        <div>
            <label for="message">Сообщение:</label>
            <textarea id="message" name="message" required></textarea>
        </div>
        
        <button type="submit">Отправить</button>
    </form>

    <?php
    if ($_SERVER['REQUEST_METHOD'] === 'POST') {
        // Обработка данных формы
        $username = $_POST['username'];
        $age = $_POST['age'];
        $message = $_POST['message'];
        
        // Валидация имени пользователя
        $username = preg_replace('/[^a-zA-Zа-яА-ЯёЁ\s]/u', '', $username);
        $username = trim($username);
        
        // Валидация сообщения
        $message = strip_tags($message);
        $message = preg_replace('/[^a-zA-Zа-яА-ЯёЁ0-9\s.,!?-]/u', '', $message);
        $message = trim($message);
        
        // Проверка, что после фильтрации данные не пустые
        if (!empty($username) && !empty($message)) {
            // Формируем строку для записи в файл
            $data = "Имя: $username\nВозраст: $age\nСообщение: $message\n\n";
            
            // Записываем данные в файл
            file_put_contents('user_data.txt', $data, FILE_APPEND | LOCK_EX);
            
            echo "<p>Данные успешно сохранены!</p>";
        } else {
            echo "<p>Ошибка: Некорректные данные в форме!</p>";
        }
    }
    ?>
</body>
</html>
