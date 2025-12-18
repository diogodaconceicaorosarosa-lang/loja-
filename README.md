
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Acessórios Hub | Loja Oficial</title>
  <style>
    :root {
      --primary: #111;
      --accent: #25D366; 
      --bg: #f5f5f5;
      --white: #ffffff;
      --danger: #ff4757;
      --gray: #ddd;
    }

    body { margin: 0; font-family: 'Segoe UI', Tahoma, sans-serif; background: var(--bg); color: #333; }

    /* Header */
    header {
      background: var(--primary);
      color: var(--white);
      padding: 15px 5%;
      display: flex;
      justify-content: space-between;
      align-items: center;
      position: sticky;
      top: 0;
      z-index: 1000;
      box-shadow: 0 2px 10px rgba(0,0,0,0.2);
    }

    .logo { font-weight: bold; font-size: 1.5rem; }
    .logo span { color: var(--accent); }

    .cart-btn {
      background: var(--accent);
      padding: 10px 20px;
      border-radius: 50px;
      cursor: pointer;
      font-weight: bold;
      transition: transform 0.2s;
    }
    .cart-btn:active { transform: scale(0.95); }

    /* Vitrine */
    .container { max-width: 1100px; margin: 40px auto; padding: 0 20px; }
    
    .produtos {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
      gap: 25px;
    }

    .produto {
      background: var(--white);
      padding: 20px;
      border-radius: 15px;
      text-align: center;
      box-shadow: 0 4px 15px rgba(0,0,0,0.05);
      transition: 0.3s;
    }
    .produto:hover { transform: translateY(-5px); }
    
    .produto img { width: 100%; height: 220px; object-fit: cover; border-radius: 10px; margin-bottom: 15px; }

    .btn-add {
      background: var(--primary);
      color: white;
      border: none;
      padding: 12px;
      width: 100%;
      border-radius: 8px;
      cursor: pointer;
      font-weight: bold;
      transition: 0.3s;
    }
    .btn-add:hover { opacity: 0.9; background: #333; }

    /* Modal */
    .modal-overlay {
      position: fixed;
      top: 0; left: 0; width: 100%; height: 100%;
      background: rgba(0,0,0,0.85);
      display: none;
      justify-content: center;
      align-items: center;
      z-index: 2000;
    }

    .modal-content {
      background: var(--white);
      padding: 25px;
      border-radius: 20px;
      width: 90%;
      max-width: 450px;
      max-height: 85vh;
      overflow-y: auto;
    }

    .carrinho-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 10px 0;
      border-bottom: 1px solid #eee;
    }

    .btn-remove {
      background: var(--danger);
      color: white;
      border: none;
      padding: 5px 10px;
      border-radius: 5px;
      cursor: pointer;
    }

    /* Form */
    .form-group { margin-top: 15px; }
    .form-group label { display: block; font-size: 13px; font-weight: bold; margin-bottom: 5px; }
    .form-group input {
      width: 100%;
      padding: 12px;
      border: 1px solid var(--gray);
      border-radius: 8px;
      box-sizing: border-box;
    }

    footer { text-align: center; padding: 40px; color: #888; font-size: 14px; }
  </style>
</head>
<body>

  <header>
    <div class="logo">Acessórios<span>Hub</span></div>
    <div class="cart-btn" onclick="toggleModal(true)">
      🛒 <span id="cart-count">0</span> itens
    </div>
  </header>

  <div class="container">
    <div id="vitrine" class="produtos"></div>
  </div>

  <div id="cart-modal" class="modal-overlay">
    <div class="modal-content">
      <h2 style="margin-top:0">Seu Pedido</h2>
      
      <div id="cart-items-list"></div>
      
      <div style="display:flex; justify-content:space-between; margin: 20px 0; font-size: 1.2rem; font-weight:bold;">
        <span>Total:</span>
        <span id="total-view">R$ 0,00</span>
      </div>

      <div style="background: #f9f9f9; padding: 15px; border-radius: 10px;">
        <div class="form-group">
          <label>NOME COMPLETO:</label>
          <input type="text" id="cliente-nome" placeholder="Como podemos te chamar?">
        </div>
        <div class="form-group">
          <label>ENDEREÇO DE ENTREGA:</label>
          <input type="text" id="cliente-endereco" placeholder="Rua, número e bairro">
        </div>
      </div>

      <button class="btn-add" style="background: var(--accent); margin-top: 20px;" onclick="enviarWhatsApp()">
        ✅ Finalizar Pedido no WhatsApp
      </button>
      
      <button onclick="toggleModal(false)" style="background:none; border:none; color:#888; width:100%; margin-top:10px; cursor:pointer;">
        Voltar para a loja
      </button>
    </div>
  </div>

  <footer>
    &copy; 2025 Acessórios Hub. Todos os direitos reservados.
  </footer>

  <script>
    // --- CONFIGURAÇÃO ATUALIZADA ---
    const MEU_NUMERO = "5598999331050"; 

    const produtos = [
      { id: 1, nome: "Relógio Black Minimalist", preco: 250.0, img: "https://images.unsplash.com/photo-1523275335684-37898b6baf30?w=500" },
      { id: 2, nome: "Óculos de Sol Aviador", preco: 150.0, img: "https://images.unsplash.com/photo-1511499767390-90342f16b20f?w=500" },
      { id: 3, nome: "Pulseira Prata 925", preco: 85.0, img: "https://images.unsplash.com/photo-1573408302355-4e0b7caf3ad6?w=500" },
      { id: 4, nome: "Boné Streetwear", preco: 75.0, img: "https://images.unsplash.com/photo-1588850561447-417f33188db0?w=500" },
      { id: 5, nome: "Carteira de Couro", preco: 120.0, img: "https://images.unsplash.com/photo-1627123424574-724758594e93?w=500" },
      { id: 6, nome: "Corrente de Aço", preco: 95.0, img: "https://images.unsplash.com/photo-1599643478518-a784e5dc4c8f?w=500" }
    ];

    let carrinho = JSON.parse(localStorage.getItem('acessorios_hub_cart')) || [];

    function renderizarVitrine() {
      const vitrine = document.getElementById('vitrine');
      vitrine.innerHTML = produtos.map(p => `
        <div class="produto">
          <img src="${p.img}" alt="${p.nome}">
          <h3>${p.nome}</h3>
          <p style="color:var(--accent); font-weight:bold; font-size: 1.2rem">R$ ${p.preco.toFixed(2)}</p>
          <button class="btn-add" onclick="adicionar(${p.id})">Adicionar</button>
        </div>
      `).join('');
    }

    function adicionar(id) {
      const p = produtos.find(item => item.id === id);
      carrinho.push(p);
      salvar();
      atualizarInterface();
    }

    function remover(index) {
      carrinho.splice(index, 1);
      salvar();
      atualizarInterface();
    }

    function salvar() {
      localStorage.setItem('acessorios_hub_cart', JSON.stringify(carrinho));
    }

    function atualizarInterface() {
      document.getElementById('cart-count').innerText = carrinho.length;
      const lista = document.getElementById('cart-items-list');
      
      if (carrinho.length === 0) {
        lista.innerHTML = '<p style="text-align:center; color:#888">Seu carrinho está vazio.</p>';
      } else {
        lista.innerHTML = carrinho.map((item, index) => `
          <div class="carrinho-item">
            <span>${item.nome}</span>
            <div style="display:flex; align-items:center; gap:10px">
              <span>R$ ${item.preco.toFixed(2)}</span>
              <button class="btn-remove" onclick="remover(${index})">×</button>
            </div>
          </div>
        `).join('');
      }

      const total = carrinho.reduce((acc, i) => acc + i.preco, 0);
      document.getElementById('total-view').innerText = `R$ ${total.toFixed(2)}`;
    }

    function toggleModal(show) {
      document.getElementById('cart-modal').style.display = show ? 'flex' : 'none';
    }

    function enviarWhatsApp() {
      const nome = document.getElementById('cliente-nome').value;
      const endereco = document.getElementById('cliente-endereco').value;

      if (carrinho.length === 0) return alert("Seu carrinho está vazio!");
      if (!nome || !endereco) return alert("Preencha seu nome e endereço para entrega!");

      const total = carrinho.reduce((acc, i) => acc + i.preco, 0);
      
      let mensagem = `*📦 NOVO PEDIDO - ACESSÓRIOS HUB*\n\n`;
      mensagem += `👤 *Cliente:* ${nome}\n`;
      mensagem += `📍 *Entrega:* ${endereco}\n`;
      mensagem += `\n--------------------------\n`;
      
      carrinho.forEach(item => {
        mensagem += `• ${item.nome} (R$ ${item.preco.toFixed(2)})\n`;
      });
      
      mensagem += `--------------------------\n`;
      mensagem += `💰 *TOTAL: R$ ${total.toFixed(2)}*`;

      const url = `https://wa.me/${MEU_NUMERO}?text=${encodeURIComponent(mensagem)}`;
      window.open(url, '_blank');
    }

    renderizarVitrine();
    atualizarInterface();
  </script>
</body>
</html>
