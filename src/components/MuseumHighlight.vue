<template>
  <div :class="['museum-highlight', 'is-' + theme, highlight.isPartner ? 'is-partner' : '']">
    <div :class="['museum-highlight__image', loading ? 'is-loading' : '']">
      <i class="museum-highlight__image-loading fa-solid fa-compass fa-spin fa-2xl"></i>
      <template v-if="typeof compImage === 'string'">
        <img :src="compImage" :alt="`Image of ${highlight.name}`" />
        <button
          type="button"
          class="museum-highlight__refresh-image"
          @click="fetchImage()"
          :disabled="loading"
          aria-label="Refresh Image"
        >
          <i class="fa-solid fa-arrows-rotate"></i>
        </button>
      </template>
    </div>
    <div class="museum-highlight__badge">
      <slot name="badge"></slot>
    </div>

    <div class="museum-highlight__content">
      <div class="museum-highlight__name">{{ highlight.name }}</div>
      <div class="museum-highlight__date">{{ formatDate(highlight.date) }}</div>
      <div class="museum-highlight__description">
        <p>{{ highlight.description }}</p>
      </div>
      <div v-if="$slots.extra" class="museum-highlight__extra">
        <slot name="extra"></slot>
      </div>
      <div v-if="highlight.news" class="museum-highlight__news">
        <div class="museum-highlight__news__header">
          <div>News</div>
          <div v-if="highlight.news.date" class="museum-highlight__news__date">
            {{ formatDate(highlight.news.date) }}
          </div>
        </div>
        <div class="museum-highlight__news__title">
          <p>{{ highlight.news.title }}</p>
        </div>
      </div>
    </div>

    <div v-if="$slots.link" class="museum-highlight__link">
      <slot name="link"></slot>
    </div>
  </div>
</template>

<script lang="js">
export default {
  name: 'MuseumHighlight',

  mixins: [],
  props: {
    highlight: {
      type: Object,
      required: true
    },
    theme: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      image: null,
      loading: true
    };
  },
  computed: {
    compImage() {
      return this.image;
    }
  },
  methods: {
    async fetchImage() {
      try {
        this.loading = true;
        const searchQuery = this.highlight.name.replace(/\s/g, '%20');
        const url = `https://source.unsplash.com/featured/320x180/?${this.theme}%20${searchQuery}`;

        const response = await fetch(url);
        this.image = response.url;
        this.loading = false;
      } catch (error) {
        console.error(error);
        this.loading = false;
      }
    },
    formatDate(date) {
      date = new Date(date);
      return date.toLocaleDateString();
    }
  },
  mounted() {
    this.image = this.highlight.image || this.fetchImage();
  }
};
</script>
