# Emotion
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>感情記録アプリ</title>
    <style>
        body {
            font-family: sans-serif;
            max-width: 500px;
            margin: auto;
            padding: 20px;
            background: #f8f8f8;
        }
        h2 { color: #333; }
        .card {
            background: white;
            padding: 15px;
            margin: 15px 0;
            border-radius: 10px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }
        button {
            width: 100%;
            padding: 12px;
            background: #4da3ff;
            border: none;
            border-radius: 10px;
            color: white;
            font-size: 16px;
            margin-top: 10px;
        }
        .emotion-btn {
            display: block;
            width: 100%;
            margin: 8px 0;
            padding: 12px;
            background: #e0f0ff;
            border-radius: 10px;
            border: none;
            font-size: 18px;
        }
        .problem-item {
            padding: 10px;
            margin: 8px 0;
            background: #ffe8c6;
            border-radius: 10px;
        }
        .selected {
            background: #ffc48a !important;
        }
    </style>
</head>
<body>

<div id="app"></div>

<script>
    const app = document.getElementById("app");
    let record = {
        emotion: "",
        reason: "",
        problems: []
    };

    // ① 感情選択画面
    function showEmotionPage() {
        app.innerHTML = `
            <div class="card">
                <h2>今の気持ちは？</h2>
                ${["😊 嬉しい", "😢 悲しい", "😡 イライラ", "😰 不安", "😴 つかれた", "😕 もやもや"]
                .map(e => `<button class="emotion-btn" onclick="selectEmotion('${e}')">${e}</button>`).join("")}
            </div>
        `;
    }

    function selectEmotion(e) {
        record.emotion = e;
        showReasonPage();
    }

    // ② 理由入力
    function showReasonPage() {
        app.innerHTML = `
            <div class="card">
                <h2>その気持ちの理由は？</h2>
                <textarea id="reason" rows="4" style="width:100%;"></textarea>
                <button onclick="saveReason()">次へ</button>
            </div>
        `;
    }

    function saveReason() {
        record.reason = document.getElementById("reason").value;
        showProblemPage();
    }

    // ③ 悩み選択
    const problemList = ["学校", "友達", "家族", "勉強", "恋愛", "体調", "将来", "その他"];

    function showProblemPage() {
        app.innerHTML = `
            <div class="card">
                <h2>悩んでいることは？（複数選択可）</h2>
                ${problemList.map(p => `
                    <div class="problem-item" onclick="toggleProblem('${p}', this)">
                        ${p}
                    </div>
                `).join("")}
                <button onclick="showAdvicePage()">アドバイスを見る</button>
            </div>
        `;
    }

    function toggleProblem(p, el) {
        if (record.problems.includes(p)) {
            record.problems = record.problems.filter(x => x !== p);
            el.classList.remove("selected");
        } else {
            record.problems.push(p);
            el.classList.add("selected");
        }
    }

    // ④ アドバイス生成
    function showAdvicePage() {
        const advice = `
            今の気持ち：${record.emotion}<br><br>
            理由：${record.reason}<br><br>
            悩み：${record.problems.join("・")}<br><br>

            <b>🌱 アドバイス</b><br>
            状況を整理できていてえらいです。<br>
            まずは今日できる「小さな1つの行動」を決めてみてください。<br>
            深呼吸して、心をゆっくり落ち着かせましょう。
        `;

        app.innerHTML = `
            <div class="card">
                <h2>今日のアドバイス</h2>
                <p>${advice}</p>
                <button onclick="showEmotionPage()">もう一度記録する</button>
            </div>
        `;
    }

    showEmotionPage();
</script>

</body>
</html>
