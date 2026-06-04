<template>
    <section class="local-section">
  <div class="local-grid">
    <div class="local-mapa">
      <iframe
        src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d15560.78074395232!2d-38.280288212841796!3d-12.8306603!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x7163f6e7fd49665%3A0xd4a80360c8b6de4d!2sSal%C3%A3o%20Do%20Reino%20Das%20Testemunhas%20De%20Jeov%C3%A1!5e0!3m2!1spt-BR!2sbr!4v1780582940008!5m2!1spt-BR!2sbr"
        loading="lazy">
      </iframe>
    </div>

    <div class="local-info">
      <span class="tag">Discurso</span>
      <h2>Salão do Reino das Testemunhas de Jeová</h2>

      <p>
        Será neste local que acontecerá nosso discurso de casamento. A celebração começa ás 13:30 e tem a duração de 1 hora.
      </p>
    </div>
  </div>
</section>

<section class="local-section">
  <div class="local-grid reverse ">
    <div class="local-info">
      <span class="tag">Recepção</span>
      <h2>Espaço Paraíso</h2>

      <p>
        Após a cerimônia, esperamos você para celebrarmos juntos este dia tão especial.
        Celebraremos com as pessoas especiais que fazem parte do nosso casamento.
      </p>
    </div>

    <div class="local-mapa">
      <iframe
        src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d7779.76757674504!2d-38.27426790471173!3d-12.85078176721948!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x7166bbf0354fabb%3A0xd6c4e4a3aef064ef!2zRXNwYcOnbyBQYXJhw61zbw!5e0!3m2!1spt-BR!2sbr!4v1780582998845!5m2!1spt-BR!2sbr"
        loading="lazy">
      </iframe>
    </div>
  </div>
</section>
  </template>
  
  <style scoped>.local-section {
    padding: 0px 20px;
    padding-top: 0px;
    padding-bottom: 72px;
  }
  
  .local-grid {
    max-width: 1200px;
    margin: 0 auto;
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 60px;
    align-items: center;
  }
  
  .local-grid.reverse {
    grid-template-columns: 1fr 1fr;
  }
  
  .local-mapa iframe {
    width: 100%;
    height: 400px;
    border: none;
    border-radius: 16px;
    box-shadow: 0 15px 40px rgba(0,0,0,0.1);
  }
  
  .local-info {
    max-width: 500px;
  }
  
  .tag {
    display: inline-block;
    padding: 6px 16px;
    border-radius: 30px;
    background: #f4ece7;
    color: var(--main-primary-color);
    font-size: 0.9rem;
    margin-bottom: 15px;
  }
  
  .local-info h2 {
    font-size: 2.3rem;
    margin-bottom: 20px;
    color: #333;
  }
  
  .local-info p {
    color: #666;
    line-height: 1.8;
    font-size: 1.05rem;
  }
  
  @media (max-width: 900px) {
    .local-grid {
      grid-template-columns: 1fr;
      gap: 40px;
    }
    .local-grid.reverse {
        grid-template-columns: 1fr;
      gap: 30px;
    }

    .local-grid.reverse > div:last-child {
    grid-column: 1;
    grid-row: 1;
}
  
    .local-info {
      max-width: 100%;
      text-align: center;
    }
  
    .local-info h2 {
      font-size: 1.8rem;
    }
  
    .local-mapa iframe {
      height: 300px;
    }
  }
  </style>