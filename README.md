# tough-action
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Click Boom Effect with Local Images</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            width: 100vw;
            height: 100vh;
            overflow: hidden;
            position: relative;
            background-color: #f0f0f0;
            touch-action: manipulation;
        }
        
        .particle {
            position: absolute;
            pointer-events: none;
            width: 30px;
            height: 30px;
            background-size: contain;
            background-repeat: no-repeat;
            animation: boom 1s ease-out forwards;
            opacity: 0;
        }
        
        @keyframes boom {
            0% {
                transform: translate(0, 0) scale(0);
                opacity: 1;
            }
            50% {
                opacity: 1;
            }
            100% {
                transform: translate(var(--tx), var(--ty)) scale(1);
                opacity: 0;
            }
        }
    </style>
</head>
<body>
    <script>
        // Local image filenames - change these to match your downloaded image filenames
  const localImages = [
    "set-small-flowers-transparent-background-psd-file_2.png",
    "set-small-flowers-transparent-background-psd-file_3.png",
    "set-small-flowers-transparent-background-psd-file_4.png",
    "set-small-flowers-transparent-background-psd-file_904887-44.png"
];

        
        // Handle both mouse clicks and touch events
        document.addEventListener('click', handleInteraction);
        document.addEventListener('touchstart', function(e) {
            e.preventDefault(); // Prevent scrolling on touch devices
            handleInteraction(e);
        }, {passive: false});
        
        function handleInteraction(e) {
            // Get the position of the interaction
            const x = e.clientX || e.touches[0].clientX;
            const y = e.clientY || e.touches[0].clientY;
            
            // Create 16 particles (4 for each image type)
            for (let i = 0; i < 16; i++) {
                const particle = document.createElement('div');
                particle.className = 'particle';
                
                // Assign one of the four images (cycling through them)
                const imgIndex = i % 4;
                particle.style.backgroundImage = `url('${localImages[imgIndex]}')`;
                
                // Random angle and distance for boom effect
                const angle = Math.random() * Math.PI * 2;
                const distance = 50 + Math.random() * 100;
                const tx = Math.cos(angle) * distance;
                const ty = Math.sin(angle) * distance;
                
                // Set CSS variables for animation
                particle.style.setProperty('--tx', `${tx}px`);
                particle.style.setProperty('--ty', `${ty}px`);
                
                // Randomize animation duration slightly
                const duration = 0.8 + Math.random() * 0.4;
                particle.style.animationDuration = `${duration}s`;
                
                // Position at click/touch point
                particle.style.left = `${x - 15}px`; // Center on click point
                particle.style.top = `${y - 15}px`;
                
                // Random small rotation
                particle.style.transform = `rotate(${Math.random() * 360}deg)`;
                
                document.body.appendChild(particle);
                
                // Remove particle after animation completes
                setTimeout(() => {
                    particle.remove();
                }, duration * 1000);
            }
        }
    </script>
</body>
</html>
