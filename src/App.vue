<template>
  <div id="app">
    <Header title="MyDrugs" />
    <SearchBar @supply-search="supplySearchInput" />
    <DrugInfoModal
      v-bind:modalState="infoModalState"
      v-bind:drugData="infoDrugData"
      v-if="infoModalState"
      @set-ratinginapp="setRating"
      @handle-comment="addComment"
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
            @handle-sort="handleSort"
          />
        </b-col>
        <b-col xl="6" l="6" md="12" sm="12" class="inventoryContainer">
          <DrugList
            v-bind:drugs="UserCartItems"
            v-bind:isInventoryCard="false"
            title="My Cart"
            @handle-drugtransaction="handleDrugTransaction"
            @handle-druginfostate="displayDrugInfo"
            @handle-sort="handleSort"
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
    handleSort: function (type) {
      this.DrugInventoryItems = this.DrugInventoryItems.sort(
        (a, b) => b.Ratings[type].val - a.Ratings[type].val
      );
    },
    addComment: function (data) {
      const { drugInfo, input } = data;
      if (input.trim() === "") {
        return;
      }
      // update drug inventory
      let _drug = this.DrugInventoryItems.filter(
        (d) => d.DrugId == drugInfo.DrugId
      )[0];
      let userDrugs;
      if (this.UserCartItems.length !== 0) {
        userDrugs = this.UserCartItems.filter(
          (d) => d.DrugId == drugInfo.DrugId
        );
      }
      _drug.Comments = [..._drug.Comments, input];
      for (var i = 0; i < this.DrugInventoryItems.length; i++) {
        if (this.DrugInventoryItems[i].DrugId === _drug.DrugId) {
          this.DrugInventoryItems[i] = _drug;
          break;
        }
      }
      if (userDrugs.length !== 0) {
        // update userInventory
        for (var d = 0; d < userDrugs.length; d++) {
          userDrugs[d].Comments = [...userDrugs[d].Comments, input];
        }
        const cartNums = userDrugs.map((d) => d.cartNumber);
        for (var i = 0; i < this.UserCartItems.length; i++) {
          if (cartNums.includes(this.UserCartItems[i].cartNumber)) {
            this.UserCartItems[i] = userDrugs.filter(
              (d) => d.cartNumber === this.UserCartItems[i].cartNumber
            )[0];
          }
        }
      }
    },
    setRating: function (data) {
      const { starDt, type, drug } = data;
      //find that drug
      let _drug = this.DrugInventoryItems.filter(
        (d) => d.DrugId == drug.DrugId
      )[0];
      const prevAvg = _drug.Ratings[type].val;
      const prevCount = _drug.Ratings[type].totalRatings;
      const newCount = prevCount + 1;
      const prevSum = prevAvg * prevCount;
      let newAvg = (prevSum + starDt.id) / newCount;
      newAvg = newAvg.toFixed(1);
      _drug.Ratings[type].val = newAvg;
      _drug.Ratings[type].totalRatings = newCount;
      for (var i = 0; i < this.DrugInventoryItems.lengthl; i++) {
        if (this.DrugInventoryItems[i].DrugId === _drug.DrugId) {
          this.DrugInventoryItems[i] = _drug;
        }
      }
    },
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
  height: 600px;
  overflow-y: scroll;
}
::-webkit-scrollbar {
  width: 0; /* Remove scrollbar space */
  background: transparent; /* Optional: just make scrollbar invisible */
}
</style>
