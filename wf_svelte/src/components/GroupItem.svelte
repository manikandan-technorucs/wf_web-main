<li class="wf-list-item {active ? 'wf-active' : ''}">
    <!-- svelte-ignore a11y-label-has-associated-control -->
    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <!-- svelte-ignore a11y-no-noninteractive-element-interactions -->
    <label class="wf-menu-label" on:click={() => openGroup(group.id)}>
        <span class="wf-menu-label-text">{ group.name }</span>
        <svg
            class="wf-chevron {active ? 'wf-chevron-open' : ''}"
            viewBox="0 0 24 24"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
        >
            <path
                d="M6 9L12 15L18 9"
                stroke="currentColor"
                stroke-width="1.8"
                stroke-linecap="round"
                stroke-linejoin="round"
            />
        </svg>
    </label>
    {#if active}
        <div transition:slide={{ duration: 300, easing: quintOut }}>
            <ul class="wf-group-items">
                {#each group.children as subgroup}
                    <GroupItem group={subgroup} active={subgroup.id == currentGroup} on:clicked={() => openSubGroup(subgroup.id)} on:poiclicked={openPOI}></GroupItem>
                {/each}
            </ul>
            <ul class="wf-poi-list">
                {#each group.pois as poi}
                    <POIItem poi={poi} on:clicked={openPOI} active={poi.id == currentPOI}></POIItem>
                {/each}
            </ul>
        </div>
    {/if}
</li>

<script>
    import { createEventDispatcher } from 'svelte';
    import { slide } from 'svelte/transition';
    import { quintOut } from 'svelte/easing';
    const dispatch = createEventDispatcher();

    import GroupItem from './GroupItem.svelte';
    import POIItem from './POIItem.svelte';

    export let group;
    export let active = false;

    let currentGroup = -1;
    let currentPOI = -1;

    $: if (!active) {
        currentGroup = -1;
        currentPOI = -1;
    }

    function openGroup(id) {
        dispatch('clicked', id);
    }

    function openSubGroup(id) {
        if (id !== currentGroup) {
            currentGroup = id;
        } else {
            currentGroup = -1;
        }
    }

    function openPOI(e) {
        currentPOI = e.detail;
        dispatch('poiclicked', e.detail);
    }
</script>

<style>
    /* Label: flex row, space between text and chevron */
    .wf-menu-label {
        display: flex;
        align-items: center;
        justify-content: space-between;
        cursor: pointer;
        width: 100%;
        padding: 11px 12px 11px 14px;
        box-sizing: border-box;
        gap: 8px;
        color: inherit;
        /* No border-left here — active state handled via box-shadow in app.css */
    }

    .wf-menu-label-text {
        flex: 1;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
        font-size: 13px;
    }

    /* Chevron */
    .wf-chevron {
        width: 14px;
        height: 14px;
        flex-shrink: 0;
        opacity: 0.4;
        transition: transform 0.35s cubic-bezier(0.16, 1, 0.3, 1), opacity 0.2s ease, color 0.2s ease;
    }

    .wf-menu-label:hover .wf-chevron {
        opacity: 0.85;
        color: #BCA474;
    }

    .wf-chevron-open {
        transform: rotate(180deg);
        opacity: 1;
        color: #BCA474;
    }

    .wf-group-items {
        padding-left: 0;
    }
</style>