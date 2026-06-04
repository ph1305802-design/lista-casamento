<script>
    export default {
        data() {
            return {
                dias: "00",
                horas: "00",
                minutos: "00",
                segundos: "00",
                intervalId: null,
            };
        },

        mounted() {
            const dataAlvo = new Date("2026-09-20T00:00:00");

            const atualizarContador = () => {
                const agora = new Date();
                const diferenca = dataAlvo - agora;

                if (diferenca <= 0) {
                    clearInterval(this.intervalId);
                    return;
                }

                const dias = Math.floor(diferenca / (1000 * 60 * 60 * 24));
                const horas = Math.floor((diferenca % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
                const minutos = Math.floor((diferenca % (1000 * 60 * 60)) / (1000 * 60));
                const segundos = Math.floor((diferenca % (1000 * 60)) / 1000);

                this.dias = String(dias).padStart(2, "0");
                this.horas = String(horas).padStart(2, "0");
                this.minutos = String(minutos).padStart(2, "0");
                this.segundos = String(segundos).padStart(2, "0");
            };

            atualizarContador();
            this.intervalId = setInterval(atualizarContador, 1000);
        },

        beforeUnmount() {
            clearInterval(this.intervalId);
        },
    };
</script>
<template>
    <div class="countdown">
        <div class="timer">
            <div class="unit">
                <div class="value">{{ dias }}</div>
                <div class="label">Dias</div>
            </div>

            <div class="separator">:</div>

            <div class="unit">
                <div class="value">{{ horas }}</div>
                <div class="label">Horas</div>
            </div>

            <div class="separator">:</div>

            <div class="unit">
                <div class="value">{{ minutos }}</div>
                <div class="label">Minutos</div>
            </div>

            <div class="separator">:</div>

            <div class="unit">
                <div class="value">{{ segundos }}</div>
                <div class="label">Segundos</div>
            </div>
        </div>
    </div>
</template>

<style scoped>
    .countdown {
        display: flex;
        justify-content: center;
        padding: 20px;
    }

    .timer {
        display: flex;
        align-items: flex-start;
        gap: 12px;
    }

    .unit {
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    .value {
        font-size: 48px;
        font-weight: 700;
        line-height: 1;
        font-family: monospace;
        color: var(--main-font-color);
        font-family: "Yeseva One", serif;
    }

    .label {
        font-family: "Yeseva One", serif;
        color: var(--main-font-color);
        margin-top: 8px;
        font-size: 14px;
        text-transform: uppercase;
        opacity: 0.7;
    }

    .separator {
        color: var(--main-font-color);
        font-size: 48px;
        font-weight: 700;
        line-height: 1;
        margin-top: 0;
    }

    @media (max-width: 768px) {
        .separator {
            font-size: 32px;
        }
        .timer {
            gap: 8px;
        }
        .value {
            font-size: 32px;
        }
        .label {
            font-size: 10px;
        }
    }
</style>
