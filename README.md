<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>排班計算工具</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body { 
            background-color: #f3f4f6; 
            font-family: system-ui, -apple-system, "Segoe UI", Roboto, Helvetica, Arial, sans-serif; 
        }
        .card { 
            background: white; 
            border-radius: 1.25rem; 
            box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1), 0 8px 10px -6px rgba(0, 0, 0, 0.1); 
        }
        input::-webkit-outer-spin-button,
        input::-webkit-inner-spin-button {
            -webkit-appearance: none;
            margin: 0;
        }
    </style>
</head>
<body class="min-h-screen flex items-center justify-center p-4">

    <div class="card w-full max-w-md p-8">
        <!-- 修正標題 -->
        <h1 class="text-2xl font-bold text-gray-800 mb-8 text-center">排班計算工具</h1>
        
        <div class="space-y-5">
            <div>
                <label class="block text-sm font-semibold text-gray-600 mb-1.5 ml-1">總班數</label>
                <input type="number" id="totalShifts" placeholder="請輸入總班數" 
                    class="w-full p-3.5 border border-gray-300 rounded-xl text-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none transition-all">
            </div>
            
            <div>
                <label class="block text-sm font-semibold text-gray-600 mb-1.5 ml-1">排班人數</label>
                <input type="number" id="totalPeople" placeholder="請輸入排班人數" 
                    class="w-full p-3.5 border border-gray-300 rounded-xl text-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none transition-all">
            </div>

            <button onclick="calculateShifts()" 
                class="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-4 px-4 rounded-xl transition duration-200 shadow-lg active:transform active:scale-95">
                開始計算
            </button>
        </div>

        <!-- 結果顯示區 -->
        <div id="resultArea" class="mt-10 hidden">
            <h2 class="text-lg font-bold text-gray-700 mb-4 text-center border-b pb-2">計算結果</h2>
            <div class="overflow-hidden border border-gray-100 rounded-xl shadow-sm">
                <table class="min-w-full divide-y divide-gray-200 text-center">
                    <thead class="bg-gray-50">
                        <tr>
                            <th class="px-4 py-3.5 text-sm font-bold text-gray-500">排班數</th>
                            <th class="px-4 py-3.5 text-sm font-bold text-gray-500">人數</th>
                        </tr>
                    </thead>
                    <tbody id="resultBody" class="bg-white divide-y divide-gray-100 text-lg">
                        <!-- 結果將顯示於此 -->
                    </tbody>
                </table>
            </div>
            <p id="summaryText" class="mt-5 text-sm text-center text-gray-400 font-medium"></p>
        </div>
    </div>

    <script>
        function calculateShifts() {
            const totalShifts = parseInt(document.getElementById('totalShifts').value);
            const totalPeople = parseInt(document.getElementById('totalPeople').value);
            const resultArea = document.getElementById('resultArea');
            const resultBody = document.getElementById('resultBody');

            // 基礎驗證
            if (!totalShifts || !totalPeople || totalPeople <= 0) {
                return;
            }

            // 清空舊結果
            resultBody.innerHTML = '';
            
            const baseShifts = Math.floor(totalShifts / totalPeople);
            const remainingShifts = totalShifts % totalPeople;

            // 顯示邏輯：先顯示班數較多的組別
            if (remainingShifts > 0) {
                const row = `
                    <tr class="hover:bg-blue-50 transition-colors">
                        <td class="px-4 py-4 text-gray-700 font-medium">${baseShifts + 1} 班</td>
                        <td class="px-4 py-4 text-blue-600 font-black">${remainingShifts} 人</td>
                    </tr>`;
                resultBody.innerHTML += row;
            }

            // 顯示班數基礎的組別
            const basePeopleCount = totalPeople - remainingShifts;
            if (basePeopleCount > 0) {
                const row = `
                    <tr class="hover:bg-blue-50 transition-colors">
                        <td class="px-4 py-4 text-gray-700 font-medium">${baseShifts} 班</td>
                        <td class="px-4 py-4 text-blue-600 font-black">${basePeopleCount} 人</td>
                    </tr>`;
                resultBody.innerHTML += row;
            }

            document.getElementById('summaryText').textContent = `總共 ${totalShifts} 班 / ${totalPeople} 人`;
            resultArea.classList.remove('hidden');
        }
    </script>
</body>
</html>
