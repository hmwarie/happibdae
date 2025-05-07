<script>
  import { onMount } from 'svelte';
  import { gsap } from 'gsap';
  import { link } from 'svelte-spa-router';

  let balloons = [];
  let titleVisible = false;
  let buttonVisible = false;

  function createBalloon() {
    const colors = ['#FF69B4', '#9370DB', '#87CEEB', '#FFB6C1', '#FFA07A'];
    const size = 40 + Math.random() * 20;
    const x = Math.random() * 100;
    const delay = Math.random() * 2;
    const duration = 3 + Math.random() * 2;
    const swayAmount = 10 + Math.random() * 20;

    return {
      x,
      y: 120,
      size,
      color: colors[Math.floor(Math.random() * colors.length)],
      delay,
      duration,
      swayAmount,
      rotation: Math.random() * 360
    };
  }

  function createBalloons() {
    for (let i = 0; i < 15; i++) {
      balloons.push(createBalloon());
    }
    balloons = balloons;
  }

  onMount(() => {
    createBalloons();

    // Animate balloons
    balloons.forEach((balloon, index) => {
      const element = document.querySelector(`[data-balloon="${index}"]`);
      if (element) {
        gsap.to(element, {
          y: -150,
          x: `+=${balloon.swayAmount}`,
          rotation: balloon.rotation + 360,
          duration: balloon.duration,
          delay: balloon.delay,
          ease: "power1.inOut",
          repeat: -1,
          yoyo: true
        });
      }
    });

    // Animate title
    gsap.to(".title", {
      opacity: 1,
      y: 0,
      duration: 1,
      delay: 0.5,
      ease: "back.out(1.7)",
      onComplete: () => {
        titleVisible = true;
      }
    });

    // Animate date
    gsap.to(".date", {
      opacity: 1,
      y: 0,
      duration: 1,
      delay: 0.8,
      ease: "back.out(1.7)"
    });

    // Animate buttons
    gsap.to(".button-container", {
      opacity: 1,
      y: 0,
      duration: 1,
      delay: 1,
      ease: "back.out(1.7)",
      onComplete: () => {
        buttonVisible = true;
      }
    });
  });
</script>

<div class="min-h-screen bg-gradient-to-br from-pink-100 to-purple-100 p-4 md:p-8 overflow-hidden relative flex items-center justify-center">
  <!-- Decorative elements -->
  <div class="absolute inset-0 overflow-hidden">
    <div class="absolute top-0 left-0 w-48 md:w-64 h-48 md:h-64 bg-pink-200 rounded-full mix-blend-multiply filter blur-xl opacity-70 animate-blob"></div>
    <div class="absolute top-0 right-0 w-48 md:w-64 h-48 md:h-64 bg-purple-200 rounded-full mix-blend-multiply filter blur-xl opacity-70 animate-blob animation-delay-2000"></div>
    <div class="absolute -bottom-8 left-20 w-48 md:w-64 h-48 md:h-64 bg-pink-300 rounded-full mix-blend-multiply filter blur-xl opacity-70 animate-blob animation-delay-4000"></div>
  </div>

  {#each balloons as balloon, i}
    <div 
      class="balloon absolute"
      data-balloon={i}
      style="
        left: {balloon.x}vw;
        bottom: {balloon.y}vh;
        width: {balloon.size}px;
        height: {balloon.size * 1.2}px;
        background-color: {balloon.color};
        border-radius: 50% 50% 50% 50% / 60% 60% 40% 40%;
        transform: rotate({balloon.rotation}deg);
        box-shadow: inset -5px -5px 15px rgba(0,0,0,0.1);
      "
    >
      <div class="absolute bottom-0 left-1/2 transform -translate-x-1/2 w-1 h-8 bg-gray-300"></div>
      <div class="absolute bottom-0 left-1/2 transform -translate-x-1/2 w-2 h-2 bg-gray-400 rounded-full"></div>
      <div class="absolute top-1/4 left-1/4 w-1/4 h-1/4 bg-white/30 rounded-full"></div>
    </div>
  {/each}

  <div class="container mx-auto max-w-4xl text-center relative z-10 px-4 sm:px-6">
    <p class="date text-xl sm:text-2xl text-transparent bg-clip-text bg-gradient-to-r from-pink-500 to-purple-500 mb-3 sm:mb-4 opacity-0 transform translate-y-8">
      May 11, 2025
    </p>

    <h1 class="title text-4xl sm:text-5xl md:text-7xl font-bold text-transparent bg-clip-text bg-gradient-to-r from-pink-500 to-purple-500 mb-3 sm:mb-4 opacity-0 transform translate-y-8">
      Happy Birthday~!
    </h1>

    <div class="button-container opacity-0 transform translate-y-8 space-y-4 sm:space-y-6">
      <a 
        href="/mom-message" 
        use:link
        class="block w-full sm:w-auto sm:inline-block px-6 sm:px-8 py-3 sm:py-4 bg-gradient-to-r from-pink-500 to-purple-500 text-white text-lg sm:text-xl font-semibold rounded-full shadow-lg hover:shadow-xl transform hover:scale-105 transition-all duration-300 relative overflow-hidden group"
      >
        <span class="relative z-10">Mommy 💝</span>
        <div class="absolute inset-0 bg-gradient-to-r from-pink-600 to-purple-600 opacity-0 group-hover:opacity-100 transition-opacity duration-300"></div>
      </a>
      <a 
        href="/sister-message" 
        use:link
        class="block w-full sm:w-auto sm:inline-block px-6 sm:px-8 py-3 sm:py-4 bg-gradient-to-r from-purple-500 to-pink-500 text-white text-lg sm:text-xl font-semibold rounded-full shadow-lg hover:shadow-xl transform hover:scale-105 transition-all duration-300 relative overflow-hidden group"
      >
        <span class="relative z-10">Chong 😜</span>
        <div class="absolute inset-0 bg-gradient-to-r from-purple-600 to-pink-600 opacity-0 group-hover:opacity-100 transition-opacity duration-300"></div>
      </a>
    </div>
  </div>
</div>

<style>
  .balloon {
    position: absolute;
    will-change: transform;
  }

  .balloon::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: radial-gradient(circle at 30% 30%, rgba(255,255,255,0.4) 0%, rgba(255,255,255,0) 50%);
    border-radius: inherit;
  }

  @keyframes blob {
    0% {
      transform: translate(0px, 0px) scale(1);
    }
    33% {
      transform: translate(30px, -50px) scale(1.1);
    }
    66% {
      transform: translate(-20px, 20px) scale(0.9);
    }
    100% {
      transform: translate(0px, 0px) scale(1);
    }
  }

  .animate-blob {
    animation: blob 7s infinite;
  }

  .animation-delay-2000 {
    animation-delay: 2s;
  }

  .animation-delay-4000 {
    animation-delay: 4s;
  }

  /* Mobile-specific adjustments */
  @media (max-width: 640px) {
    .container {
      padding-top: 2rem;
      padding-bottom: 2rem;
    }

    .title {
      line-height: 1.2;
      margin-bottom: 1rem;
    }

    .date {
      line-height: 1.2;
      margin-bottom: 2rem;
    }

    .button-container {
      padding: 0 1rem;
    }
  }
</style> 