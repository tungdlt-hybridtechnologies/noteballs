<template>
  <nav class="navbar is-success" aria-label="main navigation" role="navigation">
    <div class="container is-max-desktop px-2">
      <div class="navbar-brand">
        <div class="navbar-item is-size-4 is-family-monospace">Noteballs</div>
        <a
          @click.prevent="showMobileNav = !showMobileNav"
          ref="navBarBurger"
          role="button"
          class="navbar-burger"
          :class="{ 'is-active': showMobileNav }"
          aria-label="menu"
          aria-expanded="false"
          data-target="navbarBasicExample">
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
        </a>
      </div>

      <div
        id="navbarBasicExample"
        ref="navBarMenuRef"
        :class="{ 'is-active': showMobileNav }"
        class="navbar-menu">
        <div class="navbar-start">
          <button
            v-if="storeAuth.user.uid"
            @click="handleLogoutClick"
            class="button is-small is-info mt-3 ml-3">
            Log Out {{ storeAuth.user.email }}
          </button>
        </div>
        <div class="navbar-end" v-if="storeAuth.user.uid">
          <RouterLink
            @click="showMobileNav = false"
            to="/"
            class="navbar-item"
            active-class="is-active">
            Notes
          </RouterLink>
          <RouterLink
            @click="showMobileNav = false"
            to="/stats"
            class="navbar-item"
            active-class="is-active">
            Stats
          </RouterLink>
        </div>
      </div>
    </div>
  </nav>
</template>

<script setup>
  import { ref } from 'vue';
  import { onClickOutside } from '@vueuse/core';
  import { useStoreAuth } from '@/stores/storeAuth';

  const showMobileNav = ref(false);
  const navBarMenuRef = ref(null);
  const navBarBurger = ref(null);

  onClickOutside(navBarMenuRef, () => (showMobileNav.value = false), {
    ignore: [navBarBurger],
  });

  //Logout
  const storeAuth = useStoreAuth();

  function handleLogoutClick() {
    showMobileNav.value = false;
    storeAuth.logoutUser();
  }
</script>

<style>
  @media (max-width: 1023px) {
    .navbar-menu {
      position: absolute;
      left: 0;
      width: 100%;
    }
  }
</style>
