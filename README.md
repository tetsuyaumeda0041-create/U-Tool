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
