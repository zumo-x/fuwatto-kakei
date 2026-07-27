<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <title>ふわっと家計</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body {
      font-family: sans-serif;
      background: #f7f7f7;
      margin: 0;
      padding: 20px;
    }
    h1 {
      text-align: center;
    }
    .container {
      max-width: 500px;
      margin: auto;
      background: white;
      padding: 20px;
      border-radius: 12px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.1);
    }
    input, select, button {
      width: 100%;
      margin-top: 10px;
      padding: 10px;
      border-radius: 8px;
      border: 1px solid #ccc;
      font-size: 16px;
    }
    button {
      background: #4CAF50;
      color: white;
      border: none;
      cursor: pointer;
    }
    button:hover {
      background: #45a049;
    }
    .list {
      margin-top: 20px;
    }
    .item {
      display: flex;
      justify-content: space-between;
      padding: 8px 0;
      border-bottom: 1px solid #eee;
    }
    .total {
      margin-top: 20px;
      font-weight: bold;
      text-align: right;
    }
  </style>
</head>
<body>
  <h1>ふわっと家計</h1>
  <div class="container">
    <input type="text" id="title" placeholder="内容（例：ランチ）">
    <input type="number" id="amount" placeholder="金額">
    <select id="type">
      <option value="expense">支出</option>
      <option value="income">収入</option>
    </select>
    <button onclick="addItem()">追加</button>

    <div class="list" id="list"></div>
    <div class="total" id="total">合計: 0円</div>
  </div>

  <script>
    let items = JSON.parse(localStorage.getItem('items')) || [];

    function save() {
      localStorage.setItem('items', JSON.stringify(items));
    }

    function render() {
      const list = document.getElementById('list');
      const totalEl = document.getElementById('total');
      list.innerHTML = '';

      let total = 0;

      items.forEach((item, index) => {
        const div = document.createElement('div');
        div.className = 'item';

        const sign = item.type === 'expense' ? -1 : 1;
        total += item.amount * sign;

        div.innerHTML = `
          <span>${item.title} (${item.type === 'expense' ? '支出' : '収入'})</span>
          <span>${item.amount}円 <button onclick="deleteItem(${index})">削除</button></span>
        `;

        list.appendChild(div);
      });

      totalEl.textContent = '合計: ' + total + '円';
    }

    function addItem() {
      const title = document.getElementById('title').value;
      const amount = parseInt(document.getElementById('amount').value);
      const type = document.getElementById('type').value;

      if (!title || !amount) return alert('入力してください');

      items.push({ title, amount, type });
      save();
      render();

      document.getElementById('title').value = '';
      document.getElementById('amount').value = '';
    }

    function deleteItem(index) {
      items.splice(index, 1);
      save();
      render();
    }

    render();
  </script>
</body>
</html>
