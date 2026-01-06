<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>排班計算器</title>
    <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 min-h-screen flex items-center justify-center p-6">
    <div class="bg-white p-8 rounded-xl shadow-lg w-full max-w-md">
        <h1 class="text-2xl font-bold mb-6 text-center text-gray-800">排班計算器</h1>
        <div class="space-y-4">
            <div>
                <label class="block text-sm font-medium text-gray-700">總班數</label>
                <input type="number" id="totalShifts" class="w-full p-3 border rounded-lg mt-1" placeholder="例如: 30">
            </div>
            <div>
                <label class="block text-sm font-medium text-gray-700">排班人數</label>
                <input type="number" id="totalPeople" class="w-full p-3 border rounded-lg mt-1" placeholder="例如: 7">
            </div>
            <button onclick="calculate()" class="w-full bg-blue-600 text-white py-3 rounded-lg font-bold hover:bg-blue-700 transition">開始計算</button>
        </div>
        <div id="result" class="mt-8 hidden">
            <table class="w-full border-collapse border border-gray-200 text-center">
                <thead>
                    <tr class="bg-gray-50">
                        <th class="border p-2">排班數</th>
                        <th class="border p-2">人數</th>
                    </tr>
                </thead>
                <tbody id="tbody"></tbody>
            </table>
        </div>
    </div>
    <script>
        function calculate() {
            const s = parseInt(document.getElementById('totalShifts').value);
            const p = parseInt(document.getElementById('totalPeople').value);
            if (!s || !p) return;
            const base = Math.floor(s / p);
            const rem = s % p;
            let h = '';
            if (rem > 0) h += `<tr><td class="border p-2">${base + 1} 班</td><td class="border p-2 text-blue-600 font-bold">${rem} 人</td></tr>`;
            if (p - rem > 0) h += `<tr><td class="border p-2">${base} 班</td><td class="border p-2 text-blue-600 font-bold">${p - rem} 人</td></tr>`;
            document.getElementById('tbody').innerHTML = h;
            document.getElementById('result').classList.remove('hidden');
        }
    </script>
</body>
</html>
