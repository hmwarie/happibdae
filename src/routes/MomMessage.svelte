<script>
  import { onMount } from 'svelte';
  import { gsap } from 'gsap';
  import { link } from 'svelte-spa-router';
  import confetti from 'canvas-confetti';

  let photos = [];

  const galleryPhotos = [
    { id: 1, placeholder: "Add Photo 1", defaultImage: "/images/mom1.jpg" },
    { id: 2, placeholder: "Add Photo 2", defaultImage: "/images/mom2.jpg" },
    { id: 3, placeholder: "Add Photo 3", defaultImage: "/images/mom3.jpg" },
    { id: 4, placeholder: "Add Photo 4", defaultImage: "/images/mom4.jpg" }
  ];

  function handleFileInput(event, photoId) {
    const file = event.target.files[0];
    if (file) {
      const reader = new FileReader();
      reader.onload = (e) => {
        const existingPhotoIndex = photos.findIndex(p => p.id === photoId);
        if (existingPhotoIndex !== -1) {
          photos[existingPhotoIndex] = {
            ...photos[existingPhotoIndex],
            src: e.target.result
          };
        } else {
          photos = [...photos, {
            id: photoId,
            src: e.target.result
          }];
        }
        photos = photos;
      };
      reader.readAsDataURL(file);
    }
  }

  onMount(() => {
    // Load default images
    galleryPhotos.forEach(photo => {
      photos = [...photos, {
        id: photo.id,
        src: photo.defaultImage
      }];
    });

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
      Happy Birthday, Mommy! 🎉
    </h1>

    <div class="content-section">
      <div class="message text-center space-y-4 md:space-y-6 mb-8">
        <p class="text-lg md:text-xl text-gray-700 leading-relaxed">
          May this day be as special as you are! Here's to another year of amazing memories, 
          endless laughter, and beautiful moments. You bring so much joy to everyone around you, 
          and today we celebrate the wonderful person you are.
        </p>
        
        <p class="text-lg md:text-xl text-gray-700 leading-relaxed">
          Wishing you all the happiness your heart can hold, all the smiles a day can bring, 
          and all the blessings a life can unfold.
        </p>
      </div>

      <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 md:gap-6 mb-8">
        {#each galleryPhotos as photo}
          <div 
            class="photo-placeholder group relative bg-gradient-to-br from-pink-100 to-purple-100 rounded-xl p-4 aspect-square flex items-center justify-center text-gray-500 transform hover:scale-105 transition-all duration-300 cursor-pointer shadow-md overflow-hidden"
            role="button"
            tabindex="0"
            aria-label="Photo placeholder - click to upload"
          >
            <input 
              type="file" 
              id="photo-upload-{photo.id}" 
              accept="image/*" 
              class="hidden" 
              on:change={(e) => handleFileInput(e, photo.id)}
            />
            <label 
              for="photo-upload-{photo.id}"
              class="absolute inset-0 flex flex-col items-center justify-center cursor-pointer"
            >
              <div class="absolute inset-0 bg-gradient-to-br from-pink-200/20 to-purple-200/20 opacity-0 group-hover:opacity-100 transition-opacity duration-300"></div>
              <div class="relative z-10 flex flex-col items-center">
                <svg class="w-10 h-10 md:w-12 md:h-12 mb-2 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" aria-hidden="true">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l1.586-1.586a2 2 0 012.828 0L20 14m-6-6h.01M6 20h12a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v12a2 2 0 002 2z" />
                </svg>
                <span class="text-sm md:text-base font-medium">{photo.placeholder}</span>
              </div>
            </label>
          </div>
        {/each}
      </div>

      <div class="flex justify-center space-x-4 md:space-x-6 mb-8">
        <span class="decorative-emoji text-3xl md:text-4xl transform hover:scale-125 transition-transform duration-300 cursor-pointer">🎈</span>
        <span class="decorative-emoji text-3xl md:text-4xl transform hover:scale-125 transition-transform duration-300 cursor-pointer">🎁</span>
        <span class="decorative-emoji text-3xl md:text-4xl transform hover:scale-125 transition-transform duration-300 cursor-pointer">🎂</span>
        <span class="decorative-emoji text-3xl md:text-4xl transform hover:scale-125 transition-transform duration-300 cursor-pointer">✨</span>
        <span class="decorative-emoji text-3xl md:text-4xl transform hover:scale-125 transition-transform duration-300 cursor-pointer">🎊</span>
      </div>

      <p class="signature text-center text-gray-500 italic mb-6">
        With love and best wishes ❤️
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

  .photo-placeholder {
    position: relative;
    overflow: hidden;
    transition: all 0.3s ease;
  }

  .photo-placeholder::before {
    content: '';
    position: absolute;
    top: -50%;
    left: -50%;
    width: 200%;
    height: 200%;
    background: linear-gradient(
      45deg,
      rgba(255,255,255,0) 0%,
      rgba(255,255,255,0.8) 50%,
      rgba(255,255,255,0) 100%
    );
    transform: rotate(45deg);
    animation: shine 3s infinite;
  }

  .photo-placeholder:hover::before {
    animation: shine 1.5s infinite;
  }

  @keyframes shine {
    0% {
      transform: translateX(-100%) rotate(45deg);
    }
    100% {
      transform: translateX(100%) rotate(45deg);
    }
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