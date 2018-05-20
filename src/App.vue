<template>
  <div id="app" class="flyout">
    <navbar dark position="top" class="default-color" name="My Bonsai" href="#/" scrolling>
      <navbar-collapse>
        <navbar-nav right>
          <navbar-item href="https://www.youtube.com/results?search_query=cuidados+bonsai" waves-fixed>Videos</navbar-item>
          <navbar-item @click="cerrarSesion()" waves-fixed>Cerrar sesion</navbar-item>
        </navbar-nav>
      </navbar-collapse>
    </navbar>
    <main :style="{marginTop: '60px'}">
      <router-view></router-view>
    </main>
    <Ftr color="default-color">
      <p class="footer-copyright mb-0 py-3 text-center">
        {{new Date().getFullYear()}} Sebastian Diez Buades
      </p>
    </Ftr>
  </div>
</template>

<script>
import Navbar from '@/components/Navbar.vue';
import NavbarItem from '@/components/NavbarItem.vue';
import NavbarNav from '@/components/NavbarNav.vue';
import NavbarCollapse from '@/components/NavbarCollapse.vue';
import Ftr from '@/components/Footer.vue';
import EdgHd from '@/components/EdgeHeader.vue';

export default {
  name: 'app',
  components: {
    Navbar,
    NavbarItem,
    NavbarNav,
    NavbarCollapse,
    Ftr,
    EdgHd,
  },
  methods: {
    cerrarSesion(){
      console.log('cerrar sesion');
      let router = this.$router;
      router.push('/login');
      localStorage.clear();
    }
  }
};

</script>

<style>
.flyout {
	display:flex;
	flex-direction: column;
	min-height:100vh;
	justify-content: space-between;
}

</style>
