<template>
    <div class="div-presentes">
    <h1 class="titulo-presentes">Presentes</h1>
    <p class="subtitle-presentes">Esta lista reúne algumas ideias de presentes que combinam com o nosso gosto e nossos planos. Ela serve apenas como inspiração, sem a necessidade de seguir as opções à risca. 💕 (2 Cor. 9:7)</p>
    <div class="lista-presentes">
        <div v-for="item in presentesDisponiveis" :key="item.id" class="presente">
            <div class="presente__imagem">
                <img :src="item.imagem" :alt="item.nome" />
            </div>

            <div class="presente__conteudo">
                <h3>{{ item.nome }}</h3>

                <p v-if="item.valor" class="valor">R$ {{ Number(item.valor.replace(",", ".")).toFixed(2).replace(".", ",") }}</p>

                <p v-if="!item.valor">
                    Valor Indefinido
                </p>

                <a :href="item['Link do produto']" target="_blank" rel="noopener noreferrer" class="btn-produto">
                    Ver produto
                </a>
            </div>
        </div>
    </div>
</div>
</template>
<script setup>
    import { ref, onMounted, computed } from "vue";

    const presentes = ref([]);

    const presentesDisponiveis = computed(() => {
        return presentes.value.filter((item) => item.ja_comprado !== "Sim");
    });
    onMounted(async () => {
        const response = await fetch(
            "https://opensheet.elk.sh/1lAGOKV6g-_f-zx7dUK45Yz2tJu7GMh9HL6F0XjpUdtA/P%C3%A1gina1"
        );

        presentes.value = await response.json();
    });
</script>
<style scoped>
    .div-presentes {
        padding: 42px 0px;
    }
    .subtitle-presentes {
        font-family: "Lexend", sans-serif;
        text-align: center;
        color: var(--main-font-color);
        font-size: 18px;
        font-weight: 400;
        max-width: 450px;
        padding-bottom: 42px;
        margin: 0 auto;
    }
    .titulo-presentes {
        font-family: "Yeseva One", serif;
        text-align: center;
        color: var(--main-font-color);
        font-size: 52px;
        text-transform: uppercase;
    }
    .lista-presentes {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
        gap: 24px;
        padding: 24px;
    }

    .presente {
        background: #fff;
        border-radius: 20px;
        overflow: hidden;
        box-shadow: 0 8px 30px rgba(0, 0, 0, 0.08);
        transition: all 0.3s ease;
        display: flex;
        flex-direction: column;
    }

    .presente:hover {
        transform: translateY(-6px);
        box-shadow: 0 14px 40px rgba(0, 0, 0, 0.12);
    }

    .presente__imagem {
        width: 100%;
        aspect-ratio: 1;
        overflow: hidden;
        background: #f7f7f7;
    }

    .presente__imagem img {
        width: 100%;
        height: 100%;
        object-fit: cover;
        transition: transform 0.4s ease;
    }

    .presente:hover .presente__imagem img {
        transform: scale(1.05);
    }

    .presente__conteudo {
        padding: 20px;
        display: flex;
        flex-direction: column;
        flex: 1;
    }

    .presente h3 {
        font-size: 1.1rem;
        font-weight: 600;
        margin: 0 0 12px;
        color: #333;
    }

    .valor {
        font-size: 1.4rem;
        font-weight: 700;
        color: var(--main-font-color);
        margin-bottom: 20px;
    }

    .btn-produto {
        margin-top: auto;
        display: block;
        text-align: center;
        text-decoration: none;
        background: var(--main-primary-color);
        color: white;
        padding: 12px 16px;
        border-radius: 12px;
        font-weight: 600;
        transition: all 0.3s ease;
    }

    .btn-produto:hover {
        background: #c28d5e;
        transform: translateY(-2px);
    }
</style>
