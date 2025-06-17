<script>
    import { onMount } from "svelte";
    import { X } from "lucide-svelte";

    let showError = false;
    let lineCount = 10;
    let radius = 12;
    let animationDuration = 1.2;

    $: stepAngle = 360 / lineCount;
    $: delayStep = animationDuration / lineCount;

    $: lines = Array.from({ length: lineCount }, (_, i) => ({
        angle: i * stepAngle,
        delay: -(i * delayStep),
    }));

    onMount(() => {
        setTimeout(() => {
            showError = true;
        }, 50000);
    });
</script>

<div class="relative min-h-screen max-h-screen overflow-hidden">
    <div class="blob blob-1"></div>
    <div class="blob blob-2"></div>
    <div class="blob blob-3"></div>
    <div class="blob blob-4"></div>

    <section class="relative h-screen flex flex-col items-center justify-center px-4 sm:px-6 lg:px-8 overflow-hidden">
        <div class="relative max-w-7xl mx-auto h-full">
            <div class="flex flex-col items-center justify-center h-full text-center max-w-5xl py-2">
                <div class="flex items-center justify-center">
                    {#if !showError}
                        <div class="relative">
                            <div class="relative w-6 h-6 left-3">
                                {#each lines as line}
                                    <div
                                        class="absolute w-0.5 h-1.75 bg-gray-400 rounded-sm origin-center animate-ios-fade"
                                        style="transform: rotate({line.angle}deg); animation-delay: {line.delay}s; transform-origin: center {radius}px;"
                                    ></div>
                                {/each}
                            </div>
                        </div>
                    {/if}
                </div>
                
                <p class="mt-4 text-sm text-gray-700 {!showError ? 'animate-pulse' : ''}">
                    {showError ? 'An unexpected error occurred while creating the session. Please try again later.' : 'Initialize Axon Studio...'}
                </p>
                
                <p class="absolute bottom-4 text-xs text-gray-500 font-mono tracking-tight">
                    Release 0.14.0.b
                </p>
            </div>
        </div>
    </section>
</div>

<style>
    .blob {
        position: absolute;
        border-radius: 50%;
        opacity: 0.2;
        mix-blend-mode: screen;
        filter: blur(100px);
        animation: morph 25s ease-in-out infinite alternate;
        z-index: -1;
    }

    .blob-1 {
        background: radial-gradient(circle at 30% 30%, #ff4ecd, #7b2ff7);
        width: 600px;
        height: 600px;
        top: -50px;
        left: 100px;
        animation-delay: 0s;
    }

    .blob-2 {
        background: radial-gradient(circle at 70% 70%, #00ffe0, #0072ff);
        width: 500px;
        height: 500px;
        bottom: -150px;
        right: -200px;
        animation-delay: 3s;
    }

    .blob-3 {
        background: radial-gradient(circle at 50% 50%, #ffc800, #ff6f00);
        width: 450px;
        height: 450px;
        top: 35%;
        left: -100px;
        animation-delay: 6s;
    }

    .blob-4 {
        background: radial-gradient(circle at 50% 50%, #00ff8c, #00c2ff);
        width: 400px;
        height: 400px;
        bottom: 5%;
        left: 60%;
        animation-delay: 9s;
    }

    @keyframes morph {
        0% {
            transform: scale(1) translate(0, 0) rotate(0deg);
            border-radius: 42% 58% 67% 33% / 33% 33% 67% 67%;
        }
        25% {
            transform: scale(1.1) translate(20px, -25px) rotate(45deg);
            border-radius: 50% 50% 40% 60% / 60% 40% 60% 40%;
        }
        50% {
            transform: scale(1.3) translate(-30px, 30px) rotate(90deg);
            border-radius: 60% 40% 40% 60% / 60% 60% 40% 40%;
        }
        75% {
            transform: scale(1.1) translate(25px, 10px) rotate(135deg);
            border-radius: 35% 65% 65% 35% / 35% 35% 65% 65%;
        }
        100% {
            transform: scale(1) translate(-20px, 0px) rotate(180deg);
            border-radius: 42% 58% 67% 33% / 33% 33% 67% 67%;
        }
    }

    .animate-ios-fade {
        animation: ios-fade var(--duration, 1.2s) linear infinite;
    }

    @keyframes ios-fade {
        0% {
            opacity: 1;
        }
        100% {
            opacity: 0;
        }
    }
</style>