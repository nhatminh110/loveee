<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Fireworks</title>
    <style>
        body {
            margin: 0;
            overflow: hidden;
            background: black;
        }
        canvas {
            position: absolute;
            top: 0;
            left: 0;
        }
    </style>
</head>
<body>
    <canvas id="canvas"></canvas>
    <script>
        const canvas = document.getElementById('canvas');
        const ctx = canvas.getContext('2d');
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;

        class Firework {
            constructor(x, y) {
                this.x = x;
                this.y = y;
                this.sparks = [];
                this.maxSparks = 100;
                this.createSparks();
            }

            createSparks() {
                for (let i = 0; i < this.maxSparks; i++) {
                    this.sparks.push({
                        x: this.x,
                        y: this.y,
                        dx: Math.random() * 6 - 3,
                        dy: Math.random() * 6 - 3,
                        life: Math.random() * 60 + 60,
                        color: `hsl(${Math.random() * 360}, 100%, 50%)`
                    });
                }
            }

            update() {
                this.sparks.forEach(spark => {
                    spark.x += spark.dx;
                    spark.y += spark.dy;
                    spark.life -= 1;
                    ctx.fillStyle = spark.color;
                    ctx.fillRect(spark.x, spark.y, 2, 2);
                });
                this.sparks = this.sparks.filter(spark => spark.life > 0);
            }

            isAlive() {
                return this.sparks.length > 0;
            }
        }

        const fireworks = [new Firework(canvas.width / 2, canvas.height / 2)];

        function draw() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            fireworks.forEach(firework => {
                firework.update();
            });
            fireworks.forEach((firework, index) => {
                if (!firework.isAlive()) {
                    fireworks.splice(index, 1);
                }
            });
            requestAnimationFrame(draw);
        }

        draw();

        function addLoveText() {
            ctx.font = 'bold 100px Arial';
            ctx.fillStyle = 'white';
            ctx.textAlign = 'center';
            ctx.textBaseline = 'middle';
            ctx.fillText('LOVE', canvas.width / 2, canvas.height / 2);
        }

        window.addEventListener('load', addLoveText);
    </script>
</body>
</html>
