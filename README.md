[Uploading app.html…]()
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Web Page</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #0f172a;
            color: #f8fafc;
            font-family: 'Inter', sans-serif;
            font-size: 5rem;
            font-weight: bold;
            perspective: 1000px;
            overflow: hidden;
        }

        .wrapper {
            animation: float 3s ease-in-out infinite;
            transform-style: preserve-3d;
        }
        
        .animated-text {
            background: linear-gradient(270deg, #ff007f, #7f00ff, #00ffff, #ff007f);
            background-size: 300% 300%;
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            animation: gradient-shift 4s ease infinite;
            filter: drop-shadow(0 15px 15px rgba(127, 0, 255, 0.4));
            cursor: pointer;
            user-select: none;
            transition: transform 0.1s ease-out;
            display: inline-block;
        }

        .animated-text:active {
            transform: scale(0.9) !important;
            filter: drop-shadow(0 5px 5px rgba(127, 0, 255, 0.6));
        }

        @keyframes gradient-shift {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }

        @keyframes float {
            0% { transform: translateY(0px); }
            50% { transform: translateY(-20px); }
            100% { transform: translateY(0px); }
        }

        /* Particles */
        .particle {
            position: absolute;
            border-radius: 50%;
            pointer-events: none;
            animation: explode 1s cubic-bezier(0, 0.9, 0.3, 1) forwards;
            z-index: -1;
        }

        @keyframes explode {
            0% { transform: translate(-50%, -50%) scale(1); opacity: 1; }
            100% { transform: translate(var(--tx), var(--ty)) scale(0); opacity: 0; }
        }
    </style>
</head>
<body>
    <div class="wrapper">
        <div class="animated-text">fuck you</div>
    </div>

    <script>
        const text = document.querySelector('.animated-text');

        // 3D tilt effect tracking mouse position
        document.addEventListener('mousemove', (e) => {
            const xAxis = (window.innerWidth / 2 - e.pageX) / 20;
            const yAxis = (window.innerHeight / 2 - e.pageY) / 20;
            text.style.transform = `rotateY(${xAxis}deg) rotateX(${yAxis}deg)`;
        });

        // Reset rotation when mouse leaves the window
        document.addEventListener('mouseleave', () => {
            text.style.transform = `rotateY(0deg) rotateX(0deg)`;
            text.style.transition = 'transform 0.5s ease';
            setTimeout(() => { text.style.transition = 'transform 0.1s ease-out'; }, 500);
        });

        // Click interaction: Particles & Color Change
        text.addEventListener('click', (e) => {
            // Spawn 25 particles
            for(let i = 0; i < 25; i++) {
                createParticle(e.clientX, e.clientY);
            }
            
            // Randomize text gradient
            const colors = ['#ff007f', '#7f00ff', '#00ffff', '#ff5e62', '#ff9966', '#00c9ff', '#92fe9d', '#f857a6', '#ff5858', '#4facfe', '#00f2fe'];
            const c1 = colors[Math.floor(Math.random() * colors.length)];
            const c2 = colors[Math.floor(Math.random() * colors.length)];
            const c3 = colors[Math.floor(Math.random() * colors.length)];
            text.style.background = `linear-gradient(270deg, ${c1}, ${c2}, ${c3}, ${c1})`;
            text.style.backgroundSize = '300% 300%';
            text.style.webkitBackgroundClip = 'text';
        });

        function createParticle(x, y) {
            const particle = document.createElement('div');
            particle.className = 'particle';
            document.body.appendChild(particle);

            // Random size between 5px and 20px
            const size = Math.random() * 15 + 5;
            particle.style.width = `${size}px`;
            particle.style.height = `${size}px`;
            
            // Random colors for particles
            const colors = ['#ff007f', '#7f00ff', '#00ffff', '#ffffff'];
            particle.style.background = colors[Math.floor(Math.random() * colors.length)];
            particle.style.left = `${x}px`;
            particle.style.top = `${y}px`;

            // Calculate random scattered destination
            const angle = Math.random() * Math.PI * 2;
            const velocity = 150 + Math.random() * 250;
            const tx = Math.cos(angle) * velocity;
            const ty = Math.sin(angle) * velocity;

            particle.style.setProperty('--tx', `${tx}px`);
            particle.style.setProperty('--ty', `${ty}px`);

            // Remove particle element from DOM after animation completes
            setTimeout(() => {
                particle.remove();
            }, 1000);
        }
    </script>
</body>
</html>
