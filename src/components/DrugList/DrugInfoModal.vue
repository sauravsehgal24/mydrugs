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
      <div class="comments">
        <h3>Comments</h3>
        <hr />
        <h6 v-for="comment in drugData.Comments" style="height: 50px">
          {{ comment }}
          <hr />
        </h6>
        <b-form-input
          style="width: 40%; border: 1px solid black"
        ></b-form-input>
      </div>
    </b-modal>
  </div>
</template>
<script>
export default {
  name: "DrugInfoModal",
  props: {
    modalState: Boolean,
    drugData: Object,
  },
};
</script>

<style>
.comments {
  height: 100px;
  display: block;
  padding: 1%;
}
</style>
