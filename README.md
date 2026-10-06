<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MyBank</title>

    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: Arial, sans-serif;
            background: #f1f5f9;
            color: #172033;
        }

        .container {
            width: 90%;
            max-width: 1000px;
            margin: 30px auto;
        }

        header {
            background: #2563eb;
            color: white;
            padding: 30px;
            border-radius: 18px;
            margin-bottom: 20px;
        }

        header h1 {
            margin-bottom: 8px;
        }

        .card {
            background: white;
            padding: 25px;
            border-radius: 18px;
            box-shadow: 0 5px 20px rgba(0, 0, 0, 0.06);
            margin-bottom: 20px;
        }

        .balance-card {
            background: linear-gradient(135deg, #1d4ed8, #3b82f6);
            color: white;
        }

        .balance-card h2 {
            font-size: 38px;
            margin: 15px 0;
        }

        .actions {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 20px;
        }

        .actions .card {
            margin-bottom: 0;
        }

        h3 {
            margin-bottom: 15px;
        }

        input {
            width: 100%;
            padding: 12px;
            margin-bottom: 10px;
            border: 1px solid #d1d5db;
            border-radius: 10px;
            font-size: 15px;
        }

        button {
            width: 100%;
            padding: 12px;
            border: none;
            border-radius: 10px;
            background: #2563eb;
            color: white;
            font-size: 15px;
            cursor: pointer;
        }

        button:hover {
            background: #1d4ed8;
        }

        #history {
            list-style: none;
        }

        #history li {
            display: flex;
            justify-content: space-between;
            padding: 15px 0;
            border-bottom: 1px solid #e5e7eb;
        }

        .positive {
            color: #16a34a;
        }

        .negative {
            color: #dc2626;
        }

        footer {
            text-align: center;
            color: #64748b;
            margin-top: 30px;
        }

        @media (max-width: 700px) {
            .actions {
                grid-template-columns: 1fr;
            }

            .balance-card h2 {
                font-size: 30px;
            }
        }
    </style>
</head>

<body>

<div class="container">

    <header>
        <h1>🏦 MyBank</h1>
        <p>Учебная банковская система</p>
    </header>

    <main>

        <section class="card balance-card">
            <p>Текущий баланс</p>
            <h2 id="balance">100 000 ₸</h2>
            <span>Счёт: KZ00 0000 0000 0000</span>
        </section>

        <section class="actions">

            <div class="card">
                <h3>Пополнить счёт</h3>

                <input
                    type="number"
                    id="depositAmount"
                    placeholder="Введите сумму"
                    min="1"
                >

                <button onclick="deposit()">
                    Пополнить
                </button>
            </div>


            <div class="card">
                <h3>Снять деньги</h3>

                <input
                    type="number"
                    id="withdrawAmount"
                    placeholder="Введите сумму"
                    min="1"
                >

                <button onclick="withdraw()">
                    Снять
                </button>
            </div>


            <div class="card">
                <h3>Перевод</h3>

                <input
                    type="text"
                    id="recipient"
                    placeholder="Номер счёта"
                >

                <input
                    type="number"
                    id="transferAmount"
                    placeholder="Сумма"
                    min="1"
                >

                <button onclick="transfer()">
                    Перевести
                </button>
            </div>

        </section>


        <section class="card">
            <h3>История операций</h3>

            <ul id="history">

                <li>
                    <span>Начальный баланс</span>
                    <strong class="positive">
                        +100 000 ₸
                    </strong>
                </li>

            </ul>
        </section>

    </main>

    <footer>
        <p>MyBank © 2026 — учебный проект</p>
    </footer>

</div>


<script>

    // Текущий баланс пользователя
    let balance = 100000;


    // Элемент, в котором показывается баланс
    const balanceElement = document.getElementById("balance");


    // Список истории операций
    const historyElement = document.getElementById("history");


    // Форматирование денег
    function formatMoney(amount) {

        return amount.toLocaleString("ru-RU") + " ₸";

    }


    // Обновление баланса на странице
    function updateBalance() {

        balanceElement.textContent = formatMoney(balance);

    }


    // Добавление операции в историю
    function addHistory(text, amount, type) {

        const li = document.createElement("li");

        const description = document.createElement("span");

        description.textContent = text;


        const value = document.createElement("strong");

        value.textContent =
            (amount >= 0 ? "+" : "") +
            formatMoney(amount);


        value.className = type;


        li.appendChild(description);

        li.appendChild(value);


        historyElement.prepend(li);

    }


    // Пополнение счёта
    function deposit() {

        const input =
            document.getElementById("depositAmount");

        const amount = Number(input.value);


        if (amount <= 0) {

            alert("Введите корректную сумму.");

            return;
        }


        balance += amount;


        updateBalance();


        addHistory(
            "Пополнение счёта",
            amount,
            "positive"
        );


        input.value = "";

    }


    // Снятие денег
    function withdraw() {

        const input =
            document.getElementById("withdrawAmount");

        const amount = Number(input.value);


        if (amount <= 0) {

            alert("Введите корректную сумму.");

            return;
        }


        if (amount > balance) {

            alert("Недостаточно средств.");

            return;
        }


        balance -= amount;


        updateBalance();


        addHistory(
            "Снятие наличных",
            -amount,
            "negative"
        );


        input.value = "";

    }


    // Перевод денег
    function transfer() {

        const recipientInput =
            document.getElementById("recipient");

        const amountInput =
            document.getElementById("transferAmount");


        const recipient =
            recipientInput.value.trim();


        const amount =
            Number(amountInput.value);


        if (!recipient) {

            alert("Введите номер счёта получателя.");

            return;
        }


        if (amount <= 0) {

            alert("Введите корректную сумму.");

            return;
        }


        if (amount > balance) {

            alert("Недостаточно средств.");

            return;
        }


        balance -= amount;


        updateBalance();


        addHistory(
            "Перевод на счёт " + recipient,
            -amount,
            "negative"
        );


        recipientInput.value = "";

        amountInput.value = "";


        alert("Перевод успешно выполнен!");

    }


    // Показываем начальный баланс
    updateBalance();

</script>

</body>
</html>
