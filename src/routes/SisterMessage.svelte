<script>
  import { onMount } from 'svelte';
  import { gsap } from 'gsap';
  import { link } from 'svelte-spa-router';
  import confetti from 'canvas-confetti';

  const photos = [
    { src: "/happibdae/images/sister1.jpeg" },
    { src: "/happibdae/images/sister2.jpeg" },
    { src: "/happibdae/images/sister3.jpeg" },
    { src: "/happibdae/images/sister4.jpeg" }
  ];

  onMount(() => {
    // Trigger confetti animation
    const duration = 3 * 1000;
    const animationEnd = Date.now() + duration;
    const defaults = { startVelocity: 30, spread: 360, ticks: 60, zIndex: 0 };

    function randomInRange(min, max) {
      return Math.random() * (max - min) + min;
    }

    const interval = setInterval(function() {
      const timeLeft = animationEnd - Date.now();

      if (timeLeft <= 0) {
        return clearInterval(interval);
      }

      const particleCount = 50 * (timeLeft / duration);
      
      confetti({
        ...defaults,
        particleCount,
        origin: { x: randomInRange(0.1, 0.3), y: Math.random() - 0.2 }
      });
      confetti({
        ...defaults,
        particleCount,
        origin: { x: randomInRange(0.7, 0.9), y: Math.random() - 0.2 }
      });
    }, 250);

    // Initial card reveal animation
    gsap.from(".card", {
      duration: 1.5,
      scale: 0.8,
      opacity: 0,
      rotationX: -45,
      transformOrigin: "50% 0%",
      ease: "power4.out"
    });

    // Animate content with better timing
    const timeline = gsap.timeline({ delay: 0.8 });
    
    timeline.from(".title", {
      duration: 1,
      y: 30,
      opacity: 0,
      ease: "back.out(1.7)"
    })
    .from(".content-section", {
      duration: 0.8,
      y: 20,
      opacity: 0,
      ease: "power2.out"
    }, "-=0.3");
  });
</script>

<main class="min-h-screen bg-gradient-to-br from-pink-100 to-purple-100 p-4 md:p-8 overflow-hidden flex items-center justify-center">
  <!-- Main card -->
  <div class="card w-full max-w-4xl bg-white rounded-3xl shadow-2xl p-6 md:p-8 lg:p-12 relative">
    <h1 class="title text-3xl md:text-4xl lg:text-5xl font-bold text-center text-transparent bg-clip-text bg-gradient-to-r from-pink-500 to-purple-500 mb-6 md:mb-8">
      HBD CHONG! 🎉
    </h1>

    <div class="content-section">
      <div class="message text-center space-y-4 md:space-y-6 mb-8">
        <p class="text-lg md:text-xl text-gray-700 leading-relaxed">
          HBD CHOOONGGGGGGUUUSSSSSS LOE,,
        </p>
        
        <p class="text-lg md:text-xl text-gray-700 leading-relaxed">
          Tralalero Tralala, porco dio e porco Allah. Ero con il mio fottuto figlio merdardo a giocare a Fortnite, quando a un punto arriva mia nonna slmuch sa pagjjudge sken, napapaaral aq,,
        </p>
      </div>

      <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 md:gap-6 mb-8 max-w-2xl mx-auto">
        {#each photos as photo}
          <div class="photo-container relative rounded-xl overflow-hidden shadow-lg transform hover:scale-105 transition-all duration-300 aspect-square w-full">
            <img 
              src={photo.src} 
              alt="Sister photos" 
              class="absolute inset-0 w-full h-full object-cover"
            />
          </div>
        {/each}
      </div>

      <div class="flex justify-center space-x-4 md:space-x-6 mb-8">
        <span class="decorative-emoji text-3xl md:text-4xl transform hover:scale-125 transition-transform duration-300 cursor-pointer">💝</span>
        <span class="decorative-emoji text-3xl md:text-4xl transform hover:scale-125 transition-transform duration-300 cursor-pointer">🎁</span>
        <span class="decorative-emoji text-3xl md:text-4xl transform hover:scale-125 transition-transform duration-300 cursor-pointer">🎂</span>
        <span class="decorative-emoji text-3xl md:text-4xl transform hover:scale-125 transition-transform duration-300 cursor-pointer">✨</span>
        <span class="decorative-emoji text-3xl md:text-4xl transform hover:scale-125 transition-transform duration-300 cursor-pointer">🎊</span>
      </div>

      <p class="signature text-center text-gray-500 italic mb-6">
        slmuch sa lht bez,, 
      </p>

      <a 
        href="/"
        use:link
        class="block text-center text-pink-500 hover:text-pink-600 transition-colors duration-300"
      >
        ← Back to Home
      </a>
    </div>
  </div>
</main>

<style>
  .card {
    transform-style: preserve-3d;
    backface-visibility: hidden;
    max-height: 90vh;
    overflow-y: auto;
  }

  .confetti {
    position: absolute;
    border-radius: 4px;
    will-change: transform;
  }

  /* Custom scrollbar for the card */
  .card::-webkit-scrollbar {
    width: 8px;
  }

  .card::-webkit-scrollbar-track {
    background: rgba(255, 255, 255, 0.1);
    border-radius: 4px;
  }

  .card::-webkit-scrollbar-thumb {
    background: linear-gradient(to bottom, #FF69B4, #9370DB);
    border-radius: 4px;
  }

  .card::-webkit-scrollbar-thumb:hover {
    background: linear-gradient(to bottom, #FF1493, #8A2BE2);
  }
</style> 