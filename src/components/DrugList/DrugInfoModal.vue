<template>
  <div>
    <b-modal
      v-modal="modalState"
      id="modal-scrollable"
      scrollable
      :title="drugData.DrugName"
      ok-title="Save"
      hide-footer
      class="customClass"
    >
      <div class="ratingsContainer">
        <h5>
          Effectiveness Rating: {{ drugData.Ratings.effectiveness.val }}/5
        </h5>
        <hr />
        <div class="inputRating">
          <h6 style="display: inline; margin-right: 7px">Rate me</h6>
          <RateList
            style="display: inline"
            :drug="drugData"
            type="effectiveness"
            @set-rating="supplyRatingData"
          />
        </div>
        <hr />
        <h5>
          SideEffects Rating: {{ drugData.Ratings.sideEffectsNumber.val }}/5
        </h5>
        <hr />
        <div class="inputRating">
          <h6 style="display: inline; margin-right: 7px">Rate me</h6>
          <RateList
            style="display: inline"
            v-bind:drug="drugData"
            type="sideEffectsNumber"
            @set-rating="supplyRatingData"
          />
          <hr />
        </div>
      </div>
      <div class="comments">
        <h3>Comments</h3>
        <hr />
        <h6 v-for="comment in drugData.Comments" style="height: 50px">
          {{ comment }}
          <hr />
        </h6>
        <b-form-input
          v-show="!isCart"
          v-model="comment"
          style="width: 40%; border: 1px solid black; display: inline"
          placeholder="Add Comment"
        ></b-form-input>
        <b-button
          v-show="!isCart"
          style="display: inline; margin-left: 4px; margin-top: -4px"
          v-on:click="supplyComment"
          >Save Comment</b-button
        >
      </div>
    </b-modal>
  </div>
</template>
<script>
import RateList from "../atoms/RateList.vue";
export default {
  name: "DrugInfoModal",
  props: {
    modalState: Boolean,
    drugData: Object,
  },
  data: function () {
    return {
      comment: "",
    };
  },
  computed: {
    isCart: function () {
      return !!this.drugData.cartNumber;
    },
  },
  methods: {
    supplyComment: function (e) {
      this.$emit("handle-comment", {
        drugInfo: this.drugData,
        input: this.comment,
      });
    },
    supplyRatingData: function (data) {
      this.$emit("set-ratinginapp", data);
    },
  },
  components: {
    RateList,
  },
};
</script>

<style>
.comments {
  height: 100px;
  display: block;
  padding: 1%;
}
,
.modal-dialog {
  height: 90%; /* = 90% of the .modal-backdrop block = %90 of the screen */
}
.modal-content {
  height: 100%; /* = 100% of the .modal-dialog block */
}
</style>
