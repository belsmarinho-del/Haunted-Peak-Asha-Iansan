# Análise dos gifs — Lovie Simone (visão computacional)

Método: detecção facial com OpenCV 4.11 (Haar cascades — `frontalface_default`, `frontalface_alt2`, `profileface`), sobre 12 quadros de cada gif, com upscale 2,5×. Sobre a região de cabelo (faixa acima e laterais do rosto detectado), duas métricas:

- **Coerência** — concentração do histograma de orientação dos gradientes, janelado em 15°. Tranças produzem feixes de gradientes paralelos; cabelo crespo/solto é mais isotrópico. Quanto maior, mais orientação dominante.
- **Periodicidade** — pico do espectro radial (FFT) na faixa de 6 a 34 px, dividido pela energia média da faixa. Tranças regulares geram pico espectral; textura difusa não.
- **Energia** — magnitude média dos gradientes: indica região texturizada (cabelo detalhado) vs. lisa/fundo.

## Resultados

| Opção | Rosto detectado | Tipo | Coerência | Periodicidade | Energia |
|---|---|---|---|---|---|
| F1 | 14,3% | frontal | 2,54 | 5,92 | 37,3 |
| F2 | 17,9% | perfil | 3,64 | 5,56 | 53,5 |
| F3 | 10,1% | perfil | 2,82 | 8,18 | 32,0 |
| F4 | 13,9% | frontal | 1,49 | 7,75 | 10,0 |
| F5 | 14,8% | frontal | 2,44 | 5,14 | 5,2 |
| L1 | 18,6% | frontal | 4,63 | 6,35 | 27,9 |
| L2 | — | **sem rosto detectado** | — | — | — |
| L3 | 18,4% | alt2 | 1,97 | 10,96 | 7,3 |
| L4 | 23,3% | frontal | 2,40 | 8,89 | 5,9 |
| L5 | 35,8% | perfil | 2,73 | 10,07 | 7,4 |
| L6 | 21,1% | frontal | 2,28 | 7,41 | 7,3 |
| **L7** | **45,2%** | perfil | **6,45** | 6,72 | **37,8** |
| L8 | 36,8% | frontal | 3,30 | 7,87 | 28,7 |
| L9 | 18,2% | alt2 | 4,00 | 10,46 | 38,1 |
| L10 | 18,3% | frontal | 2,93 | 7,30 | 3,5 |

## Ranking (escore combinado, z-normalizado)

1. **L7** — +1,15 (maior rosto do conjunto: 45% do quadro; maior coerência de todas)
2. **L9** — +1,10
3. L1 — +0,30
4. L5 — +0,25
5. L3 — +0,17

## Ressalvas

- A região "cabelo" inclui fundo; roupa listrada ou cenário com linhas paralelas pode inflar a coerência.
- Haar cascades erram mais em perfil e em rostos negros com pouca luz — L2 não retornou detecção, o que **não** significa ausência de rosto.
- É uma heurística de textura, não uma identificação de penteado.

Folha com os recortes de rosto: `gifs/analise-rostos.png`.
