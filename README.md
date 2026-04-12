# Traços

> Jogo de palavras diário em português com display de sete segmentos

**[tracos.net](https://tracos.net)** — Um novo desafio todo dia. Adivinhe a palavra do dia em até 4 tentativas.

---

## O que é

Traços é um jogo de palavras no estilo Wordle, mas com uma diferença visual marcante: as letras são exibidas como **displays de sete segmentos** — aquela estética de relógio digital ou calculadora LED. A ideia nasceu da observação de que certos segmentos iluminados podem representar letras ambíguas, tornando a leitura um desafio adicional sutil.

O jogo é inteiramente em **português brasileiro**, com um dicionário de 891 palavras comuns rotativas e um banco de validação de mais de 133 mil palavras.

---

## Modos de jogo

### Diário
Uma palavra por dia, igual para todos os jogadores. A palavra tem 5, 6 ou 7 letras — o tamanho varia deterministicamente ao longo dos dias. Você tem **4 tentativas**. O ciclo completo dura 891 dias (~2,4 anos) antes de repetir qualquer palavra.

### Infinito
Palavras sem fim, em sequência. Ideal para praticar. Cada sequência tem **5 dicas** e **1 pulo** disponíveis. A pontuação é medida pela maior sequência de acertos consecutivos.

---

## Como o feedback funciona

Cada letra digitada é exibida como um display de sete segmentos com cor indicando o resultado:

| Cor | Significado |
|-----|-------------|
| Verde (brilho forte) | Letra correta na posição correta |
| Amarelo/dourado | Letra está na palavra, mas na posição errada |
| Cinza apagado | Letra não está na palavra |

O teclado virtual na tela reflete o estado acumulado de todas as letras já tentadas.

---

## Modo difícil

Quando ativado, impõe restrições adicionais:
- Letras confirmadas em verde devem permanecer na mesma posição nas tentativas seguintes
- Não é possível reutilizar uma letra numa posição onde já foi descartada

O modo difícil pode ser ativado separadamente para o modo Diário e para o Infinito.

---

## Tecnologia

Construído inteiramente com HTML, CSS e JavaScript puro — sem frameworks, sem bundlers, sem dependências de tempo de execução. Um único arquivo `index.html` contém toda a lógica do jogo, interface e estilos.

**Dependências externas (CDN):**
- [`html2canvas`](https://html2canvas.hertzen.com/) v1.4.1 — para gerar imagem de compartilhamento de resultado
- Google Fonts: DM Serif Display, Orbitron, Outfit

**Dados:**
- `dicionario.js` — carregado separadamente; contém `COMUM` (891 palavras do pool diário) e `DICIONARIO` (133.752 palavras para validação)

**Armazenamento:**
- Todo o estado do jogo é salvo em `localStorage` sob a chave `tracos_v2`
- Nenhum dado é enviado a servidores — o jogo funciona offline após o carregamento inicial

---

## Estrutura do projeto

```
Traços.net/
├── index.html          # Aplicação completa (HTML + CSS + JS)
├── dicionario.js       # Banco de palavras (COMUM + DICIONARIO)
├── tracos-url-logo.svg # Logotipo / favicon
├── apple-touch-icon.png
└── apple-touch-icon-dark.png
```

---

## Rodar localmente

Nenhuma etapa de build necessária. Sirva os arquivos com qualquer servidor HTTP estático:

```bash
# Python (qualquer versão moderna)
python3 -m http.server 8080

# Node.js (com npx)
npx serve .

# PHP
php -S localhost:8080
```

Abra `http://localhost:8080` no navegador.

> **Atenção:** o jogo requer um servidor HTTP — não abre diretamente como `file://` por restrições de módulos e fetch.

---

## Como a palavra do dia é determinada

A seleção é **determinística e sem servidor**:

1. O array `COMUM` (891 palavras) é embaralhado com um seed fixo por ciclo de 891 dias
2. O índice do dia atual (relativo a 17 de março de 2026, fuso BRT UTC-3) determina qual palavra usar
3. Todo usuário com o mesmo índice de dia recebe a mesma palavra — sem comunicação com backend

O sistema inclui detecção de manipulação de horário no dispositivo para mitigar trapaças.

---

## Seleção de tamanho de palavra

A sequência de tamanhos (5, 6 ou 7 letras) ao longo dos 891 dias é gerada deterministicamente, distribuída de forma equilibrada. O tamanho do dia é visível antes de começar a jogar.

---

## Compartilhamento de resultado

Ao terminar uma partida, o botão de compartilhar gera uma **imagem** (via `html2canvas`) com o grid de tentativas no estilo do display de sete segmentos, preservando a estética visual do jogo. Alternativa: cópia de texto com emoji no padrão Wordle.

---

## Recursos adicionais

- **Calendário** — acesse e replique partidas de dias anteriores
- **Estatísticas** — taxa de vitória, distribuição de tentativas, sequências; separadas por modo Diário e Infinito
- **Alfabeto** — modal mostrando como cada letra é renderizada no display de sete segmentos
- **Tutorial** — introdução interativa ao Modo Difícil e ao Modo Infinito
- **Tema escuro/claro** — segue a preferência do sistema operacional via `prefers-color-scheme`
- **Responsivo** — otimizado para desktop e mobile

---

## Deploy

O site é hospedado via **GitHub Pages** com domínio customizado `tracos.net`. Qualquer push para a branch principal reflete diretamente em produção — não há pipeline de build.

---

## Dicionário

O arquivo `dicionario.js` não está no `.gitignore` e faz parte do repositório. Ele expõe duas constantes globais:

```js
const COMUM = [/* 891 palavras comuns, 5-7 letras */]
const DICIONARIO = [/* 133.752 palavras para validação */]
```

As palavras são armazenadas sem acentos (normalização NFD + remoção de diacríticos), todas em maiúsculas.

---

## Licença

Código disponível para fins de referência e aprendizado. Caso queira adaptar para outro idioma ou criar um fork, sinta-se à vontade — mas dê os créditos.

---

*Feito com cuidado por [guiselig](https://github.com/guiselig)*
