<template>
<header>


  <div class="bg-black text-gray-400 w-full flex justify-between text-center items-center text-sm sticky top-0 z-10">
    <button class="hover:text-white ml-6">
      <a href="/discover">
        <img class="w-48 hidden sm:block" src="/packs/media/images/logo_14-929fb1cd43f62a5bdca9540d9017df3b.png">
        <img class="w-8 block sm:hidden border border-gray-800 rounded-full bg-gray-200" src="/packs/media/images/logo_10-2079fa85cdb67f6aa527d43bef9680a9.png">
      </a>
    </button>
    <button class="hover:text-white hidden sm:block md:px-8 py-2"><router-link :to="{ name: 'discover'}">Home</router-link></button>
    <button class="hover:text-white md:px-8 py-2"><router-link :to="{ name: 'stream'}" class="hidden sm:block">Stream</router-link>
    <router-link :to="{ name: 'stream'}" class="block sm:hidden"><i class="fas fa-bolt"></i></router-link></button>
    <button class="hover:text-white md:px-8 py-2"><router-link :to="{ name: 'library'}" class="hidden sm:block">Library</router-link></button>
    <router-link :to="{ name: 'library'}" class="block sm:hidden"><i class="fas fa-music"></i></router-link></button>

    <form id="search_bar" action="/search/result"  data-remote="true" class="w-3/12">
      <input class="px-2 m-2 bg-gray-200 rounded w-full" placeholder="Search" type="text" name="search" id="search" v-model="search" @keyup.enter="searchRails">
      <!-- <input type="submit" name="commit" value="Save " data-disable-with="Save "> -->
      <!-- <div class="search-result flex flex-col absolute mx-auto"></div> -->
    </form>

    <div class="hover:text-white md:px-8 py-2">
      <router-link :to="{ name: 'upload' }" class="hidden sm:block">Upload</router-link>
      <router-link :to="{ name: 'upload' }" class="block sm:hidden"><i class="fas fa-cloud-upload-alt"></i></router-link>
    </div>
    <div class="hover:text-white flex items-center md:px-8 py-2">
      <a href="/rooms" class="flex">
        <span class="sm:hidden">
          <i class="fas fa-microphone-alt"></i>
        </span>
        <div class="hidden sm:block">On Air</div>
      </a>
    </div>

    <div class="dropdown inline-block relative ml-4 mr-6">

    <button class="text-gray-300 py-2 rounded inline-flex item-center md:px-8">
      <span class="mr-1 hidden sm:block"><a :href="`/users/${this.user_id}`"><img class="w-6 h-6 object-cover inline mr-1"  :src="this.user_avatar"></i>{{ this.user_name }}</a></span>
      <span class="mr-1 block sm:hidden"><a :href="`/users/${this.user_id}`"><img class="w-6 h-6 object-cover inline mr-1"  :src="this.user_avatar"></a></span>
    </button>

    <ul class="dropdown-menu absolute right-0 hidden text-gray-700 text-left">
      <li class="bg-gray-200 py-2 px-4 block whitespace-no-wrap rounded-t"><a :href="`/users/${this.user_id}`">Profile</a></li>
      <!-- <li class="bg-gray-200 py-2 px-4 block whitespace-no-wrap"><a href="/you/likes">Likes</a>    </li> -->
      <!-- <li class="bg-gray-200 py-2 px-4 block whitespace-no-wrap"><a href="/you/sets">Playlists</a></li> -->
      <!-- <li class="bg-gray-200 py-2 px-4 block whitespace-no-wrap"><a href="/users/1/songs">Tracks</a></li> -->
      <li class="bg-gray-200 py-2 px-4 block whitespace-no-wrap"><a :href="`/users/${this.user_id}/edit`">Edit</a></li>
      <li class="bg-gray-200 py-2 px-4 block whitespace-no-wrap rounded-b"><a href="#" @click="signout">Sign out</a></li>
    </ul>
    </div>

  </div>


</header>
</template>

<script>
import {
  mapState,
  mapGetters,
  mapActions
} from 'vuex'
import Api from '../../api/api'

export default {
  data(){
    return{
      search: this.search
    }
  },
  computed: {
    ...mapGetters({
      user_id: 'user/user_id',
      user_name: 'user/user_name',
      user_avatar: 'user/user_avatar'
    }),
  },
    ...mapActions('user', ['loadUser']),
  beforeCreate(){
    this.$store.dispatch('user/loadUser')
  },
  methods:{
    signout(){
      Api().delete('/users/sign_out').then(response => {
        window.location.href = '/';
      })
    },
    searchRails(){
       var searchItem = this.search
         console.log(searchItem)
        window.location.href = `/search/result?search=${searchItem}`;
      }
    }
  }

</script>


<style media="screen">
.dropdown:hover .dropdown-menu {
display: block;
}
</style>
