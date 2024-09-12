<script setup>
import { ref, onMounted } from "vue";

// Counter
const count = ref(0);
function increase() {
  count.value++;
}
function decrease() {
  count.value--;
}


// Box Color
const bgColor = ref("#ffffff");
function randomColor() {
  bgColor.value = `#${Math.floor(Math.random() * 16777215).toString(16)}`;
}

// Carousel
const images = ref([]);
const currentIndex = ref(0);
const totalSlides = ref(0);

const fetchImages = async () => {
  try {
    const response = await fetch(
      "https://picsum.photos/v2/list?page=1&limit=10"
    );
    const data = await response.json();
    images.value = data;
    totalSlides.value = data.length;
  } catch (error) {
    console.error("Error fetching images:", error);
  }
};

const nextSlide = () => {
  currentIndex.value = (currentIndex.value + 1) % totalSlides.value;
};

const prevSlide = () => {
  currentIndex.value = (currentIndex.value - 1 + totalSlides.value) % totalSlides.value;
};

onMounted(fetchImages);
</script>

<template>
  <main>
    <div class="counter-container">
      <div>
        <h1>Counter</h1>
        <span id="value">{{ count }}</span>
      </div>
      <div>
        <button class="btn-counter" @click="decrease" id="decrement">-</button>
        <button class="btn-counter" @click="increase" id="increment">+</button>
      </div>
    </div>
    <div class="boxColor-container">
      <h1 :style="{ textAlign: center }">Box Color</h1>
      <div>
        <input type="color" name="color" id="color" v-model="bgColor" />
        <button class="btn-counter" @click="randomColor">Random Color</button>
      </div>
      <div class="box-color" :style="{ backgroundColor: bgColor }"></div>
    </div>
    <div class="carousel-container">
      <div class="carousel">
        <div class="carousel-track" :style="{ transform: `translateX(-${currentIndex * 100}%)` }">
          <div v-for="(image, index) in images" :key="index" class="carousel-slide">
            <img :src="image.download_url" :alt="'Image by ' + image.author" class="carousel-image" />
          </div>
        </div>
      </div>
      <button class="prev-button" @click="prevSlide">❮</button>
      <button class="next-button" @click="nextSlide">❯</button>
    </div>
  </main>
</template>

<style scoped>
.counter-container {
  text-align: center;
  background-color: #333333;
  padding: 20px;
  border-radius: 8px;
  width: 200px;
  margin: 0 auto;
}

#value {
  font-size: 50px;
}

.btn-counter {
  font-size: 16px;
  margin: 0 5px;
  background-color: #555555;
  color: #ffffff;
  border: none;
  padding: 10px 20px;
  cursor: pointer;
  border-radius: 4px;
}

.btn-counter:hover {
  background-color: #777777;
}

.btn-counter:active {
  animation: pulse 0.5s linear;
}

@keyframes pulse {
  0% {
    transform: scale(1);
  }

  50% {
    transform: scale(1.1);
  }

  100% {
    transform: scale(1);
  }
}

.boxColor-container>h1,
.boxColor-container>div {
  text-align: center;
}

.boxColor-container {
  text-align: center;
  background-color: #333333;
  padding: 20px;
  border-radius: 8px;
  margin: 20px auto;
  width: 500px;
}

.box-color {
  width: 400px;
  height: 200px;
  margin: 20px auto;
  border: 2px double #ffffff;
}

/* Carousel */
.carousel-container {
  position: relative;
  width: 100%;
  max-width: 600px;
  margin: 0 auto;
}

.carousel {
  display: flex;
  overflow: hidden;
  width: 100%;
}

.carousel-track {
  display: flex;
  transition: transform 1s ease-in-out;
}

.carousel-slide {
  min-width: 100%;
  box-sizing: border-box;
}

.carousel-image {
  width: 100%;
  height: auto;
  border-radius: 10px;
}

.prev-button,
.next-button {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background-color: rgba(0, 0, 0, 0.5);
  color: white;
  border: none;
  padding: 10px;
  cursor: pointer;
  font-size: 18px;
  z-index: 10;
}

.prev-button {
  left: 10px;
}

.next-button {
  right: 10px;
}

.prev-button:hover,
.next-button:hover {
  background-color: rgba(0, 0, 0, 0.8);
}
</style>