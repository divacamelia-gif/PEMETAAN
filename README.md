<!doctype html>
<html lang="id">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>MAP KECAMATAN MAPANGET</title>

  <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@600;700&family=Poppins:wght@300;400;500;600&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"/>

  <style>
    *{ box-sizing:border-box; }

    body{
      margin:0;
      font-family:'Poppins', sans-serif;
      background:linear-gradient(135deg, #08152f, #102a59, #0d3f83);
      color:white;
    }

    .wrapper{
      max-width:1280px;
      margin:0 auto;
      padding:18px;
    }

    .header{
      text-align:center;
      padding:24px 12px 14px;
    }

    .header h1{
      margin:0;
      font-family:'Playfair Display', serif;
      font-size:clamp(24px, 4vw, 38px);
      letter-spacing:0.5px;
      text-shadow:0 0 10px rgba(77,166,255,0.35);
    }

    .header span{
      display:block;
      margin-top:8px;
      font-size:clamp(16px, 2.3vw, 24px);
      font-weight:500;
      color:#cfe8ff;
    }

    .grid{
      display:grid;
      grid-template-columns: 2fr 1fr;
      gap:18px;
      align-items:start;
    }

    #map{
      height:620px;
      border-radius:24px;
      border:3px solid #44a6ff;
      box-shadow:
        0 0 20px rgba(68,166,255,0.45),
        0 0 40px rgba(68,166,255,0.12);
      overflow:hidden;
    }

    .side{
      display:flex;
      flex-direction:column;
      gap:16px;
    }

    .card{
      padding:18px 18px 20px;
      border-radius:22px;
      background:rgba(255,255,255,0.08);
      backdrop-filter:blur(10px);
      border:1px solid rgba(77,166,255,0.35);
      box-shadow:0 10px 24px rgba(0,0,0,0.12);
      transition:0.3s;
    }

    .card:hover{
      transform:translateY(-4px);
      box-shadow:0 14px 28px rgba(77,166,255,0.22);
    }

    .card h3{
      margin:0 0 10px;
      color:#9ed0ff;
      font-size:18px;
    }

    .card p, .card li{
      font-size:14px;
      line-height:1.7;
      color:#f3f8ff;
    }

    .card ul{
      margin:0;
      padding-left:18px;
    }

    .btn-cute{
      display:inline-block;
      margin-top:12px;
      padding:10px 18px;
      border-radius:25px;
      background:linear-gradient(135deg, #3d9cff, #63b5ff);
      color:white;
      font-size:14px;
      cursor:pointer;
      border:none;
      transition:0.3s;
      box-shadow:0 8px 18px rgba(77,166,255,0.28);
      font-weight:600;
    }

    .btn-cute:hover{
      background:linear-gradient(135deg, #67b8ff, #8ac9ff);
      transform:scale(1.04);
    }

    .mini-info{
      display:grid;
      grid-template-columns:1fr 1fr;
      gap:10px;
      margin-top:10px;
    }

    .pill{
      background:rgba(255,255,255,0.08);
      border:1px solid rgba(77,166,255,0.28);
      border-radius:16px;
      padding:10px 12px;
      font-size:13px;
      color:#eef7ff;
    }

    .footer{
      text-align:center;
      padding:18px 10px 22px;
      font-size:12px;
      opacity:0.85;
      color:#d7ebff;
    }

    .leaflet-popup-content-wrapper{
      border-radius:16px;
      background:rgba(10,26,58,0.95);
      color:white;
      border:1px solid rgba(77,166,255,0.35);
      box-shadow:0 10px 24px rgba(0,0,0,0.25);
    }

    .leaflet-popup-tip{
      background:rgba(10,26,58,0.95);
    }

    .popup-title{
      font-weight:700;
      font-size:15px;
      color:#8fd0ff;
      margin-bottom:6px;
    }

    .popup-desc{
      font-size:13px;
      line-height:1.6;
      color:#eff7ff;
    }

    .leaflet-control-layers{
      border-radius:16px !important;
      overflow:hidden;
      border:1px solid rgba(77,166,255,0.35) !important;
      box-shadow:0 10px 24px rgba(0,0,0,0.18) !important;
    }

    .leaflet-control-layers-expanded{
      background:rgba(255,255,255,0.96) !important;
      color:#12284d !important;
      padding:12px 14px !important;
      max-height:280px;
      overflow-y:auto;
    }

    .note{
      margin-top:10px;
      font-size:12px;
      color:#d4e8ff;
      opacity:0.9;
    }

    @media (max-width: 980px){
      .grid{
        grid-template-columns:1fr;
      }

      #map{
        height:500px;
      }
    }
  </style>
</head>
<body>

  <div class="wrapper">
    <div class="header">
      <h1>🗺️ MAP KECAMATAN MAPANGET</h1>
      <span>Kelurahan Paniki Dua 💙</span>
    </div>

    <div class="grid">
      <div id="map"></div>

      <div class="side">
        <div class="card">
          <h3>✨ Tentang Peta</h3>
          <p>
            Peta ini menampilkan wilayah Paniki Dua, Kecamatan Mapanget, dengan fokus pada
            kawasan rawan banjir, rawan longsor, pemukiman, drainase utama, lahan pengembangan,
            dan akses jalan lingkungan untuk kebutuhan pemetaan lapangan.
          </p>
          <button class="btn-cute" onclick="zoomToPanikiDua()">📍 Lihat Fokus Lokasi</button>
        </div>

        <div class="card">
          <h3>📌 Objek yang Dipetakan</h3>
          <ul>
            <li>Pin rawan banjir</li>
            <li>Pin rawan longsor</li>
            <li>Pin titik rawan spesifik Tanah Coklat</li>
            <li>Pin pemukiman / perumahan</li>
            <li>Garis drainase</li>
            <li>Pin hunian dekat sempadan / tebing</li>
            <li>Pin lahan kosong / pengembangan</li>
            <li>Garis jalur evakuasi</li>
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
            <div class="pill">Marker: Pin Maps</div>
            <div class="pill">Wilayah: Paniki Dua</div>
          </div>

          <button class="btn-cute" onclick="showMessage()">💬 Klik Aku!</button>
        </div>

        <div class="card">
          <h3>⚠️ Catatan</h3>
          <p>
            Sekarang wilayah tidak lagi ditandai dengan kotak besar, tapi memakai pin seperti
            di Maps agar tampilannya lebih rapi dan mudah dipahami.
          </p>
          <div class="note">
            Jalankan lewat <strong>Live Server</strong> supaya peta satelit tampil normal.
          </div>
        </div>
      </div>
    </div>

    <div class="footer">
      dibuat dengan 💙 menggunakan Web GIS • Paniki Dua, Mapanget, Manado
    </div>
  </div>

  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

  <script>
    const centerPanikiDua = [1.512130, 124.925439];
    const map = L.map('map', { zoomControl: true }).setView(centerPanikiDua, 15);

    // Basemap satellite
    const esriSatellite = L.tileLayer(
      'https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}',
      { attribution: 'Tiles &copy; Esri' }
    );

    const esriLabels = L.tileLayer(
      'https://services.arcgisonline.com/ArcGIS/rest/services/Reference/World_Boundaries_and_Places/MapServer/tile/{z}/{y}/{x}',
      { attribution: 'Labels &copy; Esri' }
    );

    esriSatellite.addTo(map);
    esriLabels.addTo(map);

    function popupTemplate(title, desc){
      return `
        <div class="popup-title">${title}</div>
        <div class="popup-desc">${desc}</div>
      `;
    }

    // Custom pin icon ala maps
    function makePin(iconUrl, size = 34){
      return L.icon({
        iconUrl: iconUrl,
        iconSize: [size, size],
        iconAnchor: [size / 2, size],
        popupAnchor: [0, -size]
      });
    }

    const bluePin = makePin('https://cdn-icons-png.flaticon.com/512/684/684908.png', 34);
    const redPin = makePin('https://cdn-icons-png.flaticon.com/512/684/684913.png', 34);
    const greenPin = makePin('https://cdn-icons-png.flaticon.com/512/447/447031.png', 32);
    const purplePin = makePin('https://cdn-icons-png.flaticon.com/512/2776/2776067.png', 32);
    const yellowPin = makePin('https://cdn-icons-png.flaticon.com/512/854/854878.png', 32);

    const pusatLayer = L.layerGroup();
    const banjirLayer = L.layerGroup();
    const longsorLayer = L.layerGroup();
    const rawanSpesifikLayer = L.layerGroup();
    const pemukimanLayer = L.layerGroup();
    const drainaseLayer = L.layerGroup();
    const sempadanLayer = L.layerGroup();
    const lahanLayer = L.layerGroup();
    const jalanLayer = L.layerGroup();

    // Pusat wilayah
    L.marker([1.512130, 124.925439], { icon: bluePin })
      .bindPopup(
        popupTemplate(
          'Pusat Paniki Dua',
          'Titik acuan wilayah Paniki Dua, Kecamatan Mapanget, Kota Manado.'
        )
      )
      .addTo(pusatLayer);

    // Rawan banjir + lingkaran tipis
    [
      [1.512650, 124.926200, 'Titik Rawan Banjir 1', 'Area genangan potensial di dekat jalur drainase utama.'],
      [1.511450, 124.925250, 'Titik Rawan Banjir 2', 'Kawasan yang dapat tergenang saat hujan deras.']
    ].forEach(item => {
      const [lat, lng, title, desc] = item;

      L.marker([lat, lng], { icon: redPin })
        .bindPopup(popupTemplate(title, desc))
        .addTo(banjirLayer);

      L.circle([lat, lng], {
        radius: 90,
        color: '#76d7ff',
        weight: 2,
        fillColor: '#76d7ff',
        fillOpacity: 0.08
      }).addTo(banjirLayer);
    });

    // Rawan longsor
    [
      [1.511150, 124.923750, 'Titik Rawan Longsor 1', 'Area lereng / tebing yang perlu diawasi saat musim hujan.'],
      [1.510650, 124.924200, 'Titik Rawan Longsor 2', 'Wilayah dengan potensi gangguan stabilitas lereng.']
    ].forEach(item => {
      const [lat, lng, title, desc] = item;

      L.marker([lat, lng], { icon: purplePin })
        .bindPopup(popupTemplate(title, desc))
        .addTo(longsorLayer);

      L.circle([lat, lng], {
        radius: 85,
        color: '#ff9a9a',
        weight: 2,
        fillColor: '#ffb8b8',
        fillOpacity: 0.08
      }).addTo(longsorLayer);
    });

    // Titik rawan spesifik
    L.marker([1.510950, 124.923950], { icon: yellowPin })
      .bindPopup(
        popupTemplate(
          'Titik Rawan Spesifik: Tanah Coklat',
          'Titik pengamatan penting pada kawasan Tanah Coklat yang berkaitan dengan kerawanan lingkungan dan perkembangan hunian.'
        )
      )
      .addTo(rawanSpesifikLayer);

    // Pemukiman
    [
      [1.512700, 124.922650, 'Zona Pemukiman Barat', 'Kawasan pemukiman padat di sisi barat Paniki Dua.'],
      [1.511950, 124.927150, 'Zona Pemukiman Timur', 'Wilayah perumahan dan hunian terencana di Paniki Dua.']
    ].forEach(item => {
      const [lat, lng, title, desc] = item;

      L.marker([lat, lng], { icon: greenPin })
        .bindPopup(popupTemplate(title, desc))
        .addTo(pemukimanLayer);

      L.circle([lat, lng], {
        radius: 110,
        color: '#95ffba',
        weight: 2,
        fillColor: '#95ffba',
        fillOpacity: 0.06
      }).addTo(pemukimanLayer);
    });

    // Hunian dekat sempadan / tebing
    [
      [1.511250, 124.924250, 'Hunian Dekat Tebing 1', 'Hunian yang dekat lereng dan perlu diperhatikan.'],
      [1.510850, 124.924650, 'Hunian Dekat Tebing 2', 'Titik hunian dekat sempadan / tebing.']
    ].forEach(item => {
      const [lat, lng, title, desc] = item;
      L.marker([lat, lng], { icon: purplePin })
        .bindPopup(popupTemplate(title, desc))
        .addTo(sempadanLayer);
    });

    // Lahan pengembangan
    L.marker([1.510350, 124.923900], { icon: yellowPin })
      .bindPopup(
        popupTemplate(
          'Lahan Kosong / Pengembangan',
          'Area lahan yang dapat diamati untuk rencana pembangunan baru.'
        )
      )
      .addTo(lahanLayer);

    L.circle([1.510350, 124.923900], {
      radius: 95,
      color: '#81ffe8',
      weight: 2,
      fillColor: '#81ffe8',
      fillOpacity: 0.07
    }).addTo(lahanLayer);

    // Drainase
    L.polyline([
      [1.513000, 124.926900],
      [1.512450, 124.926750],
      [1.511900, 124.926300],
      [1.511300, 124.925850],
      [1.510850, 124.925250]
    ], {
      color:'#00c8ff',
      weight:4
    }).bindPopup(
      popupTemplate(
        'Drainase Utama 1',
        'Jalur saluran utama untuk pengamatan debit air dan potensi sumbatan.'
      )
    ).addTo(drainaseLayer);

    L.polyline([
      [1.512600, 124.924200],
      [1.512150, 124.924850],
      [1.511750, 124.925400]
    ], {
      color:'#66dcff',
      weight:3
    }).bindPopup(
      popupTemplate(
        'Drainase Utama 2',
        'Cabang drainase yang menghubungkan pemukiman ke aliran utama.'
      )
    ).addTo(drainaseLayer);

    // Jalan evakuasi
    L.polyline([
      [1.513150, 124.922200],
      [1.512700, 124.923250],
      [1.512250, 124.924300],
      [1.511700, 124.925250],
      [1.511200, 124.926350]
    ], {
      color:'#ffffff',
      weight:4,
      opacity:0.9
    }).bindPopup(
      popupTemplate(
        'Jalur Evakuasi Utama',
        'Koridor jalan penting untuk akses lingkungan dan evakuasi darurat.'
      )
    ).addTo(jalanLayer);

    L.polyline([
      [1.512100, 124.927750],
      [1.511850, 124.926950],
      [1.511650, 124.926250]
    ], {
      color:'#dfefff',
      weight:3,
      opacity:0.95
    }).bindPopup(
      popupTemplate(
        'Jalan Lingkungan Tambahan',
        'Jalur penghubung antar-zona dalam wilayah Paniki Dua.'
      )
    ).addTo(jalanLayer);

    pusatLayer.addTo(map);
    banjirLayer.addTo(map);
    longsorLayer.addTo(map);
    rawanSpesifikLayer.addTo(map);
    pemukimanLayer.addTo(map);
    drainaseLayer.addTo(map);
    sempadanLayer.addTo(map);
    lahanLayer.addTo(map);
    jalanLayer.addTo(map);

    L.control.layers(
      { "🛰️ Satellite": esriSatellite },
      {
        "📍 Pusat Paniki Dua": pusatLayer,
        "💧 Rawan Banjir": banjirLayer,
        "⛰️ Rawan Longsor": longsorLayer,
        "⚠️ Titik Rawan Spesifik": rawanSpesifikLayer,
        "🏠 Pemukiman": pemukimanLayer,
        "🌀 Drainase": drainaseLayer,
        "🏘️ Hunian Sempadan": sempadanLayer,
        "🌱 Lahan Pengembangan": lahanLayer,
        "🛣️ Jalur Evakuasi": jalanLayer
      },
      { collapsed:false }
    ).addTo(map);

    function zoomToPanikiDua(){
      map.setView([1.512130, 124.925439], 15);
    }

    function showMessage(){
      alert("Haii dari Diva Camelia 💙✨\nSekarang penandanya sudah pakai pin ala Maps yaa");
    }
  </script>
</body>
</html>
