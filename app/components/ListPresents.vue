<template>
    <div class="div-presentes">
      <h1 class="titulo-presentes">Presentes</h1>
      <p class="subtitle-presentes">
        Esta lista reúne algumas ideias de presentes que combinam com o nosso gosto e nossos planos. 
        Ela serve apenas como inspiração, sem a necessidade de seguir os valores à risca. 💕 (2 Cor. 9:7)
      </p>
  
      <div class="lista-presentes">
        <div v-for="item in presentesDisponiveis" :key="item.id" class="presente">
          <div class="presente__imagem">
            <img :src="item.imagem" :alt="item.nome" loading="lazy" />
          </div>
  
          <div class="presente__conteudo">
            <h3>{{ item.nome }}</h3>
            
            <p v-if="item.valor" class="valor">
              {{ formatarMoeda(item.valor) }}
            </p>
            <p v-else class="valor-indefinido">
              Valor Indefinido
            </p>
  
            <button class="btn-produto" @click="abrirModal(item)">
              Presentear
            </button>
          </div>
        </div>
      </div>
    </div>
  
    <div v-if="modalAberto" class="pix-modal-overlay" @click="fecharModal">
      <div class="pix-modal" @click.stop>
        <button class="pix-close" @click="fecharModal" aria-label="Fechar janela">
          ✕
        </button>
  
        <h3>Quanto deseja contribuir?</h3>
  
        <div class="valor-box">
          <span>R$</span>
          <input 
            type="text" 
            inputmode="numeric" 
            v-model="valorPixFormatado" 
          />
        </div>
  
        <div class="valores-rapidos">
          <button @click="setValor(50)">R$ 50</button>
          <button @click="setValor(100)">R$ 100</button>
          <button @click="setValor(obterValorNumerico(presenteSelecionado?.valor))">
            Valor sugerido
          </button>
        </div>
  
        <div class="qrcode-box">
          <img :src="qrCode" alt="Pix QRCode" />
        </div>
  
        <p class="pix-info">
          Escaneie o QR Code com o aplicativo do seu banco
        </p>
  
        <button class="btn-copia-cola" @click="copiarPix">
          📋 Pix Copia e Cola
        </button>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, computed, onMounted } from "vue";
  import QRCode from "qrcode";
  
  // --- CONFIGURAÇÕES / CONSTANTES ---
  const PIX_KEY = "86611288546"; // CPF sem pontuação
  const PIX_NAME = "PEDRO HENRIQUE";
  const PIX_CITY = "SALVADOR";
  
  // --- ESTADOS REATIVOS ---
  const presentes = ref([]);
  const modalAberto = ref(false);
  const presenteSelecionado = ref(null);
  const qrCode = ref("");
  const valorPix = ref(0);
  const payloadPix = ref("");
  
  // --- PROPRIEDADES COMPUTADAS ---
  const presentesDisponiveis = computed(() => {
    return presentes.value.filter(item => item.ja_comprado !== "Sim");
  });
  
  // Computed property para lidar com a máscara de moeda no input
  const valorPixFormatado = computed({
    get() {
      return valorPix.value.toLocaleString("pt-BR", {
        minimumFractionDigits: 2,
        maximumFractionDigits: 2,
      });
    },
    set(novoValor) {
      // Remove tudo que não for número (ex: pontos, vírgulas, letras)
      const apenasNumeros = novoValor.replace(/\D/g, "");
      
      // Divide por 100 para criar as casas decimais simulando a digitação da direita pra esquerda
      valorPix.value = Number(apenasNumeros) / 100;
      
      atualizarQrCode();
    }
  });
  
  // --- CICLO DE VIDA (LIFECYCLE) ---
  onMounted(async () => {
    try {
      const response = await fetch(
        "https://opensheet.elk.sh/1lAGOKV6g-_f-zx7dUK45Yz2tJu7GMh9HL6F0XjpUdtA/P%C3%A1gina1"
      );
      presentes.value = await response.json();
    } catch (error) {
      console.error("Erro ao carregar a lista de presentes:", error);
    }
  });
  
  // --- MÉTODOS DE MANIPULAÇÃO ---
  
  // Função auxiliar para converter "1.000,00" ou "1000,00" para número real (1000.00)
  function obterValorNumerico(valorString) {
    if (!valorString) return 0;
    // Remove pontos de milhares e troca a vírgula decimal por ponto
    const stringLimpa = String(valorString).replace(/\./g, "").replace(",", ".");
    return Number(stringLimpa) || 0;
  }
  
  function formatarMoeda(valor) {
    if (!valor) return "";
    const num = obterValorNumerico(valor);
    return num.toLocaleString("pt-BR", { style: "currency", currency: "BRL" });
  }
  
  async function abrirModal(item) {
    presenteSelecionado.value = item;
    valorPix.value = obterValorNumerico(item.valor);
    await atualizarQrCode();
    modalAberto.value = true;
  }
  
  function fecharModal() {
    modalAberto.value = false;
  }
  
  async function atualizarQrCode() {
    payloadPix.value = gerarPayloadPix(valorPix.value.toFixed(2));
    qrCode.value = await QRCode.toDataURL(payloadPix.value, {
      width: 300,
      margin: 1,
    });
  }
  
  async function copiarPix() {
    await navigator.clipboard.writeText(payloadPix.value);
    alert("Pix copiado!");
  }
  
  function setValor(valor) {
    valorPix.value = valor;
    atualizarQrCode();
  }
  
  // --- GERADORES AUXILIARES DO PIX ---
  function crc16(str) {
    let crc = 0xffff;
    for (let c = 0; c < str.length; c++) {
      crc ^= str.charCodeAt(c) << 8;
      for (let i = 0; i < 8; i++) {
        if ((crc & 0x8000) !== 0) {
          crc = (crc << 1) ^ 0x1021;
        } else {
          crc <<= 1;
        }
        crc &= 0xffff;
      }
    }
    return crc.toString(16).toUpperCase().padStart(4, "0");
  }
  
  function gerarPayloadPix(valor, descricao) {
    const merchantAccount =
      "0014BR.GOV.BCB.PIX" +
      "01" +
      PIX_KEY.length.toString().padStart(2, "0") +
      PIX_KEY;
  
    const txid = "***";
  
    let payload =
      "000201" +
      "26" +
      merchantAccount.length.toString().padStart(2, "0") +
      merchantAccount +
      "52040000" +
      "5303986" +
      "54" +
      valor.length.toString().padStart(2, "0") +
      valor +
      "5802BR" +
      "59" +
      PIX_NAME.length.toString().padStart(2, "0") +
      PIX_NAME +
      "60" +
      PIX_CITY.length.toString().padStart(2, "0") +
      PIX_CITY +
      "62070503" +
      txid;
  
    payload += "6304";
    payload += crc16(payload);
  
    return payload;
  }
  </script>
  
  <style scoped>
  /* --- LAYOUT GERAL --- */
  .div-presentes {
    padding: 42px 24px;
  }
  
  .titulo-presentes {
    font-family: "Yeseva One", serif;
    text-align: center;
    color: var(--main-font-color, #222);
    font-size: 52px;
    text-transform: uppercase;
    margin-bottom: 12px;
  }
  
  .subtitle-presentes {
    font-family: "Lexend", sans-serif;
    text-align: center;
    color: var(--main-font-color, #666);
    font-size: 16px;
    font-weight: 400;
    max-width: 600px;
    padding: 0 24px 42px 24px;
    margin: 0 auto;
    line-height: 1.6;
  }
  
  /* --- GRID DE PRESENTES --- */
  .lista-presentes {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    gap: 32px;
    padding: 24px;
    max-width: 1200px;
    margin: 0 auto;
  }
  
  /* --- CARDS DE PRESENTES --- */
  .presente {
    background: #ffffff;
    border: 1px solid rgba(0, 0, 0, 0.05);
    border-radius: 24px;
    overflow: hidden;
    transition: transform 0.3s cubic-bezier(0.25, 0.8, 0.25, 1), box-shadow 0.3s ease;
    display: flex;
    flex-direction: column;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.02);
  }
  
  .presente:hover {
    transform: translateY(-6px);
    box-shadow: 0 12px 30px rgba(0, 0, 0, 0.08);
  }
  
  .presente__imagem {
    aspect-ratio: 1.1;
    background: #fdfbf7;
    overflow: hidden;
    position: relative;
  }
  
  .presente__imagem img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    transition: transform 0.5s ease;
  }
  
  .presente:hover .presente__imagem img {
    transform: scale(1.05);
  }
  
  .presente__conteudo {
    padding: 22px;
    display: flex;
    flex-direction: column;
    gap: 14px;
    flex: 1;
    text-align: center;
  }
  
  .presente h3 {
    font-family: "Lexend", sans-serif;
    font-size: 1.1rem;
    line-height: 1.4;
    font-weight: 600;
    color: #2c3e50;
    margin: 0;
    min-height: 44px;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  .valor {
    font-size: 1.4rem;
    font-weight: 700;
    color: var(--main-primary-color, #b8977e);
    margin: auto 0 0 0;
  }
  
  .valor-indefinido {
    font-size: 1rem;
    font-weight: 500;
    color: #a0a0a0;
    font-style: italic;
    margin: auto 0 0 0;
  }
  
  /* --- BOTÃO DO CARD --- */
  .btn-produto {
    border: none;
    background: var(--main-primary-color);
    color: #ffffff;
    border-radius: 16px;
    padding: 14px;
    font-size: 1rem;
    font-weight: 600;
    cursor: pointer;
    transition: background 0.2s ease, color 0.2s ease, transform 0.1s ease;
    width: 100%;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.02);
  }
  
  .btn-produto:hover {
    background: #bb4420;
    color: white;
  }
  
  .btn-produto:active {
    transform: scale(0.98);
  }
  
  /* --- MODAL PIX --- */
  .pix-modal-overlay {
    position: fixed;
    inset: 0;
    background: rgba(0, 0, 0, 0.4);
    backdrop-filter: blur(8px);
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 24px;
    z-index: 9999;
    animation: fadeIn 0.3s ease;
  }
  
  .pix-modal {
    width: 100%;
    max-width: 480px;
    background: white;
    border-radius: 32px;
    padding: 32px;
    position: relative;
    box-shadow: 0 20px 40px rgba(0, 0, 0, 0.15);
    animation: scaleUp 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
  }
  
  .pix-close {
    position: absolute;
    right: 24px;
    top: 24px;
    border: none;
    background: none;
    cursor: pointer;
    font-size: 20px;
    color: #999;
    transition: color 0.2s;
  }
  
  .pix-close:hover {
    color: #333;
  }
  
  .pix-modal h3 {
    margin-top: 0;
    margin-bottom: 20px;
    font-size: 20px;
    font-weight: 600;
    color: #333;
    text-align: center;
  }
  
  .valor-box {
    background: #f7f4f0;
    border-radius: 20px;
    padding: 12px 20px;
    display: flex;
    align-items: center;
    gap: 12px;
    border: 1px solid rgba(0, 0, 0, 0.02);
  }
  
  .valor-box span {
    font-size: 1.4rem;
    font-weight: 600;
    color: #9d8f84;
  }
  
  .valor-box input {
    border: none;
    background: transparent;
    width: 100%;
    font-size: 1.4rem;
    font-weight: 700;
    outline: none;
    color: #333;
  }
  
  .valores-rapidos {
    display: flex;
    gap: 10px;
    margin-top: 16px;
  }
  
  .valores-rapidos button {
    flex: 1;
    border: none;
    background: #f7f4f0;
    padding: 12px;
    border-radius: 14px;
    cursor: pointer;
    font-weight: 600;
    font-size: 0.9rem;
    color: #5c534c;
    transition: background 0.2s, color 0.2s;
  }
  
  .valores-rapidos button:hover {
    background: #ede7df;
    color: #222;
  }
  
  .qrcode-box {
    width: 200px;
    margin: 28px auto 16px;
    padding: 16px;
    border-radius: 24px;
    border: 1px solid #f0f0f0;
    background: #fff;
  }
  
  .qrcode-box img {
    width: 100%;
    display: block;
  }
  
  .pix-info {
    text-align: center;
    color: #777;
    font-size: 14px;
    margin-bottom: 24px;
    line-height: 1.4;
  }
  
  .btn-copia-cola {
    width: 100%;
    border: none;
    background: #ede7df;
    color: #5c534c;
    padding: 16px;
    border-radius: 18px;
    font-size: 16px;
    font-weight: 600;
    cursor: pointer;
    transition: background 0.2s;
  }
  
  .btn-copia-cola:hover {
    background: #e3dacd;
  }
  
  /* --- ANIMAÇÕES --- */
  @keyframes fadeIn {
    from { opacity: 0; }
    to { opacity: 1; }
  }
  
  @keyframes scaleUp {
    from { transform: scale(0.95); opacity: 0; }
    to { transform: scale(1); opacity: 1; }
  }
  
  /* --- RESPONSIVIDADE --- */
  @media screen and (max-width: 600px) {
    .titulo-presentes {
      font-size: 2.2rem;
    }
    .lista-presentes {
      grid-template-columns: 1fr;
      gap: 24px;
      padding: 16px;
    }
    .pix-modal {
      padding: 24px;
      border-radius: 24px;
    }
  }
  </style>