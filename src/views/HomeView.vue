<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";
import Navbar from "@/components/Navbar.vue";
import Footer from "@/components/Footer.vue";

import hero1 from "@/assets/bg.jpg";
import hero2 from "@/assets/bg1.png";
import hero3 from "@/assets/bg3.jpg";

const images = [hero1, hero2, hero3, hero3, hero2, hero1];
const currentImageIndex = ref(0);
const upcomingEvents = ref([]);

const fetchEvents = async () => {
  try {
    const response = await axios.get("http://127.0.0.1:8000/api/events/");
    const events = response.data;
    const currentDate = new Date();

    // Filter only upcoming events
    upcomingEvents.value = events.filter((event) => {
      const eventDate = new Date(event.date);
      return eventDate >= currentDate;
    });
  } catch (error) {
    console.error("Failed to fetch events", error);
  }
};

onMounted(() => {
  setInterval(() => {
    currentImageIndex.value = (currentImageIndex.value + 1) % images.length;
  }, 5000);

  fetchEvents();
});
</script>

<template>
  <div>
    <div
      class="body"
      :style="{
        backgroundImage: `url(${images[currentImageIndex]})`,
        backgroundSize: 'cover',
        backgroundPosition: 'center',
        minHeight: '100vh',
        transition: 'background-image 5s ease-in-out',
      }"
    >
      <!-- Navbar -->
      <header>
        <Navbar />
      </header>

      <!-- Hero Section -->
      <section class="hero">
        <div class="hero-overlay"></div>
        <div class="hero-content container text-center text-white">
          <p class="mt-2">
            Welcome to Artificial Intelligence Alliance in Agriculture (AIAA)
          </p>
          <p class="mt-3">
            Enhancing agricultural productivity and sustainability in Kenya
            through AI technologies.
          </p>
          <a
            class="btn btn-outline-success btn-lg mt-4 text-white border-2"
            href="#about"
            role="button"
            >Get Started</a
          >
        </div>
      </section>
    </div>

    <!-- Main Content -->
    <main>
      <!-- Introduction Section -->
      <section class="introduction py-5">
        <div class="container" id="about">
          <div class="row align-items-center">
            <div class="col-md-6 text-center text-md-start mb-4 mb-md-0">
              <h2 class="h2">About Us</h2>
              <hr class="bg-warning w-25 mx-auto mx-md-0" />
              <p class="mt-3">
                AI AA NGO is dedicated to enhancing agricultural productivity
                and sustainability in Kenya. We utilize artificial intelligence
                to optimize farming practices, forecast yields, and improve
                market accessibility for smallholder farmers.
              </p>
            </div>
            <div class="col-md-6 d-flex justify-content-center">
              <img
                src="@/assets/il2.jpg"
                class="img-fluid rounded"
                alt="About Us"
              />
            </div>
          </div>
        </div>
      </section>

      <!-- Vision, Mission, and Areas of Coverage Section -->
      <section class="vision-mission py-2">
        <div class="container">
          <div class="row">
            <div
              class="col-md-6 d-flex justify-content-center order-md-1 order-2"
            >
              <img
                src="@/assets/maize.jpg"
                class="img-fluid rounded"
                alt="About Us"
              />
            </div>
            <div class="col-md-6 mb-4 order-md-2 order-1">
              <h2 class="h2">Vision, Mission, and Areas of Coverage</h2>
              <p class="fs-6">
                ~To revolutionize Kenyan agriculture through artificial
                intelligence, creating a sustainable, productive, and prosperous
                farming sector.
              </p>
              <p class="fs-6">
                ~We aim to empower Kenyan farmers and agricultural stakeholders
                with cutting-edge AI technologies and data-driven insights.
              </p>
              <p class="fs-6">
                ~Counties such as Kisii, Baringo, Mombasa, Lamu, and Turkana.
              </p>
            </div>
          </div>
        </div>
      </section>

      <!-- Services Section -->
      <section class="services py-5 mt-5 bg-light">
        <div class="container">
          <h2 class="h2 text-center">Our Objectives</h2>
          <hr class="bg-warning mx-auto" style="width: 15%" />
          <div class="row mt-4">
            <!-- Objective 1 -->
            <div class="col-md-4 mb-4">
              <div class="card shadow-sm border-0">
                <img
                  src="@/assets/bg.jpg"
                  class="card-img-top"
                  alt="Innovation Integration"
                />
                <div class="card-body text-center">
                  <h5 class="card-title fw-bold">Innovation Integration</h5>
                  <p class="card-text">
                    Introduce AI-driven tools and techniques to 500 farms across
                    Kenya by 2025 to increase crop yields and resource
                    efficiency.
                  </p>
                </div>
              </div>
            </div>

            <!-- Objective 2 -->
            <div class="col-md-4 mb-4">
              <div class="card shadow-sm border-0">
                <img
                  src="@/assets/training.png"
                  class="card-img-top"
                  alt="Education and Training"
                />
                <div class="card-body text-center">
                  <h5 class="card-title fw-bold">Education and Training</h5>
                  <p class="card-text">
                    Develop and provide training programs for at least 10,000
                    farmers by 2025 on utilizing AI for sustainable farming
                    practices.
                  </p>
                </div>
              </div>
            </div>

            <!-- Objective 3 -->
            <div class="col-md-4 mb-4">
              <div class="card shadow-sm border-0">
                <img
                  src="@/assets/bg1.png"
                  class="card-img-top"
                  alt="Research and Development"
                />
                <div class="card-body text-center">
                  <h5 class="card-title fw-bold">Environmental Conservation</h5>
                  <p class="card-text">
                    Collaborate with agricultural and tech experts to innovate
                    new AI applications tailored to the unique challenges of
                    Kenyan agriculture.
                  </p>
                </div>
              </div>
            </div>
            <div class="container text-center">
              <a
                class="btn btn-outline-success btn-lg mt-4 border-2"
                href="/impact"
                role="button"
                >See More</a
              >
            </div>
          </div>
        </div>
      </section>

      <!-- Upcoming Events Section -->
      <section class="upcoming-events py-5">
        <div class="container">
          <h2 class="h2 text-center">Upcoming Events</h2>
          <hr class="bg-warning mx-auto" style="width: 15%" />
          <ul class="list-unstyled mt-4">
            <li v-for="event in upcomingEvents" :key="event.id" class="mb-3">
              <strong>{{ new Date(event.date).toLocaleDateString() }}:</strong>
              {{ event.title }} - {{ event.description }}
            </li>
          </ul>
        </div>
      </section>

      <!-- Footer -->
      <footer>
        <Footer />
      </footer>
    </main>
  </div>
</template>

<style scoped>
.body {
  font-family: "Helvetica Neue", Arial, sans-serif;
  color: #333;
}
.hero {
  position: relative;
  background-size: cover;
  background-position: center;
  height: 90vh;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.hero-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.4);
  z-index: 1;
}

.hero-content {
  position: relative;
  z-index: 2;
  text-align: center;
}

.hero-content p {
  font-size: 40px;
  font-weight: bold;
  margin-top: 20px;
}

.hero-content p.mt-3 {
  font-size: 1.5rem;
  font-weight: normal;
}

.introduction img {
  max-width: 100%;
  height: auto;
}

.services .card {
  height: 100%;
}

.card-img-top {
  height: 200px;
  object-fit: cover;
}

.upcoming-events ul {
  list-style: none;
  padding: 0;
}

@media (max-width: 768px) {
  .vision-mission .row {
    display: flex;
    flex-direction: column;
  }

  .vision-mission .col-md-6 {
    order: 1;
    margin-bottom: 1rem;
  }

  .hero-content p.fs-4 {
    font-size: 2rem; /* Adjust the font size for smaller screens */
  }

  .hero-content p.mt-3 {
    font-size: 1.2rem; /* Adjust the font size for the subtitle on smaller screens */
  }
}
</style>
