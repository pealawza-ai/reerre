<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    
    <title>My Money App</title>
    <style>
        :root { --p: #5d5fef; --in: #2ecc71; --out: #e74c3c; }
        body { font-family: sans-serif; margin: 0; background: #f5f6fa; display: flex; justify-content: center; }
        .app { width: 100%; max-width: 500px; background: white; min-height: 100vh; display: flex; flex-direction: column; }
        .header { background: var(--p); color: white; padding: 40px 20px; text-align: center; border-radius: 0 0 25px 25px; }
        .balance { font-size: 2.5rem; font-weight: bold; }
        .content { padding: 20px; flex: 1; }
        input { width: 100%; padding: 15px; margin: 10px 0; border: 1px solid #ddd; border-radius: 12px; box-sizing: border-box; font-size: 1rem; }
        .btns { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
        button { padding: 15px; border: none; border-radius: 12px; color: white; font-weight: bold; cursor: pointer; }
        .btn-in { background: var(--in); } .btn-out { background: var(--out); }
        .history { margin-top: 20px; border-top: 1px solid #eee; padding-top: 10px; }
        .item { display: flex; justify-content: space-between; padding: 10px 0; border-bottom: 1px solid #f9f9f9; }
    </style>
</head>
<body>
    <div class="app">
        <div class="header">
            <div style="opacity: 0.8;">ยอดคงเหลือปัจจุบัน</div>
            <div class="balance">฿ <span id="total">0</span></div>
        </div>
        <div class="content">
            <input type="text" id="note" placeholder="บันทึกรายการ (เช่น ค่าข้าว)">
            <input type="number" id="amount" placeholder="จำนวนเงิน">
            <div class="btns">
                <button class="btn-in" onclick="add('in')">รายรับ</button>
                <button class="btn-out" onclick="add('out')">รายจ่าย</button>
            </div>
            <div class="history" id="history"></div>
            <button onclick="clearData()" style="background:none; color:#999; margin-top:20px; width:100%">ล้างข้อมูลทั้งหมด</button>
        </div>
    </div>

    <script>
        let data = JSON.parse(localStorage.getItem('wallet')) || [];
        function render() {
            let total = 0;
            const h = document.getElementById('history');
            h.innerHTML = '';
            data.forEach(i => {
                total += i.type === 'in' ? i.amount : -i.amount;
                h.innerHTML += `<div class="item"><span>${i.note}</span><span style="color:${i.type==='in'?'#2ecc71':'#e74c3c'}">${i.type==='in'?'+':'-'}${i.amount}</span></div>`;
            });
            document.getElementById('total').innerText = total.toLocaleString();
            localStorage.setItem('wallet', JSON.stringify(data));
        }
        function add(type) {
            const n = document.getElementById('note');
            const a = document.getElementById('amount');
            if(!a.value) return;
            data.unshift({ note: n.value || (type==='in'?'รายรับ':'รายจ่าย'), amount: parseFloat(a.value), type });
            n.value = ''; a.value = ''; render();
        }
        function clearData() { if(confirm('ล้างข้อมูล?')) { data = []; render(); } }
        render();
    </script>
</body>
</html>
