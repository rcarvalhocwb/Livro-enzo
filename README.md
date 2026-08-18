# Enzo e a Missão de Cura

Livro interativo em HTML5 que explica ao Enzo — e à família — o que aconteceu no corpo dele:
a infecção por *Staphylococcus aureus*, a osteomielite, os exames, as cirurgias e a recuperação.

São 23 páginas em português, com virada de página em 3D, narração em voz alta, mapa corporal
interativo, gráfico da inflamação, quiz, checklist e um espaço pessoal para escrever.

Material educativo para o paciente e a família, inspirado no acolhimento do Hospital Pequeno
Príncipe. **Não é um documento oficial do hospital** e não substitui a orientação da equipe.

---

## Como abrir

**Arquivo único.** Não precisa instalar nada, nem servidor, nem build.

- **No computador:** dê dois cliques em `index.html`.
- **Mandar para alguém:** envie o `index.html` — ele funciona sozinho, inclusive offline.
- **Publicar na web:** _Settings → Pages → Source: Deploy from a branch → `main` / `root`_.
  O livro fica em `https://<usuário>.github.io/Livro-enzo/`.

> Abrindo por `file://` (dois cliques), a cena 3D de fundo não carrega, porque o navegador
> bloqueia buscar bibliotecas externas de um arquivo local. Todo o resto funciona igual.
> Para ver o 3D, publique no GitHub Pages ou rode `python3 -m http.server` na pasta.

## O que dá para fazer

| | |
|---|---|
| **Virar a página** | arraste o canto, deslize o dedo, use `←` `→` `PageUp` `PageDown` `Home` `End`, ou toque nas setas |
| **Ouvir** | botão **Ouvir** lê a página em voz alta e vai destacando a frase |
| **Favoritos** | botão **Favorito** (ou tecla `F`); as favoritas aparecem com ♥ no índice |
| **Modo leitor** | texto grande, uma coluna, sem efeitos — bom para ler cansado |
| **Tema** | segue o claro/escuro do aparelho; o botão da lua fixa a sua escolha |
| **Zoom e tela cheia** | na barra de baixo |
| **Imprimir** | gera o livro inteiro, uma página por folha |

Página atual, favoritos, quiz, checklist e tudo o que for escrito em **Meu espaço** ficam
guardados **só no navegador do aparelho**. Nada é enviado para lugar nenhum — não há servidor,
conta ou rastreamento. Limpar os dados do navegador apaga tudo.

---

## Editar o conteúdo clínico

Os dados ficam todos juntos no topo do bloco de módulos do `index.html`, marcados com
`DADOS CLÍNICOS`. Procure por `var SITES` para achar a seção.

### `SITES` — as marcas do mapa corporal (página 5)

```js
{ id:'escapula', view:'posterior', side:'direita', cat:'osseo', cir:true, x:76, y:52,
  label:'Escápula direita',
  txt:'Foco de infecção confirmado no osso...' }
```

| campo | o que significa |
|---|---|
| `view` | `'frontal'` ou `'posterior'` — em qual desenho a marca aparece |
| `side` | lado **do Enzo**: `'direita'` ou `'esquerda'` |
| `cat` | `'osseo'` foco no osso confirmado · `'artic'` articulação/tendão · `'acomp'` **área apenas acompanhada** |
| `cir` | `true` se o local foi abordado cirurgicamente (desenha o anel branco) |
| `x`,`y` | posição no desenho, no sistema `viewBox="0 0 120 200"` |

**Sobre lateralidade** — é o ponto mais fácil de errar. Na vista **frontal** o lado direito do
Enzo aparece à **esquerda** da figura; na vista **posterior**, à **direita**. O código não
converte isso sozinho: `x` é a posição na tela e `side` é o texto lido em voz alta, então os
dois precisam combinar. Referências úteis no desenho: eixo do corpo em `x=60`, ombros em
`y≈52`, mãos em `y≈112`, tornozelos em `y≈180`, pés em `y≈191`.

A distinção entre as quatro categorias é proposital: **área acompanhada não é diagnóstico
fechado**, e o mapa mostra isso por cor, por forma do marcador e por texto — nunca só por cor.

