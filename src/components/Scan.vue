<template>
  <div class="scan">
    <b-tab title="Scan" active>
      <template #title>
        <b-icon icon="upc-scan" font-scale="1"></b-icon>
      </template>
      {{ error }}
      <p class="error">{{ error }}</p>

      <p class="decode-result">
        Last result: <b>{{ result }}</b>
      </p>

      <qrcode-stream @decode="onDecode" @init="onInit" />
    </b-tab>
  </div>
</template>

<script>
import { BIcon, BIconUpcScan } from "bootstrap-vue";
import { QrcodeStream } from "vue-qrcode-reader";

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
    };
  },
  mounted() {
    this.$nextTick(this.setUpScanner);
  },
  methods: {
    onDecode(result) {
      this.result = result;
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