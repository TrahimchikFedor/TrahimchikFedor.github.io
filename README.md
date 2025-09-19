<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Регистрация продавца</title>
  <script src="https://telegram.org/js/telegram-web-app.js"></script>
  <style>
    body { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif; padding: 15px; background: var(--tg-theme-bg-color); color: var(--tg-theme-text-color); }
    .form-group { margin-bottom: 12px; }
    input { width: 100%; padding: 10px; border-radius: 6px; border: 1px solid #ccc; box-sizing: border-box; }
    button { width: 100%; padding: 12px; border-radius: 6px; border: none; font-size: 16px; cursor: pointer; }
    #error { color: #c00; margin-top: 8px; }
  </style>
</head>
<body>
  <h3>Профиль продавца</h3>
  <div class="form-group">
    <label>Имя</label>
    <input id="first_name" placeholder="Иван" />
  </div>
  <div class="form-group">
    <label>Фамилия</label>
    <input id="last_name" placeholder="Иванов" />
  </div>
  <p id="error"></p>
  <button id="submit_btn">Сохранить профиль</button>

  <script>
    const tg = window.Telegram.WebApp;
    tg.MainButton.text = "Сохранить профиль";
    tg.MainButton.show();

    function sendProfile() {
      const first = document.getElementById('first_name').value.trim();
      const last = document.getElementById('last_name').value.trim();
      const err = document.getElementById('error');
      err.innerText = "";

      if (first.length < 2 || last.length < 2) {
        err.innerText = "Имя и фамилия должны содержать минимум 2 символа.";
        return;
      }

      const payload = { first_name: first, last_name: last };
      try {
        tg.sendData(JSON.stringify(payload));
        // Закрываем mini app
        tg.close();
      } catch (e) {
        err.innerText = "Ошибка отправки данных. Попробуйте ещё раз.";
        console.error("sendData error:", e);
      }
    }

    tg.onEvent('mainButtonClicked', function() { sendProfile(); });
    document.getElementById('submit_btn').addEventListener('click', sendProfile);
  </script>
</body>
</html>
