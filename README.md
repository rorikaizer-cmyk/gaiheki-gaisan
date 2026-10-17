[gaiheki-gaisan.html](https://github.com/user-attachments/files/33594395/gaiheki-gaisan.html)
<!doctype html>
<html lang="ja">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>外壁面積・概算計算</title>
  <style>
    :root { color-scheme: light; --navy:#163d5b; --blue:#eaf5fb; --line:#cbd8df; }
    * { box-sizing:border-box; }
    body { margin:0; background:#f4f7f8; color:#17232b; font-family:"Meiryo","Yu Gothic UI",sans-serif; }
    main { max-width:620px; margin:0 auto; padding:28px 18px 42px; }
    h1 { color:var(--navy); font-size:25px; margin:0 0 8px; }
    .service-name { color:var(--navy); font-size:25px; font-weight:700; margin:0 0 8px; }
    .lead { margin:0 0 22px; line-height:1.7; }
    .notice { background:#fff5d9; border-left:5px solid #e4a925; border-radius:6px; padding:12px 14px; font-size:14px; line-height:1.6; margin-bottom:20px; }
    section { background:#fff; border:1px solid var(--line); border-radius:12px; padding:20px; box-shadow:0 2px 8px #17344a0a; }
    label { display:block; font-weight:700; margin:0 0 8px; }
    .field { margin:0 0 19px; }
    input { width:100%; padding:13px; border:2px solid #9fb4c0; border-radius:8px; font-size:19px; }
    input:focus { outline:3px solid #92d1ef; border-color:#397a9c; }
    .unit { display:block; color:#53636d; font-size:14px; margin-top:6px; }
    .formula { background:var(--blue); border-radius:8px; padding:12px; font-size:14px; line-height:1.6; }
    .result { margin-top:20px; background:var(--navy); color:#fff; border-radius:10px; padding:20px; }
    .result h2 { font-size:16px; margin:0 0 6px; }
    output { display:block; font-size:37px; font-weight:800; letter-spacing:.02em; }
    .result small { font-size:14px; }
    footer { color:#52636d; font-size:13px; line-height:1.6; margin-top:18px; }
    .subhead { color:var(--navy); font-size:19px; margin:30px 0 16px; }
    .paint-result { margin-top:20px; background:#e9f5e8; border:1px solid #b9d8b7; border-radius:10px; padding:18px; }
    .paint-result h2 { color:#174b25; font-size:16px; margin:0 0 8px; }
    .paint-result strong { color:#174b25; font-size:29px; }
  </style>
</head>
<body>
  <main>
    <p class="service-name">ネクストワン専用</p>
    <h1>外壁面積・概算計算（建物1面）</h1>
    <p class="lead">足場のアンチ枚数と段数から、建物1面の外壁面積の目安を計算します。</p>
    <p class="lead">塗料の缶数計算も自動でできます。</p>
    <p class="notice">今回のプログラム作成では開口部・建物の凹凸には対応してないので、これから作り直していきます。<br>中田</p>

    <section aria-label="計算条件">
      <div class="field">
        <label for="count">横のアンチ枚数</label>
        <input id="count" type="number" min="0" step="1" value="10" inputmode="numeric">
        <span class="unit">枚</span>
      </div>
      <div class="field">
        <label for="plankLength">アンチ1枚の長さ</label>
        <input id="plankLength" type="number" min="0" step="0.01" value="1.8" inputmode="decimal">
        <span class="unit">m（初期値は 1.8 m。現場に合わせて変更できます）</span>
      </div>
      <div class="field">
        <label for="stages">足場の段数</label>
        <input id="stages" type="number" min="0" step="1" value="5" inputmode="numeric">
        <span class="unit">段</span>
      </div>
      <div class="field">
        <label for="stageHeight">1段の高さ</label>
        <input id="stageHeight" type="number" min="0" step="0.01" value="1.7" inputmode="decimal">
        <span class="unit">m（初期値は 1.7 m。現場に合わせて変更できます）</span>
      </div>
      <div class="formula">計算式：横幅（アンチ枚数 × 1枚の長さ）× 高さ（段数 × 1段の高さ）</div>
      <div class="result" aria-live="polite">
        <h2>建物1面の外壁面積・概算</h2>
        <output id="area">153.0 ㎡</output>
        <small id="details">横幅 18.0 m × 高さ 8.5 m</small>
      </div>

      <h2 class="subhead">塗料の概算</h2>
      <div class="field">
        <label for="paintRate">1㎡あたりの使用量（1回塗り）</label>
        <input id="paintRate" type="number" min="0" step="0.01" value="0.15" inputmode="decimal">
        <span class="unit">L／㎡（塗料の仕様書に合わせて変更）</span>
      </div>
      <div class="field">
        <label for="coats">塗り回数</label>
        <input id="coats" type="number" min="0" step="1" value="2" inputmode="numeric">
        <span class="unit">回</span>
      </div>
      <div class="field">
        <label for="canSize">塗料缶の容量</label>
        <input id="canSize" type="number" min="0" step="0.1" value="16" inputmode="decimal">
        <span class="unit">L／缶</span>
      </div>
      <div class="paint-result" aria-live="polite">
        <h2>必要な塗料（概算）</h2>
        <strong id="paintAmount">45.9 L</strong>
        <p id="canResult">16 L缶なら、3缶必要です（端数は切り上げ）。</p>
      </div>
    </section>
    <footer>数字を入力すると、結果は自動で更新されます。塗料の使用量は製品・下地・塗り方で変わるため、必ず製品の仕様書も確認してください。</footer>
  </main>
  <script>
    const fields = ["count", "plankLength", "stages", "stageHeight", "paintRate", "coats", "canSize"].map(id => document.getElementById(id));
    const number = input => Math.max(0, Number(input.value) || 0);
    function calculate() {
      const [count, plankLength, stages, stageHeight, paintRate, coats, canSize] = fields.map(number);
      const width = count * plankLength;
      const height = stages * stageHeight;
      const area = width * height;
      const paintAmount = area * paintRate * coats;
      document.getElementById("area").textContent = `${(width * height).toLocaleString("ja-JP", { minimumFractionDigits: 1, maximumFractionDigits: 1 })} ㎡`;
      document.getElementById("details").textContent = `横幅 ${width.toFixed(1)} m × 高さ ${height.toFixed(1)} m`;
      document.getElementById("paintAmount").textContent = `${paintAmount.toLocaleString("ja-JP", { minimumFractionDigits: 1, maximumFractionDigits: 1 })} L`;
      const cans = canSize > 0 ? Math.ceil(paintAmount / canSize) : 0;
      document.getElementById("canResult").textContent = canSize > 0
        ? `${canSize.toLocaleString("ja-JP")} L缶なら、${cans}缶必要です（端数は切り上げ）。`
        : "缶の容量を入力してください。";
    }
    fields.forEach(field => field.addEventListener("input", calculate));
    calculate();
  </script>
</body>
</html>
