<script>
	import * as THREE from 'three'
	import * as SC from 'svelte-cubed'

	let showMessage = false

	let spin = 0

	SC.onFrame(() => {
		spin += 0.01
	})
</script>

<button on:click="{() => (showMessage = !showMessage)}" class="relative w-full mt-8 h-80">
	<SC.Canvas background="{new THREE.Color('#faf7f5')}">
		<SC.Mesh
			geometry="{new THREE.BoxGeometry()}"
			scale="{[1, 1, 1]}"
			material="{new THREE.MeshMatcapMaterial({})}"
			rotation="{[spin, spin, spin]}"
		/>
		<SC.PerspectiveCamera position="{[1, 1, 3]}" />
	</SC.Canvas>
</button>

{#if showMessage}
	<p>Now sign up!</p>
{/if}
