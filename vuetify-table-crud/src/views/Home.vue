<template>
	<div class="home">
		<HomeUsers msg="Welcome to Your Vue.js/Vuetify App" />
	</div>
</template>

<script>
// @ is an alias to /src
import HomeUsers from '@/components/HomeUsers.vue'

export default {
	name: 'home',
	components: {
		HomeUsers
	}
}
</script>
