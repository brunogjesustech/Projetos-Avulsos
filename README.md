# 📻 SilenceFM

Um rádio interativo feito com Arduino Uno R3, display OLED 0,96" e buzzer passivo — criado como presente de Dia dos Namorados

Ao girar o potenciômetro, você sintoniza uma das 6 estações. Cada uma exibe uma mensagem, toca uma melodia 8bit e em seguida exibe uma animação temática no display.

---

## ✨ Funcionalidades

- **6 estações de rádio** selecionáveis via potenciômetro
- **Intro animada** com jingle 8bit, tela de dedicatória e bitmap da Frieren
- **Barra de progresso** que conta os 5 segundos antes da animação
- **Melodia temática** ao entrar em cada estação
- **Animações pixel art** feitas sem float, rodando a ~28fps
- **Mensagem especial** com slides de texto + imagens da Maomao

---

## 📻 As Estações

| Frequência | Tema       | Mensagem                                   | Melodia             | Animação              |
|------------|------------|--------------------------------------------|---------------------|-----------------------|
| 88.1 FM    | Estrelas   | "As primeiras estrelas aparecem antes da noite." | Zelda's Lullaby     | 20 estrelas piscando com brilho em cruz |
| 92.5 FM    | Coração    | "Algumas pessoas acalmam o mundo sem perceber." | Pokémon Center     | Coração batendo com onda de pulso |
| 96.7 FM    | Carro      | "Toda jornada merece um momento pra respirar." | Super Mario Bros   | Carro atravessando a tela com estrada animada |
| 101.3 FM   | Chuva      | "Depois da chuva, até o silêncio parece bonito." | Song of Storms    | Nuvem com chuva e poça ondulando |
| 104.6 FM   | Casinha    | "Alguns lugares parecem casa. Algumas pessoas também." | Tetris        | Casinha com janelas acendendo, fumaça e lua crescente |
| 107.9 FM   | Especial ♥ | Slides românticos + imagens da Maomao  / killjoy    | Undertale: His Theme | Slides de texto com borda animada + bitmaps |

---

## 🔧 Hardware

| Componente              | Conexão              |
|-------------------------|----------------------|
| Arduino Uno  R3         | —                    |
| OLED SSD1306 128×64     | SDA → A4, SCL → A5  |
| Potenciômetro           | Pino analógico A0    |
| Buzzer passivo          | Pino digital 8, GND  |

### Esquema de conexão do buzzer na protoboard

```
Buzzer pino + (maior)  →  linha da protoboard  →  Pino 8 do Arduino
Buzzer pino - (menor)  →  linha da protoboard  →  GND do Arduino
```

---

## 📦 Bibliotecas necessárias

Instale pelo **Gerenciador de Bibliotecas** do Arduino IDE:

- `Adafruit GFX Library`
- `Adafruit SSD1306`

A biblioteca `Wire.h` já vem com o Arduino IDE.

---

## 🚀 Como usar

1. Clone ou baixe este repositório
2. Abra `SilenceFM.ino` no Arduino IDE
3. Instale as bibliotecas listadas acima
4. Monte o hardware conforme a tabela de conexões
5. Faça o upload para o Arduino Uno
6. Gire o potenciômetro para sintonizar as estações

> **Atenção:** coloque apenas o arquivo `SilenceFM.ino` na pasta do projeto. O Arduino IDE compila todos os `.ino` da mesma pasta juntos, o que causaria erros de redefinição se houver outros arquivos.

---

## 🗂️ Estrutura do código

```
SilenceFM.ino
│
├── Bitmaps (PROGMEM)
│   ├── frieren__        — exibida na intro
│   ├── maomaoMaomao     — imagem principal da mensagem especial
│   └── mamao2baixados   — segunda imagem da mensagem especial
│
├── Utilitários
│   ├── textoCentralizado()  — centraliza texto em qualquer textSize
│   └── beep()               — gera tom no buzzer via digitalWrite
│
├── Intro
│   ├── tocarJingleIntro()   — jingle 8bit de abertura
│   └── introRadio()         — sequência completa de intro
│
├── Melodias por estação
│   ├── somEstrelas()    — Zelda's Lullaby
│   ├── somCoracao()     — Pokémon Center
│   ├── somCarro()       — Super Mario Bros
│   ├── somChuva()       — Song of Storms
│   ├── somCasinha()     — Tetris
│   └── somEspecial()    — Undertale: His Theme
│
├── Animações por estação
│   ├── animacaoEstrelas()
│   ├── animacaoCoracao()
│   ├── animacaoCarro()
│   ├── animacaoChuva()
│   ├── animacaoCasinha()
│   └── mensagemFinal()
│
└── setup() / loop()
```

---

## 🎨 Detalhes técnicos

- **Bitmaps** gerados via [image2cpp](https://javl.github.io/image2cpp/) em formato horizontal 1bpp e armazenados em `PROGMEM` para não consumir RAM
- **Buzzer** controlado com `digitalWrite` + `delayMicroseconds` em vez de `tone()`, evitando conflito com o Timer2 usado pelo I2C do display
- **Animações** usam apenas aritmética inteira (sem `float`) para rodar suavemente no Uno
- **Taxa de atualização** de ~28fps (`delay(35)` no loop)
- **Fluxo de cada estação:** 5s de texto + melodia → animação em loop até trocar de estação

---

## 📁 Imagens incluídas

As imagens foram convertidas para arrays C via image2cpp e estão embutidas diretamente no `.ino`. Não é necessário nenhum arquivo externo.

---
