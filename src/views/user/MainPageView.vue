<template>
  <div class="main-container">
    <!-- 背景效果 -->
    <div class="background-effect"></div>

    <div class="content z-10 text-center">
      <div class="text-sm mb-4 text-gray-500 animate-fade-in">
        皮皮翔----Online Judge
      </div>

      <h1
        class="text-4xl md:text-6xl font-bold mb-12 leading-tight animate-slide-up"
      >
        Debug your limits<br />Compile your future
      </h1>

      <div
        class="gradient-bar-container w-full max-w-2xl mx-auto mb-16 relative animate-expand"
      >
        <div class="gradient-bar w-full h-16"></div>
        <div
          class="typing-animation absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 text-2xl font-bold text-white"
        >
          {{ typedText }}
        </div>
      </div>

      <p
        class="text-gray-500 mb-6 animate-fade-in"
        style="animation-delay: 0.5s"
      >
        Elevate your coding, one problem at a time
      </p>

      <button
        class="start-button px-8 py-3 rounded-full bg-transparent border border-gray-600 text-white hover:bg-white hover:text-black transition-all duration-300 mb-12 animate-fade-in"
        style="animation-delay: 0.7s"
        @click="goToPage"
      >
        Start
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();
const typedText = ref("");
const fullText = "Coding...";

const goToPage = () => {
  router.push("/questions");
};

const typeText = () => {
  let i = 0;
  const typing = setInterval(() => {
    if (i < fullText.length) {
      typedText.value += fullText.charAt(i);
      i++;
    } else {
      clearInterval(typing);
      setTimeout(() => {
        typedText.value = "";
        typeText();
      }, 1000);
    }
  }, 150);
};

onMounted(() => {
  typeText();
});
</script>

<style scoped>
.main-container {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  position: relative;
  color: white;
  padding: 1rem;
}

.background-effect {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: radial-gradient(
    circle,
    rgba(0, 0, 0, 0.8) 0%,
    rgba(0, 0, 0, 0.4) 70%,
    rgba(0, 0, 0, 0) 100%
  );
  z-index: -1;
}

.gradient-bar-container {
  position: relative;
}

.gradient-bar {
  background: linear-gradient(90deg, #ff6b6b, #4ecdc4, #45b7d1);
  border-radius: 8px;
  overflow: hidden;
  position: relative;
}

.gradient-bar::before {
  content: "";
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(
    90deg,
    transparent,
    rgba(255, 255, 255, 0.2),
    transparent
  );
  transition: 0.5s;
  animation: shine 3s infinite;
}

@keyframes shine {
  0% {
    left: -100%;
  }
  100% {
    left: 100%;
  }
}

.start-button:hover {
  transform: translateY(-3px);
  box-shadow: 0 4px 15px rgba(255, 255, 255, 0.1);
}

.animate-fade-in {
  animation: fadeIn 1s ease-out forwards;
  opacity: 0;
}

.animate-slide-up {
  animation: slideUp 1s ease-out forwards;
  opacity: 0;
}

.animate-expand {
  animation: expand 1s ease-out forwards;
  transform: scaleX(0);
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

@keyframes slideUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes expand {
  from {
    transform: scaleX(0);
  }
  to {
    transform: scaleX(1);
  }
}

.typing-animation {
  display: inline-block;
  overflow: hidden;
  border-right: 2px solid white;
  white-space: nowrap;
  animation: typing 3.5s steps(40, end) infinite,
    blink-caret 0.75s step-end infinite;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
}

@keyframes typing {
  from {
    width: 0;
  }
  to {
    width: 100%;
  }
}

@keyframes blink-caret {
  from,
  to {
    border-color: transparent;
  }
  50% {
    border-color: white;
  }
}
</style>
