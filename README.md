<style>
    .form-group {
        border: 1px solid #222;
        border-radius: 8px;
        padding: 14px;
        margin-bottom: 16px;
        background-color: #ffffff;
    }
</style>

<!-- ================= 画面1：入力画面 ================= -->
<div id="screen-input" class="screen active">
    <h2>現場残業・ルート計算ツール</h2>

    <!-- ① 現在時刻（AM / PM・1分刻み） -->
    <div class="form-group">
        <label>現在時刻 (AM / PM・1分刻み)</label>
        <div class="hint">AM：1:00〜11:59 ／ PM：12:00〜24:59（1分刻みで選択）</div>
        <div class="time-inputs">
            <select id="ampm" onchange="updateNowHourOptions()">
                <option value="AM">AM</option>
                <option value="PM" selected>PM</option>
            </select>
            <select id="now_hour">
                <option value="12">12時</option>
                <option value="13" selected>13時</option>
                <option value="14">14時</option>
                <option value="15">15時</option>
                <option value="16">16時</option>
                <option value="17">17時</option>
                <option value="18">18時</option>
                <option value="19">19時</option>
                <option value="20">20時</option>
                <option value="21">21時</option>
                <option value="22">22時</option>
                <option value="23">23時</option>
                <option value="24">24時</option>
            </select>
            <select id="now_min">
                <option value="0">00分</option><option value="1">01分</option><option value="2">02分</option><option value="3">03分</option><option value="4">04分</option>
                <option value="5">05分</option><option value="6">06分</option><option value="7">07分</option><option value="8">08分</option><option value="9">09分</option>
                <option value="10">10分</option><option value="11">11分</option><option value="12">12分</option><option value="13">13分</option><option value="14">14分</option>
                <option value="15">15分</option><option value="16">16分</option><option value="17">17分</option><option value="18">18分</option><option value="19">19分</option>
                <option value="20">20分</option><option value="21">21分</option><option value="22">22分</option><option value="23">23分</option><option value="24">24分</option>
                <option value="25">25分</option><option value="26">26分</option><option value="27">27分</option><option value="28">28分</option><option value="29">29分</option>
                <option value="30" selected>30分</option><option value="31">31分</option><option value="32">32分</option><option value="33">33分</option><option value="34">34分</option>
                <option value="35">35分</option><option value="36">36分</option><option value="37">37分</option><option value="38">38分</option><option value="39">39分</option>
                <option value="40">40分</option><option value="41">41分</option><option value="42">42分</option><option value="43">43分</option><option value="44">44分</option>
                <option value="45">45分</option><option value="46">46分</option><option value="47">47分</option><option value="48">48分</option><option value="49">49分</option>
                <option value="50">50分</option><option value="51">51分</option><option value="52">52分</option><option value="53">53分</option><option value="54">54分</option>
                <option value="55">55分</option><option value="56">56分</option><option value="57">57分</option><option value="58">58分</option><option value="59">59分</option>
            </select>
        </div>
    </div>

    <div class="form-group">
        <label>計算モード</label>
        <select id="mode" onchange="toggleMode()">
            <option value="route">ルート数ベース (分・秒指定)</option>
            <option value="item">物量ベース (PPH)</option>
        </select>
    </div>

    <div class="form-group">
        <label id="total-label">残りの総ルート数</label>
        <input type="number" id="total_work" placeholder="例: 390" value="390">
    </div>

    <!-- 1ルートあたりの所要時間（分・秒選択式） -->
    <div class="form-group" id="speed-group">
        <label id="speed-label">1ルートあたりの所要時間</label>
        <div class="hint">例：5分15秒なら「5分」と「15秒」を選択します</div>
        <div class="time-inputs">
            <select id="speed_min">
                <option value="1">1分</option>
                <option value="2">2分</option>
                <option value="3">3分</option>
                <option value="4" selected>4分</option>
                <option value="5">5分</option>
                <option value="6">6分</option>
                <option value="7">7分</option>
                <option value="8">8分</option>
                <option value="9">9分</option>
                <option value="10">10分</option>
            </select>
            <select id="speed_sec">
                <option value="0" selected>0秒 (.0)</option>
                <option value="15">15秒 (.25)</option>
                <option value="30">30秒 (.5)</option>
                <option value="45">45秒 (.75)</option>
            </select>
        </div>
    </div>

    <div class="form-group">
        <label>①グループ人数 (14:30退勤)</label>
        <input type="number" id="g1_count" placeholder="例: 12" value="12">
    </div>

    <div class="form-group">
        <label>②グループ人数 (18:00退勤)</label>
        <input type="number" id="g2_count" placeholder="例: 8" value="8">
    </div>

    <!-- ② 定時までの残り時間（何時間・1分刻みプルダウン） -->
    <div class="form-group">
        <label>定時までの残り時間</label>
        <div class="hint">例：13:30から14:30までなら「1時間」「00分」</div>
        <div class="time-inputs">
            <select id="before_hour">
                <option value="0">0時間</option>
                <option value="1" selected>1時間</option>
                <option value="2">2時間</option>
                <option value="3">3時間</option>
                <option value="4">4時間</option>
                <option value="5">5時間</option>
                <option value="6">6時間</option>
            </select>
            <select id="before_min">
                <option value="0" selected>00分</option><option value="1">01分</option><option value="2">02分</option><option value="3">03分</option><option value="4">04分</option>
                <option value="5">05分</option><option value="6">06分</option><option value="7">07分</option><option value="8">08分</option><option value="9">09分</option>
                <option value="10">10分</option><option value="11">11分</option><option value="12">12分</option><option value="13">13分</option><option value="14">14分</option>
                <option value="15">15分</option><option value="16">16分</option><option value="17">17分</option><option value="18">18分</option><option value="19">19分</option>
                <option value="20">20分</option><option value="21">21分</option><option value="22">22分</option><option value="23">23分</option><option value="24">24分</option>
                <option value="25">25分</option><option value="26">26分</option><option value="27">27分</option><option value="28">28分</option><option value="29">29分</option>
                <option value="30">30分</option><option value="31">31分</option><option value="32">32分</option><option value="33">33分</option><option value="34">34分</option>
                <option value="35">35分</option><option value="36">36分</option><option value="37">37分</option><option value="38">38分</option><option value="39">39分</option>
                <option value="40">40分</option><option value="41">41分</option><option value="42">42分</option><option value="43">43分</option><option value="44">44分</option>
                <option value="45">45分</option><option value="46">46分</option><option value="47">47分</option><option value="48">48分</option><option value="49">49分</option>
                <option value="50">50分</option><option value="51">51分</option><option value="52">52分</option><option value="53">53分</option><option value="54">54分</option>
                <option value="55">55分</option><option value="56">56分</option><option value="57">57分</option><option value="58">58分</option><option value="59">59分</option>
            </select>
        </div>
    </div>

    <!-- ③ 目標までの超過時間（何時間・1分刻みプルダウン） -->
    <div class="form-group">
        <label>目標までの超過時間</label>
        <div class="hint">例：14:30定時から15:00目標なら「0時間」「30分」</div>
        <div class="time-inputs">
            <select id="after_hour">
                <option value="0" selected>0時間</option>
                <option value="1">1時間</option>
                <option value="2">2時間</option>
                <option value="3">3時間</option>
            </select>
            <select id="after_min">
                <option value="0">00分</option><option value="1">01分</option><option value="2">02分</option><option value="3">03分</option><option value="4">04分</option>
                <option value="5">05分</option><option value="6">06分</option><option value="7">07分</option><option value="8">08分</option><option value="9">09分</option>
                <option value="10">10分</option><option value="11">11分</option><option value="12">12分</option><option value="13">13分</option><option value="14">14分</option>
                <option value="15">15分</option><option value="16">16分</option><option value="17">17分</option><option value="18">18分</option><option value="19">19分</option>
                <option value="20">20分</option><option value="21">21分</option><option value="22">22分</option><option value="23">23分</option><option value="24">24分</option>
                <option value="25">25分</option><option value="26">26分</option><option value="27">27分</option><option value="28">28分</option><option value="29">29分</option>
                <option value="30" selected>30分</option><option value="31">31分</option><option value="32">32分</option><option value="33">33分</option><option value="34">34分</option>
                <option value="35">35分</option><option value="36">36分</option><option value="37">37分</option><option value="38">38分</option><option value="39">39分</option>
                <option value="40">40分</option><option value="41">41分</option><option value="42">42分</option><option value="43">43分</option><option value="44">44分</option>
                <option value="45">45分</option><option value="46">46分</option><option value="47">47分</option><option value="48">48分</option><option value="49">49分</option>
                <option value="50">50分</option><option value="51">51分</option><option value="52">52分</option><option value="53">53分</option><option value="54">54分</option>
                <option value="55">55分</option><option value="56">56分</option><option value="57">57分</option><option value="58">58分</option><option value="59">59分</option>
            </select>
        </div>
    </div>

    <button onclick="calculateAndSwitch()">計算する</button>
