<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bank System</title>
    <link rel="stylesheet" href="style.css">
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
                    placeholder="Сумма"
                    min="1"
                >
                <button onclick="deposit()">Пополнить</button>
            </div>

            <div class="card">
                <h3>Снять деньги</h3>
                <input
                    type="number"
                    id="withdrawAmount"
                    placeholder="Сумма"
                    min="1"
                >
                <button onclick="withdraw()">Снять</button>
            </div>

            <div class="card">
                <h3>Перевод</h3>
                <input
                    type="text"
                    id="recipient"
                    placeholder="Номер счёта получателя"
                >
                <input
                    type="number"
                    id="transferAmount"
                    placeholder="Сумма"
                    min="1"
                >
                <button onclick="transfer()">Перевести</button>
            </div>
        </section>

        <section class="card">
            <h3>История операций</h3>

            <ul id="history">
                <li>
                    <span>Начальный баланс</span>
                    <strong>+100 000 ₸</strong>
                </li>
            </ul>
        </section>
    </main>

    <footer>
        <p>MyBank © 2026 — учебный проект</p>
    </footer>

</div>

<script src="script.js"></script>
</body>
</html>
let balance = 100000;

const balanceElement = document.getElementById("balance");
const historyElement = document.getElementById("history");

function formatMoney(amount) {
    return amount.toLocaleString("ru-RU") + " ₸";
}

function updateBalance() {
    balanceElement.textContent = formatMoney(balance);
}

function addHistory(text, amount, type) {
    const li = document.createElement("li");

    const description = document.createElement("span");
    description.textContent = text;

    const value = document.createElement("strong");
    value.textContent =
        (amount >= 0 ? "+" : "") + formatMoney(amount);

    value.className = type;

    li.appendChild(description);
    li.appendChild(value);

    historyElement.prepend(li);
}

function deposit() {
    const input = document.getElementById("depositAmount");
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

function withdraw() {
    const input = document.getElementById("withdrawAmount");
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

function transfer() {
    const recipientInput = document.getElementById("recipient");
    const amountInput = document.getElementById("transferAmount");

    const recipient = recipientInput.value.trim();
    const amount = Number(amountInput.value);

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
        `Перевод на счёт ${recipient}`,
        -amount,
        "negative"
    );

    recipientInput.value = "";
    amountInput.value = "";

    alert("Перевод успешно выполнен!");
}

updateBalance();
