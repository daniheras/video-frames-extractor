<template>
  <section class="" style="display: flex; flex-direction: column; height: 100%;">
    <LayoutHeader></LayoutHeader>
    <FrameExtractor></FrameExtractor>
  </section>
</template>

<script>
import LayoutHeader from './layout/header/LayoutHeader'
import FrameExtractor from './FrameExtractor'

  export default {
    name: 'landing-page',
    components: {
      LayoutHeader,
      FrameExtractor
    },
  }
</script>