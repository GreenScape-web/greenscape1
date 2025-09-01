<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="google-site-verification" content="TdfSehrSN6V" />
  <title>معرض الطبيعة - GreenScape</title>
  <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;700&family=Montserrat:wght@700&display=swap" rel="stylesheet">
  <style>
    :root {
      --primary: #00a876;
      --primary-dark: #04364A;
      --secondary: #15c6e7;
      --accent: #fff500;
      --background: #eefcf7;
      --card-bg: #fff;
      --nav-bg: linear-gradient(90deg, #04364A 0%, #00a876 100%);
      --border-radius: 20px;
      --transition: .35s cubic-bezier(.7,0,.3,1);
      --shadow: 0 4px 24px #04364a22, 0 1.5px 10px #0001;
    }
    body {
      font-family: 'Cairo', Arial, sans-serif;
      background: var(--background);
      margin: 0;
      color: var(--primary-dark);
      letter-spacing: .01em;
      min-height: 100vh;
      transition: background .3s;
    }
    body.dark {
      --primary: #43ffbf;
      --secondary: #41baff;
      --background: #181f25;
      --card-bg: #202a34;
      --accent: #ffe36e;
      color: #fff;
    }

    /* Navbar */
    .navbar {
      background: var(--nav-bg);
      color: #fff;
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 0 6vw;
      height: 64px;
      position: sticky;
      top: 0;
      z-index: 10;
      box-shadow: 0 2px 24px #0002;
    }
    .navbar .logo {
      font-family: 'Montserrat', 'Cairo', Arial, sans-serif;
      font-size: 2em;
      letter-spacing: 1.5px;
      font-weight: bold;
      color: var(--accent);
      text-shadow: 0 1px 14px #0005;
      transition: color .3s;
    }
    .navbar nav {
      display: flex;
      gap: 24px;
      font-weight: bold;
      font-size: 1.08em;
    }
    .navbar nav a {
      color: #fff;
      text-decoration: none;
      position: relative;
      padding: 3px 6px;
      border-radius: 7px;
      transition: background .2s, color .2s;
    }
    .navbar nav a:hover, .navbar nav a.active {
      background: var(--accent);
      color: var(--primary-dark);
    }
    .navbar .actions {
      display: flex;
      gap: 16px;
      align-items: center;
    }
    .navbar .mode-toggle {
      background: #fff2;
      color: var(--accent);
      font-family: inherit;
      border: none;
      border-radius: 50%;
      width: 38px;
      height: 38px;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      font-size: 1.45em;
      transition: background .22s, color .22s;
    }
    .navbar .mode-toggle:hover {
      background: var(--accent);
      color: var(--primary-dark);
    }
    .navbar .cta {
      background: var(--secondary);
      color: #fff;
      border: none;
      padding: 10px 22px;
      border-radius: 14px;
      font-size: 1em;
      font-weight: bold;
      cursor: pointer;
      box-shadow: 0 2px 12px #00a87642;
      transition: background .2s, color .2s, transform .2s;
      text-decoration: none;
    }
    .navbar .cta:hover {
      background: var(--accent);
      color: var(--primary-dark);
      transform: translateY(-2px) scale(1.03);
    }

    header {
      background: linear-gradient(90deg, #04364A 0%, #00a876 100%);
      color: #fff;
      text-align: center;
      padding: 60px 0 32px;
      box-shadow: 0 2px 26px #04364a30;
      border-bottom-left-radius: 24px;
      border-bottom-right-radius: 24px;
      position: relative;
      overflow: hidden;
    }
    header h1 {
      font-family: 'Montserrat', 'Cairo', Arial, sans-serif;
      letter-spacing: 2.5px;
      font-size: 2.7em;
      margin-bottom: 12px;
      text-shadow: 0 2px 16px #0006;
      position: relative;
      z-index: 2;
      animation: fadeInDown .9s cubic-bezier(.6,0,.4,1);
    }
    header p {
      font-size: 1.25em;
      opacity: .98;
      margin: 0;
      position: relative;
      z-index: 2;
      animation: fadeInDown 1.2s .2s backwards;
    }
    /* Header decoration */
    header::after {
      content: "";
      position: absolute;
      left: 0; right: 0; bottom: 0;
      height: 90px;
      background: radial-gradient(circle at 50% 120%, var(--accent) 0 8%, transparent 70%);
      z-index: 1;
    }

    main {
      flex: 1;
      padding: 44px 8vw 0 8vw;
      box-sizing: border-box;
    }
    section {
      margin-bottom: 56px;
      background: var(--card-bg);
      border-radius: var(--border-radius);
      box-shadow: var(--shadow);
      padding: 38px 18px 32px;
      animation: fadeInUp 1s cubic-bezier(.7,0,.3,1);
    }
    h2 {
      color: var(--primary);
      font-size: 2.1em;
      margin-top: 0;
      margin-bottom: 22px;
      text-align: center;
      font-family: 'Montserrat', 'Cairo', Arial, sans-serif;
      letter-spacing: 1.5px;
      text-shadow: 0 1px 8px #00a87625;
      animation: fadeInDown 1.05s .12s backwards;
    }
    .gallery, .videos {
      display: flex;
      flex-wrap: wrap;
      gap: 22px;
      justify-content: center;
      align-items: flex-start;
    }
    .gallery img {
      width: 220px;
      height: 150px;
      object-fit: cover;
      border-radius: var(--border-radius);
      box-shadow: 0 2px 14px #04364a15, 0 0px 0px #00a87600;
      border: 2.5px solid var(--accent);
      background: #eee;
      cursor: pointer;
      transition:
        transform .35s cubic-bezier(.7,0,.3,1),
        box-shadow .34s,
        border-color .26s;
      animation: fadeInUp .7s .14s backwards;
    }
    .gallery img:hover {
      transform: scale(1.09) rotate(-2.5deg);
      box-shadow: 0 12px 38px #00a87655, 0 0px 0px #fff00000;
      border-color: var(--secondary);
      z-index: 2;
    }
    .videos iframe, .videos video {
      width: 340px;
      height: 190px;
      margin: 0;
      border-radius: var(--border-radius);
      box-shadow: 0 2px 18px #04364a1a, 0 0px 0px #fff50000;
      border: 2.5px solid var(--accent);
      background: #f8f8f8;
      transition: box-shadow .4s, border-color .18s;
      animation: fadeInUp .8s .18s backwards;
    }
    .videos iframe:hover, .videos video:hover {
      box-shadow: 0 14px 38px #41baff80, 0 0px 0px #fff00000;
      border-color: var(--secondary);
      z-index: 2;
    }
    .note {
      color: #666;
      font-size: 1.03em;
      text-align: center;
      margin-top: 26px;
      opacity: .86;
    }
    .share-buttons {
      display: flex;
      gap: 16px;
      justify-content: center;
      margin: 22px 0 0 0;
    }
    .share-btn {
      display: flex;
      align-items: center;
      gap: 6px;
      padding: 8px 18px;
      font-size: 1em;
      font-family: inherit;
      border: none;
      border-radius: 9px;
      background: var(--primary);
      color: #fff;
      cursor: pointer;
      box-shadow: 0 1px 8px #00a87629;
      transition: background .2s, color .2s, transform .16s;
      text-decoration: none;
      font-weight: bold;
    }
    .share-btn:hover {
      background: var(--accent);
      color: var(--primary-dark);
      transform: translateY(-2px) scale(1.04);
    }
    .contact-bar {
      display: flex;
      justify-content: center;
      margin-top: 32px;
    }
    .contact-link {
      background: var(--secondary);
      color: #fff;
      padding: 14px 38px;
      border-radius: 18px;
      font-weight: bold;
      font-size: 1.07em;
      text-decoration: none;
      transition: background .19s, color .19s, transform .16s;
      box-shadow: 0 1px 7px #15c6e766;
      margin: 0 6px;
    }
    .contact-link:hover {
      background: var(--accent);
      color: var(--primary-dark);
      transform: scale(1.06);
    }

    footer {
      background: linear-gradient(90deg, #04364A 0%, #00a876 100%);
      color: #fff;
      text-align: center;
      padding: 26px 0 16px;
      font-size: 1.10em;
      margin-top: 44px;
      border-top-left-radius: 28px;
      border-top-right-radius: 28px;
      box-shadow: 0 -2px 24px #04364a22;
      position: relative;
      overflow: hidden;
    }
    footer .footer-links {
      margin-bottom: 8px;
      display: flex;
      justify-content: center;
      gap: 18px;
      flex-wrap: wrap;
    }
    footer a {
      color: var(--accent);
      text-decoration: none;
      margin: 0 8px;
      font-weight: bold;
      transition: color .16s;
    }
    footer a:hover {
      color: var(--secondary);
    }
    @media (max-width: 900px) {
      main { padding: 20px 2vw 0 2vw; }
      .gallery img, .videos iframe, .videos video { width: 92vw; max-width: 330px; height: 120px; }
      .navbar { padding: 0 2vw;}
    }
    @media (max-width: 600px) {
      header h1 { font-size: 1.2em; }
      h2 { font-size: 1.06em; }
      section { padding: 16px 3px 12px; }
      .gallery img, .videos iframe, .videos video { max-width: 98vw; width: 98vw; height: 110px; }
      .navbar { flex-direction: column; height: auto; padding: 0 2vw; }
      .navbar nav { flex-direction: column; gap: 7px; }
      .navbar .actions { margin: 6px 0;}
    }

    /* Animations */
    @keyframes fadeInUp {
      0% { opacity: 0; transform: translateY(48px);}
      100% { opacity: 1; transform: none; }
    }
    @keyframes fadeInDown {
      0% { opacity: 0; transform: translateY(-44px);}
      100% { opacity: 1; transform: none;}
    }
    ::selection { background: var(--secondary); color: #fff; }
  </style>
</head>
<body>
  <div class="navbar">
    <div class="logo">GreenScape</div>
    <nav>
      <a href="#" class="active">الرئيسية</a>
      <a href="#gallery">صور</a>
      <a href="#videos">فيديوهات</a>
      <a href="#contact">اتصل بنا</a>
    </nav>
    <div class="actions">
      <button class="mode-toggle" id="modeToggle" title="تغيير الوضع">
        🌙
      </button>
      <a href="#contact" class="cta">تواصل</a>
    </div>
  </div>
  <header>
    <h1>معرض الطبيعة - GreenScape</h1>
    <p>استكشف جمال وروعة الطبيعة في أضخم معرض صور وفيديوهات طبيعية عالية الجودة 🌿</p>
  </header>

  <main>
    <section id="gallery">
      <h2>صور طبيعية مختارة</h2>
      <div class="gallery">
        <img src="https://images.unsplash.com/photo-1506744038136-46273834b3fb?auto=format&fit=crop&w=600&q=80" alt="غابة ضبابية">
        <img src="https://images.unsplash.com/photo-1465101046530-73398c7f28ca?auto=format&fit=crop&w=600&q=80" alt="جبال خضراء">
        <img src="https://images.unsplash.com/photo-1444065381814-865dc9da92c0?auto=format&fit=crop&w=600&q=80" alt="شلال منعش">
        <img src="https://images.unsplash.com/photo-1465101178521-c1a5bc3d4c2a?auto=format&fit=crop&w=600&q=80" alt="سماء صافية">
        <img src="https://images.unsplash.com/photo-1500534314209-a25ddb2bd429?auto=format&fit=crop&w=600&q=80" alt="طريق بين الأشجار">
        <img src="https://cdn.pixabay.com/photo/2015/06/19/21/24/avenue-815297_1280.jpg" alt="أشجار متراصة">
        <img src="https://cdn.pixabay.com/photo/2016/11/29/09/32/forest-1868415_1280.jpg" alt="غابة كثيفة">
        <img src="https://cdn.pixabay.com/photo/2013/10/02/23/03/mountains-190055_1280.jpg" alt="جبال شاهقة">
        <img src="https://cdn.pixabay.com/photo/2015/04/23/22/00/tree-736885_1280.jpg" alt="شجرة منفردة">
        <img src="https://cdn.pixabay.com/photo/2017/08/01/00/25/people-2563491_1280.jpg" alt="منظر طبيعي">
        <img src="https://images.unsplash.com/photo-1465101046530-73398c7f28ca?auto=format&fit=crop&w=600&q=80" alt="جبال خضراء 2">
        <img src="https://images.unsplash.com/photo-1465101178521-c1a5bc3d4c2a?auto=format&fit=crop&w=600&q=80" alt="سماء صافية 2">
        <img src="https://images.unsplash.com/photo-1506744038136-46273834b3fb?auto=format&fit=crop&w=600&q=80" alt="غابة ضبابية 2">
        <img src="https://cdn.pixabay.com/photo/2016/11/29/09/32/forest-1868415_1280.jpg" alt="غابة كثيفة 2">
        <img src="https://images.unsplash.com/photo-1444065381814-865dc9da92c0?auto=format&fit=crop&w=600&q=80" alt="شلال منعش 2">
        <img src="https://cdn.pixabay.com/photo/2015/06/19/21/24/avenue-815297_1280.jpg" alt="أشجار متراصة 2">
        <img src="https://cdn.pixabay.com/photo/2013/10/02/23/03/mountains-190055_1280.jpg" alt="جبال شاهقة 2">
        <img src="https://images.unsplash.com/photo-1465101046530-73398c7f28ca?auto=format&fit=crop&w=600&q=80" alt="جبال خضراء 3">
        <img src="https://cdn.pixabay.com/photo/2017/08/01/00/25/people-2563491_1280.jpg" alt="منظر طبيعي 2">
        <img src="https://images.unsplash.com/photo-1506744038136-46273834b3fb?auto=format&fit=crop&w=600&q=80" alt="غابة ضبابية 3">
      </div>
      <div class="share-buttons">
        <a class="share-btn" href="https://twitter.com/intent/tweet?text=شاهد+معرض+صور+الطبيعة+GreenScape!&url=https://al-mthman.github.io/" target="_blank">شارك على X (تويتر)</a>
        <a class="share-btn" href="https://wa.me/?text=شاهد+معرض+صور+الطبيعة+GreenScape!+https://al-mthman.github.io/" target="_blank">مشاركة واتساب</a>
        <a class="share-btn" href="mailto:?subject=معرض%20الطبيعة%20GreenScape&body=شاهد%20معرض%20صور%20الطبيعة%20GreenScape!%20https://al-mthman.github.io/" target="_blank">مشاركة عبر البريد</a>
      </div>
      <div class="note">إذا رغبت بالحصول على 100 صورة أصلية بروابطها جاهزة، راسلنا ليتم إرسال ملف كامل أو جدول منسق يشمل جميع الصور.</div>
    </section>

    <section id="videos">
      <h2>فيديوهات طبيعية عالية الجودة</h2>
      <div class="videos">
        <iframe src="https://www.youtube.com/embed/eKFTSSKCzWA" title="Nature Video 1" allowfullscreen></iframe>
        <iframe src="https://www.youtube.com/embed/BHACKCNDMW8" title="Nature Video 2" allowfullscreen></iframe>
        <iframe src="https://www.youtube.com/embed/Ti6S8OWNWqA" title="Nature Video 3" allowfullscreen></iframe>
        <iframe src="https://www.youtube.com/embed/PRa6t_e7dgI" title="Nature Video 4" allowfullscreen></iframe>
        <iframe src="https://www.youtube.com/embed/LXb3EKWsInQ" title="Nature Video 5" allowfullscreen></iframe>
        <video controls poster="https://images.unsplash.com/photo-1506744038136-46273834b3fb?auto=format&fit=crop&w=600&q=80">
          <source src="https://www.w3schools.com/html/mov_bbb.mp4" type="video/mp4">
          متصفحك لا يدعم تشغيل الفيديو.
        </video>
        <video controls poster="https://images.unsplash.com/photo-1465101178521-c1a5bc3d4c2a?auto=format&fit=crop&w=600&q=80">
          <source src="https://www.w3schools.com/html/movie.mp4" type="video/mp4">
          متصفحك لا يدعم تشغيل الفيديو.
        </video>
        <iframe src="https://www.youtube.com/embed/eKFTSSKCzWA" title="Nature Video 6" allowfullscreen></iframe>
        <iframe src="https://www.youtube.com/embed/BHACKCNDMW8" title="Nature Video 7" allowfullscreen></iframe>
        <iframe src="https://www.youtube.com/embed/Ti6S8OWNWqA" title="Nature Video 8" allowfullscreen></iframe>
      </div>
      <div class="share-buttons">
        <a class="share-btn" href="https://twitter.com/intent/tweet?text=شاهد+مجموعة+فيديوهات+الطبيعة+GreenScape!&url=https://al-mthman.github.io/" target="_blank">شارك على X (تويتر)</a>
        <a class="share-btn" href="https://wa.me/?text=شاهد+مجموعة+فيديوهات+الطبيعة+GreenScape!+https://al-mthman.github.io/" target="_blank">مشاركة واتساب</a>
      </div>
      <div class="note">لقائمة فيديوهات كاملة أو ملفات mp4 أصلية (عدد 100)، تواصل معنا لنرسل لك ملف منفصل أو جدول روابط حصري.</div>
    </section>

    <section id="contact">
      <h2>تواصل معنا</h2>
      <div class="contact-bar">
        <a class="contact-link" href="mailto:info@greenscape.com">✉️ البريد الإلكتروني</a>
        <a class="contact-link" href="https://wa.me/1234567890" target="_blank">واتساب مباشر</a>
        <a class="contact-link" href="https://instagram.com/greenscape" target="_blank">انستجرام</a>
      </div>
      <div class="note">لأي استفسار أو طلب صور وفيديوهات إضافية أو اقتراحات تطوير الموقع، لا تتردد بالتواصل معنا!</div>
    </section>
  </main>

  <footer>
    <div class="footer-links">
      <a href="#">الرئيسية</a>
      <a href="#gallery">صور</a>
      <a href="#videos">فيديوهات</a>
      <a href="#contact">اتصل بنا</a>
    </div>
    <p>© 2025 GreenScape - جميع الحقوق محفوظة | تصميم وإبداع أسطوري</p>
  </footer>

  <script>
    const modeToggle = document.getElementById('modeToggle');
    modeToggle.onclick = function() {
      document.body.classList.toggle('dark');
      modeToggle.textContent = document.body.classList.contains('dark') ? "☀️" : "🌙";
    }
  </script>
</body>
</html>

