<script lang="ts">
    import { ProgramFilter, ComputerProgram } from '../../programs';
    import WindowBase from '../components/WindowBase.svelte';
    import WebPagePortfolio from '../components/WebPagePortfolio.svelte';
    import { CmdContentTracker, CommandManager } from '../stores';
    import { afterUpdate, onDestroy, onMount } from 'svelte';
    import { browser } from '$app/environment';
    import { CommandStatus, ConsoleCommand, ConsoleCommandHelper } from '../../commands';

    //
    // Command Prompt
    //

    let program: ComputerProgram = ProgramFilter.Find('cmd')!;

    onMount(() => {});

    function OnCommandEnter(event: KeyboardEvent) {
        var desiredKey = 'Enter';
        var cmd_input = document.getElementById('cmd_input_' + ($CmdContentTracker - 1));

        if (event.key == desiredKey) {
            ConsoleCommandHelper.RunCommand(cmd_input?.value?.toLowerCase());
        }
    }
</script>

<WindowBase {program} isWebSite={false} showTitle={true} title="C:\Windows\system32\cmd.exe" width={800} height={400}>
    <div id={program.GetWebPage().string()} class="program_cmd">
        <div>Microsoft Windows [Version 6.1.7601]</div>
        <div>Copyright (c) 2009 Microsoft Corporation. All rights reserved.</div>
        <div>&nbsp;</div>

        <div id="cmd_manager">
            {#each { length: $CmdContentTracker } as _, i}
                <div id="cmd_input_box" class="program_cmd__input__box">
                    C:\Users\zeljko><input
                        on:keydown={OnCommandEnter}
                        maxlength="64"
                        tabindex="0"
                        id="cmd_input_{i}"
                        class="program_cmd__input"
                    />
                </div>
                <div id="cmd_results_{i}" />
            {/each}
        </div>
    </div>
</WindowBase>

<style>
    :root {
        --win7-cmd-background: black;
        --win7-cmd-color: lightgray;

        --win7-cmd-font-family: monospace;
        --win7-cmd-font-size: 0.9rem;
    }

    .program_cmd {
        display: flex;
        flex-direction: column;
        font-family: var(--win7-cmd-font-family);
        background: var(--win7-cmd-background);
        color: var(--win7-cmd-color);
        font-size: var(--win7-cmd-font-size);
        user-select: text;
        height: inherit;
        overflow-y: auto;
        padding-left: 5px;
        padding-right: 5px;
    }

    .program_cmd *::selection {
        background: rgba(211, 211, 211, 0.5);
    }

    .program_cmd__input__box {
        display: flex;
        align-items: center;
        gap: 10px;
    }

    .program_cmd__input {
        background: transparent;
        position: relative;
        right: 7px;
        border: none;
        outline: none;
        width: 78%;
        color: var(---win7-cmd-color);
    }

    .program_cmd__input::selection {
        background: rgba(211, 211, 211, 0.5);
    }
</style>
