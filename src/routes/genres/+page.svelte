<script lang="ts">
    import { onMount } from 'svelte';
    import axios from 'axios';
  
    interface Genre {
      id: number;
      name: string;
    }
  
    interface Movie {
      id: number;
      title: string;
      poster_path: string;
      overview: string;
    }
  
    let genres: Genre[] = [];
    let movies: Movie[] = [];
    let expandedMovieId: number | null = null;
    const apiKey = '63e52f72e01d5a43109095c16f7b0132';
    const genresApiUrl = 'https://api.themoviedb.org/3/genre/movie/list';
    const moviesApiUrl = 'https://api.themoviedb.org/3/discover/movie';
  
    onMount(async () => {
      await fetchGenres();
    });
  
    async function fetchGenres() {
      try {
        const response = await axios.get(genresApiUrl, { params: { api_key: apiKey } });
        genres = response.data.genres;
      } catch (error) {
        console.error('Error fetching genres:', error);
      }
    }
  
    async function fetchMoviesByGenre(genreId: number) {
      try {
        const response = await axios.get(moviesApiUrl, {
          params: { api_key: apiKey, with_genres: genreId.toString() }
        });
        movies = response.data.results;
        expandedMovieId = null; // Reset expanded movie
      } catch (error) {
        console.error('Error fetching movies by genre:', error);
      }
    }
  
    function toggleMovieDetails(movieId: number) {
      expandedMovieId = expandedMovieId === movieId ? null : movieId;
    }
  </script>
  
  <svelte:head>
    <title>Movie Genres</title>
  </svelte:head>
  
  <section class="genres">
    <h1>Movie Genres</h1>
    <div class="genre-list">
      {#each genres as genre}
        <button class="genre-btn" on:click={() => fetchMoviesByGenre(genre.id)}>{genre.name}</button>
      {/each}
    </div>
  </section>
  
  <article class="movies">
    {#each movies as movie (movie.id)}
      <div
        class={`movie ${expandedMovieId === movie.id ? 'expanded' : ''}`}
        role="button"
        tabindex="0"
        on:click={() => toggleMovieDetails(movie.id)}
        on:keydown={(event) => event.key === 'Enter' && toggleMovieDetails(movie.id)}
        >
        <img src={`https://image.tmdb.org/t/p/w500${movie.poster_path}`} alt={movie.title} />
        <h2>{movie.title}</h2>
        {#if expandedMovieId === movie.id}
          <div class="movie-details">
            <p>{movie.overview}</p>
          </div>
        {/if}
      </div>
    {/each}
  </article>
  
  
  
  
  <style>
    :global(body) {
    background-color: transparent;
    background-image: url('/neon.jpeg'); /* background image */
    background-size: cover; /* Covers entire page */
    background-position: center; 
    background-attachment: fixed; /* Keep the background fixed during scroll */
    min-height: 100vh; /* Ensure body takes at least the viewport height */
    margin: 0; 
    padding: 0; 
    color: #ffffff;
  }
    .genres, .movies {
      text-align: center;
      margin: 20px auto;
      min-height: 0;
     
      
    }

    h1 {
        color: white;
        font-weight: bold;
        padding-bottom: 50px;
    }
  
    .genre-list, .movies {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 10px;
      
    }
  
    .genre-btn {
      padding: 10px 20px;
      background-color: #e736db;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      transition: background-color 0.2s;
    }
  
    .genre-btn:hover {
      background-color: #0056b3;
    }
  
    .movie {
    display: flex;
    flex-direction: column;
    width: 200px;
    margin: 10px;
    text-align: center;
    background-color: rgba(0, 0, 0, 0.7); 
    border-radius: 10px; /* Rounded corners for the card */
    padding: 10px; /* Padding inside the card */
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.5); /* Shadow for depth */
    transition: transform 0.3s ease;
    cursor: pointer; 
  }
  
    .movie img {
      width: 100%;
      border-radius: 5px;
      border-bottom: 1px solid #ffffff;
    }
  
    h2 {
      margin-top: 10px;
      font-size: larger;
      
    }

.movie.expanded {
  position: fixed; /* Position fixed to make it overlay */
  top: 0;
  left: 0;
  width: 100vw; /* Full width */
  height: 100vh; /* Full height */
  z-index: 100; /* Ensure it's above all other content */
  display: flex; 
  flex-direction: column; /* Stack elements vertically */
  justify-content: center; /* Center content vertically */
  align-items: center; /* Center content horizontally */
  background-color: rgba(0, 0, 0, 0.9); 
  transform: none; /* Override any scale transforms */
  border-radius: 0; 
}

.movie.expanded img {
  max-width: 400px; /* Limit image size */
  max-height: 400px; /* Limit image height */
  margin-bottom: 20px; /* Space between image and text */
}

.movie.expanded h2, .movie.expanded p {
  color: white; /* Ensure text is readable on the dark background */
  text-align: center; /* Center the text */
  max-width: 80%;
   /* Limit width of text for readability */
}

.movie.expanded p {
  color: white; /* Ensure text is readable on the dark background */
  text-align: center; /* Center the text horizontally */
  max-width: 80%; /* Limit width of text for readability */
  margin: auto; /* Center the text vertically */
  margin-top: 8vh; /* Adjust as needed to vertically center */
  transform: translateY(-50%); /* Correct the vertical centering */
}

.movie:hover {
    transform: scale(1.05); /* Slightly enlarges the card on hover */
  }
  </style>
  