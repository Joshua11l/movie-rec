<script>
    import { fade, slide } from 'svelte/transition';
    import 'bootstrap/dist/css/bootstrap.min.css';
  </script>
  
  <svelte:head>
    <title>About - Movie App</title>
  </svelte:head>
  
  <section class="container mt-5">
    <div class="text-center">
      <h1 class="mb-3" in:fade={{ duration: 1000 }}>About Movie Recommendation App</h1>
      <p class="lead" in:slide={{ duration: 1000 }}>
        This app was developed as a project for the FullyHacks Hackathon hosted by Cal State University Fullerton. 
        Embracing the hackathon's neon theme, this was my first time using Svelte, creating a visually striking 
        and user-friendly movie recommendation tool.
      </p>
    </div>
  
    <div class="row row-cols-1 row-cols-md-2 g-4">
      <div class="col d-flex" in:slide={{ duration: 1000, delay: 500 }}>
        <div class="card bg-dark text-white h-100">
          <div class="card-body d-flex flex-column">
            <h2 class="card-title">Features</h2>
            <ul class="text-white">
              <li>Search functionality on the home page to find movies by title.</li>
              <li>Genre-based discovery through dynamic tabs, allowing users to explore movies by their favorite genres.</li>
            </ul>
            <p class="mt-auto text-white">Explore a wide range of movies with ease.</p>
          </div>
        </div>
      </div>
      <div class="col d-flex" in:slide={{ duration: 1000, delay: 1000 }}>
        <div class="card bg-dark text-white h-100">
          <div class="card-body d-flex flex-column">
            <h2 class="card-title">Technologies Used</h2>
            <ul class="text-white">
              <li><strong>Frontend:</strong> HTML, JavaScript, TypeScript</li>
              <li><strong>Framework:</strong> SvelteKit for server-side rendering and client-side navigation</li>
              <li><strong>API:</strong> The Movie Database (TMDB) for fetching movie data</li>
              <li><strong>Styling:</strong> Bootstrap for responsive design and attractive UI components, inspired by neon aesthetics</li>
            </ul>
            <p class="mt-auto text-white">Built using modern technologies for a seamless experience.</p>
          </div>
        </div>
      </div>
    </div>
  
    <div class="text-center" in:fade={{ duration: 1500, delay: 1500 }}>
      <p class="mt-4">Embarking on this project for the FullyHacks Hackathon was an extremly fun journey. Not only was it my first time using Svelte, but it also challenged me to incorporate a neon theme into the app's design. The goal was to make movie discovery as easy and enjoyable as possible. By leveraging the powerful TMDB API, I was able to provide up-to-date information and a wide variety of movies to explore. Whether you're in the mood for a classic drama or the latest big hit, my app is here to guide you to your next watch.</p>
    </div>

    <div class="text-center mt-4">
        <p class="text-white">Made by Joshua Lopez</p>
    </div>
  </section>
  
  <style>
    .container {
      max-width: 800px;
    }

    h1, h2 {
      color: rgb(255, 31, 248); 
      font-weight: bold;/* Adjusted for better visibility on dark backgrounds, reflecting the neon theme */
    }
    .card {
      background-color: #212529; /* Bootstrap dark background, ideal for neon-themed designs */
    }
    .card ul, .card ul li, .card-body, .text-center p {
      color: white; /* Ensures all text, except headers, is white */
    }
    .card ul {
      list-style-type: none;
      padding: 0;
    }
    .card ul li::before {
      
      font-weight: bold;
      display: inline-block; 
      width: 1em;
      margin-left: -1em;
    }
    .text-center {
      padding-bottom: 3rem;
      color: white;
    }
  </style>
  