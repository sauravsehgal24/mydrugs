<template>
  <b-card class="drugCard">
    <b-card-text style="display: inline">
      {{ drugInfo.DrugName }}
    </b-card-text>

    <BIconPlusCircle
      style="float: right"
      v-if="isInventoryCard"
      v-on:click="handleDrugTransaction({ ...drugInfo, action: 'add' })"
    />
    <BIconDashCircle
      style="float: right"
      v-else
      v-on:click="handleDrugTransaction({ ...drugInfo, action: 'remove' })"
    />
    <BIconInfoCircle
      style="float: right; margin-right: 10px"
      v-on:click="openDrugInfoModal(drugInfo)"
      v-b-modal.modal-scrollable
    />
  </b-card>
</template>

<script>
import {
  BIconPlusCircle,
  BIconDashCircle,
  BIconInfoCircle,
} from "bootstrap-vue";

export default {
  name: "DrugCard",
  props: {
    drugInfo: Object,
    isInventoryCard: Boolean,
  },
  methods: {
    handleDrugTransaction: function (drugData) {
      this.$emit("supply-drugaction", drugData);
    },
    openDrugInfoModal: function (drugInfo) {
      this.$emit("supply-infostate", drugInfo);
    },
  },
  components: {
    BIconPlusCircle,
    BIconDashCircle,
    BIconInfoCircle,
  },
};
</script>

<style>
.drugCard {
  box-shadow: 5px 8px 15px 1px rgba(0, 0, 0, 0.28);
}
</style>
