<li class="{active ? 'wf-active' : ''}">
    <!-- svelte-ignore a11y-label-has-associated-control -->
    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <!-- svelte-ignore a11y-no-noninteractive-element-interactions -->
    <label class="wf-menu-label"  on:click={openPOI(poi.id)}>{ poi.name }</label>
</li>

<script>
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();

    export let poi;
    export let active = false;

    function openPOI (id) {
        dispatch('clicked', id);
    }
</script>