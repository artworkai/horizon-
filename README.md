# horizon-

<!DOCTYPE html>
<html lang="en">
<head>
  <!-- iOS compatibility enhancements -->
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
  <meta name="format-detection" content="telephone=no">
  <link rel="apple-touch-icon" href="favicon.png">
  <meta name="theme-color" content="#000000">
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
  <title>FRIENDS | Horizon</title>
  <style>
    body, html {
      height: 100%;
      width: 100%;
      max-height: 100vh;
      max-width: 100vw;
      overscroll-behavior: none;
      -webkit-touch-callout: none;
      -webkit-text-size-adjust: 100%;
      -webkit-overflow-scrolling: touch;
      -webkit-tap-highlight-color: transparent;
      touch-action: manipulation;
      margin: 0;
      padding: 0;
      font-family: 'Georgia', serif;
      background-color: black;
      overflow: hidden;
      display: flex;
      align-items: center;
      justify-content: center;
    }

    .envelope {
      width: 80vw;
      height: 45vw;
      max-height: 56.25vh;
      background: black;
      color: white;
      border: 2px solid white;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 2rem;
      transition: opacity 1s ease, transform 1s ease;
      z-index: 3;
      cursor: pointer;
      position: absolute;
      box-sizing: border-box;
    }

    .envelope.opened {
      opacity: 0;
      transform: scale(0.8);
      pointer-events: none;
    }

    .slide {
      width: 100vw;
      height: 100vh;
      max-width: 177.78vh;
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      display: flex;
      align-items: center;
      justify-content: center;
      opacity: 0;
      transition: opacity 1s ease;
      pointer-events: none;
    }

    .slide.visible {
      opacity: 1;
      z-index: 2;
      pointer-events: auto;
    }

    .flyer {
      height: 80vh;
      width: 80vw;
      background-color: black;
      background-image: url('https://i.postimg.cc/dQhvnZ4M/Screenshot-2025-07-08-213827.png');
      background-size: contain;
      background-repeat: no-repeat;
      background-position: center;
      cursor: pointer;
    }

    .final {
      background-color: black;
      background-image: url('https://i.postimg.cc/13MPzGJd/Screenshot-2025-07-08-224214.png');
      background-size: contain;
      background-repeat: no-repeat;
      background-position: center;
      cursor: pointer;
    }

    .pulse {
      display: inline-block;
      animation: pulse 1.5s infinite;
    }

    @keyframes pulse {
      0% { opacity: 1; transform: scale(1); }
      50% { opacity: 0.6; transform: scale(1.05); }
      100% { opacity: 1; transform: scale(1); }
    }
  </style>
  <link href="https://fonts.googleapis.com/css2?family=Lora&display=swap" rel="stylesheet">
</head>
<body>
  <div class="envelope" id="envelope" onclick="handleSlideClick()"><span class="pulse">FRIENDS</span></div>
  <div class="slide flyer" id="flyer" onclick="handleSlideClick()"></div>
  <div class="slide final" id="final" onclick="handleSlideClick()"></div>

  <audio id="shutter-sound" src="https://www.soundjay.com/mechanical/camera-shutter-click-01a.mp3" preload="auto"></audio>

  <script>
    const slides = ['flyer', 'final'];
    let currentSlide = -1;

    const envelope = document.getElementById('envelope');
    const shutterSound = document.getElementById('shutter-sound');

    function showSlide(index) {
      if (index < 0 || index >= slides.length) return;
      const current = currentSlide >= 0 ? document.getElementById(slides[currentSlide]) : null;
      const next = document.getElementById(slides[index]);

      if (current) current.classList.remove('visible');
      setTimeout(() => {
        next.classList.add('visible');
        currentSlide = index;
      }, 1000);
    }

    function handleSlideClick() {
      shutterSound.currentTime = 0;
      shutterSound.play();

      if (currentSlide === -1) {
        envelope.classList.add('opened');
        setTimeout(() => showSlide(0), 1000);
      } else if (currentSlide < slides.length - 1) {
        showSlide(currentSlide + 1);
      } else {
        document.getElementById(slides[currentSlide]).classList.remove('visible');
        setTimeout(() => location.reload(), 1000);
      }
    }

    setTimeout(() => {
      if (currentSlide === 0) showSlide(1);
    }, 15000);
  </script>

  <div id="browser-warning" style="display:none; position:fixed; z-index:9999; background:black; color:white; top:0; left:0; width:100vw; height:100vh; padding:20px; text-align:center; font-family:Georgia, serif; font-size:1.2rem;">
    This invitation may not work properly in this app's browser.<br><br>
    Please open it in <strong>Safari</strong> for the full experience.<br><br>
    <button onclick="copyLink()" style="margin-top:20px; padding:10px 20px; font-size:1rem; cursor:pointer;">Copy Link</button><br>
    <a href="#" onclick="openInSafari()" style="margin-top:10px; padding:10px 20px; font-size:1rem; cursor:pointer; display: inline-flex; align-items: center; gap: 10px; text-decoration: none; color: white; border: 1px solid white; background: transparent;">
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/5/52/Apple_Safari_icon_%282021%29.svg/64px-Apple_Safari_icon_%282021%29.svg.png" alt="Safari Icon" style="width: 24px; height: 24px; vertical-align: middle;">
  Open in Safari
</a>
  </div>

  <script>
    function isInAppBrowser() {
      const ua = navigator.userAgent || navigator.vendor || window.opera;
      return /FBAN|FBAV|Instagram|WhatsApp/i.test(ua);
    }

    function copyLink() {
      navigator.clipboard.writeText(window.location.href).then(() => {
        alert('Link copied! Paste it in Safari.');
      });
    }

    function openInSafari() {
      const url = window.location.href;
      window.open(url, '_blank');
    }

    window.addEventListener('DOMContentLoaded', () => {
      if (isInAppBrowser()) {
        document.getElementById('browser-warning').style.display = 'block';
        document.body.style.overflow = 'hidden';
      }
    });
  </script>
</body>
</html>
