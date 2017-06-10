<template>
  <div class="col-sm-6 col-sm-offset-3">
    <button class="btn btn-warning" v-on:click="getQuote()">Get a Quote</button>
    <div class="quote-area" v-if="quote">
      <h2>
        <blockquote><b>{{ quote.title }}</b> - {{ quote.content }}</blockquote>
      </h2>
    </div>
  </div>
</template>

<script>
  import auth from '../auth'

  export default {

    data() {
      return {
        quote: ''
      }
    },

    methods: {
      getQuote() {
        this.$http
          .get('https://quotesondesign.com/wp-json/posts?filter[orderby]=rand&filter[posts_per_page]=1&callback=', (data) => {
            this.quote = data[0];
          }, {
            headers: auth.getAuthHeader()
          })
          .error((err) => console.log(err))
      }
    },

    route: {
      canActivate() {
        console.log("auth...")
        return auth.user.authenticated
      }
    }

  }
</script>