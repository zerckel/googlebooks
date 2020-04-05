<template>
    <div>
        <ion-header>
            <ion-toolbar color="primary">
                <ion-title>Google Books</ion-title>
                <ion-button slot="end" v-on:click="closeModal">
                    Close
                </ion-button>
            </ion-toolbar>

        </ion-header>
        <ion-content class="ion-padding">
            <ion-toolbar>
                <ion-title>
                    {{ book.volumeInfo.title }}
                </ion-title>
            </ion-toolbar>
            <ion-text>
                <b>Authors</b>: <span v-for="author in book.volumeInfo.authors">{{ author }}</span>
            </ion-text>
            <br/>
            <ion-text>
                <b>Date de Publication</b>: {{ book.volumeInfo.publishedDate }}
            </ion-text>
        </ion-content>
    </div>
</template>

<script>
  export default {
    name: 'Modal',
    data() {
      return {}
    },
    methods: {
      closeModal: function () {
        return this.$ionic.modalController.dismiss()
      }
    },
    created() {
      console.log(this.book)
    }
  }
</script>

<style scoped>

</style>