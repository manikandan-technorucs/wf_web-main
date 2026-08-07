<ul id="wf-menu-items" class="wf-list wf-accordion">
    {#each groups as group}
        <GroupItem group={group} active={group.id == currentGroup} on:clicked={() => openGroup(group.id)} on:poiclicked={openPOI}></GroupItem>
    {/each}
</ul>

<script>
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();

    import GroupItem from './GroupItem.svelte';
    export let groups;

    let currentGroup = -1;

    function openGroup (id) {
        console.log('openGroup', id, currentGroup)
        if (id != currentGroup) {
            currentGroup = id;
        }
        else {
            currentGroup = -1;
        }
    }

    function openPOI(e) {
        dispatch('poiclicked', e.detail);
    }
</script>

<style>


    /* Track */
    #wf-menu-items::-webkit-scrollbar-track {
        background: #E0E0DF;
    }

    /* Handle */
    #wf-menu-items::-webkit-scrollbar-thumb {
        background: var(--wf-active-color);
    }

</style>