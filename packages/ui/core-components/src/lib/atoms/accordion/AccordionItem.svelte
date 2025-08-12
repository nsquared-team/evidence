<script context="module">
	export const evidenceInclude = true;
</script>

<script>
	import * as BaseAccordion from '../shadcn/accordion';
	import InlineError from '../inputs/InlineError.svelte';
	import Info from '../../unsorted/ui/Info.svelte';
	/** @type {string | undefined} */
	import { toBoolean } from '$lib/utils.js';
	export let title = undefined;
	export let compact = false;
	export let description = undefined;
	export let alwaysRender = false;
	import checkRequiredProps from '../inputs/checkRequiredProps.js';
	$: compact = toBoolean(compact);
	$: alwaysRender = toBoolean(alwaysRender);

	let className = undefined;
	export { className as class };

	/** @type {[string] | []} */
	const errors = [];

	try {
		if (!$$slots.default) {
			throw new Error(
				'<AccordionItem> requires content to be provided e.g <AccordionItem>Content</AccordionItem>'
			);
		}

		checkRequiredProps({ title });
	} catch (err) {
		errors.push(err);
	}
</script>

{#if errors.length > 0}
	<InlineError inputType="AccordionItem" height="52" width="100%" error={errors} />
{:else}
	{#key title}
		<BaseAccordion.Item value={title} class={className}>
			<BaseAccordion.Trigger class={compact ? 'py-0' : ''}>
				<span>
					<slot name="title">
						{title}
						{#if description}
							<Info {description} />
						{/if}
					</slot>
				</span>
			</BaseAccordion.Trigger>
			{#if alwaysRender}
				<div class="overflow-hidden text-sm accordion-render-content">
					<div class="pb-4 pt-0">
						<slot />
					</div>
				</div>
			{:else}
				<BaseAccordion.Content>
					<slot />
				</BaseAccordion.Content>
			{/if}
		</BaseAccordion.Item>
	{/key}
{/if}

<style>
	.accordion-render-content {
		height: 0;
		overflow: hidden;
		opacity: 0;
		transition: all 0.2s ease;
	}
	:global([data-state="open"] .accordion-render-content) {
		height: auto;
		opacity: 1;
	}
</style>