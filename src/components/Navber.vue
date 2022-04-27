<template>
  <nav id="navbar">
    <ul id="navmenu">
      <li>
        1
        <div class="submenu">submenu</div>
      </li>

      <li>2</li>
      <li>3</li>
      <li>4</li>
    </ul>
  </nav>
</template>

<style>
#navber {
  box-sizing: border-box;
  background-color: aliceblue;
  display: flex;
  width: 100%;
  min-height: 3rem;
}

#navmenu {
  display: flex;
}

#navmenu li {
  display: block;
  width: 5rem;
  height: 2rem;
  box-sizing: border-box;
  border: 1px solid black;
  overflow-y: visible;
}

#navmenu li:hover > .submenu {
  display: block;
  opacity: 1;
  height: 2rem;
}

.submenu {
  display: none;
  opacity: 0;
  height: 15rem;
  background-color: aqua;
  transition: all 1s;
}
</style>
