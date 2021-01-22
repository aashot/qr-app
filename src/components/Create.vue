<template>
    <div class="create">
        <b-tab title="Create">
          <template #title>
            <b-icon icon="plus-circle" font-scale="1"></b-icon>
          </template>
        </b-tab>
    </div>
</template>

<script>
import { BIcon, BIconPlusCircle } from 'bootstrap-vue'

export default {
    name: "Create",
    components: {
        BIcon,
        BIconPlusCircle,
    }
}
</script>

<style>

</style>