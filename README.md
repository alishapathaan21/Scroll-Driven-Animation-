<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Scroll-Driven Hero Section</title>

  <!-- External Libraries for Smooth Animations -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/gsap.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/ScrollTrigger.min.js"></script>

  <style>
    /* Reset & Base Styling */
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }

    body {
      background-color: #0b0f19;
      color: #ffffff;
      overflow-x: hidden;
    }

    /* 1. HERO SECTION (Above the fold) */
    .hero {
      position: relative;
      width: 100%;
      height: 100vh;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      align-items: center;
      padding: 40px 20px;
      overflow: hidden;
    }

    /* Headline Styling */
    .headline {
      font-size: 2.8rem;
      font-weight: 900;
      letter-spacing: 0.3em; /* Letter-spaced headline requirement */
      text-transform: uppercase;
      background: linear-gradient(90deg, #60a5fa, #a855f7);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      text-align: center;
      opacity: 0; /* Animated in via GSAP */
      transform: translateY(30px);
    }

    /* Central Visual Element Wrapper */
    .car-container {
      position: relative;
      width: 100%;
      max-width: 600px;
      display: flex;
      justify-content: center;
      align-items: center;
      margin: auto 0;
      will-change: transform; /* Hardware acceleration for performant rendering */
    }

    .car-image {
      width: 100%;
      height: auto;
      object-fit: contain;
      filter: drop-shadow(0px 15px 25px rgba(96, 165, 250, 0.3));
    }

    /* Impact Metrics / Statistics */
    .metrics-container {
      display: flex;
      gap: 20px;
      justify-content: center;
      width: 100%;
      max-width: 900px;
      flex-wrap: wrap;
    }

    .metric-card {
      background: rgba(17, 24, 39, 0.8);
      border: 1px solid rgba(255, 255, 255, 0.1);
      border-radius: 12px;
      padding: 15px 25px;
      text-align: center;
      flex: 1;
      min-width: 180px;
      backdrop-filter: blur(8px);
      opacity: 0; /* Animated in via GSAP */
      transform: translateY(30px);
    }

    .metric-card h3 {
      font-size: 2rem;
      color: #60a5fa;
      margin-bottom: 5px;
    }

    .metric-card p {
      font-size: 0.85rem;
      color: #9ca3af;
    }

    /* Extra Section to allow Page Scrolling */
    .content-section {
      height: 100vh;
      background-color: #111827;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      text-align: center;
      padding: 20px;
    }

    .content-section h2 {
      font-size: 2rem;
      color: #e5e7eb;
      margin-bottom: 10px;
    }

    .content-section p {
      color: #9ca3af;
      max-width: 500px;
    }
  </style>
</head>
<body>

  <!-- HERO SECTION -->
  <section class="hero" id="hero">
    
    <!-- Headline -->
    <h1 class="headline" id="headline">WELCOMEITZFIZZ</h1>

    <!-- Central Interactive Visual Element -->
    <div class="car-container" id="car-wrapper">
      <img 
        src="https://images.unsplash.com/photo-1617814076367-b759c7d7e738?q=80&w=1000&auto=format&fit=crop" 
        alt="Car Visual" 
        class="car-image"
      />
    </div>

    <!-- Impact Metrics / Statistics -->
    <div class="metrics-container" id="metrics">
      <div class="metric-card">
        <h3>99.9%</h3>
        <p>System Reliability</p>
      </div>
      <div class="metric-card">
        <h3>2.5x</h3>
        <p>Speed Optimization</p>
      </div>
      <div class="metric-card">
        <h3>10M+</h3>
        <p>Active Users</p>
      </div>
    </div>

  </section>

  <!-- NEXT SECTION (FOR SCROLL SPACE) -->
  <section class="content-section">
    <h2>Scroll Driven Demo Section</h2>
    <p>Scroll back up to watch the hero element perform smoothly without performance lag.</p>
  </section>

  <!-- ANIMATION LOGIC (GSAP + SCROLLTRIGGER) -->
  <script>
    // Register GSAP ScrollTrigger plugin
    gsap.registerPlugin(ScrollTrigger);

    window.addEventListener("DOMContentLoaded", () => {
      
      // 1. INITIAL LOAD ANIMATION (Fade + Staggered Reveal)
      const introTl = gsap.timeline({ defaults: { ease: "power3.out" } });

      introTl
        .to("#headline", {
          opacity: 1,
          y: 0,
          duration: 1.2
        })
        .from("#car-wrapper", {
          scale: 0.8,
          opacity: 0,
          duration: 1.2
        }, "-=0.8")
        .to(".metric-card", {
          opacity: 1,
          y: 0,
          stagger: 0.2, // Delays each metric card load
          duration: 0.8
        }, "-=0.8");

      // 2. SCROLL-BASED ANIMATION (Tied to Scroll Progress)
      gsap.to("#car-wrapper", {
        scale: 1.35,            // Performant transform property
        y: 160,                // Translates smoothly down on scroll
        rotate: -3,            // Subtle smooth tilt
        ease: "none",          // Keeps motion continuous with scroll position
        scrollTrigger: {
          trigger: "#hero",
          start: "top top",     // Starts when top of hero touches viewport top
          end: "bottom top",    // Ends when bottom of hero reaches viewport top
          scrub: 1              // Smooth 1-second interpolation delay for fluidity
        }
      });

      // Fade out non-central elements on scroll
      gsap.to("#headline, #metrics", {
        opacity: 0,
        y: -40,
        scrollTrigger: {
          trigger: "#hero",
          start: "top top",
          end: "40% top",
          scrub: true
        }
      });

    });
  </script>
</body>
</html>