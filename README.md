
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Регистрация продавца</title>
    <!-- Подключаем скрипт от Telegram -->
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
        body { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif; padding: 15px; color: var(--tg-theme-text-color); background-color: var(--tg-theme-bg-color); }
        .form-group { margin-bottom: 15px; }
        label { display: block; margin-bottom: 5px; }
        input { width: 95%; padding: 10px; border: 1px solid var(--tg-theme-hint-color); border-radius: 5px; background-color: var(--tg-theme-secondary-bg-color); color: var(--tg-theme-text-color); }
        button { width: 100%; padding: 12px; background-color: var(--tg-theme-button-color); color: var(--tg-theme-button-text-color); border: none; border-radius: 5px; cursor: pointer; font-size: 16px; }
        #error { color: var(--tg-theme-destructive-text-color); margin-top: 10px; }
    </style>
</head>
<body>
    <h3>Профиль продавца</h3>
    <p>Пожалуйста, заполните ваши данные. Они будут видны покупателям.</p>
    <div class="form-group">
        <label for="first_name">Имя:</label>
        <input type="text" id="first_name" placeholder="Иван">
    </div>
    <div class="form-group">
        <label for="last_name">Фамилия:</label>
        <input type="text" id="last_name" placeholder="Иванов">
    </div>
    <p id="error"></p>
    <button id="submit_btn">Сохранить профиль</button>

    <script>
        // Получаем объект WebApp от Telegram
        let tg = window.Telegram.WebApp;

        // Настраиваем основную кнопку
        tg.MainButton.text = "Сохранить профиль";
        tg.MainButton.textColor = "#FFFFFF";
        tg.MainButton.color = "#2481CC";

        let submitBtn = document.getElementById("submit_btn");

        // Показываем основную кнопку, когда форма готова
        tg.MainButton.show();

        // Обработчик нажатия на основную кнопку
        tg.onEvent('mainButtonClicked', function(){
            let firstName = document.getElementById('first_name').value;
            let lastName = document.getElementById('last_name').value;
            let errorDiv = document.getElementById('error');

            if (firstName.trim().length < 2 || lastName.trim().length < 2) {
                errorDiv.innerText = "Имя и фамилия должны содержать минимум 2 символа.";
                return;
            }

            // Формируем JSON-объект для отправки боту
            let data = {
                first_name: firstName,
                last_name: lastName,
            };

            // Отправляем данные боту
            tg.sendData(JSON.stringify(data));
            
            // Закрываем веб-приложение
            tg.close();
        });

        // Дублируем логику для кнопки внутри HTML для удобства
        submitBtn.addEventListener('click', function(){
            tg.MainButton.onClick();
        });
    </script>
</body>
</html>
