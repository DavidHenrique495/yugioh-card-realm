# ⚔️ Yu-Gi-Oh! Card Realm

Uma página web que consome a API pública de Yu-Gi-Oh!, com animações temáticas do anime, interface responsiva e estilizada.

## 🔗 Demo

Abra o arquivo `yugioh.html` diretamente no navegador — sem servidor necessário.

---

## ✨ Funcionalidades

- **Tela de intro animada** com o Olho do Milênio, anéis de energia e estrelas
- **Grid responsivo** de cartas com animação de entrada
- **Busca em tempo real** por nome (debounce de 450ms)
- **Filtros por tipo**: Monstro, Magia, Armadilha
- **Modal de detalhes**: imagem, ATK/DEF, nível, atributo, arquétipo, texto do efeito e preço
- **Paginação** com "Carregar Mais" (24 cartas por vez)
- **Contadores globais** de cartas por categoria

---

## 🛠️ Decisões Técnicas

### Stack escolhida
| Tecnologia | Motivo |
|---|---|
| HTML + JS puro | Sem build step — abre direto no browser |
| Tailwind CSS (CDN) | Utilitários rápidos sem configuração local |
| Google Fonts (Cinzel + Rajdhani) | Cinzel dá o peso dramático dos títulos; Rajdhani mantém legibilidade no corpo |

### API: YGOProDeck
- **Endpoint base**: `https://db.ygoprodeck.com/api/v7/cardinfo.php`
- **Por que essa API**: gratuita, sem autenticação, com CORS liberado para requisições client-side
- **Parâmetros usados**:
  - `num` + `offset` — paginação
  - `fname` — busca parcial por nome
  - `type` — filtro por categoria
  - `sort=name` — ordem alfabética consistente

### Busca com debounce
A função `onSearch()` usa `setTimeout` de 450ms para evitar uma requisição a cada tecla digitada, reduzindo a carga na API e melhorando a experiência.

### Paginação client-side
A variável `offset` acumula quantas cartas já foram carregadas. A cada "Carregar Mais", o offset avança em 24. Em buscas e troca de filtro, o offset é zerado e o grid é reconstruído.

### Skeletons de carregamento
Em vez de uma tela em branco, 12 divs animadas simulam os cards enquanto a API responde, dando feedback visual imediato.

---

## ⚠️ Limitações Conhecidas

| Limitação | Detalhe |
|---|---|
| **CORS dependente da API** | Se a YGOProDeck tirar o CORS, o site para de funcionar sem um proxy |
| **Sem cache local** | Cada visita refaz as requisições; não há localStorage ou Service Worker |
| **Filtro + busca simultâneos** | A API não combina `fname` com `type` de forma 100% previsível em todos os casos |
| **Imagens de terceiros** | As URLs das imagens das cartas vêm da própria API — se o servidor deles cair, as imagens somem |
| **Sem autenticação** | A API é pública com rate limit não documentado; uso intensivo pode gerar bloqueio temporário de IP |
| **Arquivo único** | Todo o CSS, HTML e JS estão no mesmo arquivo — prático para demo, mas não escala para projetos maiores |

---

## 📁 Estrutura

```
yugioh-card-realm/
└── yugioh.html   # Aplicação completa (HTML + CSS + JS)
└── README.md     # Este arquivo
```

---

## 🚀 Como usar

```bash
# Clone o repositório
git clone https://github.com/seu-usuario/yugioh-card-realm.git

# Abra no navegador
open yugioh.html
# ou simplesmente arraste o arquivo para o Chrome/Firefox
```

---

## 📡 Referência da API

Documentação completa: [ygoprodeck.com/api-guide](https://ygoprodeck.com/api-guide/)

```
GET https://db.ygoprodeck.com/api/v7/cardinfo.php
  ?num=24          # cartas por página
  &offset=0        # paginação
  &fname=dark      # busca por nome (parcial)
  &type=Spell+Card # filtro por tipo
  &sort=name       # ordenação
```

---

## 🎨 Créditos

- Dados e imagens: [YGOProDeck API](https://db.ygoprodeck.com)
- Fontes: [Google Fonts](https://fonts.google.com)
- CSS utilitário: [Tailwind CSS](https://tailwindcss.com)
