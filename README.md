Web sinyal trading AlZzz.

      <div class="price-head">
        <div>
          <div class="sym" id="curSym">BTC/USDT</div>
        </div>
        <div class="price" id="curPrice">0.00</div>
        <div class="chg-pill up" id="curChg">+0.00%</div>
      </div>

      <div class="legend">
        <span><i style="background:#5b8cff"></i>MA 9</span>
        <span><i style="background:#ffb340"></i>MA 21</span>
        <span><i style="background:rgba(255,255,255,.28)"></i>Volume</span>
      </div>

      <div class="chart-wrap">
        <canvas id="chart"></canvas>
      </div>
    </section>

    <!-- ============ KANAN: PANEL SINYAL ============ -->
    <aside class="side">

      <div class="card signal-card" id="sigCard">
        <div class="sig-label">Sinyal Saat Ini</div>
        <div class="sig-main neutral" id="sigMain">NETRAL</div>
        <div class="sig-sub" id="sigSub">Menunggu konfirmasi indikator…</div>

        <div class="meter"><div class="meter-fill" id="meterFill" style="width:50%;background:#7b8798"></div></div>
        <div class="meter-txt"><span>Kekuatan Sinyal</span><span id="meterVal">50%</span></div>

        <div class="levels">
          <div class="lvl entry"><small>Entry</small><b id="lvEntry">—</b></div>
          <div class="lvl tp"><small>Take Profit</small><b id="lvTp">—</b></div>
          <div class="lvl sl"><small>Stop Loss</small><b id="lvSl">—</b></div>
        </div>
      </div>

      <div class="card ind-card">
        <div class="ind-title">Indikator Teknikal</div>
        <div class="ind-row"><span>MA 9 (Cepat)</span><b id="indFast">—</b></div>
        <div class="ind-row"><span>MA 21 (Lambat)</span><b id="indSlow">—</b></div>
        <div class="ind-row"><span>Tren MA</span><span class="badge n" id="indTrend">NETRAL</span></div>
        <div class="ind-row"><span>RSI (14)</span><b id="indRsi">—</b></div>
        <div class="ind-row"><span>Zona RSI</span><span class="badge n" id="indRsiZone">NORMAL</span></div>
        <div class="ind-row"><span>Momentum</span><span class="badge n" id="indMom">DATAR</span></div>
      </div>

      <div class="card hist-card">
        <div class="hist-head">
          <div class="hist-title">Riwayat Sinyal</div>
          <div class="winrate">Win rate: <b id="winRate">—</b></div>
        </div>
        <div class="hist-list" id="histList">
          <div class="empty">Belum ada sinyal terdeteksi</div>
        </div>
      </div>

    </aside>
  

  
