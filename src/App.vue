<template>
  <div id="app">
    <Header title="MyDrugs" />
    <SearchBar @supply-search="supplySearchInput" />
    <DrugInfoModal
      v-bind:modalState="infoModalState"
      v-bind:drugData="infoDrugData"
      v-if="infoModalState"
    />
    <b-container fluid>
      <b-row>
        <b-col xl="6" l="6" md="12" sm="12" class="inventoryContainer">
          <DrugList
            v-bind:drugs="filteredInventory"
            v-bind:isInventoryCard="true"
            title="Drug Inventory"
            v-bind:searchText="searchTextSupply"
            @handle-drugtransaction="handleDrugTransaction"
            @handle-druginfostate="displayDrugInfo"
          />
        </b-col>
        <b-col xl="6" l="6" md="12" sm="12" class="inventoryContainer">
          <DrugList
            v-bind:drugs="UserCartItems"
            v-bind:isInventoryCard="false"
            title="My Cart"
            @handle-drugtransaction="handleDrugTransaction"
            @handle-druginfostate="displayDrugInfo"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import SearchBar from "./components/SearchBar.vue";
import DrugInventoryWH from "./mockData.js";
import DrugList from "./components/DrugList/DrugList.vue";
import DrugInfoModal from "./components/DrugList/DrugInfoModal.vue";
import { v4 as uuidv4 } from "uuid";
export default {
  name: "App",
  components: {
    Header,
    SearchBar,
    DrugList,
    DrugInfoModal,
  },
  data: function () {
    return {
      DrugInventoryItems: DrugInventoryWH.DrugInventory,
      UserCartItems: [],
      searchTextSupply: "",
      infoModalState: false,
      infoDrugData: {},
    };
  },
  computed: {
    filteredInventory() {
      return this.DrugInventoryItems.filter((drug) => {
        return drug.DrugName.toLowerCase().includes(
          this.searchTextSupply.toLowerCase()
        );
      });
    },
    ComputedUserCartItems() {},
  },
  methods: {
    supplySearchInput: function ($event) {
      this.searchTextSupply = $event;
    },
    handleDrugTransaction: function (data) {
      if (data.action === "add") {
        this.UserCartItems = [
          ...this.UserCartItems,
          { ...data, cartNumber: uuidv4() },
        ];
      } else {
        this.UserCartItems = this.UserCartItems.filter(
          (drug) =>
            drug.DrugId !== data.DrugId || drug.cartNumber !== data.cartNumber
        );
      }
    },
    displayDrugInfo: function (data) {
      console.log(data);
      this.infoModalState = true;
      this.infoDrugData = data;
    },
  },
};
</script>

<style>
#app {
  text-align: center;
}
.inventoryContainer {
  padding: 3%;
  height: 400px;
}
</style>
