<script lang="ts">
	import LightweightResourcePicker from './LightweightResourcePicker.svelte'

	export let format: string
	export let value: any
	export let disablePortal = false
	export let disabled: boolean = false
	function isString(value: any) {
		return typeof value === 'string' || value instanceof String
	}

	let path: string = ''

	function resourceToValue() {
		if (path) {
			value = `$res:${path}`
		} else {
			value = undefined
		}
	}

	function isResource() {
		return isString(value) && value.length >= '$res:'.length
	}

	function valueToPath() {
		if (isResource()) {
			path = value.substr('$res:'.length)
		}
	}

	$: value && valueToPath()
</script>

<div class="flex flex-row w-full flex-wrap gap-x-2 gap-y-0.5">
	<LightweightResourcePicker
		{disabled}
		{disablePortal}
		on:change={(e) => {
			path = e.detail
			resourceToValue()
		}}
		bind:value={path}
		resourceType={format.split('-').length > 1 ? format.substring('resource-'.length) : undefined}
	/>
</div>
