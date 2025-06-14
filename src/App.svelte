<script lang="ts">
    import { Loader, AlertTriangle, RefreshCw } from "lucide-svelte";
    import { onMount } from "svelte";

    let status = "loading"; // "loading", "error", "retrying"
    let countdown = 5;

    let statusTitle = "Connecting to Axon Studio";
    let statusMessage = "Please wait a little bit...";

    let retryTimeout: NodeJS.Timeout;
    let countdownInterval: NodeJS.Timeout;

    function updateStatus(title: string, message: string) {
        statusTitle = title;
        statusMessage = message;
    }

    function attemptConnection() {
        clearTimeout(retryTimeout);
        clearInterval(countdownInterval);
        status = "loading";
        updateStatus(
            "Connecting to Axon Studio",
            "Please wait a little bit..."
        );

        setTimeout(() => {
            connectionFailed();
        }, 50000);
    }

    function connectionFailed() {
        status = "error";
        countdown = 10;
        updateStatus("Connection failed", `Retrying in ${countdown}s...`);

        countdownInterval = setInterval(() => {
            countdown--;
            if (countdown > 0) {
                updateStatus(
                    "Connection failed",
                    `Retrying in ${countdown}s...`
                );
            } else {
                clearInterval(countdownInterval);
                status = "loading";
                updateStatus(
                    "Reconnecting...",
                    "Attempting to restore connection..."
                );
                retryTimeout = setTimeout(attemptConnection, 5000);
            }
        }, 1000);
    }

    onMount(() => {
        localStorage.setItem("theme", "light");
        attemptConnection();
    });
</script>

<main
    class="h-screen w-screen overflow-hidden text-gray-900 flex flex-col justify-center items-center px-4 sm:px-6 relative"
>
    <div class="blob blob-1"></div>
    <div class="blob blob-2"></div>
    <div class="blob blob-3"></div>
    <div class="blob blob-4"></div>

    <!-- Branding -->
    <div class="absolute top-10 flex justify-center items-center gap-2 mb-8">
        <img
            src="favicon.png"
            alt="AxonAI logo"
            class="w-7 h-7 rounded-2xl p-0.5 border border-gray-200"
        />
        <span class="text-lg font-medium tracking-tight text-gray-700"
            >AxonAI</span
        >
    </div>

    <!-- Status -->
    <div class="px-8 py-12 w-full max-w-md text-center space-y-2">
        {#if status === "loading"}
            <div class="flex justify-center">
                <Loader class="w-6 h-6 text-neutral-500 animate-spin" />
            </div>
        {:else}
            <div class="flex justify-center">
                <RefreshCw class="w-6 h-6 text-neutral-400 animate-spin" />
            </div>
        {/if}

        <h1 class="text-xl sm:text-2xl font-semibold tracking-tight mt-10">
            {statusTitle}
        </h1>
        <p class="text-sm sm:text-sm text-gray-600 leading-relaxed">
            {statusMessage}
        </p>
    </div>

    <!-- Footer -->
    <p class="absolute bottom-4 text-xs text-gray-500 font-mono tracking-tight">
        Release 0.13.4.b
    </p>
</main>

<style>
    .blob {
        position: absolute;
        border-radius: 50%;
        opacity: 0.3;
        mix-blend-mode: screen;
        filter: blur(100px);
        animation: morph 25s ease-in-out infinite alternate;
        z-index: -1;
    }

    .blob-1 {
        background: radial-gradient(circle at 30% 30%, #ff4ecd, #7b2ff7);
        width: 600px;
        height: 600px;
        top: -200px;
        left: -250px;
        animation-delay: 1s;
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
</style>
