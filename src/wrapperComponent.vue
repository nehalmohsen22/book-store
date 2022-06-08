<template>
  <div class="container my-2">
    <div class="row">
      <HeaderComponent />
    </div>
    <div class="row">
      <div class="col-md-4">
        <LinksComponent />
      </div>
      <div class="col-md-8">
        <ContentComponent />
      </div>
    </div>
    <div class="row">
      <p class="text-center">
        <FooterComponent />
      </p>
    </div>
  </div>
</template>

<script>
  import HeaderComponent from './components/HeaderComponent.vue'
  import LinksComponent from './components/LinksComponent.vue'
  import ContentComponent from './components/ContentComponent.vue'
  import FooterComponent from './components/FooterComponent.vue'
  export default {
    name: 'wrapperComponent',
    data() {

    },
    components: {
      HeaderComponent,
      LinksComponent,
      ContentComponent,
      FooterComponent
    }
  }
</script>