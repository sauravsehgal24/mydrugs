<template>
  <div>
    <span v-for="star in stars" v-on:click="setRating(star)"
      ><Star :styleObj="star.style" :starObj="star"
    /></span>
  </div>
</template>

<script>
import Star from "../electrons/CustomStarIcon.vue";
export default {
  name: "RateList",
  data: function () {
    return {
      stars: [
        {
          style: {
            fill: "gray",
          },
          id: 1,
        },
        {
          style: {
            fill: "gray",
          },
          id: 2,
        },
        {
          style: {
            fill: "gray",
          },
          id: 3,
        },
        {
          style: {
            fill: "gray",
          },
          id: 4,
        },
        {
          style: {
            fill: "gray",
          },
          id: 5,
        },
      ],
    };
  },
  components: {
    Star,
  },
  props: {
    drug: Object,
    type: String,
  },
  computed: {
    starDt: function () {
      const {
        Ratings: { sideEffectsNumber, effectiveness },
      } = this.drug;
      console.log(sideEffectsNumber);
      this.stars = this.stars.map((star, index) => {
        if (this.type == "effectiveness") {
          if (index + 1 <= Math.round(effectiveness.val)) {
            star.style.fill = "Yellow";
          }
        } else {
          if (index + 1 <= Math.round(sideEffectsNumber.val)) {
            star.style.fill = "Yellow";
          }
        }
      });
    },
  },
  mounted() {
    const {
      Ratings: { sideEffectsNumber, effectiveness },
    } = this.drug;
    console.log(sideEffectsNumber);
    this.stars = this.stars.map((star, index) => {
      if (this.type == "effectiveness") {
        if (index + 1 <= effectiveness.val) {
          star.style.fill = "Yellow";
        }
      } else {
        if (index + 1 <= sideEffectsNumber.val) {
          star.style.fill = "Yellow";
        }
      }
    });
  },
  methods: {
    setRating: function (starData) {
      this.$emit("set-rating", {
        drug: this.drug,
        starDt: starData,
        type: this.type,
      });
    },
  },
};
</script>
