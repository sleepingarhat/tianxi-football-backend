# tianxi-football-backend

天喜足球引擎 · 服務層（對應賽馬 `tianxi-backend`，Cloudflare Worker）

## 職責
- Elo / pi-rating 逐場迭代更新（禁止全歷史重擬合，跨季回歸 `Elo_next = R×0.75 + 0.25×聯盟平均`）
- Poisson / Dixon-Coles 攻防參數擬合 → 比分分布 → 1X2 邊際
- LGB 推理、α 集成（`blendZ = α·lgbZ + (1−α)·eloZ`）、Platt／isotonic 校準
- 紅黃綠燈鎖定 API：T-24h 暫定（黃）→ T-1h 鎖定（綠）→ 賽後對帳；快照存指紋＋凍結時間
- admin 分析端點（只限內部頁）

## 鐵律
- `market_beta = 0`：賠率只作基準線、離線對照軌、殘差診斷，永不進入排名
- 凍結範圍連「揀邊個盤口」規則一齊凍結並公示
- 所有歷史預測公開，包括差的預測同虧損期，不刪不改

## 狀態
S0 骨架。基準線見 `tianxi-football-database/snapshots/baseline_snapshot.json`：
prior_asof RPS 0.2261（S2 必須打贏），market_devig RPS 0.2047（最終逼近目標）。
