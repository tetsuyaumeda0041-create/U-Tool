<style>
    .form-group {
        border: 1px solid #222;
        border-radius: 8px;
        padding: 14px;
        margin-bottom: 16px;
        background-color: #ffffff;
    }
    .now-clock-box {
        font-size: 1.35rem;
        font-weight: bold;
        color: #1565c0;
        background-color: #f0f7ff;
        border: 1px solid #bbdefb;
        border-radius: 6px;
        padding: 10px 12px;
        margin-top: 6px;
        display: flex;
        align-items: center;
        justify-content: space-between;
    }
    .now-clock-sub {
        font-size: 0.8rem;
        font-weight: normal;
        color: #555;
    }
    .countdown-card {
        background: #f3e5f5;
        border: 1px solid #ce93d8;
        border-radius: 8px;
        padding: 14px;
        margin-bottom: 14px;
    }
    .countdown-title {
        font-size: 0.9rem;
        font-weight: bold;
        color: #6a1b9a;
        margin-bottom: 4px;
    }
    .countdown-value {
        font-size: 1.45rem;
        font-weight: bold;
        color: #4a148c;
    }
    .mode-extra-card {
        background: #fff8e1;
        border: 1px solid #ffe082;
        border-radius: 8px;
        padding: 14px;
        margin-bottom: 14px;
    }
    .mode-extra-title {
        font-size: 0.9rem;
        font-weight: bold;
        color: #e65100;
        margin-bottom: 6px;
    }
    .mode-extra-row {
        font-size: 1.05rem;
        margin-bottom: 4px;
        color: #333;
    }
    .mode-extra-highlight {
        font-size: 1.45rem;
        font-weight: bold;
        color: #d84315;
    }
</style>

<!-- ================= 画面1：入力画面 ================= -->
<div id="screen-input" class="screen active">
    <h2>現場残業・ルート計算ツール</h2>

    <!-- 現在時刻（リアルタイム表示） -->
    <div class="form-group">
        <label>現在時刻（端末の時計と連動）</label>
        <div class="now-clock-box">
            <span id="live_now_time">--:--:--</span>
            <span class="now-clock-sub" id="live_now_ampm">--</span>
        </div>
        <div class="hint">※この現在時刻と「目標終了時刻」から残り時間・①グループ(14:30退勤)の時間を計算します</div>
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

    <!-- ① ①グループ人数 (14:30退勤) ※計算内で現在時刻と紐づけ -->
    <div class="form-group">
        <label>①グループ人数 (14:30退勤)</label>
        <div class="hint">※現在時刻から14:30までの稼働時間を計算時に自動紐づけします</div>
        <input type="number" id="g1_count" placeholder="例: 12" value="12">
    </div>

    <div class="form-group">
        <label>②グループ人数 (18:00退勤)</label>
        <input type="number" id="g2_count" placeholder="例: 8" value="8">
    </div>

    <!-- ④ 新規追加：目標終了時刻（何時・何分 1分刻みプルダウン） -->
    <div class="form-group">
        <label>目標終了時刻</label>
        <div class="hint">終了させたい時刻を「何時」「何分（1分刻み）」で選択してください</div>
        <div class="time-inputs">
            <select id="target_hour">
                <option value="0">0時</option><option value="1">1時</option><option value="2">2時</option><option value="3">3時</option>
                <option value="4">4時</option><option value="5">5時</option><option value="6">6時</option><option value="7">7時</option>
                <option value="8">8時</option><option value="9">9時</option><option value="10">10時</option><option value="11">11時</option>
                <option value="12">12時</option><option value="13">13時</option><option value="14">14時</option><option value="15" selected>15時</option>
                <option value="16">16時</option><option value="17">17時</option><option value="18">18時</option><option value="19">19時</option>
                <option value="20">20時</option><option value="21">21時</option><option value="22">22時</option><option value="23">23時</option>
            </select>
            <select id="target_min">
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

    <button onclick="calculateAndSwitch()">計算する</button>
</div>


