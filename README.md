## 🌍 Live Visitor Globe

[![View the Globe](https://img.shields.io/badge/🌀_See_Visitors_Live_on_Earth!-ff69b4?style=for-the-badge)](https://mariebbz.github.io/visitor-globe)

> Real-time visitors are shown as pins on a spinning Earth  
> ✨ You’re one of them!



<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Live Visitor Globe</title>
  <script src="https://unpkg.com/three@0.120.0/build/three.min.js"></script>
  <script src="https://unpkg.com/three-globe"></script>
  <style>
    body {
      margin: 0;
      background: #ffe4f0;
      font-family: 'Segoe UI', sans-serif;
      overflow: hidden;
    }
    #globeViz {
      position: absolute;
      top: 0;
      left: 0;
    }
    .stats {
      position: absolute;
      top: 1rem;
      right: 1rem;
      background: rgba(255, 255, 255, 0.8);
      padding: 1rem;
      border-radius: 1rem;
      font-size: 14px;
      color: #333;
    }
    .flag {
      font-size: 18px;
    }
  </style>
</head>
<body>
  <div id="globeViz"></div>
  <div class="stats">
    <strong>Top 5 Countries:</strong>
    <ul id="countryList"></ul>
  </div>

  <script>
    const globeContainer = document.getElementById("globeViz");
    const countryList = document.getElementById("countryList");

    const Globe = window.Globe;

    const globe = new Globe()
      .globeImageUrl('//unpkg.com/three-globe/example/img/earth-pink.jpg')
      .pointsMerge(true)
      .pointAltitude(0.01)
      .pointColor(() => '#ff69b4')
      .pointsData([]);

    const renderer = new THREE.WebGLRenderer();
    renderer.setSize(window.innerWidth, window.innerHeight);
    globeContainer.appendChild(renderer.domElement);

    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera();
    camera.aspect = window.innerWidth / window.innerHeight;
    camera.updateProjectionMatrix();
    camera.position.z = 300;

    scene.add(globe);
    animate();

    function animate() {
      requestAnimationFrame(animate);
      globe.rotation.y += 0.001;
      renderer.render(scene, camera);
    }

    // Fake visitor data (you can later replace with real data)
    const fakeVisitors = [
      { lat: 35.6895, lng: 139.6917, country: "🇯🇵 Japan" },
      { lat: 48.8566, lng: 2.3522, country: "🇫🇷 France" },
      { lat: 51.5074, lng: -0.1278, country: "🇬🇧 UK" },
      { lat: 40.7128, lng: -74.006, country: "🇺🇸 USA" },
      { lat: -23.5505, lng: -46.6333, country: "🇧🇷 Brazil" }
    ];

    globe.pointsData(fakeVisitors);

    const countryCounts = {};
    fakeVisitors.forEach(v => {
      countryCounts[v.country] = (countryCounts[v.country] || 0) + 1;
    });

    const sortedCountries = Object.entries(countryCounts)
      .sort((a, b) => b[1] - a[1])
      .slice(0, 5);

    sortedCountries.forEach(([flag, count]) => {
      const li = document.createElement("li");
      li.innerHTML = `<span class="flag">${flag}</span>: ${count}`;
      countryList.appendChild(li);
    });
  </script>
</body>
</html>


