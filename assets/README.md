# Artes do livro

Coloque aqui os PNGs das ilustrações 3D. O livro funciona inteiro sem eles — cada espaço
mostra um texto de espera até a imagem chegar.

## Como usar uma imagem

Ache no `index.html` o espaço reservado (procure por `data-art=`):

```html
<div class="art" data-art="capa">
  <div class="art-ph">…</div>
</div>
```

Adicione a `<img>` e a classe `has-img`, que esconde o texto de espera:

```html
<div class="art has-img" data-art="capa">
  <img src="assets/enzo-capa.png" alt="Enzo com o escudo e a cápsula-herói">
  <div class="art-ph">…</div>
</div>
```

Escreva sempre o `alt` descrevendo a cena — é o que o leitor de tela vai falar.

## Nomes esperados

| arquivo | onde entra | proporção |
|---|---|---|
| `enzo-capa.png` | capa | 2,1 : 1 |
| `enzo-leitura.png` | página 1 — Olá, Enzo! | 1,5 : 1 |
| `osso.png` | página 6 — osteomielite | 1,1 : 1 |
| `capsula.png` | página 11 — antibióticos | 1,8 : 1 |
| `hemacia.png` | página 13 — anemia | 1 : 1 |

## Dicas

- **PNG com fundo transparente** cai melhor sobre o papel do livro, que muda de cor entre o
  tema claro e o escuro.
- Cerca de **1600px no lado maior** já é suficiente, inclusive em tela retina.
- Passe por um compressor (TinyPNG, Squoosh) antes de commitar: o livro é feito para abrir
  rápido no celular, às vezes no wi-fi do hospital.
- Os personagens **não humanos** — bactéria, cápsula-herói, escudo, osso e hemácias — já são
  3D de verdade, gerados em tempo real e flutuando ao redor do livro. Estes espaços existem
  principalmente para o **Enzo e as equipes médicas**, que precisam de arte pronta.
