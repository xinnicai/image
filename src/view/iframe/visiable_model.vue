<template>
    <div class="visiable_model">
        <card>
            <iframe style="border: 0px;width: 100%;padding-top: 50px;" id="visiable_model_iframe" :src=src  height="600px"></iframe>
        </card>
    </div>
</template>
<script>
export default {
  data () {
    return {
      src: ''

    }
  },
  mounted: function () {
    this.src = sessionStorage.getItem('visualmodel')
  }
}
</script>
