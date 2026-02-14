
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Immersive 3D Gallery</title>
<script src="https://cdn.tailwindcss.com"></script>
<link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

<script>
tailwind.config = {
theme: {
extend: {
colors: {
'brand-dark': '#0f172a',
'brand-accent': '#6366f1',
'glass-bg': 'rgba(255, 255, 255, 0.05)',
'glass-border': 'rgba(255, 255, 255, 0.1)',
},
fontFamily: {
sans: ['Outfit', 'sans-serif'],
},
animation: {
'float': 'float 6s ease-in-out infinite',
'pulse-slow': 'pulse 4s cubic-bezier(0.4, 0, 0.6, 1) infinite',
},
keyframes: {
float: {
'0%, 100%': { transform: 'translateY(0)' },
'50%': { transform: 'translateY(-10px)' },
}
}
}
}
}
</script>

<style>
body {
background-color: #050505;
background-image: 
radial-gradient(circle at 15% 50%, rgba(99, 102, 241, 0.15) 0%, transparent 25%),
radial-gradient(circle at 85% 30%, rgba(236, 72, 153, 0.15) 0%, transparent 25%);
overflow: hidden;
perspective: 1200px; /* Crucial for 3D effect */
}

/* The 3D Scene Container */
.scene {
width: 100%;
height: 100vh;
display: flex;
align-items: center;
justify-content: center;
transform-style: preserve-3d;
}

/* The Carousel Wrapper */
.carousel {
position: relative;
width: 300px;
height: 400px;
transform-style: preserve-3d;
transition: transform 0.8s cubic-bezier(0.2, 0.8, 0.2, 1);
}

/* Individual Cards */
.card {
position: absolute;
top: 0;
left: 0;
width: 100%;
height: 100%;
border-radius: 24px;
overflow: hidden;
background: rgba(20, 20, 30, 0.6);
backdrop-filter: blur(12px);
-webkit-backdrop-filter: blur(12px);
border: 1px solid rgba(255, 255, 255, 0.15);
box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.5);
display: flex;
flex-direction: column;
justify-content: flex-end;
transition: all 0.5s ease;
transform-origin: center;
cursor: pointer;
}

/* Card Hover Effects */
.card:hover {
border-color: rgba(255, 255, 255, 0.4);
box-shadow: 0 0 30px rgba(99, 102, 241, 0.3);
}

/* Card Image Area */
.card-image {
position: absolute;
top: 0;
left: 0;
width: 100%;
height: 100%;
object-fit: cover;
transition: transform 0.8s ease;
opacity: 0.8;
}

.card:hover .card-image {
transform: scale(1.1);
opacity: 1;
}

/* Card Content Overlay */
.card-content {
position: relative;
z-index: 10;
padding: 24px;
background: linear-gradient(to top, rgba(0,0,0,0.9), transparent);
transform: translateY(20px);
transition: transform 0.4s ease;
}

.card:hover .card-content {
transform: translateY(0);
}

/* Reflection Effect */
.card::after {
content: '';
position: absolute;
top: 0;
left: 0;
right: 0;
bottom: 0;
background: linear-gradient(135deg, rgba(255,255,255,0.1) 0%, transparent 100%);
pointer-events: none;
}

/* Controls */
.control-btn {
position: absolute;
top: 50%;
transform: translateY(-50%);
z-index: 50;
width: 56px;
height: 56px;
border-radius: 50%;
background: rgba(255, 255, 255, 0.1);
backdrop-filter: blur(10px);
border: 1px solid rgba(255, 255, 255, 0.2);
color: white;
display: flex;
align-items: center;
justify-content: center;
cursor: pointer;
transition: all 0.3s ease;
}

.control-btn:hover {
background: rgba(255, 255, 255, 0.25);
transform: translateY(-50%) scale(1.1);
}

.prev-btn { left: 5%; }
.next-btn { right: 5%; }

/* Ambient Particles */
.particle {
position: absolute;
border-radius: 50%;
background: white;
opacity: 0.3;
pointer-events: none;
animation: float 10s infinite ease-in-out;
}

</style>
</head>
<body class="text-white antialiased selection:bg-brand-accent selection:text-white">

<!-- Ambient Background Particles -->
<div id="particles" class="fixed inset-0 pointer-events-none z-0"></div>

