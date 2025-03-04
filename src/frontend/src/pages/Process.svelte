<script lang="ts">
    import Fa from "svelte-fa";
    import {
        faSort,
        faSortUp,
        faSortDown,
        faBan,
        faSkull,
        faPause,
        faPlay,
    } from "@fortawesome/free-solid-svg-icons";
    import prettyBytes from "pretty-bytes";

    interface processData {
        processes?: processes[];
    }

    interface processes {
        pid: number;
        name: string;
        cpu: number;
        ram: number;
        status: string;
    }

    export let socketData: processData;
    export let socketSend: (cmd: string, args: string[]) => void;

    let reverse = false;
    let pidSort = true;
    let nameSort = false;
    let cpuSort = false;
    let ramSort = false;
    let statusSort = false;
    let pidIcon = faSortUp;
    let nameIcon = faSort;
    let cpuIcon = faSort;
    let ramIcon = faSort;
    let statusIcon = faSort;

    $: cpuSort && socketData.processes && sortCPU(reverse);
    $: pidSort && socketData.processes && sortPid(reverse);
    $: nameSort && socketData.processes && sortName(reverse);
    $: ramSort && socketData.processes && sortRAM(reverse);
    $: statusSort && socketData.processes && sortStatus(reverse);

    function sortCPU(reverse: boolean) {
        socketData.processes.sort((a, b) => {
            if (a.cpu < b.cpu) {
                return reverse ? -1 : 1;
            } else if (a.cpu > b.cpu) {
                return reverse ? 1 : -1;
            } else {
                return 0;
            }
        });
        socketData = socketData;
    }

    function setCPU() {
        if (cpuSort == true) {
            reverse = !reverse;
            if (cpuIcon == faSortUp) {
                cpuIcon = faSortDown;
            } else {
                cpuIcon = faSortUp;
            }
        } else {
            reverse = false;
            cpuSort = true;
            pidSort = false;
            nameSort = false;
            ramSort = false;
            statusSort = false;
            cpuIcon = faSortUp;
            ramIcon = faSort;
            pidIcon = faSort;
            nameIcon = faSort;
            statusIcon = faSort;
        }
    }

    function sortName(reverse: boolean) {
        socketData.processes.sort((a, b) => {
            if (a.name < b.name) {
                return reverse ? 1 : -1;
            } else if (a.name > b.name) {
                return reverse ? -1 : 1;
            } else {
                return 0;
            }
        });
        socketData = socketData;
    }

    function setName() {
        if (nameSort == true) {
            reverse = !reverse;
            if (nameIcon == faSortUp) {
                nameIcon = faSortDown;
            } else {
                nameIcon = faSortUp;
            }
        } else {
            reverse = false;
            pidSort = false;
            cpuSort = false;
            ramSort = false;
            nameSort = true;
            statusSort = false;
            nameIcon = faSortUp;
            pidIcon = faSort;
            cpuIcon = faSort;
            ramIcon = faSort;
            statusIcon = faSort;
        }
    }

    function sortPid(reverse: boolean) {
        socketData.processes.sort((a, b) => {
            if (a.pid < b.pid) {
                return reverse ? 1 : -1;
            } else if (a.pid > b.pid) {
                return reverse ? -1 : 1;
            } else {
                return 0;
            }
        });
        socketData = socketData;
    }

    function setPid() {
        if (pidSort == true) {
            reverse = !reverse;
            if (pidIcon == faSortUp) {
                pidIcon = faSortDown;
            } else {
                pidIcon = faSortUp;
            }
        } else {
            reverse = false;
            cpuSort = false;
            ramSort = false;
            nameSort = false;
            pidSort = true;
            statusSort = false;
            pidIcon = faSortUp;
            cpuIcon = faSort;
            ramIcon = faSort;
            nameIcon = faSort;
            statusIcon = faSort;
        }
    }

    function sortRAM(reverse: boolean) {
        socketData.processes.sort((a, b) => {
            if (a.ram < b.ram) {
                return reverse ? -1 : 1;
            } else if (a.ram > b.ram) {
                return reverse ? 1 : -1;
            } else {
                return 0;
            }
        });
        socketData = socketData;
    }

    function setRAM() {
        if (ramSort == true) {
            reverse = !reverse;
            if (ramIcon == faSortUp) {
                ramIcon = faSortDown;
            } else {
                ramIcon = faSortUp;
            }
        } else {
            reverse = false;
            pidSort = false;
            cpuSort = false;
            nameSort = false;
            ramSort = true;
            statusSort = false;
            ramIcon = faSortUp;
            cpuIcon = faSort;
            nameIcon = faSort;
            pidIcon = faSort;
            statusIcon = faSort;
        }
    }

    function sortStatus(reverse: boolean) {
        socketData.processes.sort((a, b) => {
            if (a.status < b.status) {
                return reverse ? -1 : 1;
            } else if (a.status > b.status) {
                return reverse ? 1 : -1;
            } else {
                return 0;
            }
        });
        socketData = socketData;
    }

    function setStatus() {
        if (statusSort == true) {
            reverse = !reverse;
            if (statusIcon == faSortUp) {
                statusIcon = faSortDown;
            } else {
                statusIcon = faSortUp;
            }
        } else {
            reverse = false;
            pidSort = false;
            cpuSort = false;
            nameSort = false;
            statusSort = true;
            ramIcon = faSort;
            cpuIcon = faSort;
            nameIcon = faSort;
            pidIcon = faSort;
            statusIcon = faSortUp;
        }
    }