<!-- ================= 画面2：結果表示画面 ================= -->
<div id="screen-result" class="screen">
    <h2>【計算結果】</h2>

    <!-- 現在時刻から目標終了時刻までの残り時間 -->
    <div class="countdown-card">
        <div class="countdown-title">⏳ 目標終了時刻（<span id="res_target_clock">--:--</span>）までの残り時間</div>
        <div class="countdown-value" id="res_remaining_hms">--時間 --分 --秒</div>
    </div>

    <!-- 計算モード別の専用表示エリア -->
    <div class="mode-extra-card" id="res_mode_extra">
        <!-- ルート数ベース or 物量ベース(PPH) に応じて自動切り替え -->
    </div>
    
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
    let activeTargetDate = null;

    // 端末の現在時刻を表示（1秒ごとに更新）
    function updateLiveClock() {
        const now = new Date();
        const hours24 = now.getHours();
        const minutes = now.getMinutes();
        const seconds = now.getSeconds();

        const ampmText = hours24 < 12 ? 'AM' : 'PM';
        const timeText = `${String(hours24).padStart(2, '0')}:${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}`;

        const timeEl = document.getElementById('live_now_time');
        const ampmEl = document.getElementById('live_now_ampm');
        if (timeEl) timeEl.textContent = timeText;
        if (ampmEl) ampmEl.textContent = ampmText;

        updateResultRemainingCountdown(now);
    }

    // 「目標終了時刻」プルダウンから Date を取得（現在時刻より前なら翌日扱い）
    function getTargetEndDate(baseNow) {
        const targetHour = parseInt(document.getElementById('target_hour').value, 10) || 0;
        const targetMin = parseInt(document.getElementById('target_min').value, 10) || 0;

        const target = new Date(
            baseNow.getFullYear(),
            baseNow.getMonth(),
            baseNow.getDate(),
            targetHour,
            targetMin,
            0,
            0
        );

        if (target.getTime() <= baseNow.getTime()) {
            target.setDate(target.getDate() + 1);
        }
        return target;
    }

    // 結果画面の「残り何時間何分何秒」表示を更新
    function updateResultRemainingCountdown(now = new Date()) {
        if (!activeTargetDate) return;
        const resEl = document.getElementById('res_remaining_hms');
        if (!resEl) return;

        const diffMs = Math.max(0, activeTargetDate.getTime() - now.getTime());
        const totalSec = Math.floor(diffMs / 1000);
        const h = Math.floor(totalSec / 3600);
        const m = Math.floor((totalSec % 3600) / 60);
        const s = totalSec % 60;

        resEl.textContent = `${h}時間 ${String(m).padStart(2, '0')}分 ${String(s).padStart(2, '0')}秒`;
    }

    // 現在時刻（startDate）から指定の退勤・区切り時刻（cutoffHour:cutoffMin）までの稼働時間（h）を算出
    function getGroupWorkHoursFromNow(startDate, endDate, cutoffHour, cutoffMin) {
        if (endDate <= startDate) return 0;

        const cutoff = new Date(
            startDate.getFullYear(),
            startDate.getMonth(),
            startDate.getDate(),
            cutoffHour,
            cutoffMin,
            0,
            0
        );

        if (startDate >= cutoff) return 0;
        const effectiveEnd = endDate < cutoff ? endDate : cutoff;
        return Math.max(0, (effectiveEnd.getTime() - startDate.getTime()) / 3600000);
    }

    function initClock() {
        updateLiveClock();
        setInterval(updateLiveClock, 1000);
    }

    if (document.readyState === 'loading') {
        document.addEventListener('DOMContentLoaded', initClock);
    } else {
        initClock();
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
                <label id="speed-label">現在の平均PPH (1人1時間の処理数)</label>
                <div class="hint">※1時間あたりの処理個数を入力してください</div>
                <input type="number" id="speed_pph" value="400">
            `;
            document.getElementById('total_work').value = 2450;
        }
    }

    function calculateAndSwitch() {
        const now = new Date();
        activeTargetDate = getTargetEndDate(now);

        const targetHour = parseInt(document.getElementById('target_hour').value, 10) || 0;
        const targetMin = parseInt(document.getElementById('target_min').value, 10) || 0;
        document.getElementById('res_target_clock').textContent =
            `${String(targetHour).padStart(2, '0')}:${String(targetMin).padStart(2, '0')}`;
        updateResultRemainingCountdown(now);

        const mode = document.getElementById('mode').value;
        const totalWork = parseFloat(document.getElementById('total_work').value) || 0;
        const g1 = parseFloat(document.getElementById('g1_count').value) || 0;
        const g2 = parseFloat(document.getElementById('g2_count').value) || 0;

        // 現在時刻から目標終了時刻までの総時間（h）
        const totalRemainingHours = Math.max(0.0001, (activeTargetDate.getTime() - now.getTime()) / 3600000);

        // ①グループ（14:30退勤）と現在時刻の紐づけ：現在時刻から14:30までの稼働時間（h）と、14:30〜目標終了時刻までの超過時間（h）
        const g1Before1430Hours = getGroupWorkHoursFromNow(now, activeTargetDate, 14, 30);
        const after1430Hours = Math.max(0, totalRemainingHours - g1Before1430Hours);

        // ②グループ（18:00退勤）の現在時刻から目標終了時刻までの稼働時間（h）
        const g2WorkHours = getGroupWorkHoursFromNow(now, activeTargetDate, 18, 0);

        // ①グループは定時＋最大25分（25/60h）まで稼働できる前提の最大稼働時間
        const g1MaxHours = Math.min(totalRemainingHours, g1Before1430Hours + (25 / 60));
        const totalManHours = (g1 * g1MaxHours) + (g2 * g2WorkHours);

        let totalOtHours = 0;
        let perPersonMin = 0;
        let modeExtraHtml = "";

        if (mode === 'route') {
            const m = parseInt(document.getElementById('speed_min').value, 10) || 0;
            const s = parseInt(document.getElementById('speed_sec').value, 10) || 0;
            const speedMinutes = Math.max(0.0167, m + (s / 60));

            const routesPerHr = 60 / speedMinutes;
            const procBefore1430 = (g1 + g2) * routesPerHr * g1Before1430Hours;
            const remAt1430 = totalWork - procBefore1430;
            const procG2After1430 = g2 * routesPerHr * after1430Hours;
            const remOtRoutes = Math.max(0, remAt1430 - procG2After1430);

            const totalOtMin = remOtRoutes * speedMinutes;
            totalOtHours = totalOtMin / 60;
            if (g1 > 0) {
                perPersonMin = (totalOtHours / g1) * 60;
            }

            // 目標終了時刻までに終わらせるために必要な1ルートあたりの所要時間（分・秒）
            let requiredRouteText = "達成不要（余裕あり）";
            if (totalWork > 0 && totalManHours > 0) {
                const reqMinutesPerRoute = (totalManHours * 60) / totalWork;
                let reqMin = Math.floor(reqMinutesPerRoute);
                let reqSec = Math.round((reqMinutesPerRoute - reqMin) * 60);
                if (reqSec === 60) {
                    reqMin += 1;
                    reqSec = 0;
                }
                requiredRouteText = `${reqMin}分 ${reqSec}秒 / ルート`;
            }

            // ①グループ＋②グループの合計人数で目標終了時刻までに終わらない場合の必要増員人数
            const currentCapacityRoutes = totalManHours * routesPerHr;
            const shortageRoutes = Math.max(0, totalWork - currentCapacityRoutes);
            const shortageHours = shortageRoutes / routesPerHr;
            const additionalPeople = shortageHours > 0 ? Math.ceil(shortageHours / totalRemainingHours) : 0;

            modeExtraHtml = `
                <div class="mode-extra-title">🚚 ルート数ベース詳細</div>
                <div class="mode-extra-row">⏱ 現在の設定：<strong>${m}分 ${s}秒</strong></div>
                <div class="mode-extra-row">🎯 目標達成に必要な1ルート所要時間：<span class="mode-extra-highlight">${requiredRouteText}</span></div>
                <div class="mode-extra-row">➕ 増員人数（①＋②合計基準）：<span class="mode-extra-highlight">${additionalPeople} 人</span></div>
            `;
        } else {
            const pph = parseFloat(document.getElementById('speed_pph').value) || 1;
            const procBefore1430 = (g1 + g2) * pph * g1Before1430Hours;
            const remAt1430 = totalWork - procBefore1430;
            const procG2After1430 = g2 * pph * after1430Hours;
            const remForOt = Math.max(0, remAt1430 - procG2After1430);

            totalOtHours = remForOt / pph;
            if (g1 > 0) {
                perPersonMin = (totalOtHours / g1) * 60;
            }

            // 目標終了時刻までに完了させるために必要な1人あたりの平均PPH
            const safeManHours = Math.max(0.01, totalManHours);
            const requiredPph = totalWork > 0 ? Math.ceil(totalWork / safeManHours) : 0;

            // ①グループ＋②グループの合計人数で目標終了時刻までに終わらない場合の必要増員人数
            const currentCapacityItems = totalManHours * pph;
            const shortageItems = Math.max(0, totalWork - currentCapacityItems);
            const shortageHours = shortageItems / pph;
            const additionalPeoplePph = shortageHours > 0 ? Math.ceil(shortageHours / totalRemainingHours) : 0;

            modeExtraHtml = `
                <div class="mode-extra-title">📦 物量ベース（PPH）詳細</div>
                <div class="mode-extra-row">🎯 目標達成に必要な1人あたりPPH：<span class="mode-extra-highlight">${requiredPph}</span> 個/h</div>
                <div class="mode-extra-row">➕ 増員人数（①＋②合計基準）：<span class="mode-extra-highlight">${additionalPeoplePph} 人</span></div>
                <div class="mode-extra-row" style="font-size: 0.9rem; color: #555;">（入力した現在の平均PPH：${pph} 個/h）</div>
            `;
        }

        document.getElementById('res_mode_extra').innerHTML = modeExtraHtml;
        document.getElementById('res_total_ot').innerText = totalOtHours.toFixed(2);
        document.getElementById('res_per_person').innerText = perPersonMin.toFixed(1);

        let adviceHtml = "";
        if (perPersonMin <= 0) {
            adviceHtml = "<strong>【判定：残業なし】</strong><br>目標終了時刻内に余裕で完了できます！残業募集は不要です。";
        } else if (perPersonMin <= 25) {
            adviceHtml = "<strong>【判定：目標達成可能】</strong><br>①グループ全員に約 <strong>" + Math.ceil(perPersonMin) + "分</strong> 残ってもらえば目標終了時刻に完了します（25分制限内OK）。";
        } else {
            adviceHtml = "<strong>⚠️【警告：制限時間超過】</strong><br>必要な残業時間が25分を超えています！人員追加やペースアップが必要です。";
        }
        document.getElementById('res_advice').innerHTML = adviceHtml;

        document.getElementById('screen-input').classList.remove('active');
        document.getElementById('screen-result').classList.add('active');
        window.scrollTo(0, 0);
    }

    function backToInput() {
        activeTargetDate = null;
        document.getElementById('screen-result').classList.remove('active');
        document.getElementById('screen-input').classList.add('active');
        window.scrollTo(0, 0);
    }
</script>
