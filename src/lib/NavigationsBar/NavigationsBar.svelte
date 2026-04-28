<script >
  let isOpen = $state(false);
  let {onbukaHalamanKeranjang, onbukaHalamanHome} = $props();

  // @ts-ignore
  function show(event) {
    isOpen = !isOpen;
  }
</script>

<nav class="navbar navbar-expand-lg bg-body-tertiary">
  <div class="container-fluid">
    <a class="navbar-brand" href="/">Navbar</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation"
        onclick={show}>
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse {isOpen ? 'show' : ''}" id="navbarSupportedContent">
      <ul class="navbar-nav me-auto mb-2 mb-lg-0">
        <li class="nav-item mx-2 my-2">
          <button class="btn btn-outline-success" type="button" onclick={onbukaHalamanHome}>Home</button>
        </li>
        <li class="nav-item mx-2 my-2">
          <button class="btn btn-outline-success" type="button" onclick={onbukaHalamanKeranjang}>Keranjang</button>
        </li>
      </ul>
      <form class="d-flex" role="search">
        <!-- <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search"/>
        <button class="btn btn-outline-success" type="submit">Search</button> -->
      </form>
    </div>
  </div>
</nav>