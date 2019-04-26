<template>
  <div id="app">
    <div class="alert alert-success alert-dismissible fade show my-alert" v-if="updateExists" role="alert">
      New version available! Click to update
      <button class="btn btn-outline-success float-right"  @click="refreshApp">
        Refresh Page
      </button>
      <button type="button" class="close" data-dismiss="alert" aria-label="Close">
        <span aria-hidden="true">&times;</span>
      </button>
    </div>
    <nav class="bg-light navbar" v-if="installBtn === 'block'">
      <div 
        class="btn btn-outline-success float-right"
        @click="installer()"
        :style="{'display': installBtn}"
        >Add to Home Screen
      </div>
      <button type="button" class="close" aria-label="Close" @click="close">
        <span aria-hidden="true">&times;</span>
      </button>
    </nav>
     <nav-bar @search="searchPage" />
    <router-view /> 
  </div>
</template>

<script>
import NavBar from "@/components/NavBar.vue";
export default {
  data() {
    return {
      installBtn: "none",
      installer: undefined,
      refreshing: false,
      registration: null,
      updateExists: false,
    }
  },
  components: {
    NavBar
  },
  methods: {
    searchPage(text) {
      this.$router.push({ name: 'search', query: { plan: text } })
    },
    close() {
      this.installBtn = "none"
    },
    showRefreshUI (e) {
      // console.log(e.detail, "refreshing UI")
      this.registration = e.detail;
      this.updateExists = true;
    },
    refreshApp () {
      this.updateExists = false;
      if (!this.registration || !this.registration.waiting) { return; }
      this.registration.waiting.postMessage('skipWaiting');
    },
  }, 
  created() {
    let installPrompt;

    window.addEventListener("beforeinstallprompt", e => {
      e.preventDefault();

      installPrompt = e;
      this.installBtn = "block"
    })

    this.installer = () => {
      this.installBtn = "none";
      installPrompt.prompt();

      installPrompt.userChoice
        .then( result => {
          if(result.outcome === "accepted") {
            console.log("User accepted")
          } else {
            console.log("User denied");
          }
          installPrompt = null
        })
    }

    document.addEventListener(
      'swUpdated', this.showRefreshUI, { once: true }
    );

    navigator.serviceWorker.addEventListener(
      'controllerchange', () => {
        if (this.refreshing) return;
        this.refreshing = true;
        window.location.reload();
      }
    );
  }
}

</script>



<style scoped>

.my-alert {
  display: flex;
    align-items: center;
    justify-content: space-between;
    position: fixed;
    bottom: 0;
    z-index: 1000;
    margin-bottom: 0;
    width: 100%;
    border-radius: 0;
}
  
</style>
