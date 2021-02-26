<template>
  <div class="scan">
    <b-tab title="Scan" active>
      <template #title>
        <b-icon icon="upc-scan" font-scale="1"></b-icon>
      </template>
      {{ error }}
      <p class="error">{{ error }}</p>

      <qrcode-stream
        v-if="!destroyed"
        @decode="onDecode" 
        @init="onInit" 
      />

      <b-modal 
        centered
        id="result-modal"
        size="xl"
        :ok-only="true"
        @ok="reInit"
      >
        <p class="decode-result">
          <b-link 
            v-if="isResultLink"
            :href="result"
          >
            {{ result }}
          </b-link>
          <b v-else>{{ result }}</b>
        </p>
      </b-modal>
    </b-tab>
  </div>
</template>

<script>
import { BIcon, BIconUpcScan } from "bootstrap-vue";
import { QrcodeStream } from "vue-qrcode-reader";

const isLinkValid = (str) => {
  const pattern = new RegExp('^(https?:\\/\\/)?'+
    '((([a-z\\d]([a-z\\d-]*[a-z\\d])*)\\.)+[a-z]{2,}|'+
    '((\\d{1,3}\\.){3}\\d{1,3}))'+
    '(\\:\\d+)?(\\/[-a-z\\d%_.~+]*)*'+
    '(\\?[;&a-z\\d%_.~+=-]*)?'+
    '(\\#[-a-z\\d_]*)?$','i')
  return !!pattern.test(str)
}

export default {
  name: "Scan",
  components: {
    BIcon,
    BIconUpcScan,
    QrcodeStream,
  },
  data() {
    return {
      result: "",
      error: "",
      isResultLink: false
    };
  },
  mounted() {
    this.$nextTick(this.setUpScanner);
  },
  methods: {
    onDecode(result) {
      this.result = result;
      this.$bvModal.show('result-modal')
      this.isResultLink = isLinkValid(result)
    },
    async reInit() {
      this.destroyed = true
      await this.$nextTick()
      this.destroyed = false
      this.result = 1
    },
    async onInit(promise) {
      try {
        await promise;
      } catch (error) {
          this.error = error
        if (error.name === "NotAllowedError") {
          this.error = "ERROR: you need to grant camera access permisson";
        } else if (error.name === "NotFoundError") {
          this.error = "ERROR: no camera on this device";
        } else if (error.name === "NotSupportedError") {
          this.error = "ERROR: secure context required (HTTPS, localhost)";
        } else if (error.name === "NotReadableError") {
          this.error = "ERROR: is the camera already in use?";
        } else if (error.name === "OverconstrainedError") {
          this.error = "ERROR: installed cameras are not suitable";
        } else if (error.name === "StreamApiNotSupportedError") {
          this.error = "ERROR: Stream API is not supported in this browser";
        }
      }
    },
  },
};
</script>

<style lang="scss">
.error {
  font-weight: bold;
  color: red;
}
</style>