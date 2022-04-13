<template>
  <div>
    <h1>Events Listing</h1>
    <EventCard v-for="event in events" :key="event.id" :event="event"/>
    <div class="pagination">
      <router-link 
        id="page-prev"
        v-if="page != 1" 
        :to="{name: 'event-list', query: {page: page - 1}}" 
        rel="prev"
      >
        &#60; Previous
        </router-link>
        
      <router-link 
        id="page-next"
        v-if="hasNextPage" 
        :to="{name: 'event-list', query: {page: page + 1}}" 
        rel="next"
      >
       Next &#62;
      </router-link>
    </div>
  </div>
</template>

<script>
import EventCard from '@/components/EventCard.vue'
import { mapState } from 'vuex'

export default {
  components: {
    EventCard
  },
  created() {
    this.perPage = 3 // Setting perPage here and not in data means it won't be reactive.
    // We don't need it to be reactive, and this way our component has access to it.
    this.$store.dispatch('fetchEvents', {
      perPage: this.perPage,
      page: this.page
    })
  },
  computed: {
    page() {
      return parseInt(this.$route.query.page) || 1
    },
    hasNextPage() {
      var totalPags = Math.ceil(this.eventsTotal / this.perPage)

      return this.page < totalPags
    },
    ...mapState(['events', 'eventsTotal'])
  }
}
</script>
  
<style scoped>
.pagination {
  display: flex;
  width: 460px;
}

.pagination a {
  flex: 1;
  text-decoration: none;
  color: #2c3e50;
}

#page-prev {
  text-align: left;
}

#page-next {
  text-align: right;
}
</style>
