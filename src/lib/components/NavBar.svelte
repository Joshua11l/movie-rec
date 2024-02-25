<script>
  import { goto } from '$app/navigation';
  import { page } from '$app/stores';
  import movieLogo from './movie.jpeg';

  let searchTerm = '';
  
  function performSearch() {
    console.log('Searching for:', searchTerm);
    // Navigate to a search results page, passing the search term as a query parameter
    goto(`/search?query=${encodeURIComponent(searchTerm)}`);
  }

  const navItems = [
    { label: "Genres", href: "/genres" },
    { label: "About", href: "/about"}
  ];
</script>

<nav class="navbar">
  <div class="logo">
    <a href="/" class:active="{$page.url.pathname === '/'}">
      <img src={movieLogo} alt="movie logo">
    </a>
  </div>

  <div class="nav-items">
    <ul class="menu-items">
      {#each navItems as item}
        <li>
          <a href={item.href} class:active={$page.url.pathname === item.href}>
            {item.label}
          </a>
        </li>
      {/each}
    </ul>

   
  </div>
</nav>


<style>
  .navbar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    
  }

  .logo img {
    width: 80px;
    height: auto;
    border-radius: 5px;
    margin-right: 10px;
  }

  .nav-items {
    display: flex;
    align-items: center;
    
    
  }

  .menu-items {
    list-style: none;
    padding: 0;
    margin: 0;
    display: flex;
    font-weight: bold;
    font-size: x-large;
   
  }

  .menu-items li a {
    color: white;
    padding: 25px 15px;
    text-decoration-color: rgb(255, 31, 248);
  }

  .menu-items li a.active {

    font-weight: bold;
    text-decoration-color: rgb(255, 31, 248);
  }

 

</style>
