<script lang="ts">
    export let header: string;
    export let id = "";
</script>

<div
    class="bg-white dark:bg-black p-2 rounded-lg border-t-4 border-gray-300 dark:border-gray-700 flex-1 font-sans shadow-md min-w-0"
    {id}
>
    <h2
        class="border-b-2 border-gray-200 dark:border-gray-800 h-auto pb-2 mb-2"
    >
        {header}
    </h2>
    <slot />
</div>