<!-- Main 3D Scene -->
<div class="scene z-10">
    
    <!-- Navigation Controls -->
    <button class="control-btn prev-btn group" id="prevBtn">
        <i class="fa-solid fa-chevron-left text-xl group-hover:-translate-x-1 transition-transform"></i>
    </button>

    <!-- The Carousel -->
    <div class="carousel" id="carousel">
        <!-- Cards will be injected here via JS -->
    </div>

    <button class="control-btn next-btn group" id="nextBtn">
        <i class="fa-solid fa-chevron-right text-xl group-hover:translate-x-1 transition-transform"></i>
    </button>

</div>

<!-- UI Overlay / Header -->
<div class="fixed top-0 left-0 w-full p-8 z-50 flex justify-between items-start pointer-events-none">
    <div>
        <h1 class="text-3xl font-bold tracking-tight bg-clip-text text-transparent bg-gradient-to-r from-white to-gray-400">
            LUMINA
        </h1>
        <p class="text-sm text-gray-400 mt-1 font-light tracking-widest uppercase">Digital Art Collection</p>
    </div>
    <div class="pointer-events-auto">
        <button class="bg-white/10 hover:bg-white/20 backdrop-blur-md border border-white/10 px-4 py-2 rounded-full text-sm font-medium transition-colors">
            <i class="fa-solid fa-share-nodes mr-2"></i> Share
        </button>
    </div>
</div>

<!-- UI Overlay / Footer -->
<div class="fixed bottom-0 left-0 w-full p-8 z-50 flex justify-center pointer-events-none">
    <div class="flex space-x-2 pointer-events-auto bg-black/30 backdrop-blur-lg px-4 py-2 rounded-full border border-white/5" id="indicators">
        <!-- Indicators injected via JS -->
    </div>
</div>

