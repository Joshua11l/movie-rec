<script lang="ts">
  import { onMount } from 'svelte';
  import axios from 'axios';

  let movies: Movie[] = [];
  let expandedMovieId: number | null = null;
  let searchQuery = '';

  interface Movie {
    title: string;
    poster_path: string;
    overview: string;
    id: number; // Assuming each movie has a unique ID
    // Include any other details you want to display
  }

  const apiKey = '63e52f72e01d5a43109095c16f7b0132';
  const discoverApiUrl = 'https://api.themoviedb.org/3/discover/movie';
  const searchApiUrl = 'https://api.themoviedb.org/3/search/movie';

  async function fetchMovies(url: string, params: object) {
    try {
      const response = await axios.get(url, { params });
      movies = response.data.results;
    } catch (error) {
      console.error('Error fetching data:', error);
    }
  }

  // Fetch movies when the component is mounted
  onMount(() => {
    fetchMovies(discoverApiUrl, { api_key: apiKey });
  });

  // Function to handle search
  function handleSearch() {
    const params = { api_key: apiKey, query: searchQuery };
    if (searchQuery.trim()) {
      fetchMovies(searchApiUrl, params);
    } else {
      fetchMovies(discoverApiUrl, { api_key: apiKey });
    }
  }

  function toggleMovieDetails(movieId: number) {
    expandedMovieId = expandedMovieId === movieId ? null : movieId;
  }
</script>

<svelte:head>
  <title>MovieRecs</title>
</svelte:head>

<h1>Movie Hub</h1>

<div class="search-container">
  <input type="text" bind:value={searchQuery} placeholder="Search movies...">
  <!-- Add class "search-btn" to this button -->
  <button class="search-btn" on:click={handleSearch}>Search</button>
</div>


<article class="movies">
  {#each movies as movie}
    <button class="movie" on:click={() => toggleMovieDetails(movie.id)} class:expanded={expandedMovieId === movie.id}>
      <img src={`https://image.tmdb.org/t/p/w500${movie.poster_path}`} alt="{movie.title}" />
      <h2>{movie.title}</h2>
      {#if expandedMovieId === movie.id}
        <p class="details">{movie.overview}</p>
      {/if}
    </button>
  {/each}
</article>

<style>
  :global(body) {
    background-image: url('/neon.jpeg');
    background-size: cover;
    background-position: center;
    background-attachment: fixed; /* This makes the background image fixed during scrolling */
    color: #ffffff; /* Ensures text color is white for better visibility */
    
  }

  h1 {
    font-weight: bold;
    color: white;
  }

  .movies {
    display: flex;
    flex-wrap: wrap;
    gap: 20px;
    justify-content: center; /* Centers the movie cards */
    padding: 20px; /* Adds some padding around the entire movies container */
  }
  .movie {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width: 300px; /* Adjust width as needed */
    background-color: rgba(0, 0, 0, 0.7); /* Semi-transparent black background */
    border-radius: 10px; /* Rounded corners for the card */
    overflow: hidden; /* Ensures nothing overflows the rounded corners */
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.5); /* Adds a subtle shadow to each card */
    transition: transform 0.2s; /* Smooth transition for hover effect */
    cursor: pointer; /* Indicates the card is clickable */
    transition: all 0.3s ease;
    position: relative; /* Needed for z-index to work */
    z-index: 1;
    transition: transform 0.3s ease, z-index 0s linear;
    
  }

  
  .movie img {
    width: 100%; /* Ensures image covers the width of the card */
    height: auto; /* Maintains aspect ratio */
    border-bottom: 1px solid #ffffff; /* Adds a subtle border between the image and the text */
  }
  .movie h2 {
    text-align: center; 
    margin: 10px; /* Adds margin around the title */
    font-size: large;
    /* Adjust title size */
  }
  .movie p {
    margin: 0 10px 10px; /* Adds margin around the overview */
    font-size: 14px; /* Adjust text size for readability */
    text-overflow: ellipsis; /* Ensures text does not overflow */
    overflow: hidden; /* Hide overflow */
    display: -webkit-box;
    -webkit-line-clamp: 3; /* Limits to 3 lines of text */
    -webkit-box-orient: vertical;
    transition: max-height 0.5s ease-out;
    max-height: 0;
    overflow: hidden;
  }

  .movie.expanded {
  position: fixed; /* Position fixed to make it overlay */
  top: 0;
  left: 0;
  width: 100vw; /* Full width */
  height: 100vh; /* Full height */
  z-index: 100; /* Ensure it's above all other content */
  display: flex; /* Use flexbox for layout */
  flex-direction: column; /* Stack elements vertically */
  justify-content: center; /* Center content vertically */
  align-items: center; /* Center content horizontally */
  background-color: rgba(0, 0, 0, 0.9); /* Semi-transparent background */
  transform: none; /* Override any scale transforms */
  border-radius: 0; /* No border radius */
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

.movie.expanded h2{
  font-size: larger;
  font-weight: bold;
}

.movie.expanded p {
  max-height: none; /* Show full description */
  -webkit-line-clamp: none; /* Display all content */
  margin: 10px 0; /* Add some margin around the paragraph */
  overflow: visible; 
  font-size: large;/* Show overflow */
}


.movie:hover {
    transform: scale(1.05); /* Slightly enlarges the card on hover */
  }
  .search-container {
    display: flex;
    justify-content: center; /* Center the search container */
    padding: 20px; /* Adds padding for spacing */
    gap: 10px;
  }
  input[type="text"] {
    width: 40%; /* Adjust input width */
    padding: 10px; /* Adds padding for better usability */
    border-radius: 20px; /* Rounded corners for the input */
    border: 1px solid #ffffff; /* White border */
    background-color: rgba(255, 255, 255, 0.2); /* Semi-transparent background */
    color: #ffffff; /* White text */
  }
  button {
    padding: 5px 10px; /* Adds padding for better usability */
    border-radius: 20px; /* Rounded corners for the button */
    border: none; /* Removes default border */
    cursor: pointer; /* Changes cursor to pointer */
    background-color: rgb(201, 51, 255); /* Initial button background color */
    color: #ffffff; /* White text */
  }
  .search-btn {
  margin-top: 8px;
  height: 50px;
  padding: 10px 20px;
  background-color: #e736db; /* Example: bright blue background */
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-weight: bold;
}

.search-btn:hover {
  background-color: #a647b8; /* Darker blue on hover */
}




</style>