> ⚠️ Confira a tabela contra o prontuário antes de entregar o livro. Os valores que estão lá
> vieram dos mockups, não de um registro clínico.

### `PCR` — o gráfico da página 17

O gráfico é desenhado a partir da **tabela HTML** da própria página 17 (`id="pcr-table"`),
para não existir o mesmo dado em dois lugares. Edite as linhas da tabela e o gráfico acompanha.

> ⚠️ Os valores que estão lá são **exemplo**. Troque pelos exames reais e ajuste o texto da
> legenda da tabela.

### `QUIZ` — as cinco perguntas da página 18

Cada item tem a pergunta, as alternativas, o índice da certa (`ok`, começando em zero) e o
texto de retorno. O quiz é acolhedor de propósito: nunca diz "errado", diz "Quase! Olha só".

---

## Trocar as ilustrações

Alguns lugares têm um espaço reservado para arte, marcados assim no HTML:

```html
<div class="art" data-art="capa">
  <div class="art-ph">…</div>
</div>
```

Para usar uma imagem sua, coloque o arquivo em `assets/` e adicione a `<img>` dentro do `div`:

```html
<div class="art has-img" data-art="capa">
  <img src="assets/enzo-capa.png" alt="Enzo com o escudo e a cápsula-herói">
  <div class="art-ph">…</div>
</div>
```

A classe `has-img` esconde o texto de espera. Os nomes sugeridos estão em `assets/README.md`.

**Por que existem esses espaços:** os personagens não humanos — a bactéria, a cápsula-herói, o
escudo, o osso e as hemácias — são **3D de verdade**, gerados em tempo real com geometria e
materiais PBR, e aparecem flutuando ao redor do livro. Já o Enzo e as equipes médicas são
pessoas: não dá para gerar isso proceduralmente com aparência realista, então ficam como
espaço para você colocar a arte pronta.

---

## Como foi construído

Sem framework, sem build, sem dependência instalada. Uma única página, em camadas que caem
umas para as outras conforme o que o aparelho suporta:

```
4. WebGPU              cena 3D com materiais PBR
3. WebGL 2             mesma cena (o Three.js cai sozinho)
2. CSS 3D              livro, virada de página, profundidade
1. HTML + CSS puro     documento rolável com todo o texto
```

O Three.js (0.185.1) vem de CDN por `importmap`, com import dinâmico dentro de `try/catch`.
Se o wi-fi bloquear a CDN — o que acontece bastante em rede de hospital — o livro continua
inteiro, só sem a cena de fundo. Nenhuma informação existe apenas no 3D.

**Acessibilidade:** cada página é um `<article>` com títulos em ordem, navegação completa por
teclado, foco visível, contraste AA nos dois temas, alvos de toque de 44px, `aria-live` no
número da página e `prefers-reduced-motion` desligando animações. O canvas 3D é `aria-hidden`.

### Uma limitação que vale registrar

A ideia inicial era virar a folha como uma **malha curvada em WebGL**, texturizada com um
retrato da própria página. Isso não é possível hoje: os navegadores tratam todo SVG que
contenha `<foreignObject>` como dado de origem cruzada, então o retrato da página não pode
virar textura (`SecurityError` no `texImage2D`) nem ser lido de volta de um canvas. A única
saída seria trocar as páginas por imagens prontas — o que acabaria com o texto vivo de que a
narração, o quiz, o checklist e os campos do "Meu espaço" dependem.

Por isso a virada é feita em CSS 3D, com estufamento, brilho de dobra e sombra que acompanha o
movimento; e o WebGL cuida da cena ao redor, onde ele rende de verdade.

### Endereços úteis para teste

| | |
|---|---|
| `?no3d=1` | desliga a camada 3D |
| `?renderer=webgl` | força WebGL 2 em vez de WebGPU |

## Navegadores

Chrome, Edge, Firefox e Safari atuais, no computador e no celular. Em navegadores antigos o
livro cai para o documento rolável, com todo o texto legível.
