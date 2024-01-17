<template>
  <h1>Space</h1>
  <div class="page-highlights">
    <MuseumHighlight
      v-for="highlight in combinedHighlights"
      :highlight="highlight"
      theme="space"
      :key="highlight.id"
    >
      <template #badge>
        <img src="../assets/images/star.jpg" alt="Star" />
      </template>
      <template v-if="highlight.quiz" #link>
        <a :href="highlight.quiz" target="quiz">Take the Quiz</a>
      </template>
    </MuseumHighlight>
  </div>
</template>

<script lang="js">
import MuseumHighlight from '@/components/MuseumHighlight.vue';

export default {
  name: 'SpacePage',
  components: {
    MuseumHighlight
  },
  mixins: [],
  props: {},
  data() {
    return {
      spaceHighlights: [
        {
          date: '2020-04-20 12:20:00',
          description:
            'Asteroids are minor planets, especially of the inner Solar System. Larger asteroids have also been called planetoids.',
          id: 1,
          image: '',
          name: 'Asteroids'
        },
        {
          date: '2020-05-20 15:50:00',
          description:
            'A comet is an icy, small Solar System body that, when passing close to the Sun, warms and begins to release gases, a process called outgassing.',
          id: 9,
          image: '',
          name: 'Comets'
        },
        {
          date: '2020-05-01 9:22:00',
          description:
            'The term planet is ancient, with ties to history, astrology, science, mythology, and religion.',
          id: 7,
          image: '',
          name: 'Planets',
          news: {
            date: '2020-08-18 18:00:00',
            title: 'Attend our talk about Jupiter with Dr. Hogarth'
          },
          quiz: 'https://planetquiz.space'
        },
        {
          date: '2020-07-05 4:10:00',
          description:
            'A meteor shower is a celestial event in which a number of meteors are observed to radiate, or originate, from one point in the night sky.',
          id: 12,
          image: '',
          name: 'Meteor showers',
          news: {
            title: 'The Lyrids will peak at on April 21-22 2021, at night'
          }
        }
      ],
      spacePartners: {
        observatory: {
          createdAt: '2020-06-01 11:45:00',
          info: 'The Mauna Kea Observatories (MKO) are a number of independent astronomical research facilities and large telescope observatories that are located at the summit of Mauna Kea on the Big Island of Hawaiʻi, United States.',
          image: '',
          name: 'Mauna Kea Observatories'
        }
      }
    };
  },
  computed: {
    combinedHighlights() {
      // Convert the spacePartners object into an array of partner highlights
      const partnerHighlights = Object.keys(this.spacePartners).map((key) => {
        const partner = this.spacePartners[key];
        return {
          date: partner.createdAt,
          description: partner.info,
          id: `partner-${key}`, // Create a unique id for the partner
          image: partner.image,
          name: partner.name,
          isPartner: true
        };
      });

      // Combine the museum's spaceHighlights with the partnerHighlights
      const allHighlights = [...this.spaceHighlights, ...partnerHighlights].sort(this.sortByDate);

      return allHighlights;
    }
  },
  methods: {
    sortByDate(a, b) {
      //sort by most recent first
      return new Date(b.date) - new Date(a.date);
    }
  },
  mounted() {
    document.querySelector('html').classList.add('space-page');
  }
};
</script>
