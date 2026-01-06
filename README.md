# Daze
Car game 🚨🚨🚨 Drift
<!DOCTYPE html>
<html>
<head>
    <title>سباق بورش | Porsche Challenge</title>
    <style>
        body { text-align: center; background: #1a1a1a; color: white; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
        canvas { background: #333; border: 4px solid #d4af37; box-shadow: 0 0 20px rgba(0,0,0,0.5); display: block; margin: 20px auto; }
        .stats { font-size: 24px; color: #d4af37; }
    </style>
</head>
<body>
    <h1>سباق السيارات المشهورة 🏎️</h1>
    <div class="stats">السيارة الحالية: <span style="color:white">Porsche 911</span></div>
    <p>استخدم الأسهم لتجاوز الزحام</p>
    <canvas id="raceCanvas" width="400" height="600"></canvas>

    <script>
        const canvas = document.getElementById("raceCanvas");
        const ctx = canvas.getContext("2d");

        let carX = 175;
        let enemies = [{x: 50, y: -100, speed: 5, color: "#e74c3c"}];
        let score = 0;

        document.addEventListener("keydown", (e) => {
            if (e.key === "ArrowLeft" && carX > 10) carX -= 30;
            if (e.key === "ArrowRight" && carX < 340) carX += 30;
        });

        function drawCar(x, y, color, isPlayer) {
            // جسم السيارة الرئيسي
            ctx.fillStyle = color;
            ctx.fillRect(x, y, 50, 90);
            
            // قمرة القيادة (الزجاج)
            ctx.fillStyle = "#222";
            ctx.fillRect(x + 5, y + 20, 40, 30);

            // المصابيح (لمسة بورش)
            ctx.fillStyle = isPlayer ? "white" : "yellow";
            ctx.fillRect(x + 5, y, 10, 5); 
            ctx.fillRect(x + 35, y, 10, 5);
            
            // الجناح الخلفي (Spoiler)
            if(isPlayer) {
                ctx.fillStyle = color;
                ctx.fillRect(x - 5, y + 80, 60, 10);
            }
        }

        function update() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            
            // رسم خطوط الطريق المتحركة
            ctx.setLineDash([20, 20]);
            ctx.strokeStyle = "white";
            ctx.beginPath();
            ctx.moveTo(200, 0); ctx.lineTo(200, 600);
            ctx.stroke();

            // رسم سيارتك (بورش فضية)
            drawCar(carX, 480, "#bdc3c7", true);

            // تحديث ورسم الأعداء
            enemies.forEach((enemy, index) => {
                drawCar(enemy.x, enemy.y, enemy.color, false);
                enemy.y += enemy.speed;

                if (enemy.y > 600) {
                    enemy.y = -100;
                    enemy.x = Math.random() * 350;
                    score++;
                    enemy.speed += 0.1;
                }

                // كشف الاصطدام
                if (enemy.y + 90 > 480 && enemy.y < 480 + 90 && enemy.x < carX + 50 && enemy.x + 50 > carX) {
                    alert("تحطمت البورش! مجموع نقاطك: " + score);
                    document.location.reload();
                }
            });

            ctx.fillStyle = "#d4af37";
            ctx.font = "bold 20px Arial";
            ctx.fillText("Score: " + score, 20, 40);
            requestAnimationFrame(update);
        }
        update();
    </script>
</body>
</html>
