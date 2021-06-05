<template>
<div class="content">
	<h3 class="subtitle is-3">Farmer : {{ farmer.login }}</h3>
	<table>
		<thead>
			<tr>
				<th>ID</th>
				<th>Name</th>
				<th>Level</th>
				<th>Ai</th>
				<th>Select</th>
			</tr>
		</thead>
		<tbody>
			<tr v-for="leek of Object.values(farmer.leeks)" :key="leek.id">
				<td>{{ leek.id }}</td>
				<td>{{ leek.name }}</td>
				<td>{{ leek.level }}</td>
				<td>{{ leek.ai }}</td>
				<td>
					<b-checkbox v-model="selectedLeeks"
					:native-value="leek.id" @input="selectedUpdate($event)">
				</b-checkbox>
			</td>
		</tr>
	</tbody>
</table>
</div>
</template>


<script>
export default {
	data: () => {
    return {
    	selectedLeeks: []
    };
  },
	props: ['farmer'],
	methods: {
		selectedUpdate($event) {
			this.$emit('childToParent', this.selectedLeeks);
		},
	},
}
</script>