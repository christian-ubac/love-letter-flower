<script setup>
import { ref, onMounted } from "vue";

const started = ref(false);
const showFinalMessage = ref(false);
const showLetter = ref(false); // Changed to false initially

const images = [
  "/images/552695394_1958636704985836_840471227951268747_n.jpg",
  "/images/552755676_1990469865034745_2052691995805818406_n.jpg",
  "/images/552959764_1067076765313326_8074673612818758816_n.jpg",
  "/images/552984246_815725274485880_9041123124537552615_n.jpg",
  "/images/552988454_1319645739663430_508104464861593075_n.jpg",
  "/images/553069770_1468053791097407_8926176518062053062_n.jpg",
  "/images/553207760_2208540586292663_7317930103253802704_n.jpg",
  "/images/553498529_23966606409679665_5723428415768529443_n.jpg",
  "/images/553512159_1385866896204439_7504415003124421612_n.jpg",
  "/images/553631247_804128215495659_9166532849030867510_n.jpg",
  "/public/images/552567622_1494979808303901_2510029422889710575_n.jpg",
  "/public/images/553547884_806506761864777_5528090484260553669_n.jpg",
  "public/images/552976119_1525766392179333_5844929765314816695_n.jpg"
];
const currentImage = ref(0);

const music = "/Your Universe (Acoustic).mp3";
const bgMusic = ref(null);

const hearts = Array.from({ length: 20 }, (_, i) => i);

const letter = `
My Love,

Today marks nine beautiful months of our journey together. 
Every moment with you feels like a precious gift, and I'm endlessly grateful for your love.

Remember our first date? How nervous we both were? Now look at us - growing stronger together with each passing day.

Your smile still makes my heart skip a beat, and your laughter is my favorite sound in the world.

I cherish every memory we've created and look forward to all the beautiful moments yet to come.

With all my love,
Always Yours
`;

const displayedText = ref("");
let letterIndex = 0;
let typingInterval;

function typeWriter() {
  if (letterIndex < letter.length) {
    displayedText.value += letter.charAt(letterIndex);
    letterIndex++;
  } else {
    clearInterval(typingInterval);
    setTimeout(() => {
      showFinalMessage.value = true;
    }, 800);
  }
}

function startExperience() {
  started.value = true;
  bgMusic.value.play();
  typingInterval = setInterval(typeWriter, 50);
}

function openLetter() {
  showLetter.value = true;
  displayedText.value = "";
  letterIndex = 0;
  showFinalMessage.value = false;
  typingInterval = setInterval(typeWriter, 50);
}

function closeLetter() {
  showLetter.value = false;
  clearInterval(typingInterval);
}

onMounted(() => {
  setInterval(() => {
    currentImage.value = (currentImage.value + 1) % images.length;
  }, 4000);

  const canvas = document.querySelector("canvas");
  const ctx = canvas.getContext("2d");

  function resizeCanvas() {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
  }
  resizeCanvas();
  window.addEventListener("resize", resizeCanvas);

  const stars = [];
  for (let i = 0; i < 100; i++) {
    stars.push({
      x: Math.random() * canvas.width,
      y: Math.random() * canvas.height,
      size: Math.random() * 2,
      speed: Math.random() * 0.3 + 0.1,
      opacity: Math.random()
    });
  }

  function animateStars() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    stars.forEach(star => {
      ctx.beginPath();
      ctx.arc(star.x, star.y, star.size, 0, Math.PI * 2);
      ctx.fillStyle = `rgba(255, 255, 255, ${star.opacity})`;
      ctx.shadowBlur = 10;
      ctx.shadowColor = "white";
      ctx.fill();

      star.y += star.speed;
      if (star.y > canvas.height) {
        star.y = 0;
        star.x = Math.random() * canvas.width;
      }

      star.opacity += (Math.random() - 0.5) * 0.02;
      if (star.opacity < 0.2) star.opacity = 0.2;
      if (star.opacity > 1) star.opacity = 1;
    });
    requestAnimationFrame(animateStars);
  }
  animateStars();
});
</script>

