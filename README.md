<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive Holi Celebration</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            overflow: hidden;
            background-color: #1a1a1a;
            font-family: 'Arial Black', sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            user-select: none;
        }

        .container {
            text-align: center;
        }

        h1 {
            font-size: 5rem;
            margin: 0;
            color: #fff;
        }

        .happy {
            color: #ffcc00;
            text-shadow: 2px 2px #ff6600;
        }

        .holi {
            cursor: pointer;
            display: inline-block;
            transition: transform 0.2s;
            background: linear-gradient(to right, #ff0080, #ff8c00, #40e0d0, #9932cc);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            filter: drop-shadow(0 0 10px rgba(255,255,255,0.3));
        }

        .holi:hover {
            transform: scale(1.1);
        }

        .holi:active {
            transform: scale(0.9);
        }

        canvas {
            position: absolute;
            top: 0;
            left: 0;
            pointer-events: none;
        }
    </style>
</head>
<body>

    <div class="container">
        <h1 class="happy">HAPPY</h1>
        <h1 class="holi" id="holi-text">HOLI</h1>
    </div>

    <canvas id="canvas"></canvas>

    <script>
        const canvas = document.getElementById('canvas');
        const ctx = canvas.getContext('2d');
        const holiBtn = document.getElementById('holi-text');

        let particles = [];

        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;

        class Particle {
            constructor(x, y) {
                this.x = x;
                this.y = y;
                this.size = Math.random() * 12 + 4;
                this.speedX = Math.random() * 10 - 5;
                this.speedY = Math.random() * 10 - 5;
                // Vibrant Holi Colors
                const colors = ['#FF1493', '#00BFFF', '#32CD32', '#FFD700', '#FF4500', '#8A2BE2'];
                this.color = colors[Math.floor(Math.random() * colors.length)];
                this.opacity = 1;
            }

            update() {
                this.x += this.speedX;
                this.y += this.speedY;
                this.opacity -= 0.01;
                if (this.size > 0.1) this.size -= 0.05;
            }

            draw() {
                ctx.globalAlpha = this.opacity;
                ctx.fillStyle = this.color;
                ctx.beginPath();
                ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
                ctx.fill();
            }
        }

        function createExplosion(e) {
            const rect = holiBtn.getBoundingClientRect();
            const centerX = rect.left + rect.width / 2;
            const centerY = rect.top + rect.height / 2;
            
            for (let i = 0; i < 50; i++) {
                particles.push(new Particle(centerX, centerY));
            }
        }

        function animate() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            for (let i = 0; i < particles.length; i++) {
                particles[i].update();
                particles[i].draw();
                if (particles[i].opacity <= 0) {
                    particles.splice(i, 1);
                    i--;
                }
            }
            requestAnimationFrame(animate);
        }

        holiBtn.addEventListener('mousedown', createExplosion);
        window.addEventListener('resize', () => {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
        });

        animate();
    </script>
</body>
</html>
