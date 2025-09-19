<!-- register.html -->
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>Регистрация продавца</title>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
        body { font-family: -apple-system, sans-serif; padding: 15px; color: var(--tg-theme-text-color); background-color: var(--tg-theme-bg-color); }
        .form-group { margin-bottom: 15px; }
        label { display: block; margin-bottom: 5px; }
        input { width: 100%; padding: 10px; box-sizing: border-box; border: 1px solid var(--tg-theme-hint-color); border-radius: 8px; font-size: 16px; }
        #status { text-align: center; margin-top: 15px; font-weight: bold; }
    </style>
</head>
<body>
    <h3>Профиль продавца</h3>
    <div class="form-group">
        <label for="first_name">Ваше Имя:</label>
        <input type="text" id="first_name" placeholder="Иван">
    </div>
    <div class="form-group">
        <label for="last_name">Ваша Фамилия:</label>
        <input type="text" id="last_name" placeholder="Иванов">
    </div>
    <p id="status"></p>

    <script>
        // Ждем полной загрузки страницы
        document.addEventListener('DOMContentLoaded', function() {
            try {
                const tg = window.Telegram.WebApp;
                const statusEl = document.getElementById('status');
                
                // 1. Инициализация
                tg.expand();
                statusEl.textContent = 'Приложение готово.';
                statusEl.style.color = 'green';

                // 2. Настройка главной кнопки Telegram
                tg.MainButton.setText("Завершить регистрацию");
                tg.MainButton.show(); // Показываем кнопку сразу
                
                // 3. Назначаем действие на клик
                tg.MainButton.onClick(function() {
                    const firstName = document.getElementById('first_name').value;
                    const lastName = document.getElementById('last_name').value;

                    if (firstName.trim().length < 2 || lastName.trim().length < 2) {
                        tg.showAlert("Пожалуйста, введите имя и фамилию (минимум 2 символа).");
                        return;
                    }
                    
                    const data = { 
                        first_name: firstName, 
                        last_name: lastName 
                    };
                    
                    // Отправляем данные боту
                    tg.sendData(JSON.stringify(data));
                });

            } catch (e) {
                // Если что-то пошло не так, выводим ошибку на экран
                document.getElementById('status').textContent = 'Ошибка: ' + e.message;
                document.getElementById('status').style.color = 'red';
            }
        });
    </script>
</body>
</html>
