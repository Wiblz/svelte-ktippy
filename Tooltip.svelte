<svelte:options accessors={true}/>
<script>
    import tippy, {followCursor as pFollowCursor, sticky as pSticky, animateFill as pAnimateFill, inlinePositioning as pInlinePositioning} from "tippy.js";
    import {onMount as _onMount, onDestroy as _onDestroy} from "svelte";
    import "tippy.js/dist/tippy.css";
    import {tippyStore} from "./tippyStore";
    export let duration = [300,250];
    export let delay = null;
    export let inlinePositioning = false;
    export let component;
    export let props = {};
    export let sticky = false;
    export let anchorNode;
    export let placement = 'top'
    export let appendTo = anchorNode;
    export let followCursor = false;
    export let maxWidth = 'calc(100vw - 10px)';
    export let interactive = false;
    export let arrow = false;
    export let getReferenceClientRect = null;
    export let onMount = undefined;
    export let onShow = undefined;
    export let onShown = undefined;
    export let onTrigger = undefined;
    export let onUntrigger = undefined;
    export let onHide = undefined;
    export let onHidden = undefined;
    export let onDestroy = undefined;
    export let onCreate = undefined;
    export let onClickOutside = undefined;
    export let theme = undefined;
    export let trigger = 'mouseenter focus';
    export let triggerTarget = null;
    export let onBeforeUpdate = undefined;
    export let onAfterUpdate = undefined;
    export let offset = [0, 10];
    export let interactiveDebounce = 0;
    export let hideOnClick = true;
    export let animation = 'fade';
    export let interactiveBorder = 0;
    export let zIndex = 9999;
    export let role = 'tooltip';
    export let animateFill = false;
    export let showOnCreate = false;
    export let aria = {
        // `null` when interactive: true, otherwise "describedby"
        content: 'auto',
        // matches `interactive` boolean
        expanded: 'auto',
    };
    let tooltipNode;
    let instance = null;

    _onDestroy(() => {
        instance.destroy();
        if(anchorNode.hasAttribute('id')) {
            let id = anchorNode.getAttribute('id');
            delete $tippyStore[id];
        }
    });
    _onMount(() => {
        instance = tippy(anchorNode, {
            showOnCreate,
            plugins: [pFollowCursor, pSticky, pAnimateFill, pInlinePositioning],
            content: tooltipNode,
            appendTo: typeof appendTo === 'string' ? document.querySelector(appendTo) : appendTo,
            placement,
            maxWidth,
            aria,
            delay,
            sticky,
            theme,
            trigger,
            triggerTarget,
            animateFill,
            interactive,
            arrow,
            followCursor,
            interactiveBorder,
            getReferenceClientRect,
            animation,
            onMount,
            duration,
            onShown,
            onShow,
            zIndex,
            role,
            hideOnClick,
            interactiveDebounce,
            offset,
            onAfterUpdate,
            onBeforeUpdate,
            onClickOutside,
            onCreate,
            onDestroy,
            onHidden,
            onHide,
            onUntrigger,
            onTrigger,
            popperOptions: {
                strategy: 'fixed'
            }
        });
        if(anchorNode.hasAttribute('id')) {
            let id = anchorNode.getAttribute('id');
            $tippyStore[id] = instance;
        }
    });


</script>

<div bind:this={tooltipNode}>
    <svelte:component this={component} {...{anchorNode, ...props}}/>
</div>


<style>
    :global(.tippy-box) {
        background-color: transparent;
        backdrop-filter: blur(4px);
        color: #fff;
        padding: 0;
        margin: 0;
    }

    :global(.tippy-content) {
        padding: 0;
        margin: 0;
    }
</style>
