# Documentação Noxss v1.1

Bem-vindo à documentação oficial da Noxss, uma biblioteca de componentes e utilitários CSS/JS de código aberto, projetada com uma filosofia "App-First" para acelerar o desenvolvimento de interfaces de aplicativos web modernos, responsivos e temáticos.

---

## 1. Começando (Getting Started)

Esta seção guiará você pelos passos iniciais para instalar e configurar a Noxss em seu projeto.

### 1.1. Introdução

A Noxss não é apenas uma coleção de estilos, mas um pequeno framework de UI focado em resolver problemas comuns no desenvolvimento de aplicativos, como:

*   **Tematização Avançada:** Mude a aparência inteira do seu aplicativo com uma única linha de HTML, ou gere temas completos e dinâmicos a partir de uma única cor.
*   **Layout de App:** Uma estrutura de layout pronta para criar interfaces de tela cheia com navbars e conteúdo rolável.
*   **Componentes Reutilizáveis:** De botões e formulários a modais e toasts, a Noxss oferece componentes prontos e consistentes.
*   **Classes Utilitárias:** Um sistema atômico para construir layouts e estilos customizados rapidamente, sem sair do seu HTML.

### 1.2. Instalação e Uso

A maneira mais fácil de começar é incluir os arquivos CSS e JS da Noxss diretamente no seu arquivo HTML.

#### Estrutura HTML Básica

Copie e cole este template inicial em seu arquivo `index.html`. Ele já inclui a estrutura recomendada e os links para os arquivos da biblioteca.

```html
<!doctype html>
<html lang="pt-br"> <!-- O tema Dark é o padrão -->
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Meu App com Noxss</title>

    <!-- 1. CSS da Noxss -->
    <link rel="stylesheet" href="path/to/noxss/dist/noxss.css" />

    <!-- 2. Dependências Opcionais (se for usar os componentes) -->
    <script defer src="https://unpkg.com/feather-icons"></script>
    
    <!-- 3. Scripts da Noxss (no final do body) -->
    <script src="path/to/noxss/dist/noxss.js"></script>
    <script src="path/to/noxss/js/components/palette.js"></script> <!-- Opcional: Para geração de temas -->
    
    <!-- 4. Script de Inicialização (se necessário) -->
    <script>
        document.addEventListener('DOMContentLoaded', () => {
            if (window.feather) {
                feather.replace({ class: 'noxss-icon' });
            }
        });
    </script>
</body>
</html>
```
> **Nota:** Lembre-se de substituir `path/to/` pelos caminhos corretos para os arquivos da Noxss.

### 1.3. Configuração Principal

A Noxss é controlada por atributos `data-*` simples na sua estrutura HTML.

#### Temas Estáticos (Light & Dark)

Para usar os temas predefinidos, adicione o atributo `data-theme` à sua tag `<html>`.

```html
<!-- Para usar o tema claro -->
<html lang="pt-br" data-theme="light"></html>

<!-- Para usar o tema escuro (padrão) -->
<html lang="pt-br" data-theme="dark"></html>
```

#### Layout de Aplicativo

Para criar uma interface de aplicativo de tela cheia, adicione `data-noxss-layout="app"` à sua tag `<body>`.

```html
<body data-noxss-layout="app">
    <div class="noxss-layout">
        <!-- ... -->
    </div>
</body>
```

---

## 2. Core e Utilitários

Esta seção aborda os conceitos centrais, convenções e as novas ferramentas de tematização e utilitários da Noxss.

### 2.1. Variáveis (Design Tokens)

A Noxss é construída sobre um sistema de variáveis CSS (`custom properties`). Isso permite a customização da aparência da Noxss ou a criação de seus próprios temas estáticos sobrescrevendo essas variáveis.

| Variável                     | Propósito                                       |
| ---------------------------- | ----------------------------------------------- |
| `--noxss-accent-primary`     | A cor principal de destaque da sua marca.       |
| `--noxss-bg-main`            | A cor de fundo principal da página/app.         |
| `--noxss-text-primary`       | A cor de texto principal.                       |
| `--noxss-border-color`       | A cor padrão para bordas e divisores.           |

### 2.2. Ícones

A Noxss é agnóstica a bibliotecas de ícones. Para garantir consistência, use a classe `.noxss-icon` em seu elemento de ícone (`<i>` ou `<svg>`).

```html
<button class="noxss-btn">
    <i data-feather="home" class="noxss-icon"></i>
    <span>Início</span>
</button>
```

### 2.3. Gerador de Tema Dinâmico (NOVO)

Este é um dos recursos mais poderosos da Noxss. Inspirado no Material You, ele permite gerar uma paleta de cores completa e acessível (seja clara ou escura) a partir de uma única cor de destaque. Isso é feito de forma declarativa, diretamente no seu HTML.

#### Como Usar

Para ativar o gerador, adicione dois atributos ao seu elemento `<html>`:

1.  `data-noxss-palette-gen`: Define a cor de destaque em formato hexadecimal.
2.  `data-noxss-theme-gen`: Define o modo do tema (`dark` ou `light`).

```html
<!doctype html>
<!-- Gera um tema escuro baseado na cor roxa -->
<html lang="pt-br" data-noxss-theme-gen="dark" data-noxss-palette-gen="#8a2be2">

<!-- Gera um tema claro baseado na cor verde -->
<html lang="pt-br" data-noxss-theme-gen="light" data-noxss-palette-gen="#198754">
```

#### Atributos

| Atributo                  | Valores Aceitos                 | Propósito                                                                      |
| ------------------------- | ------------------------------- | ------------------------------------------------------------------------------ |
| `data-noxss-palette-gen`  | Cor hexadecimal (`#RRGGBB`)     | A cor principal que servirá de base para gerar toda a paleta.                  |
| `data-noxss-theme-gen`    | `dark` \| `light`                 | Define o modo do tema. Se omitido ou inválido, o padrão será **`dark`**.      |

