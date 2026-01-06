<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>排班計算工具</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body { background-color: #f3f4f6; font-family: system-ui, -apple-system, sans-serif; }
        .card { background: white; border-radius: 1rem; box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1); }
    </style>
</head>
<body class="min-h-screen flex items-center justify-center p-4">

    <div class="card w-full max-w-md p-8">
        <!-- 修正 1: 標題改為排班計算工具 -->
        <h1 class="text-2xl font-bold text-gray-800 mb-6 text-center">排班計算工具</h1>
        
        <div class="space-y-4">
            <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">總班數</label>
                <input type="number" id="totalShifts" placeholder="請輸入總班數" 
                    class="w-full p-3 border border-gray-300 rounded-lg text-lg focus:ring-2 focus:ring-blue-500 outline-none">
            </div>
            
            <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">排班人數</label>
                <input type="number" id="totalPeople" placeholder="請輸入排班人數" 
                    class="w-full p-3 border border-gray-300 rounded-lg text-lg focus:ring-2 focus:ring-blue-500 outline-none">
            </div>

            <button onclick="calculateShifts()" 
                class="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 px-4 rounded-lg transition duration-200 shadow-md">
                開始計算
            </button>
        </div>

        <div id="resultArea" class="mt-8 hidden">
            <h2 class="text-lg font-semibold text-gray-800 mb-3 border-b pb-2">計算結果</h2>
            <div class="overflow-hidden border border-gray-200 rounded-lg">
                <table class="min-w-full divide-y divide-gray-200 text-center">
                    <thead class="bg-gray-50">
                        <tr>
                            <th class="px-4 py-3 text-sm font-medium text-gray-500 uppercase">排班數</th>
                            <!-- 修正 2: 欄位標題顯示為人數 -->
                            <th class="px-4 py-3 text-sm font-medium text-gray-500 uppercase">人數</th>
                        </tr>
                    </thead>
                    <tbody id="resultBody" class="bg-white divide-y divide-gray-200">
                        <!-- 動態結果 -->
                    </tbody>
                </table>
            </div>
            <p id="summaryText" class="mt-4 text-sm text-center text-gray-500"></p>
        </div>
    </div>

    <script>
        function calculateShifts() {
            const totalShifts = parseInt(document.getElementById('totalShifts').value);
            const totalPeople = parseInt(document.getElementById('totalPeople').value);
            const resultArea = document.getElementById('resultArea');
            const resultBody = document.getElementById('resultBody');

            if (!totalShifts || !totalPeople || totalPeople <= 0) return;

            resultBody.innerHTML = '';
            const baseShifts = Math.floor(totalShifts / totalPeople);
            const remainingShifts = totalShifts % totalPeople;

            // 顯示邏輯：先顯示班數較多的（多排一班的人）
            if (remainingShifts > 0) {
                const row = `<tr>
                    <td class="px-4 py-3 text-gray-900">${baseShifts + 1} 班</td>
                    <td class="px-4 py-3 text-blue-600 font-bold">${remainingShifts} 人</td>
                </tr>`;
                resultBody.innerHTML += row;
            }

            // 顯示排基礎班數的人
            const basePeopleCount = totalPeople - remainingShifts;
            if (basePeopleCount > 0) {
                const row = `<tr>
                    <td class="px-4 py-3 text-gray-900">${baseShifts} 班</td>
                    <td class="px-4 py-3 text-blue-600 font-bold">${basePeopleCount} 人</td>
                </tr>`;
                resultBody.innerHTML += row;
            }

            document.getElementById('summaryText').textContent = `總計: ${totalPeople} 人分擔 ${totalShifts} 個班次`;
            resultArea.classList.remove('hidden');
        }
    </script>
</body>
</html>
