<template>
  <div>
    <b-card>
      <b-card-text style="display: inline; font-size: 20px"
        ><strong>{{ title }}</strong>
        <b-dropdown id="dropdown-1" text="Sort By" style="float: right">
          <b-dropdown-item>Effectiveness</b-dropdown-item>
          <b-dropdown-item>SIde Effects Rating</b-dropdown-item>
        </b-dropdown>
      </b-card-text>
    </b-card>
    <h5 v-for="drug in drugs" :key="drug.DrugId">
      <DrugCard
        @supply-drugaction="supplyDrugTransactionToParent"
        @supply-infostate="supplyDrugModalUp"
        v-bind:drugInfo="drug"
        v-bind:isInventoryCard="isInventoryCard"
      />
    </h5>
  </div>
</template>

<script>
import DrugCard from "./DrugCard.vue";
export default {
  name: "DrugList",
  props: {
    drugs: Array,
    isInventoryCard: Boolean,
    title: String,
  },
  data: function () {
    return {
      search: "",
    };
  },
  methods: {
    supplyDrugTransactionToParent: function (data) {
      this.$emit("handle-drugtransaction", data);
    },
    supplyDrugModalUp: function (data) {
      this.$emit("handle-druginfostate", data);
    },
  },
  components: {
    DrugCard,
  },
};
</script>
