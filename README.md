<html lang="id">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>MAP KECAMATAN MAPANGET</title>

  <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@600;700&family=Poppins:wght@300;400;500;600&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"/>

  <style>
    * { box-sizing: border-box; }

    html, body {
      margin: 0;
      padding: 0;
      height: 100%;
      font-family: 'Poppins', sans-serif;
      background: #071a3d;
      color: white;
    }

    body {
      display: flex;
      flex-direction: column;
    }

    .header {
      text-align: center;
      padding: 20px 16px 10px;
      background: linear-gradient(135deg, #08152f, #0f2b63, #0d3f83);
      border-bottom: 1px solid rgba(77,166,255,0.25);
    }

    .header h1 {
      margin: 0;
      font-family: 'Playfair Display', serif;
      font-size: clamp(24px, 4vw, 38px);
      letter-spacing: 0.5px;
      text-shadow: 0 0 12px rgba(77,166,255,0.35);
    }

    .header span {
      display: block;
      margin-top: 8px;
      font-size: clamp(15px, 2vw, 24px);
      color: #cfe8ff;
      font-weight: 500;
    }

    .layout {
      display: grid;
      grid-template-columns: 2fr 1fr;
      gap: 16px;
      padding: 16px;
      flex: 1;
    }

    #map {
      width: 100%;
      height: calc(100vh - 120px);
      min-height: 600px;
      border-radius: 22px;
      border: 3px solid #2f8fff;
      box-shadow:
        0 0 20px rgba(47,143,255,0.35),
        0 0 40px rgba(47,143,255,0.10);
      overflow: hidden;
    }

    .side {
      display: flex;
      flex-direction: column;
      gap: 14px;
    }

    .card {
      padding: 18px;
      border-radius: 20px;
      background: rgba(255,255,255,0.06);
      border: 1px solid rgba(77,166,255,0.22);
      backdrop-filter: blur(10px);
      box-shadow: 0 10px 24px rgba(0,0,0,0.12);
    }

    .card h3 {
      margin: 0 0 10px;
      color: #9fd2ff;
      font-size: 18px;
    }

    .card p, .card li {
      font-size: 14px;
      line-height: 1.7;
      color: #eef6ff;
    }

    .card ul {
      margin: 0;
      padding-left: 18px;
    }

    .btn-cute {
      display: inline-block;
      margin-top: 12px;
      padding: 10px 18px;
      border-radius: 25px;
      background: linear-gradient(135deg, #208bff, #56acff);
      color: white;
      font-size: 14px;
      cursor: pointer;
      border: none;
      transition: 0.3s;
      box-shadow: 0 8px 18px rgba(77,166,255,0.25);
      font-weight: 600;
    }

    .btn-cute:hover {
      transform: scale(1.03);
      background: linear-gradient(135deg, #4ea9ff, #7bbfff);
    }

    .mini-info {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 10px;
      margin-top: 12px;
    }

    .pill {
      background: rgba(255,255,255,0.05);
      border: 1px solid rgba(77,166,255,0.18);
      border-radius: 16px;
      padding: 10px 12px;
      font-size: 13px;
      color: #e9f4ff;
    }

    .footer {
      text-align: center;
      padding: 10px 16px 18px;
      font-size: 12px;
      color: #cfe8ff;
      opacity: 0.9;
    }

    .leaflet-control-zoom {
      border: none !important;
      box-shadow: 0 8px 24px rgba(0,0,0,0.22) !important;
      border-radius: 16px !important;
      overflow: hidden;
    }

    .leaflet-control-zoom a {
      background: rgba(255,255,255,0.90) !important;
      color: #0d4ed8 !important;
      font-weight: 700;
      width: 36px !important;
      height: 36px !important;
      line-height: 36px !important;
      font-size: 22px !important;
    }

    .leaflet-control-layers {
      border-radius: 16px !important;
      overflow: hidden;
      border: 1px solid rgba(77,166,255,0.25) !important;
      box-shadow: 0 10px 24px rgba(0,0,0,0.18) !important;
    }

    .leaflet-control-layers-expanded {
      background: rgba(255,255,255,0.96) !important;
      color: #10284d !important;
      padding: 12px 14px !important;
      max-height: 280px;
      overflow-y: auto;
      font-size: 13px;
    }

    .leaflet-popup-content-wrapper {
      border-radius: 16px;
      background: rgba(255,255,255,0.97);
      color: #0f172a;
      border: 1px solid rgba(77,166,255,0.20);
      box-shadow: 0 10px 24px rgba(0,0,0,0.22);
    }

    .leaflet-popup-tip {
      background: rgba(255,255,255,0.97);
    }

    .popup-title {
      font-weight: 700;
      font-size: 15px;
      color: #0d4ed8;
      margin-bottom: 6px;
    }

    .popup-sub {
      font-size: 12px;
      font-weight: 600;
      color: #2563eb;
      margin-bottom: 6px;
    }

    .popup-desc {
      font-size: 13px;
      line-height: 1.6;
      color: #334155;
    }

    .note {
      margin-top: 10px;
      font-size: 12px;
      color: #dbeafe;
    }

    @media (max-width: 980px) {
      .layout {
        grid-template-columns: 1fr;
      }

      #map {
        height: 500px;
        min-height: 500px;
      }
    }
  </style>
</head>
<body>

  <div class="header">
    <h1>🗺️ MAP KECAMATAN MAPANGET</h1>
    <span>Kelurahan Paniki Dua 💙</span>
  </div>

  <div class="layout">
    <div id="map"></div>

    <div class="side">
      <div class="card">
        <h3>✨ Tentang Peta</h3>
        <p>
          Peta ini menampilkan wilayah Paniki Dua, Kecamatan Mapanget, dengan fokus pada kawasan
          rawan banjir, rawan longsor, titik rawan spesifik, pemukiman, drainase, hunian dekat
          sempadan / tebing, lahan pengembangan, dan jalur evakuasi.
        </p>
        <button class="btn-cute" onclick="zoomToPanikiDua()">📍 Lihat Fokus Lokasi</button>
      </div>

      <div class="card">
        <h3>📌 Objek yang Dipetakan</h3>
        <ul>
          <li>Rawan Banjir – Drainase GPI</li>
          <li>Rawan Banjir – Kairagi Dua</li>
          <li>Rawan Banjir – Koridor A.A. Maramis</li>
          <li>Rawan Longsor – Tanah Coklat</li>
          <li>Rawan Longsor – Lereng Perumahan Baru</li>
          <li>Titik Rawan – Tanah Coklat Paniki Dua</li>
          <li>Pemukiman – Griya Paniki Indah</li>
          <li>Pemukiman – Royal Paniki Residence</li>
          <li>Pemukiman – Salak Asri</li>
          <li>Drainase – Saluran Utama GPI</li>
          <li>Drainase – Arah Kairagi</li>
          <li>Hunian Sempadan – Tanah Coklat</li>
          <li>Hunian Sempadan – Drainase GPI</li>
          <li>Lahan Pengembangan – Tanah Coklat</li>
          <li>Lahan Kosong – Paniki Dua Timur</li>
          <li>Jalur Evakuasi – A.A. Maramis</li>
          <li>Jalur Evakuasi – GPI</li>
        </ul>
      </div>

      <div class="card">
        <h3>👩‍💻 Pembuat</h3>
        <p>Nama: Diva Camelia</p>
        <p>NIM: 241011060014</p>
        <p>Sistem Informasi A</p>

        <div class="mini-info">
          <div class="pill">Tema: Biru elegan</div>
          <div class="pill">Basemap: Satellite</div>
          <div class="pill">Marker: Titik bulat biru</div>
          <div class="pill">Wilayah: Paniki Dua</div>
        </div>
      </div>

      <div class="card">
        <h3>⚠️ Catatan</h3>
        <p>
          Titik pada peta dibuat sebagai titik kerja pemetaan wilayah Paniki Dua berdasarkan nama
          lokasi yang kamu minta. Jika nanti kamu punya koordinat survei yang lebih presisi, angka
          koordinatnya bisa langsung diganti.
        </p>
        <div class="note">
          Jalankan dengan Live Server di VS Code dan upload ke GitHub Pages setelah disimpan.
        </div>
      </div>
    </div>
  </div>

  <div class="footer">
    dibuat dengan 💙 menggunakan Web GIS • Paniki Dua, Mapanget, Manado
  </div>

  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

  <script>
    const centerPanikiDua = [1.512130, 124.925439];
    const map = L.map('map', { zoomControl: true }).setView(centerPanikiDua, 15);

    // BASEMAP
    const esriSatellite = L.tileLayer(
      'https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}',
      {
        attribution: 'Tiles &copy; Esri',
        maxZoom: 19,
        crossOrigin: true
      }
    );

    const osm = L.tileLayer(
      'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
      {
        attribution: '&copy; OpenStreetMap contributors',
        maxZoom: 19,
        crossOrigin: true
      }
    );

    const esriLabels = L.tileLayer(
      'https://services.arcgisonline.com/ArcGIS/rest/services/Reference/World_Boundaries_and_Places/MapServer/tile/{z}/{y}/{x}',
      {
        attribution: 'Labels &copy; Esri'
      }
    );

    osm.addTo(map);
    esriSatellite.addTo(map);
    esriLabels.addTo(map);

    esriSatellite.on('tileerror', function () {
      console.log('Satellite gagal, OSM tetap aktif');
    });

    function popupTemplate(title, sub, desc) {
      return `
        <div class="popup-title">${title}</div>
        <div class="popup-sub">${sub}</div>
        <div class="popup-desc">${desc}</div>
      `;
    }

    function createBluePoint(lat, lng, title, sub, desc, zone = 90) {
      const group = L.layerGroup();

      L.circle([lat, lng], {
        radius: zone,
        color: '#38bdf8',
        weight: 2,
        fillColor: '#38bdf8',
        fillOpacity: 0.10
      }).addTo(group);

      L.circleMarker([lat, lng], {
        radius: 10,
        color: '#ffffff',
        weight: 3,
        fillColor: '#1d4ed8',
        fillOpacity: 1
      }).bindPopup(
        popupTemplate(title, sub, desc)
      ).addTo(group);

      return group;
    }

    const pusatLayer = L.layerGroup();
    const banjirLayer = L.layerGroup();
    const longsorLayer = L.layerGroup();
    const spesifikLayer = L.layerGroup();
    const pemukimanLayer = L.layerGroup();
    const drainaseLayer = L.layerGroup();
    const sempadanLayer = L.layerGroup();
    const lahanLayer = L.layerGroup();
    const evakuasiLayer = L.layerGroup();

    // PUSAT
    createBluePoint(
      1.512130, 124.925439,
      'Pusat Paniki Dua',
      'Titik acuan wilayah',
      'Titik pusat orientasi pemetaan untuk Kelurahan Paniki Dua, Kecamatan Mapanget, Kota Manado.',
      75
    ).addTo(pusatLayer);

    // RAWAN BANJIR
    createBluePoint(
      1.512420, 124.926540,
      'Rawan Banjir – Drainase GPI',
      'Kategori: genangan / drainase',
      'Area di sekitar drainase belakang Perumahan Griya Paniki Indah yang berpotensi mengalami genangan saat hujan deras.',
      110
    ).addTo(banjirLayer);

    createBluePoint(
      1.513020, 124.924620,
      'Rawan Banjir – Kairagi Dua',
      'Kategori: genangan perbatasan wilayah',
      'Titik ini mewakili area perbatasan Paniki Dua menuju Kairagi Dua yang cenderung lebih rendah dan rawan limpasan air.',
      105
    ).addTo(banjirLayer);

    createBluePoint(
      1.511650, 124.923900,
      'Rawan Banjir – Koridor A.A. Maramis',
      'Kategori: cekungan jalan utama',
      'Zona dekat koridor Jalan A.A. Maramis yang dapat mengalami genangan akibat limpasan permukaan dan aliran drainase yang lambat.',
      110
    ).addTo(banjirLayer);

    // RAWAN LONGSOR
    createBluePoint(
      1.510950, 124.923950,
      'Rawan Longsor – Tanah Coklat',
      'Kategori: lereng / tebing',
      'Kawasan Tanah Coklat dipetakan sebagai area rawan longsor karena berkaitan dengan lereng, tanah timbunan, dan aktivitas pembangunan.',
      100
    ).addTo(longsorLayer);

    createBluePoint(
      1.510420, 124.924420,
      'Rawan Longsor – Lereng Perumahan Baru',
      'Kategori: lereng berkembang',
      'Area lereng di belakang perumahan baru yang perlu diperhatikan, terutama saat curah hujan tinggi.',
      95
    ).addTo(longsorLayer);

    // TITIK RAWAN SPESIFIK
    createBluePoint(
      1.511160, 124.924520,
      'Titik Rawan – Tanah Coklat Paniki Dua',
      'Kategori: titik prioritas pengamatan',
      'Kompleks Tanah Coklat menjadi titik rawan spesifik yang penting untuk pengamatan karena sering dikaitkan dengan perubahan lahan dan potensi kerawanan.',
      90
    ).addTo(spesifikLayer);

    // PEMUKIMAN
    createBluePoint(
      1.512380, 124.926620,
      'Pemukiman – Griya Paniki Indah',
      'Kategori: perumahan',
      'Griya Paniki Indah menjadi salah satu kawasan pemukiman utama yang harus masuk dalam pemetaan Paniki Dua.',
      120
    ).addTo(pemukimanLayer);

    createBluePoint(
      1.512780, 124.922820,
      'Pemukiman – Royal Paniki Residence',
      'Kategori: perumahan terencana',
      'Titik ini mewakili kawasan Royal Paniki Residence sebagai bagian dari perkembangan pemukiman di Paniki Dua.',
      120
    ).addTo(pemukimanLayer);

    createBluePoint(
      1.511880, 124.927150,
      'Pemukiman – Salak Asri',
      'Kategori: perumahan',
      'Kawasan Salak Asri dimasukkan sebagai zona pemukiman yang relevan di wilayah Paniki Dua.',
      115
    ).addTo(pemukimanLayer);

    // DRAINASE
    createBluePoint(
      1.512460, 124.926760,
      'Drainase – Saluran Utama GPI',
      'Kategori: jaringan drainase',
      'Saluran utama di kawasan Griya Paniki Indah yang menjadi fokus pengamatan aliran air.',
      80
    ).addTo(drainaseLayer);

    createBluePoint(
      1.512920, 124.924980,
      'Drainase – Arah Kairagi',
      'Kategori: arah aliran drainase',
      'Titik ini mewakili saluran atau arah limpasan air menuju kawasan Kairagi.',
      80
    ).addTo(drainaseLayer);

    // HUNIAN SEMPADAN / TEBING
    createBluePoint(
      1.511000, 124.924260,
      'Hunian Sempadan – Tanah Coklat',
      'Kategori: hunian dekat lereng',
      'Hunian di belakang kawasan Tanah Coklat yang berada dekat lereng atau tebing.',
      85
    ).addTo(sempadanLayer);

    createBluePoint(
      1.512060, 124.926120,
      'Hunian Sempadan – Drainase GPI',
      'Kategori: hunian dekat saluran air',
      'Hunian yang berada dekat drainase besar di sekitar GPI dan perlu diperhatikan dalam konteks pengelolaan lingkungan.',
      85
    ).addTo(sempadanLayer);

    // LAHAN PENGEMBANGAN
    createBluePoint(
      1.510320, 124.923640,
      'Lahan Pengembangan – Tanah Coklat',
      'Kategori: lahan kosong / rencana rusun',
      'Area Tanah Coklat yang dipandang sebagai lahan pengembangan dan perlu diperhatikan dampaknya terhadap kondisi sekitar.',
      95
    ).addTo(lahanLayer);

    createBluePoint(
      1.511320, 124.927680,
      'Lahan Kosong – Paniki Dua Timur',
      'Kategori: lahan belum terbangun',
      'Titik ini mewakili lahan kosong di sisi timur Paniki Dua yang berpotensi untuk pengembangan baru.',
      95
    ).addTo(lahanLayer);

    // JALUR EVAKUASI
    L.polyline([
      [1.513200, 124.923000],
      [1.512700, 124.923450],
      [1.512150, 124.923900],
      [1.511650, 124.924200],
      [1.511120, 124.924500]
    ], {
      color: '#3b82f6',
      weight: 4,
      opacity: 0.95
    }).bindPopup(
      popupTemplate(
        'Jalur Evakuasi – A.A. Maramis',
        'Kategori: jalan utama',
        'Koridor Jalan A.A. Maramis sebagai jalur evakuasi utama dari wilayah Paniki Dua.'
      )
    ).addTo(evakuasiLayer);

    L.polyline([
      [1.512700, 124.926900],
      [1.512500, 124.926650],
      [1.512300, 124.926400],
      [1.512120, 124.926150]
    ], {
      color: '#60a5fa',
      weight: 4,
      opacity: 0.95
    }).bindPopup(
      popupTemplate(
        'Jalur Evakuasi – GPI',
        'Kategori: jalan masuk perumahan',
        'Jalur masuk Griya Paniki Indah yang dapat digunakan sebagai akses evakuasi lokal.'
      )
    ).addTo(evakuasiLayer);

    // Tambah semua layer
    pusatLayer.addTo(map);
    banjirLayer.addTo(map);
    longsorLayer.addTo(map);
    spesifikLayer.addTo(map);
    pemukimanLayer.addTo(map);
    drainaseLayer.addTo(map);
    sempadanLayer.addTo(map);
    lahanLayer.addTo(map);
    evakuasiLayer.addTo(map);

    // Fit ke semua layer
    const allLayers = L.featureGroup([
      pusatLayer,
      banjirLayer,
      longsorLayer,
      spesifikLayer,
      pemukimanLayer,
      drainaseLayer,
      sempadanLayer,
      lahanLayer,
      evakuasiLayer
    ]);
    map.fitBounds(allLayers.getBounds(), { padding: [50, 50] });

    // Layer control
    L.control.layers(
      {
        "🛰️ Satellite": esriSatellite,
        "🗺️ OpenStreetMap": osm
      },
      {
        "📍 Pusat Paniki Dua": pusatLayer,
        "💧 Rawan Banjir": banjirLayer,
        "⛰️ Rawan Longsor": longsorLayer,
        "⚠️ Titik Rawan Spesifik": spesifikLayer,
        "🏠 Pemukiman": pemukimanLayer,
        "🌀 Drainase": drainaseLayer,
        "🏘️ Hunian Sempadan": sempadanLayer,
        "🌱 Lahan Pengembangan": lahanLayer,
        "🛣️ Jalur Evakuasi": evakuasiLayer
      },
      { collapsed: false }
    ).addTo(map);

    function zoomToPanikiDua() {
      map.setView([1.512130, 124.925439], 15);
    }
  </script>
</body>
</html>
