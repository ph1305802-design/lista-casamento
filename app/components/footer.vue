<template>
    <footer class="footer">
      <div class="footer-content">
        <h3>Pedro & Polyanna ❤️</h3>
  
        <p>
          Obrigado por fazer parte deste momento tão especial em nossas vidas.
        </p>
  
        <div class="contatos">
          <a href="mailto:contatopedrohrq@gmail.com">
            ✉️ contatopedrohrq@gmail.com
          </a>
  
          <a href="https://wa.me/5571999522030" target="_blank">
            📱 (71) 99952-2030
          </a>
        </div>
  
        <p class="copyright">
          © {{ new Date().getFullYear() }} • Feito com amor para celebrar nosso casamento.
        </p>
      </div>
    </footer>
  </template>
  
  <style scoped>
  .footer {
    background: #f8f4f1;
    border-top: 1px solid var(--main-primary-color);
    padding: 50px 20px;
    margin-top: 80px;
  }
  
  .footer-content {
    max-width: 900px;
    margin: 0 auto;
    text-align: center;
  }
  
  .footer h3 {
    margin-bottom: 12px;
    font-size: 1.8rem;
    color: #7a5c58;
  }
  
  .footer p {
    color: #666;
    line-height: 1.6;
  }
  
  .contatos {
    display: flex;
    justify-content: center;
    gap: 25px;
    flex-wrap: wrap;
    margin: 25px 0;
  }
  
  .contatos a {
    color: #7a5c58;
    text-decoration: none;
    font-weight: 500;
    transition: 0.3s;
  }
  
  .contatos a:hover {
    color: #b98b84;
  }
  
  .copyright {
    margin-top: 20px;
    font-size: 0.9rem;
    color: #999;
  }
  
  @media (max-width: 600px) {
    .contatos {
      flex-direction: column;
      gap: 12px;
    }
  }
  </style>