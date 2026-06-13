<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>Perte de Calories</title>
  <style>
    body {
      font-family: Arial;
      max-width: 420px;
      margin: auto;
      background: #f5f5f5;
      padding: 20px;
    }

    h1 {
      text-align: center;
    }

    input, button {
      width: 100%;
      padding: 10px;
      margin-top: 8px;
    }

    .card {
      background: white;
      padding: 10px;
      margin-top: 10px;
      border-radius: 10px;
    }

    ul {
      padding: 0;
    }

    li {
      list-style: none;
      display: flex;
      justify-content: space-between;
      padding: 8px;
      background: white;
      margin-top: 5px;
      border-radius: 8px;
    }

    .total {
      font-size: 18px;
      font-weight: bold;
      margin-top: 10px;
    }

    .goal {
      color: green;
      font-weight: bold;
    }
  </style>
</head>

<body>

<h1>🔥 Perte de Calories</h1>

<div class="card">
  <input id="food" placeholder="Aliment">
  <input id="calories" type="number" placeholder="Calories">
  <button onclick="addFood()">Ajouter</button>
</div>

<div class="card">
  🎯 Objectif journalier :
  <input id="goal" type="number" placeholder="Ex: 2000" oninput="updateGoal()">
  <div class="total">Total consommé: <span id="total">0</span> kcal</div>
  <div>Objectif: <span class="goal" id="goalDisplay">0</span> kcal</div>
</div>

<ul id="list"></ul>

<script>
let data = JSON.parse(localStorage.getItem("caloriesApp")) || [];
let goal = 2000;

function save() {
  localStorage.setItem("caloriesApp", JSON.stringify(data));
}

function render() {
  const list = document.getElementById("list");
  list.innerHTML = "";

  let total = 0;

  data.forEach((item, index) => {
    total += item.calories;

    const li = document.createElement("li");
    li.innerHTML = `
      ${item.food} - ${item.calories} kcal
      <button onclick="remove(${index})">❌</button>
    `;
    list.appendChild(li);
  });

  document.getElementById("total").innerText = total;

  if (total > goal) {
    alert("⚠️ Tu as dépassé ton objectif calorique !");
  }
}

function addFood() {
  let food = document.getElementById("food").value;
  let calories = parseInt(document.getElementById("calories").value);

  if (!food || isNaN(calories)) return;

  data.push({ food, calories });
  save();
  render();

  document.getElementById("food").value = "";
  document.getElementById("calories").value = "";
}

function remove(index) {
  data.splice(index, 1);
  save();
  render();
}

function updateGoal() {
  goal = parseInt(document.getElementById("goal").value) || 0;
  document.getElementById("goalDisplay").innerText = goal;
}

render();
</script>

</body>
</html>