#### Como Funciona

Ao detectar esses atributos, o script `palette.js` executa um algoritmo que:
1.  Converte a cor de destaque para o formato HSL (Matiz, Saturação, Luminosidade).
2.  Usa o **Matiz (Hue)** como a "alma" da paleta, garantindo harmonia cromática.
3.  Gera cores de fundo, texto e bordas com valores de saturação e luminosidade apropriados para o modo (claro ou escuro) escolhido.
4.  **Ajusta o contraste automaticamente:** Se você escolher uma cor de destaque muito clara para um tema claro (ex: amarelo), o script a escurecerá para garantir a legibilidade.
5.  Injeta as novas variáveis CSS em uma tag `<style>` no `<head>` e ativa o tema com `data-theme="generated"`.

---

*(Seções 3, 4, 5 e 6 da documentação original permanecem aqui)*

---

## 7. Classes Utilitárias (NOVO)

A Noxss agora inclui um sistema de classes utilitárias (ou atômicas) para acelerar o desenvolvimento, permitindo aplicar estilos granulares diretamente no HTML. Isso é ideal para prototipagem rápida, ajustes finos e construção de layouts customizados sem escrever CSS adicional.

### 7.1. Estrutura e Nomenclatura

*   **Prefixo:** Todas as classes utilitárias começam com `u-` para evitar conflitos.
*   **Separador:** Usa-se hífen (`-`) para separar a propriedade do valor (ex: `u-d-flex`).
*   **Responsividade:** As classes podem ser prefixadas com breakpoints (`md:`, `lg:`) para serem aplicadas apenas a partir daquele tamanho de tela.

**Estrutura:** `[breakpoint\:]u-[propriedade]-[valor]`
*   **Exemplo 1:** `u-p-3` (aplica `padding` da escala 3 em todos os tamanhos).
*   **Exemplo 2:** `md\:u-text-center` (centraliza o texto em telas médias e maiores).

### 7.2. Espaçamento (Margin & Padding)

Usa uma escala de `0` a `5` baseada na variável `--noxss-spacing-base`.

| Classe     | Propriedade(s) CSS                    |
| ---------- | ------------------------------------- |
| `u-m-{n}`  | `margin`                              |
| `u-mt-{n}` | `margin-top`                          |
| `u-mb-{n}` | `margin-bottom`                       |
| `u-ms-{n}` | `margin-left` (start)                 |
| `u-me-{n}` | `margin-right` (end)                  |
| `u-mx-{n}` | `margin-left` & `margin-right`        |
| `u-my-{n}` | `margin-top` & `margin-bottom`        |
| `u-p-{n}`  | `padding`                             |
| `u-pt-{n}` | `padding-top`                         |
| `u-pb-{n}` | `padding-bottom`                      |
| `u-ps-{n}` | `padding-left` (start)                |
| `u-pe-{n}` | `padding-right` (end)                 |
| `u-px-{n}` | `padding-left` & `padding-right`      |
| `u-py-{n}` | `padding-top` & `padding-bottom`      |

### 7.3. Layout (Display & Flexbox)

| Classe                  | Propriedade CSS                                  |
| ----------------------- | ------------------------------------------------ |
| `u-d-block`             | `display: block`                                 |
| `u-d-flex`              | `display: flex`                                  |
| `u-d-none`              | `display: none`                                  |
| `u-flex-row`            | `flex-direction: row`                            |
| `u-flex-col`            | `flex-direction: column`                         |
| `u-justify-start`       | `justify-content: flex-start`                    |
| `u-justify-center`      | `justify-content: center`                        |
| `u-justify-between`     | `justify-content: space-between`                 |
| `u-align-center`        | `align-items: center`                            |
| `u-align-stretch`       | `align-items: stretch`                           |
| `u-gap-{n}`             | `gap` (com a mesma escala de espaçamento)        |

### 7.4. Tipografia e Cores

| Classe                  | Propriedade CSS                                  |
| ----------------------- | ------------------------------------------------ |
| `u-text-center`         | `text-align: center`                             |
| `u-font-bold`           | `font-weight: 700`                               |
| `u-font-semibold`       | `font-weight: 600`                               |
| `u-text-primary`        | `color: var(--noxss-text-primary)`               |
| `u-text-secondary`      | `color: var(--noxss-text-secondary)`             |
| `u-text-accent`         | `color: var(--noxss-accent-primary)`             |

### 7.5. Design Responsivo

A Noxss usa uma abordagem **mobile-first**. As classes sem prefixo se aplicam a todos os tamanhos de tela. Para aplicar um estilo apenas a partir de um certo breakpoint, adicione o prefixo correspondente.

| Prefixo   | Breakpoint Mínimo | Exemplo de Classe    | Significado                                            |
| --------- | ----------------- | -------------------- | ------------------------------------------------------ |
| (nenhum)  | `0px`             | `u-flex-col`         | A direção será `column` em todas as telas.             |
| `md:`     | `768px`           | `md\:u-flex-row`     | A direção será `row` em telas de 768px ou mais.        |
| `lg:`     | `992px`           | `lg\:u-p-5`          | O `padding` será da escala 5 em telas grandes e maiores. |

**Exemplo Prático:**

```html
<!--
  - Começa como um flexbox em coluna.
  - Em telas médias (md) e maiores, torna-se uma linha.
  - O espaçamento (gap) aumenta em telas grandes (lg).
-->
<div class="u-d-flex u-flex-col md:u-flex-row u-gap-2 lg:u-gap-5">
    <div>Item 1</div>
    <div>Item 2</div>
</div>
```
```