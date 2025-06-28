<!DOCTYPE html>
<html>
<head>
  <title>Qibla Direction Finder</title>
  <style>
    body {
      font-family: sans-serif;
      text-align: center;
      background: #f5f5f5;
      padding: 30px;
    }
    h1 {
      color: #2e7d32;
    }
    #arrow {
      width: 150px;
      margin-top: 40px;
      transform: rotate(0deg);
      transition: transform 0.2s ease-in-out;
    }
    #info {
      margin-top: 20px;
    }
  </style>
</head>
<body>

  <h1>🕋 Qibla Direction</h1>
  <p>Allow location and rotate your phone to find the Qibla.</p>

  <img id="arrow" src="https://upload.wikimedia.org/wikipedia/commons/thumb/4/42/Location_dot_green.svg/768px-Location_dot_green.svg.png" alt="Qibla Arrow">

  <div id="info"></div>

  <script>
    let qiblaAngle = 0;

    // Get user location and calculate Qibla
    function getLocationAndSetQibla() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(function(position) {
          const lat = position.coords.latitude;
          const lon = position.coords.longitude;
          const kaabaLat = 21.4225;
          const kaabaLon = 39.8262;

          const dLon = (kaabaLon - lon) * Math.PI / 180;
          const y = Math.sin(dLon) * Math.cos(kaabaLat * Math.PI / 180);
          const x = Math.cos(lat * Math.PI / 180) * Math.sin(kaabaLat * Math.PI / 180) -
                    Math.sin(lat * Math.PI / 180) * Math.cos(kaabaLat * Math.PI / 180) * Math.cos(dLon);

          let brng = Math.atan2(y, x) * 180 / Math.PI;
          qiblaAngle = (brng + 360) % 360;

          document.getElementById('info').innerText = "Qibla angle from North: " + qiblaAngle.toFixed(2) + "°";
        }, function(error) {
          alert("Location error: " + error.message);
        });
      } else {
        alert("Geolocation is not supported.");
      }
    }

    // Rotate arrow based on compass
    window.addEventListener('deviceorientationabsolute', function(event) {
      if (event.alpha !== null) {
        const compassHeading = 360 - event.alpha; // Device's direction in degrees
        const rotation = qiblaAngle - compassHeading;
        document.getElementById('arrow').style.transform = `rotate(${rotation}deg)`;
      }
    }, true);

    getLocationAndSetQibla();
  </script>

</body>
</html>
