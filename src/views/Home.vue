<template>
    <div class="ion-page">
        <ion-header style="background-color: white">
            <ion-toolbar color="primary">
                <ion-title>Google Books</ion-title>
            </ion-toolbar>
            <ion-searchbar show-cancel-button="focus" v-on:keyup.enter="search($event.target.value)"></ion-searchbar>
        </ion-header>
        <ion-content fullscreen>
            <ion-list inset="true" lines="full" v-if="results">
                <ion-items class="items" button v-on:click="openModal(book)" detail="true"
                           v-for="book in results">
                    <ion-thumbnail>
                        <ion-img v-if="book.volumeInfo.imageLinks"
                                 :src="book.volumeInfo.imageLinks.smallThumbnail"></ion-img>
                        <ion-img v-else
                                 src="https://upload.wikimedia.org/wikipedia/fr/b/b9/Logo_google_play_livre_material.png"></ion-img>
                    </ion-thumbnail>

                    <ion-label class="label">
                        <h2>
                            <b>Title:</b><br/> {{ book.volumeInfo.title }}
                        </h2>
                        <h3 v-if="book.volumeInfo.authors">
                            <b>Authors:</b>
                            <span v-for="authors in book.volumeInfo.authors">  {{ authors}} </span>
                        </h3>
                        <h3 v-if="book.volumeInfo.categories">
                            <b>Categories:</b>
                            <span v-for="category in book.volumeInfo.categories">{{ category }}</span>
                        </h3>
                    </ion-label>
                </ion-items>
            </ion-list>

            <ion-card v-else>
                <ion-card-header>
                    <img src="https://p.kindpng.com/picc/s/268-2686968_data-tracking-true-data-tracking-category-carrier-google.png">
                </ion-card-header>
                <ion-card-content>
                    Here will display the result of your search
                </ion-card-content>
            </ion-card>
        </ion-content>
    </div>
</template>

<script>

  import Modal from '../components/modal'

  export default {
    name: "home",
    data() {
      return {
        research: '',
        results: ''
      }
    },
    methods: {
      search: function (search) {
        this.research = search
      },
      openModal(book) {
        return this.$ionic.modalController
                .create({
                  component: Modal,
                  componentProps: {
                    data: {
                      book

                    }
                  },
                })
                .then(m => m.present())
      }
    },
    watch: {
      research: function () {
        fetch('http://www.googleapis.com/books/v1/volumes?q=' + this.research + '&key=AIzaSyAZozge-2cGsgQeKFPlhv6CV2AFEWfjA9w')
                .then((res) => res.json())
                .then((data) => {
                  this.results = data.items
                  localStorage.setItem('search', JSON.stringify(data.items))
                })
      }
    },
    created(){
      if (localStorage.getItem('search')){
        this.results = JSON.parse(localStorage.getItem('search'))
      }
    }

  }
</script>
<style scoped lang="scss">
    .items {
        display: flex;
        padding: 20px 0;
        border-bottom: 1px solid lightgrey;

        .label {
            text-align: center;
            width: 100%;
        }
    }
</style>