<script>
    // Data for the Gallery
    const galleryData = [
        {
            title: "Neon Genesis",
            desc: "Cyberpunk aesthetics in the modern age.",
            img: "https://images.unsplash.com/photo-1550684848-fac1c5b4e853?q=80&w=1000&auto=format&fit=crop",
            color: "#6366f1"
        },
        {
            title: "Abstract Flow",
            desc: "Fluid dynamics captured in time.",
            img: "https://images.unsplash.com/photo-1541701494587-cb58502866ab?q=80&w=1000&auto=format&fit=crop",
            color: "#ec4899"
        },
        {
            title: "Geometric Soul",
            desc: "Order within chaos.",
            img: "https://images.unsplash.com/photo-1557672172-298e090bd0f1?q=80&w=1000&auto=format&fit=crop",
            color: "#10b981"
        },
        {
            title: "Urban Solitude",
            desc: "The silence of the concrete jungle.",
            img: "https://images.unsplash.com/photo-1514565131-fce0801e5785?q=80&w=1000&auto=format&fit=crop",
            color: "#f59e0b"
        },
        {
            title: "Ethereal Light",
            desc: "Breaking through the darkness.",
            img: "https://images.unsplash.com/photo-1504333638930-c8787321eee0?q=80&w=1000&auto=format&fit=crop",
            color: "#8b5cf6"
        },
        {
            title: "Deep Ocean",
            desc: "Mysteries beneath the surface.",
            img: "https://images.unsplash.com/photo-1682687220742-aba13b6e50ba?q=80&w=1000&auto=format&fit=crop",
            color: "#0ea5e9"
        },
        {
            title: "Desert Mirage",
            desc: "Heat waves and endless horizons.",
            img: "https://images.unsplash.com/photo-1473580044384-7ba9967e16a0?q=80&w=1000&auto=format&fit=crop",
            color: "#f97316"
        },
        {
            title: "Crystal Structure",
            desc: "Nature's perfect geometry.",
            img: "https://images.unsplash.com/photo-1515462277126-2dd0c162007a?q=80&w=1000&auto=format&fit=crop",
            color: "#06b6d4"
        }
    ];

    // Configuration
    const radius = 450; // Distance from center
    const cardCount = galleryData.length;
    const theta = 360 / cardCount;
    let currIndex = 0;

    // DOM Elements
    const carousel = document.getElementById('carousel');
    const prevBtn = document.getElementById('prevBtn');
    const nextBtn = document.getElementById('nextBtn');
    const indicatorsContainer = document.getElementById('indicators');
    const particlesContainer = document.getElementById('particles');

    // Initialization
    function init() {
        createCards();
        createIndicators();
        createParticles();
        updateCarousel();
        
        // Event Listeners
        prevBtn.addEventListener('click', () => rotateCarousel(-1));
        nextBtn.addEventListener('click', () => rotateCarousel(1));
        
        // Keyboard navigation
        document.addEventListener('keydown', (e) => {
            if (e.key === 'ArrowLeft') rotateCarousel(-1);
            if (e.key === 'ArrowRight') rotateCarousel(1);
        });

        // Drag/Swipe support
        let startX = 0;
        let isDragging = false;

        document.addEventListener('mousedown', (e) => {
            startX = e.clientX;
            isDragging = true;
        });

        document.addEventListener('mouseup', (e) => {
            if (!isDragging) return;
            const diff = e.clientX - startX;
            if (Math.abs(diff) > 50) {
                if (diff > 0) rotateCarousel(-1);
                else rotateCarousel(1);
            }
            isDragging = false;
        });
    }

    function createCards() {
        galleryData.forEach((item, index) => {
            const card = document.createElement('div');
            card.className = 'card';
            
            // Calculate rotation for each card
            const angle = theta * index;
            card.style.transform = `rotateY(${angle}deg) translateZ(${radius}px)`;
            
            card.innerHTML = `
                <img src="${item.img}" alt="${item.title}" class="card-image" loading="lazy">
                <div class="card-content">
                    <h3 class="text-2xl font-bold mb-1">${item.title}</h3>
                    <p class="text-sm text-gray-300 font-light">${item.desc}</p>
                    <div class="mt-4 flex items-center space-x-2">
                        <span class="w-2 h-2 rounded-full" style="background-color: ${item.color}"></span>
                        <span class="text-xs uppercase tracking-wider text-gray-400">Artwork #${index + 1}</span>
                    </div>
                </div>
            `;
            
            // Click on card to rotate to it
            card.addEventListener('click', () => {
                const diff = index - currIndex;
                // Handle wrap-around logic for shortest path
                if (diff !== 0) {
                    // Simple logic: if clicking a card far away, we rotate towards it
                    // For simplicity in this demo, we just rotate the difference
                    // A more complex algo would find the shortest distance (clockwise vs counter)
                    rotateCarousel(diff);
                }
            });

            carousel.appendChild(card);
        });
    }

    function createIndicators() {
        galleryData.forEach((_, index) => {
            const dot = document.createElement('button');
            dot.className = 'w-2 h-2 rounded-full bg-gray-600 hover:bg-white transition-all duration-300';
            indicatorsContainer.appendChild(dot);
        });
    }

    function createParticles() {
        for (let i = 0; i < 20; i++) {
            const p = document.createElement('div');
            p.className = 'particle';
            const size = Math.random() * 4 + 1;
            p.style.width = `${size}px`;
            p.style.height = `${size}px`;
            p.style.left = `${Math.random() * 100}%`;
            p.style.top = `${Math.random() * 100}%`;
            p.style.animationDelay = `${Math.random() * 5}s`;
            p.style.animationDuration = `${Math.random() * 10 + 10}s`;
            particlesContainer.appendChild(p);
        }
    }

    function rotateCarousel(direction) {
        currIndex += direction;
        updateCarousel();
    }

    function updateCarousel() {
        const angle = theta * currIndex;
        carousel.style.transform = `translateZ(-${radius}px) rotateY(${-angle}deg)`;
        
        // Update Indicators
        const dots = indicatorsContainer.children;
        for (let i = 0; i < dots.length; i++) {
            if (i === currIndex % cardCount) {
                // Handle negative modulo for correct index
                const normalizedIndex = ((currIndex % cardCount) + cardCount) % cardCount;
                if (i === normalizedIndex) {
                    dots[i].className = 'w-6 h-2 rounded-full bg-brand-accent shadow-[0_0_10px_#6366f1] transition-all duration-300';
                } else {
                    dots[i].className = 'w-2 h-2 rounded-full bg-gray-600 hover:bg-white transition-all duration-300';
                }
            } else {
                 dots[i].className = 'w-2 h-2 rounded-full bg-gray-600 hover:bg-white transition-all duration-300';
            }
        }
    }

    // Run
    init();

</script>
</body>
</html>

你可以通过与我对话，继续完善或修改代码，也可以重新开始一个新对话。
