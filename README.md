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
            <select id="now_min"></select>
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
            <select id="before_min"></select>
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
            <select id="after_min"></select>
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
    function updateNowHourOptions(preferredHour = null) {
        const ampmEl = document.getElementById('ampm');
        const hourSelect = document.getElementById('now_hour');
        if (!ampmEl || !hourSelect) return;

        const ampm = ampmEl.value;
        const currentVal = preferredHour !== null ? preferredHour : parseInt(hourSelect.value, 10);

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

    // 指定した select 要素に 0〜59 の1分刻みオプションを生成
    function populateMinuteSelect(selectId, defaultMin = 0) {
        const minSelect = document.getElementById(selectId);
        if (!minSelect) return;

        minSelect.innerHTML = '';
        for (let m = 0; m <= 59; m++) {
            const option = document.createElement('option');
            option.value = m;
            option.textContent = `${String(m).padStart(2, '0')}分`;
            minSelect.appendChild(option);
        }
        minSelect.value = defaultMin;
    }

    // すべての1分刻みプルダウンを初期化
    function initAllTimeSelects() {
        updateNowHourOptions(13);
        populateMinuteSelect('now_min', 30);
        populateMinuteSelect('before_min', 0);
        populateMinuteSelect('after_min', 30);
    }

    // DOM読み込み完了時と即時実行の両方で確実に初期化
    if (document.readyState === 'loading') {
        document.addEventListener('DOMContentLoaded', initAllTimeSelects);
    } else {
        initAllTimeSelects();
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
            const m = parseFloat(document.getElement
