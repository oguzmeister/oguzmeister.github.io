<!DOCTYPE html>
<html lang="tr">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Pişti – Pembe Tema Blog Sayfası</title>
  <meta name="description" content="Pembe temalı Pişti blog sayfası: oyun ekranı, kurallar, puanlama ve strateji rehberi.">
  <style>
    body {
      margin: 0;
      font-family: 'Poppins', sans-serif;
      background: linear-gradient(180deg, #ffd1dc 0%, #ffb6c1 100%);
      color: #5a2a41;
      text-align: center;
    }

    header {
      background-color: #ff8fb2;
      color: white;
      padding: 20px;
      font-size: 28px;
      font-weight: bold;
      box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
    }

    main {
      max-width: 900px;
      margin: 40px auto;
      background: #ffe6ee;
      padding: 30px;
      border-radius: 20px;
      box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
    }

    img {
      width: 100%;
      border-radius: 16px;
      box-shadow: 0 8px 16px rgba(0, 0, 0, 0.15);
    }

    h2 {
      color: #d63384;
      border-bottom: 2px solid #ffa6c9;
      display: inline-block;
      padding-bottom: 5px;
    }

    p {
      font-size: 17px;
      line-height: 1.7;
      color: #6e3d53;
    }

    footer {
      margin-top: 40px;
      background: #ff8fb2;
      color: white;
      padding: 15px;
      border-radius: 0 0 20px 20px;
      font-size: 14px;
    }
  </style>
</head>
<body>
  <header>💖 Pişti Blog</header>
  <main>
    <h1>Pişti (Türk Kart Oyunu) – Pembe Temalı Rehber</h1>
    <p>Bu sayfa, Othello (Reversi) için hazırlanmış ders notlarının yapısını <em>Pişti</em> oyununa uyarlayarak sunar: tür, uygunluk, temel konular; ardından tanım, kurallar, strateji, tarihçe, algoritmik oyun kuramı eşlemesi, heuristik ve yapay zekâ yaklaşımları.</p>

    <img src="pişti.PNG" alt="Pişti oyunu ekran görüntüsü" />

    <h2>Özet</h2>
    <ul style="text-align:left; display:inline-block">
      <li><b>Tür:</b> Kart oyunu, 52'lik deste, tur bazlı</li>
      <li><b>Neden uygun?</b>
        <ul>
          <li>Basit kurallar + <em>pişti</em> ve özel kart puanlarıyla ilginç olasılık hesabı.</li>
          <li>Heuristik tabanlı arama ve <em>Monte Carlo</em> simülasyonlarıyla güçlü botlar yapılabilir.</li>
        </ul>
      </li>
      <li><b>Konular:</b>
        <ul>
          <li>Oyun ağacı/hamle simülasyonu (tam bilgi değil, <em>kısmi bilgi</em>)</li>
          <li>Alpha–beta (sınırlı derinlikte) veya <em>MCTS</em></li>
          <li>Tahmin (değerlendirme) fonksiyonları ve puan heuristikleri</li>
        </ul>
      </li>
    </ul>

    <h2>1. Oyunun Tanımı</h2>
    <p><b>Pişti</b>, iki (veya dörtlü eşli) oyuncuyla, 52 kartlık standart bir desteyle oynanan, yerdeki kartları toplama temelli bir oyundur. Amaç, el sonunda toplanan kartlardan ve <em>pişti</em> durumlarından en yüksek puanı elde etmektir.</p>

    <h2>2. Deste, Dağıtım ve Başlangıç</h2>
    <ul style="text-align:left; display:inline-block">
      <li><b>Deste:</b> Joker yoktur. As–Papaz–Kız–Vale–10–…–2.</li>
      <li><b>Dağıtım:</b> Her oyuncuya 4'er kart; ortaya desteden 3 kapalı + 1 açık kart bırakılır.</li>
      <li><b>Oyun sırası:</b> Genelde dağıtanın sağı başlar; herkes elindeki 4 kart bitince yeniden 4'er kart dağıtılır; deste bittiğinde tur kapanır.</li>
    </ul>

    <h2>3. Kurallar ve Oynanış</h2>
    <ol style="text-align:left; display:inline-block">
      <li>Oyuncular sırayla elinden bir kart atar.</li>
      <li>Atılan kartın <b>rütbesi</b> yerdeki en üst kartla eşleşirse, yerdeki tüm kartlar alınır.</li>
      <li><b>Vale (J)</b> <em>jokerimsi</em> davranır: Yerde en az bir kart varsa tek başına ortayı alır.</li>
      <li>Yerde <b>tek kart</b> varken eşleşmeyle ya da J ile almak <b>pişti</b> sayılır ve ekstra puan getirir.</li>
      <li>Kimse alamazsa kart, yerin üstüne kalır (yığın büyür).</li>
      <li>Deste bitince yerde kalan son yığın, <em>son alan</em> tarafa verilir ve puanlar hesaplanır.</li>
    </ol>

    <h2>4. Stratejik Özellikler</h2>
    <ul style="text-align:left; display:inline-block">
      <li><b>Pişti fırsatları kritiktir:</b> Yerde tek kart bırakmak risklidir; rakibin eşleşmesi ya da J'i olup olmadığını olasılıksal düşün.</li>
      <li><b>Değerli kartları koru:</b> ♦10 (+3) ve ♣2 (+2) gibi puan kartlarını gereksizce ortaya atma.</li>
      <li><b>Bilgi takibi:</b> Çıkan As/J sayısını ve görünen rütbeleri hatırla; bu, risk ve beklenti hesabını iyileştirir.</li>
      <li><b>Son yığın:</b> El sonunda son ortayı almak çoğu varyantta +3 getirir; planına dahil et.</li>
      <li><b>Tempo:</b> Rakibi <em>boşaltmaya</em> zorlamak için değersiz kart bırakıp değerliyi saklamak işe yarar.</li>
    </ul>

    <h2>5. Kısa Tarihçe</h2>
    <p>Pişti, Türkiye'de yaygın bir kahvehane oyunudur. Kökeni <em>Basra</em> gibi toplama temelli oyunlarla akrabadır; farklı bölgelerde puanlama küçük değişiklikler gösterir (ör. son yığın/fazla kart bonusu).</p>

    <h2>6. Pişti ve Algoritmik Oyun Kuramı (AGT) Eşlemesi</h2>
    <table style="margin:auto; text-align:left">
      <tr><th>Oyun tipi</th><td>İki kişilik, <b>kısmi bilgi</b>, stohastik unsur (kart sırası), sıfır toplamlı</td></tr>
      <tr><th>Strateji</th><td>Hangi kartın ne zaman atılacağı; kart sayma / olasılık modeli</td></tr>
      <tr><th>Oyun ağacı</th><td>Her tur eldeki kart sayısına göre dallanır; belirsiz kartlar için örnekleme gerekir</td></tr>
      <tr><th>Arama</th><td>Minimax (sınırlı derinlik) + Alpha–Beta; pratikte <b>MCTS</b> ve Monte Carlo simülasyonları</td></tr>
      <tr><th>Heuristik</th><td>Pişti/vale fırsat değeri, bırakılan kartın riski, özel kartların korunması, son yığın olasılığı</td></tr>
    </table>

    <h2>7. Heuristik (Durum Değerlendirme) Örneği</h2>
    <div style="text-align:left; display:inline-block">
      <p>Bir konumu aşağıdaki ağırlıklı toplamla değerlendirebilirsiniz:</p>
      <pre style="background:#ffd8e6; padding:12px; border-radius:12px; overflow:auto">Değer ≈ (AnlıkKazanç) + (PiştiFırsatı × 6) + (SonYığınOlasılığı × 3)
        − (VerilenDeğerliKart × 4) − (RakipYakalamaRiski × 2)</pre>
      <ul>
        <li><b>AnlıkKazanç:</b> Bu turda toplanacak puan (A/J/♦10/♣2 + yerdeki kart sayısı).</li>
        <li><b>VerilenDeğerliKart:</b> ♦10, ♣2, As, J gibi kartları boşa bırakmanın bedeli.</li>
        <li><b>RakipYakalamaRiski:</b> Rakibin eşleşme veya J ile alma ihtimali.</li>
      </ul>
    </div>

    <h2>8. Bilgisayara Karşı Pişti ve Yapay Zekâ</h2>
    <ul style="text-align:left; display:inline-block">
      <li><b>Basit bot:</b> Alabiliyorsa al; J ile uygun zamanda al; değilse en az değerli kartı bırak.</li>
      <li><b>Arama tabanlı:</b> Örneklenmiş gizli kartlarla birkaç hamle ileri <em>minimax/alpha–beta</em>.</li>
      <li><b>MCTS/Monte Carlo:</b> Kalan kartları rastgele tamamlayıp çok sayıda simülasyon çalıştırmak pratikte güçlüdür.</li>
      <li><b>Öğrenme tabanlı:</b> Simülasyonlardan elde edilen sonuçlarla heuristik ağırlıkları optimize etmek.</li>
    </ul>

    <h2>9. Eğitsel Önemi</h2>
    <ul style="text-align:left; display:inline-block">
      <li>Kısmi bilgi ve olasılık modellemesi (gizli kartlar).</li>
      <li>Arama ve kesme tekniklerinin pratik sınırlamaları (bilgi eksikliği).</li>
      <li>Heuristik tasarımı ve risk–ödül dengesi.</li>
      <li>Monte Carlo simülasyonlarının oyunlara uygulanması.</li>
    </ul>

    <h2>Püf Noktalar</h2>
    <ul style="text-align:left; display:inline-block">
      <li><b>Piştiyi zorla:</b> Yerde tek kart bırakacağın zaman rakibin eşleşme olasılığını düşün; gerekirse güvenli kartla <em>boşalt</em>.</li>
      <li><b>J zamanlaması:</b> 20 puanlık J ile pişti fırsatı nadirdir; erken harcama.</li>
      <li><b>Kayıp kontrolü:</b> Değerli kartı bırakacaksan, rakibin alamayacağı turu seç.</li>
      <li><b>Son yığın planı:</b> El sonuna yaklaşırken son ortayı alacak akışı kur.</li>
    </ul>
  </main>
  <footer>© 2025 Pişti Blog – Tüm Hakları Saklıdır</footer>
</body>
</html>