<template>
  <div class="h-screen w-screen flex items-center justify-center bg-black relative overflow-hidden">
    <!-- Star Particles -->
    <canvas class="absolute inset-0"></canvas>

    <!-- Slideshow Background -->
    <div class="absolute inset-0 flex items-center justify-center">
      <transition-group name="fade" tag="div">
        <img
          v-for="(image, index) in images"
          v-show="currentImage === index"
          :key="index"
          :src="image"
          class="slideshow-card"
        />
      </transition-group>

      <!-- Flower Overlay -->
      <div
        class="absolute inset-0 bg-gradient-to-br from-pink-900/20 via-purple-900/20 to-red-900/20 opacity-60">
      </div>

      <!-- Softer Cinema Dark Overlay -->
      <div class="absolute inset-0 bg-gradient-to-b from-black/60 via-black/40 to-black/70"></div>
    </div>

    <!-- Floating Hearts -->
    <div class="absolute inset-0 pointer-events-none z-5">
      <span
        v-for="n in hearts"
        :key="n"
        class="heart"
        :style="{
          left: Math.random() * 100 + '%',
          animationDuration: (Math.random() * 3 + 4) + 's',
          fontSize: (Math.random() * 20 + 20) + 'px'
        }"
      >❤️</span>
    </div>

    <!-- Start Button -->
    <div v-if="!started" class="fixed bottom-8 left-1/2 transform -translate-x-1/2 z-10">
      <button
        @click="startExperience"
        class="px-8 py-4 bg-gradient-to-r from-pink-500 via-red-500 to-yellow-400 hover:scale-105 transition transform 
               rounded-2xl text-white text-lg md:text-xl shadow-lg font-semibold animate-bounce"
      >
        Tap to Open 💌
      </button>
    </div>

    <!-- Main Content Area -->
    <div v-else class="relative z-10 w-full h-full flex flex-col items-center justify-center px-4 pt-4 pb-32">
      
      <!-- Letter Content -->
      <transition name="fade">
        <div v-if="showLetter" class="w-full max-w-3xl flex flex-col h-full max-h-[80vh]">
          <div class="flex justify-between items-center mb-4">
            <h1
              class="text-2xl md:text-4xl font-bold bg-gradient-to-r from-pink-400 via-red-400 to-yellow-300 
                     bg-clip-text text-transparent drop-shadow-lg animate-pulse"
            >
              Happy 9th Monthsary 💕
            </h1>
            <!-- Close Button -->
            <button
              @click="closeLetter"
              class="ml-4 text-white bg-pink-500/70 hover:bg-pink-600 rounded-full px-3 py-1 font-bold shadow-lg transition-all"
            >
              ✕
            </button>
          </div>

          <div
            class="letter-box flex-1 overflow-y-auto px-6 py-4 
                   rounded-2xl bg-white/10 backdrop-blur-xl border border-pink-200/40 shadow-glow text-left"
          >
            <p class="text-base md:text-lg leading-relaxed whitespace-pre-line text-gradient drop-shadow-md">
              {{ displayedText }}
            </p>
          </div>

          <!-- Final Message -->
          <transition name="fade">
            <p
              v-if="showFinalMessage"
              class="mt-4 text-xl md:text-2xl font-bold bg-gradient-to-r from-pink-300 via-red-400 to-yellow-300 
                     bg-clip-text text-transparent drop-shadow-lg animate-pulse text-center"
            >
              I love you forever 💖
            </p>
          </transition>
        </div>
      </transition>

      <!-- Always Visible Open Letter Button (when letter is closed) -->
      <div v-if="started && !showLetter" class="fixed bottom-8 left-1/2 transform -translate-x-1/2 z-20">
        <button
          @click="openLetter"
          class="px-6 py-3 bg-gradient-to-r from-pink-500 via-red-500 to-yellow-400 hover:scale-105 transition transform 
                 rounded-xl text-white text-lg shadow-lg font-semibold animate-pulse"
        >
          Open Letter 💌
        </button>
      </div>
    </div>

    <!-- Background Music -->
    <audio ref="bgMusic" :src="music" preload="auto"></audio>
  </div>
</template>

<style scoped>
/* Fade Transition */
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s ease, transform 0.5s ease;
}
.fade-enter-from, .fade-leave-to {
  opacity: 0;
  transform: translateY(20px);
}

/* Slideshow Image */
.slideshow-card {
  max-width: 80%;
  max-height: 70%;
  object-fit: cover;
  margin: auto;
  border-radius: 20px;
  box-shadow: 0 0 25px rgba(255, 182, 193, 0.5),
              0 0 50px rgba(255, 105, 180, 0.4),
              0 0 80px rgba(255, 215, 0, 0.3);
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

/* Floating Hearts */
.heart {
  position: absolute;
  bottom: -10%;
  text-shadow: 0 0 10px rgba(255, 100, 150, 0.8),
               0 0 20px rgba(255, 150, 200, 0.6);
  animation: floatUp linear infinite;
}
@keyframes floatUp {
  from {
    transform: translateY(0) rotate(0deg);
    opacity: 1;
  }
  to {
    transform: translateY(-120vh) rotate(360deg);
    opacity: 0;
  }
}

/* Letter Box Scrollbar */
.letter-box::-webkit-scrollbar {
  width: 8px;
}
.letter-box::-webkit-scrollbar-track {
  background: rgba(255, 255, 255, 0.05);
  border-radius: 10px;
}
.letter-box::-webkit-scrollbar-thumb {
  background: linear-gradient(180deg, #ff7eb9, #ff65a3, #fcb045);
  border-radius: 10px;
}

/* Gradient Text */
.text-gradient {
  background: linear-gradient(180deg, #fff, #ffb6c1, #ffd27f);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

/* Glowing Border */
.shadow-glow {
  box-shadow: 0 0 25px rgba(255, 182, 193, 0.6),
              0 0 50px rgba(255, 105, 180, 0.4),
              0 0 80px rgba(255, 215, 0, 0.3);
}
</style>