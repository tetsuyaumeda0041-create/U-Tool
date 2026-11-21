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

            // 終了目標時刻に終わらせるために必要な1ルートあたりの所要時間（分・秒）
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

            // ①グループ＋②グループの合計で人員が必要な場合の増員人数
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

            const safeManHours = Math.max(0.01, totalManHours);
            const requiredPph = totalWork / safeManHours;

            modeExtraHtml = `
                <div class="mode-extra-title">📦 物量ベース（PPH）詳細</div>
                <div class="mode-extra-row">🎯 必要な平均PPH：<span class="mode-extra-highlight">${Math.ceil(requiredPph)}</span> 個/h</div>
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
