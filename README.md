<!DOCTYPE html><html lang="zh">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>冠军预测系统 V20</title><style>
body {
    font-family: Arial;
    background: #0f172a;
    color: #fff;
    text-align: center;
    padding: 10px;
}
h1 {
    color: #38bdf8;
}
.box {
    background: #1e293b;
    padding: 15px;
    margin: 10px auto;
    border-radius: 10px;
    width: 95%;
    max-width: 500px;
}
button {
    padding: 10px 20px;
    margin: 5px;
    border: none;
    border-radius: 8px;
    background: #38bdf8;
    color: #000;
    font-size: 16px;
}
.result {
    font-size: 22px;
    margin-top: 10px;
    color: #facc15;
}
</style></head><body><h1>🏆 冠军预测系统 V20</h1><div class="box">
    <h3>📊 最新开奖</h3>
    <div id="latest">加载中...</div>
</div><div class="box">
    <h3>🤖 冠军预测（6个）</h3>
    <button onclick="predict()">开始预测</button>
    <div class="result" id="result">--</div>
</div><div class="box">
    <h3>⚙️ 模式</h3>
    <button onclick="setMode('online')">实时模式</button>
    <button onclick="setMode('offline')">离线模式</button>
    <div id="mode">当前：实时模式</div>
</div><script>

let mode = "online";

// 示例接口（可替换）
const API = "https://api.api68.com/pks/getPksHistoryList.do?lotCode=10001";

async function loadData() {
    if(mode === "offline"){
        document.getElementById("latest").innerText = "离线数据：5 3 8 1 6 9 2 7 10 4";
        return [5,3,8,1,6,9,2,7,10,4];
    }

    try {
        let res = await fetch(API);
        let data = await res.json();

        let nums = data.result.data[0].preDrawCode.split(",").map(Number);

        document.getElementById("latest").innerText = nums.join(" ");

        return nums;
    } catch(e){
        document.getElementById("latest").innerText = "接口失败，已切换离线";
        return [5,3,8,1,6,9,2,7,10,4];
    }
}

function predictAlgo(arr){
    // 简单概率+扰动算法（稳定版）
    let pool = [1,2,3,4,5,6,7,8,9,10];

    // 提高前3名权重
    let weight = {};
    pool.forEach(n => weight[n]=1);

    arr.slice(0,5).forEach(n=>{
        weight[n] += 3;
    });

    // 随机扰动
    let sorted = pool.sort((a,b)=>{
        return (weight[b] + Math.random()) - (weight[a] + Math.random());
    });

    return sorted.slice(0,6);
}

async function predict(){
    let data = await loadData();
    let res = predictAlgo(data);
    document.getElementById("result").innerText = res.join("  ");
}

function setMode(m){
    mode = m;
    document.getElementById("mode").innerText = "当前：" + (m==="online"?"实时模式":"离线模式");
}

loadData();

</script></body>
</html>