</div>


<!-- ================= 画面2：結果表示画面 ================= -->
<div id="screen-result" class="screen">
    <h2>【計算結果】</h2>
    
    <div class="result-card">
        <div class="result-title">📌 必要な総残業時間（延べ）</div>
        <div><span id="res_total_ot" class="result-value">--</span> 時間</div>
    </div>

    <div class="result-card" style="background: #e3f2fd; border-color: #bbdefb;">
        <div class="result-title" style="color: #1565c0;">👤 ①グループ 1人あたりの残業</div>
        <div><span id="res_per_person" class="result-value" style="color: #0277bd;">--</span> 分</div>
    </div>

    <div class="advice-box" id="res_advice">
        <!-- 判定メッセージ -->
    </div>

    <button class="secondary-btn" onclick="backToInput()">← 入力画面に戻る・修正する</button>
</div>

<script>
    // AM/PM に応じて「時」の選択肢を更新（AM: 1〜11、PM: 12〜24）
    function updateNowHourOptions() {
        const ampm = document.getElementById('ampm').value;
        const hourSelect = document.getElementById('now_hour');
        const currentVal = parseInt(hourSelect.value, 10);

        const startHour = ampm === 'AM' ? 1 : 12;
        const endHour = ampm === 'AM' ? 11 : 24;

        hourSelect.innerHTML = '';
        for (let h = startHour; h <= endHour; h++) {
            const option = document.createElement('option');
            option.value = h;
            option.textContent = `${h}時`;
            hourSelect.appendChild(option);
        }

        if (currentVal >= startHour && currentVal <= endHour) {
            hourSelect.value = currentVal;
        } else {
            hourSelect.value = ampm === 'AM' ? 9 : 13;
        }
    }

    function getCurrentMinutesFromAmPm() {
        const hour = parseInt(document.getElementById('now_hour').value, 10) || 0;
        const minute = parseInt(document.getElementById('now_min').value, 10) || 0;
        return hour * 60 + minute;
    }

    function toggleMode() {
        const mode = document.getElementById('mode').value;
        const totalLabel = document.getElementById('total-label');
        const speedGroup = document.getElementById('speed-group');

        if (mode === 'route') {
            totalLabel.innerText = "残りの総ルート数";
            speedGroup.innerHTML = `
                <label id="speed-label">1ルートあたりの所要時間</label>
                <div class="hint">例：5分15秒なら「5分」と「15秒」を選択します</div>
                <div class="time-inputs">
                    <select id="speed_min">
                        <option value="1">1分</option><option value="2">2分</option><option value="3">3分</option><option value="4" selected>4分</option><option value="5">5分</option><option value="6">6分</option><option value="7">7分</option><option value="8">8分</option><option value="9">9分</option><option value="10">10分</option>
                    </select>
                    <select id="speed_sec">
                        <option value="0" selected>0秒 (.0)</option><option value="15">15秒 (.25)</option><option value="30">30秒 (.5)</option><option value="45">45秒 (.75)</option>
                    </select>
                </div>
            `;
            document.getElementById('total_work').value = 390;
        } else {
            totalLabel.innerText = "残りの総物量 (個)";
            speedGroup.innerHTML = `
                <label id="speed-label">平均PPH (1人1時間の処理数)</label>
                <div class="hint">※1時間あたりの処理個数を入力してください</div>
                <input type="number" id="speed_pph" value="400">
            `;
            document.getElementById('total_work').value = 2450;
        }
    }

    function calculateAndSwitch() {
        const mode = document.getElementById('mode').value;
        const totalWork = parseFloat(document.getElementById('total_work').value) || 0;
        const g1 = parseFloat(document.getElementById('g1_count').value) || 0;
        const g2 = parseFloat(document.getElementById('g2_count').value) || 0;

        // 何時間・何分（1分刻み）プルダウンから時間（h）へ変換
        const beforeHour = parseFloat(document.getElementById('before_hour').value) || 0;
        const beforeMin = parseFloat(document.getElementById('before_min').value) || 0;
        const tBefore = beforeHour + (beforeMin / 60);

        const afterHour = parseFloat(document.getElementById('after_hour').value) || 0;
        const afterMin = parseFloat(document.getElementById('after_min').value) || 0;
        const tAfter = afterHour + (afterMin / 60);

        let totalOtHours = 0;
        let perPersonMin = 0;

        if (mode === 'route') {
            const m = parseFloat(document.getElementById('speed_min').value) || 0;
            const s = parseFloat(document.getElementById('speed_sec').value) || 0;
            const speedMinutes = m + (s / 60);

            const routesPerHr = 60 / speedMinutes;
            const proc1430 = (g1 + g2) * routesPerHr * tBefore;
            const rem1430 = totalWork - proc1430;
            const procG2 = g2 * routesPerHr * tAfter;
            const remOtRoutes = Math.max(0, rem1430 - procG2);

            const totalOtMin = remOtRoutes * speedMinutes;
            totalOtHours = totalOtMin / 60;
            if (g1 > 0) {
                perPersonMin = (totalOtHours / g1) * 60;
            }
        } else {
            const pph = parseFloat(document.getElementById('speed_pph').value) || 1;
            const proc1430 = (g1 + g2) * pph * tBefore;
            const rem1430 = totalWork - proc1430;
            const procG2 = g2 * pph * tAfter;
            const remForOt = Math.max(0, rem1430 - procG2);

            totalOtHours = remForOt / pph;
            if (g1 > 0) {
                perPersonMin = (totalOtHours / g1) * 60;
            }
        }

        document.getElementById('res_total_ot').innerText = totalOtHours.toFixed(2);
        document.getElementById('res_per_person').innerText = perPersonMin.toFixed(1);

        let adviceHtml = "";
        if (perPersonMin <= 0) {
            adviceHtml = "<strong>【判定：残業なし】</strong><br>定時内に余裕で完了できます！残業募集は不要です。";
        } else if (perPersonMin <= 25) {
            adviceHtml = "<strong>【判定：目標達成可能】</strong><br>①グループ全員に約 <strong>" + Math.ceil(perPersonMin) + "分</strong> 残ってもらえば目標時間に完了します（25分制限内OK）。";
        } else {
            adviceHtml = "<strong>⚠️【警告：制限時間超過】</strong><br>必要な残業時間が25分を超えています！人数を増やすか、完了時間の見直しが必要です。";
        }
        document.getElementById('res_advice').innerHTML = adviceHtml;

        document.getElementById('screen-input').classList.remove('active');
        document.getElementById('screen-result').classList.add('active');
        window.scrollTo(0, 0);
    }

    function backToInput() {
        document.getElementById('screen-result').classList.remove('active');
        document.getElementById('screen-input').classList.add('active');
        window.scrollTo(0, 0);
    }
</script>
