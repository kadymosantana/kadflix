<template>
  <router-link
    tag="li"
    class="movie"
    v-if="movie.vote_average > 0"
    :to="{ name: 'Movie', params: { movie_id: movie.id } }"
  >
    <img :src="api_images + 'w342/' + movie.poster_path" :alt="movie.title" />

    <div class="movie_details">
      <span>{{ movie.release_date | dateFilter("short") }}</span>
      <h3>{{ movie.title }}</h3>
    </div>

    <span class="vote_average">{{
      movie.vote_average.toFixed(1) | voteAverageFilter
    }}</span>

    <FavoriteButton :movie="movie" />
  </router-link>
</template>

<script>
import FavoriteButton from "@/components/FavoriteButton.vue";
import { dateFilter, voteAverageFilter } from "@/filters.js";
import averageColor from "@/utils/averageColor.js";

export default {
  name: "MoviePreview",

  filters: {
    dateFilter,
    voteAverageFilter,
  },

  components: {
    FavoriteButton,
  },

  props: ["movie"],

  data() {
    return {
      api_images: this.$store.state.api_images,
    };
  },

  mounted() {
    this.averageColor();
  },

  methods: {
    averageColor,
  },
};
</script>

<style scoped lang="scss">
@import "../scss/abstracts/_mixins.scss";

.movie {
  position: relative;
  min-width: 150px;
  max-width: 150px;
  cursor: pointer;
  transition: 0.2s;
  &:hover {
    transform: scale(1.1);

    .favorite {
      opacity: initial;
      pointer-events: initial;
    }

    .movie_details {
      display: grid;
    }
  }

  img {
    max-width: 100%;
    border-radius: 12px;
    box-shadow: 0px 6px 4px #18181d;
  }

  .movie_details {
    display: none;
    grid-template-columns: 1fr auto;
    gap: 5px 10px;
    position: absolute;
    right: 0;
    left: 0;
    bottom: 0;
    backdrop-filter: blur(30px);
    border-bottom-right-radius: 12px;
    border-bottom-left-radius: 12px;
    padding: 10px;
    overflow: hidden;
    animation: show_details 0.3s forwards;

    span {
      font-size: 14px;
      color: #eee;
    }

    span:nth-child(2) {
      grid-column: 2;
      justify-self: end;
      letter-spacing: -0.5px;
    }

    h3 {
      grid-column: 1 / -1;
      font-size: 16px;
      font-weight: 600;
      letter-spacing: -0.5px;
      color: #fff;
    }
  }
  .vote_average {
    display: flex;
    justify-content: center;
    position: absolute;
    top: 10px;
    right: 10px;
    width: 30px;
    color: #fff;
    border-radius: 6px;
    padding: 3px 0;
  }

  .favorite {
    opacity: 0;
    pointer-events: none;
    position: absolute;
    top: 10px;
    left: 10px;
    @include responsive("small") {
      opacity: initial;
      pointer-events: initial;
    }
  }
}

@keyframes show_details {
  from {
    transform: translateY(15px);
  }
  to {
    transform: translateY(0);
  }
}

.very_good {
  background: linear-gradient(#76d176, #55a855);
}

.good {
  background: linear-gradient(#aed176, #96a855);
}

.medium {
  background: linear-gradient(#fcc040, #c07505);
}

.bad {
  background: linear-gradient(#d66969, #c00505);
}
</style>