</script>

<main>
    {#if socketData.processes}
        <table
            class="border border-gray-300 dark:border-gray-700 w-full table-fixed break-words min-w-50"
        >
            <tr class="table-header">
                <th
                    >PID<span on:click={setPid}
                        ><Fa
                            icon={pidIcon}
                            class="float-right cursor-pointer"
                        /></span
                    ></th
                >
                <th
                    >Name<span on:click={setName}
                        ><Fa
                            icon={nameIcon}
                            class="float-right cursor-pointer"
                        /></span
                    ></th
                >
                <th
                    >Status<span on:click={setStatus}
                        ><Fa
                            icon={statusIcon}
                            class="float-right cursor-pointer"
                        /></span
                    ></th
                >
                <th
                    >CPU Usage<span on:click={setCPU}
                        ><Fa
                            icon={cpuIcon}
                            class="float-right cursor-pointer"
                        /></span
                    ></th
                >
                <th
                    >RAM Usage<span on:click={setRAM}
                        ><Fa
                            icon={ramIcon}
                            class="float-right cursor-pointer"
                        /></span
                    ></th
                >
                <th>Actions</th>
            </tr>
            {#each socketData.processes as process}
                <tr
                    class="mt-32 even:bg-white odd:bg-gray-200 dark:even:bg-black dark:odd:bg-gray-800  dark:border-gray-600 border-t-2 border-gray-300 border-opacity-50"
                >
                    <td class="p-2">{process.pid}</td>
                    <td class="p-2">{process.name}</td>
                    <td class="p-2">{process.status}</td>
                    <td class="p-2">{process.cpu}%</td>
                    <td class="p-2"
                        >{prettyBytes(process.ram, {
                            binary: true,
                            maximumFractionDigits: 0,
                        })}</td
                    >
                    <td class="p-2 space-x-2">
                        {#if process.name != "dietpi-dashboar"}
                            <span
                                on:click={() =>
                                    socketSend("terminate", [
                                        process.pid.toString(),
                                    ])}
                                title="Terminate"
                                ><Fa
                                    icon={faBan}
                                    class="btn rounded-sm p-0.5"
                                    size="lg"
                                /></span
                            >
                            <span
                                on:click={() =>
                                    socketSend("kill", [
                                        process.pid.toString(),
                                    ])}
                                title="Kill"
                                ><Fa
                                    icon={faSkull}
                                    class="btn rounded-sm p-0.5"
                                    size="lg"
                                /></span
                            >
                            {#if process.status != "stopped"}
                                <span
                                    on:click={() =>
                                        socketSend("suspend", [
                                            process.pid.toString(),
                                        ])}
                                    title="Suspend"
                                    ><Fa
                                        icon={faPause}
                                        class="btn rounded-sm p-0.5"
                                        size="lg"
                                    /></span
                                >
                            {:else}
                                <span
                                    on:click={() =>
                                        socketSend("resume", [
                                            process.pid.toString(),
                                        ])}
                                    title="Resume"
                                    ><Fa
                                        icon={faPlay}
                                        class="btn rounded-sm p-0.5"
                                        size="lg"
                                    /></span
                                >
                            {/if}
                        {/if}
                    </td>
                </tr>
            {/each}
        </table>
    {/if}
</main>
