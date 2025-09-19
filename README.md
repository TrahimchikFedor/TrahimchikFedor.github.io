<!-- register.html -->
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>Регистрация продавца</title>
    <!-- Подключаем скрипт Telegram. Это должно быть в <head> -->
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
        :root { --tg-color-scheme: light; }
        body { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif; padding: 15px; margin: 0; color: var(--tg-theme-text-color, #000000); background-color: var(--tg-theme-bg-color, #ffffff); }
        .container { max-width: 390px; margin: 0 auto; }
        .form-group { margin-bottom: 15px; }
        label { display: block; margin-bottom: 5px; font-weight: 500;}
        input { width: 100%; padding: 10px; box-sizing: border-box; border: 1px solid var(--tg-theme-hint-color, #dddddd); border-radius: 8px; background-color: var(--tg-theme-secondary-bg-color, #f4f4f4); color: var(--tg-theme-text-color, #000000); font-size: 16px; }
        #error, #status { color: var(--tg-theme-destructive-text-color, #ff0000); margin-top: 10px; text-align: center; font-weight: bold; }
        #status { color: green; }
    </style>
</head>
<body>
    <div class="container">
        <h3>Профиль продавца</h3>
        <p>Пожалуйста, заполните ваши данные. Они будут видны покупателям при обращении.</p>
        <div class="form-group">
            <label for="first_name">Имя:</label>
            <input type="text" id="first_name" placeholder="Иван">
        </div>
        <div class="form-group">
            <label for="last_name">Фамилия:</label>
            <input type="text" id="last_name" placeholder="Иванов">
        </div>
        <p id="error"></p>
        <!-- Диагностическое сообщение -->
        <p id="status">Инициализация...</p>
    </div>

    <script>
        // Ждем, пока весь документ загрузится
        document.addEventListener('DOMContentLoaded', function() {
            try {
                let tg = window.Telegram.WebApp;
                let statusDiv = document.getElementById('status');
                let errorDiv = document.getElementById('error');
                
                // Проверяем, что объект tg существует. Если нет - скрипт не загрузился.
                if (typeof tg === 'undefined' || !tg.initData) {
                    statusDiv.style.color = 'red';
                    statusDiv.innerText = 'Ошибка: Не удалось загрузить API Telegram.';
                    return;
                }
                
                // Если все хорошо, показываем статус
                statusDiv.innerText = 'API готово. Заполните поля.';
                statusDiv.style.color = 'green';
                
                tg.expand();
                tg.MainButton.setText("Сохранить профиль");
                
                // Основной обработчик нажатия на кнопку
                tg.MainButton.onClick(function(){
                    let firstName = document.getElementById('first_name').value;
                    let lastName = document.getElementById('last_name').value;
                    
                    if (firstName.trim().length < 2 || lastName.trim().length < 2) {
                        errorDiv.innerText = "Имя и фамилия должны содержать минимум 2 символа.";
                        tg.HapticFeedback.notificationOccurred('error');
                        return;
                    }
                    
                    let data = { 
                        first_name: firstName, 
                        last_name: lastName 
                    };
                    
                    // Самая важная команда
                    tg.sendData(JSON.stringify(data));
                });

                // Функция для показа/скрытия кнопки
                function checkFields() {
                    let firstName = document.getElementById('first_name').value;
                    let lastName = document.getElementById('last_name').value;
                    if (firstName.trim().length >= 2 && lastName.trim().length >= 2) {
                        tg.MainButton.show();
                    } else {
                        tg.MainButton.hide();
                    }
                }

                document.getElementById('first_name').addEventListener('input', checkFields);
                document.getElementById('last_name').addEventListener('input', checkFields);
                
                // При загрузке страницы кнопка скрыта
                checkFields();

            } catch (e) {
                // Если произошла какая-то непредвиденная ошибка, выводим ее
                let statusDiv = document.getElementById('status');
                statusDiv.style.color = 'red';
                statusDiv.innerText = 'Критическая ошибка JS: ' + e.message;
            }
        });
    </script>
</body>
</html>
