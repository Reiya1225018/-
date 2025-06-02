
html_code = """
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <title>得点管理システム</title>
  <style>
    body { font-family: sans-serif; text-align: center; margin: 20px; }
    .player { margin: 10px 0; }
    .score { font-size: 1.5em; margin: 0 10px; }
    button { font-size: 1em; padding: 5px 10px; }
    input.name-input, input.score-input { font-size: 1em; padding: 5px; width: 120px; }
  </style>
</head>
<body>
  <h1>得点管理システム</h1>
  <div id="players"></div>
  <button onclick="addPlayer()">プレイヤー追加</button>
  <button onclick="resetScores()">月リセット</button>

  <script>
    const defaultNames = ["みらい", "れいや", "ようすけ"];
    let players = [];

    function loadFromStorage() {
      const data = localStorage.getItem("scoreData");
      if (data) {
        players = JSON.parse(data);
      } else {
        players = defaultNames.map(name => ({ name: name, score: 0 }));
      }
    }

    function saveToStorage() {
      localStorage.setItem("scoreData", JSON.stringify(players));
    }

    function renderPlayers() {
      const container = document.getElementById("players");
      container.innerHTML = "";
      players.forEach((player, index) => {
        const div = document.createElement("div");
        div.className = "player";
        div.innerHTML = `
          <input class="name-input" value="${player.name}" onchange="updateName(${index}, this.value)">
          <button onclick="changeScore(${index}, -1)">−</button>
          <span class="score">${player.score}</span>
          <button onclick="changeScore(${index}, 1)">＋</button>
          <input class="score-input" type="number" placeholder="直接入力" onchange="setScore(${index}, this.value)">
        `;
        container.appendChild(div);
      });
    }

    function changeScore(index, delta) {
      players[index].score += delta;
      saveToStorage();
      renderPlayers();
    }

    function setScore(index, value) {
      const parsed = parseInt(value);
      if (!isNaN(parsed)) {
        players[index].score = parsed;
        saveToStorage();
        renderPlayers();
      }
    }

    function updateName(index, name) {
      players[index].name = name;
      saveToStorage();
    }

    function addPlayer() {
      if (players.length >= 5) {
        alert("プレイヤーは最大5人までです。");
        return;
      }
      players.push({ name: "新しいプレイヤー", score: 0 });
      saveToStorage();
      renderPlayers();
    }

    function resetScores() {
      players.forEach(p => p.score = 0);
      saveToStorage();
      renderPlayers();
    }

    loadFromStorage();
    renderPlayers();
  </script>
</body>
</html>
"""

# HTMLファイルとして保存
file_path = "/mnt/data/score_manager_v3.html"
with open(file_path, "w", encoding="utf-8") as f:
    f.write(html_code)

file_path
