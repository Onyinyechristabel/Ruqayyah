<template>
  <section class="hero" @mousemove="mouseMove">
    <img src="/images/ruqa.jpeg" alt="" class="hero_img" />
    <div class="hero_blur"></div>
    <div class="hero_hero">
      <div class="hero_left">
        <div class="hero_left_cta1">
          <img src="/images/greendot.png" alt="" class="hero_left_animation" />
          <h3 class="hero_left_animationp">Available For Work</h3>
        </div>
        <h2 class="hero_left_header">Ruqayyah drives growth through strategic designs</h2>
      </div>

      <div class="hero_right">
        <h2 class="hero_right_header">
          Ruqayyah is a digital product designer who blends a keen eye for detail with a business-first mindset. She collaborates
          with teams to transform ideas into intuitive, user-centered products that deliver real impact.
        </h2>

        <btn class="hero_left_animationbtn">Book a Call</btn>
      </div>
    </div>

    <div class="hero-shadow">
      <img v-for="(image, i) in images" :src="image" alt="" :data-index="i" data-status="inactive" />
    </div>
  </section>
</template>

<script setup lang="ts">
import gsap from "gsap"
import { ScrollTrigger } from "gsap/all"
let elements: HTMLElement[] = []

const images = [
  "/images/hero-work-001.avif",
  "/images/hero-work-002.avif",
  "/images/hero-work-003.avif",
  "/images/hero-work-004.avif",
  "/images/hero-work-005.avif",
  "/images/hero-work-006.avif",
  "/images/hero-work-007.avif",
  "/images/hero-work-008.avif",
]

const x = ref(0)
const y = ref(0)
const prevX = ref(0)
const prevY = ref(0)
const index = ref(0)

function mouseMove(e: MouseEvent) {
  x.value = e.clientX
  y.value = e.clientY

  if (!elements[0] || innerWidth < 768) return
  if (distanceFromPrev() > 125) {
    index.value++
    const lead = elements[index.value % images.length]
    const el2 = elements[(index.value - 1) % images.length]
    const el3 = elements[(index.value - 3) % images.length]
    const el4 = elements[(index.value - 4) % images.length]
    const trail = elements[(index.value - 5) % images.length]
    if (lead) {
      lead.dataset.status = "active"

      lead.style.left = `${x.value}px`
      lead.style.top = `${y.value}px`
    }
    if (trail) {
      // lead.style.animation = "fadeOut 0.5s 0.3s ease-in-out forwards"
      // el2.style.animation = "fadeOut 0.5s 0.2s ease-in-out forwards"
      // el3.style.animation = "fadeOut 0.5s 0.1s ease-in-out forwards"
      // el4.style.animation = "fadeOut 0.5s 0s ease-in-out forwards"
      trail.dataset.status = "inactive"
    }
    prevX.value = x.value
    prevY.value = y.value
  }
}

function distanceFromPrev() {
  return Math.hypot(x.value - prevX.value, y.value - prevY.value)
}

onMounted(() => {
  elements = gsap.utils.toArray<HTMLElement>(".hero-shadow img")
  gsap.registerPlugin(ScrollTrigger)
  ScrollTrigger.create({
    trigger: ".hero",
    start: "0% 0%",
    end: "100% 0%",
    scrub: true,
    animation: gsap.timeline().fromTo(".hero_img", { y: "0vh", scale: 1 }, { y: "12.5vh", scale: 1.25 }),
  })
})
</script>
