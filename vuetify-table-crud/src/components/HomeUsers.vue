<template>
	<v-container>	
		<v-layout text-center wrap>
			<v-flex xs12>
				<br/>				
				<v-img
					:src="require('../assets/user-photo.png')"
					class="my-3"
					contain
					height="350"
				></v-img>
			</v-flex>

			<v-flex mb-4>
				<br/>
				<br/>
				<h1 class="display-2 font-weight-bold mb-3">
					Welcome to App !
				</h1>
			</v-flex>
		</v-layout>
	</v-container>
</template>

<script>
export default {
	data: () => ({
		//
	})
}
</script>

