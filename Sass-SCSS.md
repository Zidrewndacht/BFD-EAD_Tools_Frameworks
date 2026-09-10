# Material Complementar — Sass e SCSS

Este material é indicado para quem já concluiu as partes de HTML, CSS e introdução ao Flask/JavaScript do curso. O objetivo é mostrar como usar Sass/SCSS para organizar melhor folhas de estilo em projetos web, sem transformar o assunto em algo maior do que ele precisa ser.

Sass não substitui CSS. Ele é uma ferramenta que gera CSS. O navegador continua lendo apenas o arquivo `.css` final.

---

## 1. O que é Sass?

Sass é uma linguagem de folha de estilo que estende CSS com recursos como variáveis, aninhamento de regras, arquivos parciais, mixins e funções. Depois, um compilador converte o código Sass para CSS comum.

Existem duas sintaxes principais:

| Sintaxe | Extensão | Característica |
|---|---|---|
| Sass | `.sass` | Usa indentação, sem chaves e sem ponto-e-vírgula |
| SCSS | `.scss` | Usa chaves e ponto-e-vírgula, mais próxima do CSS comum |

Neste curso, use **SCSS**.

Motivo: se você já sabe CSS, a sintaxe SCSS é mais fácil de aprender. Qualquer CSS válido também é SCSS válido.

Exemplo SCSS:

```scss
$cor-primaria: #0d9488;

.botao {
  background-color: $cor-primaria;
  color: white;
  padding: 8px 16px;
}
```

Depois de compilado, o navegador recebe algo assim:

```css
.botao {
  background-color: #0d9488;
  color: white;
  padding: 8px 16px;
}
```

---

## 2. Qual implementação usar?

Atualmente, a implementação recomendada é o **Dart Sass**.

Evite tutoriais antigos baseados em:

- Ruby Sass — descontinuado;
- LibSass — descontinuado;
- `node-sass` — descontinuado.

Use:

- Dart Sass, via executável ou npm;
- pacotes modernos como `sass` no npm, se você tiver Node.js instalado.

Em setembro de 2026, a versão estável mais recente do Dart Sass é a **1.104.0**. Como o Sass recebe atualizações frequentes, confirme sempre a versão mais recente na página de lançamentos antes de instalar.

Referências:

- Dart Sass: https://sass-lang.com/dart-sass/
- Repositório Dart Sass: https://github.com/sass/dart-sass
- Lançamentos (versões): https://github.com/sass/dart-sass/releases
- Documentação do Sass: https://sass-lang.com/documentation/

---

## 3. Instalação

Há três caminhos comuns. Escolha um.

### 3.1. Via executável Dart Sass

1. Acesse: https://github.com/sass/dart-sass/releases
2. Baixe a versão para seu sistema operacional.
3. Extraia o arquivo.
4. Adicione a pasta ao `PATH` do sistema ou execute o comando diretamente a partir da pasta extraída.

Depois teste no terminal:

```bash
sass --version
```

### 3.2. Via npm

Se você tiver Node.js instalado:

```bash
npm install -g sass
```

Depois:

```bash
sass --version
```

### 3.3. Via extensão do VS Code

Para estudo em EAD, uma extensão pode facilitar. Uma opção conhecida é a extensão **Live Sass Compiler**, que observa arquivos `.scss` e gera `.css` automaticamente ao salvar.

Procure no VS Code por:

- `Live Sass Compiler`

Prefira extensões mantidas recentemente, como a de glenn2223. Extensões abandonadas podem usar compiladores antigos.

Mesmo usando extensão, é importante aprender o comando básico do Sass.

---

## 4. Onde colocar os arquivos em um projeto Flask?

No Flask, arquivos estáticos ficam normalmente na pasta `static/`.

Uma organização possível:

```text
projeto/
├── app.py
├── static/
│   ├── sass/
│   │   ├── main.scss
│   │   ├── _variables.scss
│   │   ├── _base.scss
│   │   └── _components.scss
│   └── css/
│       └── main.css
└── templates/
    └── base.html
```

Regra importante:

- `.scss` é o arquivo de trabalho;
- `.css` é o arquivo gerado;
- o HTML deve apontar para o `.css`, nunca para o `.scss`.

No template Jinja2:

```html
<link rel="stylesheet" href="{{ url_for('static', filename='css/main.css') }}">
```

Referência do Flask para arquivos estáticos:

- https://flask.palletsprojects.com/en/stable/quickstart/#static-files

---

## 5. Primeiro arquivo SCSS

Crie o arquivo:

```text
static/sass/main.scss
```

Conteúdo:

```scss
body {
  font-family: system-ui, sans-serif;
  background-color: #f9fafb;
  color: #1f2937;
  line-height: 1.6;
}

h1 {
  font-size: 2rem;
}

p {
  margin-bottom: 16px;
}
```

Compile manualmente:

```bash
sass static/sass/main.scss static/css/main.css
```

O comando acima lê `main.scss` e gera `main.css`.

Para observar alterações automaticamente:

```bash
sass --watch static/sass/main.scss:static/css/main.css
```

O sinal de dois pontos indica:

```text
origem : destino
```

Também é possível observar uma pasta inteira:

```bash
sass --watch static/sass:static/css
```

Esse comando compila todos os arquivos `.scss` da pasta `static/sass` para `static/css`, exceto arquivos parciais, que começam com `_`.

---

## 6. Variáveis

Variáveis guardam valores repetidos: cores, espaçamentos, fontes, tamanhos de borda.

```scss
$cor-primaria: #0d9488;
$cor-primaria-hover: #0f766e;
$cor-fundo: #f9fafb;
$cor-texto: #1f2937;
$espaco-md: 16px;
$raio-borda: 8px;

body {
  background-color: $cor-fundo;
  color: $cor-texto;
}

a {
  color: $cor-primaria-hover;
}

button {
  background-color: $cor-primaria;
  border-radius: $raio-borda;
  padding: $espaco-md;
}
```

Documentação:

- https://sass-lang.com/documentation/variables/

---

## 7. Partials e `@use`

Quando o projeto cresce, dividir o CSS em vários arquivos ajuda. No Sass, arquivos que começam com `_` são chamados de **partials**.

Exemplo:

```text
static/sass/
├── main.scss
├── _variables.scss
├── _base.scss
└── _components.scss
```

O `_variables.scss` não gera um arquivo CSS próprio. Ele é importado por outro arquivo.

### `_variables.scss`

```scss
$cor-primaria: #0d9488;
$cor-primaria-hover: #0f766e;
$cor-fundo: #f9fafb;
$cor-texto: #1f2937;
$cor-borda: #9ca3af;
$espaco-sm: 8px;
$espaco-md: 16px;
$espaco-lg: 24px;
```

### `_base.scss`

```scss
@use 'variables' as v;

body {
  font-family: system-ui, sans-serif;
  background-color: v.$cor-fundo;
  color: v.$cor-texto;
  line-height: 1.6;
}

a {
  color: v.$cor-primaria-hover;
}
```

### `_components.scss`

```scss
@use 'variables' as v;

button {
  background-color: v.$cor-primaria;
  color: white;
  border: none;
  border-radius: 6px;
  padding: v.$espaco-sm v.$espaco-md;
  min-height: 44px;
}

button:hover {
  background-color: v.$cor-primaria-hover;
}
```

### `main.scss`

```scss
@use 'base';
@use 'components';
```

O comando:

```bash
sass --watch static/sass:static/css
```

vai gerar apenas `main.css`, porque `_base.scss`, `_components.scss` e `_variables.scss` são parciais.

Referências:

- `@use`: https://sass-lang.com/documentation/at-rules/use/
- Partials: https://sass-lang.com/documentation/at-rules/use/#partials
- `@forward`: https://sass-lang.com/documentation/at-rules/forward/

---

## 8. Não use `@import` em projetos novos

Tutoriais antigos usam muito:

```scss
@import 'variables';
```

Não use.

O `@import` está deprecado. Ao usá-lo, o compilador emite avisos de depreciação. A diretiva será **removida no Dart Sass 3.0.0**. Depois dessa remoção, códigos com `@import` deixarão de compilar.

O `@use` não é uma alternativa entre várias. É a única forma recomendada de importar arquivos em projetos novos.

Use:

```scss
@use 'variables' as v;
```

ou, quando quiser trazer os membros sem namespace:

```scss
@use 'variables' as *;
```

Preferência recomendada: usar namespace explícito.

```scss
@use 'variables' as v;

.botao {
  background-color: v.$cor-primaria;
}
```

Por que o `@import` foi deprecado:

- colocava tudo no mesmo escopo global, causando conflitos de nomes;
- obrigava o Sass a adivinhar de qual arquivo vinha cada variável ou mixin;
- dificultava a manutenção de projetos grandes;
- carregava o mesmo arquivo várias vezes se fosse importado em lugares diferentes.

O `@use` resolve esses problemas: cada arquivo tem seu próprio escopo, os namespaces evitam conflitos, e cada módulo é carregado uma única vez.

Referências:

- https://sass-lang.com/documentation/at-rules/use/
- https://sass-lang.com/documentation/at-rules/import/

---

## 8.1 Migrando projetos antigos com `sass-migrator` *(nova seção)*

Se você recebeu um projeto antigo que usa `@import`, não precisa converter tudo manualmente. O Sass oferece uma ferramenta oficial de linha de comando chamada **`sass-migrator`**, que automatiza a maior parte do trabalho.

A ferramenta faz três tarefas principais:

| Comando | O que faz |
| :--- | :--- |
| `sass-migrator module` | Converte `@import` para `@use` e `@forward` |
| `sass-migrator division` | Substitui divisões com `/` por `math.div()` |
| `sass-migrator namespace` | Limpa e simplifica namespaces após a migração |

### Migração de `@import` para `@use`

O comando mais útil para o nosso caso é o `module`. Ele varre os arquivos, identifica os `@import` e os converte para `@use` com namespaces apropriados:

```bash
sass-migrator module --migrate-deps static/sass/main.scss
```

No Windows (PowerShell), o `**/*.scss` pode não expandir corretamente. Se necessário, indique o caminho de outra forma:

```powershell
sass-migrator module static/sass/main.scss
```

### Migração de divisão

Se o projeto antigo usa `/` para dividir valores, o comando `division` converte para a função moderna:

```bash
sass-migrator division **/*.scss
```

Antes:

```scss
$metade: 100% / 2;
```

Depois:

```scss
@use 'sass:math';

$metade: math.div(100%, 2);
```

### Recomendações práticas

1. **Faça backup antes.** Embora a ferramenta seja confiável, sempre trabalhe sobre um repositório Git com o código commitado. Assim você pode reverter com `git diff` e `git checkout` se algo sair errado.
2. **Rode o compilador depois.** A migração automática cobre a maioria dos casos, mas não todos. Após rodar o `sass-migrator`, compile o projeto e corrija manualmente os avisos restantes.
3. **Teste visualmente.** Compare o CSS gerado antes e depois da migração para garantir que o resultado é o mesmo.

Referência:

- https://sass-lang.com/documentation/cli/migrator/

## 9. Nesting

Nesting permite escrever seletores filhos dentro de seletores pais.

CSS comum:

```css
.form-group {
  display: grid;
  gap: 10px;
}

.form-group label {
  justify-self: end;
}

.form-group input {
  padding: 8px;
}
```

SCSS:

```scss
.form-group {
  display: grid;
  gap: 10px;

  label {
    justify-self: end;
  }

  input {
    padding: 8px;
  }
}
```

### Cuidado com nesting excessivo

Nesting é útil, mas pode gerar seletores longos demais.

Evite:

```scss
.layout-wrapper {
  main {
    article {
      section {
        p {
          a {
            color: red;
          }
        }
      }
    }
  }
}
```

Isso gera seletores grandes, aumenta a especificidade e dificulta manutenção.

Boa prática simples:

- use nesting para relações diretas;
- evite mais de 2 ou 3 níveis;
- se um componente pode ser reutilizado fora daquele lugar, não prenda demais o seletor ao nesting.

---

## 10. O `&`

O `&` representa o seletor atual.

```scss
button {
  background-color: #0d9488;

  &:hover {
    background-color: #0f766e;
  }

  &:disabled {
    opacity: 0.6;
    cursor: not-allowed;
  }
}
```

Compila para algo como:

```css
button {
  background-color: #0d9488;
}

button:hover {
  background-color: #0f766e;
}

button:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}
```

---

## 11. Mixins

Mixins são blocos reutilizáveis que podem receber parâmetros.

```scss
@mixin botao($cor-fundo, $cor-texto: white) {
  background-color: $cor-fundo;
  color: $cor-texto;
  border: none;
  border-radius: 6px;
  padding: 8px 16px;
  min-height: 44px;
  cursor: pointer;

  &:hover {
    filter: brightness(0.95);
  }
}
```

Uso:

```scss
button {
  @include botao(#0d9488);
}

.botao-perigo {
  @include botao(#b91c1c);
}
```

Mixins são úteis para padrões repetidos:

- botões;
- cartões;
- formulários;
- responsividade;
- estados de foco;
- mensagens de erro/sucesso.

Documentação:

- https://sass-lang.com/documentation/at-rules/mixin/

---

## 12. Mixin para media queries

Um uso comum de mixin é simplificar media queries.

```scss
@mixin tablet {
  @media screen and (min-width: 768px) {
    @content;
  }
}

@mixin desktop {
  @media screen and (min-width: 1200px) {
    @content;
  }
}
```

Uso:

```scss
.layout-wrapper {
  display: grid;
  grid-template-columns: 1fr;
  gap: 24px;

  @include tablet {
    grid-template-columns: 2fr 240px;
  }
}
```

O `@content` permite passar um bloco para dentro do mixin.

---

## 13. `@extend`

`@extend` permite reaproveitar estilos de um seletor em outro.

```scss
%mensagem-base {
  padding: 8px 12px;
  border-radius: 6px;
  border: 1px solid;
}

.mensagem-sucesso {
  @extend %mensagem-base;
  background-color: #f0fdf4;
  border-color: #15803d;
  color: #15803d;
}

.mensagem-erro {
  @extend %mensagem-base;
  background-color: #fef2f2;
  border-color: #b91c1c;
  color: #b91c1c;
}
```

O `%mensagem-base` é um placeholder selector. Ele não gera CSS sozinho.

Use com moderação. `@extend` pode gerar seletores inesperados quando usado demais. Em muitos casos, mixin ou classe utilitária simples resolvem melhor.

Referência:

- https://sass-lang.com/documentation/at-rules/extend/

---

## 14. Módulos embutidos

O Sass possui módulos oficiais para operações matemáticas, listas, mapas, strings, cores e metaprogramação.

Use assim:

```scss
@use 'sass:color';
@use 'sass:math';
```

### Exemplo com cor

```scss
@use 'sass:color';

$cor-primaria: #0d9488;

.botao {
  background-color: $cor-primaria;
}

.botao:hover {
  background-color: color.adjust($cor-primaria, $lightness: -8%);
}
```

### Exemplo com matemática

```scss
@use 'sass:math';

$largura-coluna: math.div(100%, 3);

.coluna {
  width: $largura-coluna;
}
```

Evite usar divisão com `/` diretamente, pois esse uso antigo foi deprecado.

Prefira:

```scss
@use 'sass:math';

$resultado: math.div(100px, 4);
```

Referências:

- Módulos: https://sass-lang.com/documentation/modules/
- `sass:color`: https://sass-lang.com/documentation/modules/color/
- `sass:math`: https://sass-lang.com/documentation/modules/math/

---

## 15. Interpolação

Interpolação permite inserir valores Sass dentro de nomes de seletores, propriedades ou strings.

```scss
$espacos: (
  xs: 4px,
  sm: 8px,
  md: 16px,
  lg: 24px
);

@each $nome, $valor in $espacos {
  .espaco-#{$nome} {
    padding: $valor;
  }
}
```

Resultado aproximado:

```css
.espaco-xs {
  padding: 4px;
}

.espaco-sm {
  padding: 8px;
}

.espaco-md {
  padding: 16px;
}

.espaco-lg {
  padding: 24px;
}
```

Isso pode ser útil para gerar classes utilitárias, mas use apenas quando fizer sentido. Não transforme seu projeto em um framework gigante se você só precisa de algumas classes.

Referência:

- https://sass-lang.com/documentation/interpolation/

---

## 16. Sass variables vs CSS custom properties

Você já viu CSS com variáveis assim:

```css
:root {
  --cor-primaria: #0d9488;
}

button {
  background-color: var(--cor-primaria);
}
```

Isso são **CSS custom properties**, ou variáveis CSS nativas.

Elas são diferentes das variáveis Sass.

| Recurso | Variável Sass | CSS custom property |
|---|---|---|
| Quando existe | Durante a compilação | No navegador, em tempo de execução |
| Pode mudar com JavaScript | Não diretamente | Sim |
| Pode mudar com media query | Não diretamente | Sim |
| Pode ser usada para gerar CSS | Sim | Sim |
| Navegador lê diretamente | Não | Sim |

Exemplo de uso combinado:

```scss
$cor-primaria: #0d9488;

:root {
  --cor-primaria: #{$cor-primaria};
}

button {
  background-color: var(--cor-primaria);
}
```

A interpolação `#{$cor-primaria}` insere o valor da variável Sass dentro do CSS.


Quando usar o quê?

- Use Sass para organização de projeto: partials, mixins, funções de build, repetição de código.
- Use CSS custom properties para temas que mudam em tempo de execução, dark mode controlado por classe ou JS, preferências do usuário, etc.

Referências:

- CSS custom properties: https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties

---


## 17. Sass e recursos nativos modernos do CSS *(revisada)*

O CSS evoluiu bastante. Em 2026, vários recursos que antes justificavam o uso de um pré-processador já são nativos e amplamente suportados pelos navegadores.

### O que o CSS nativo já cobre

| Recurso | CSS nativo | Situação em 2026 |
| :--- | :--- | :--- |
| Variáveis | `--variavel` e `var()` | Suportado há anos |
| Aninhamento | Nesting nativo | Suporte amplo (Baseline) |
| Cálculos | `calc()`, `min()`, `max()`, `clamp()` | Suportado há anos |
| Mistura de cores | `color-mix()` | Suporte amplo |
| Gerenciamento de cascata | `@layer` | Suporte amplo |
| Escopo de estilos | `@scope` | Suporte em expansão |

### Nesting nativo

O aninhamento de seletores, antes um dos principais motivos para usar Sass, agora funciona direto no CSS:

```css
.card {
  padding: 16px;

  & .title {
    font-weight: bold;
  }

  &:hover {
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
  }
}
```

O `&` funciona de forma parecida com o Sass. Para projetos pequenos, isso elimina a necessidade de um pré-processador apenas para organizar seletores.

Referência:

- CSS nesting: https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_nesting

### Camadas de cascata com `@layer`

O `@layer` permite controlar a ordem de precedência dos estilos, resolvendo problemas de especificidade sem precisar de seletores mais fortes:

```css
@layer reset, base, componentes, utilitarios;

@layer reset {
  * { box-sizing: border-box; }
}

@layer base {
  body { font-family: system-ui, sans-serif; }
}

@layer componentes {
  .botao { padding: 8px 16px; }
}
```

Camadas declaradas primeiro têm prioridade menor. Isso é útil em projetos grandes, onde várias fontes de CSS precisam conviver sem conflito.

Referência:

- https://developer.mozilla.org/en-US/docs/Web/CSS/@layer

### Escopo com `@scope`

O `@scope` permite limitar estilos a uma parte específica do documento, sem vazar para fora:

```css
@scope (.card) {
  .title { font-weight: bold; }
  p { color: #555; }
}
```

Os estilos dentro de `@scope (.card)` só afetam elementos dentro de `.card`. É útil para componentes isolados.

Referência:

- https://developer.mozilla.org/en-US/docs/Web/CSS/@scope

### O que ainda é exclusivo do Sass

Com o nesting e as variáveis nativos, o papel do Sass mudou. Ele continua útil principalmente nestes casos:

| Recurso | Por que o Sass ainda é útil |
| :--- | :--- |
| **Mixins** | Blocos reutilizáveis com parâmetros. O CSS nativo não tem equivalente direto. |
| **Funções e lógica** | `@if`, `@each`, `@for` e funções personalizadas. O CSS não tem laços nem condicionais. |
| **Organização modular** | `@use` e `@forward` com namespaces. O `@import` do CSS é mais limitado e menos eficiente. |
| **Manipulação avançada de cores** | Funções do módulo `sass:color` para transformações complexas. O `color-mix()` nativo cobre casos simples. |

### Regra prática

Antes de adicionar Sass a um projeto, pergunte:

1. Preciso de mixins com parâmetros?
2. Preciso de laços ou condicionais para gerar CSS?
3. O projeto é grande o suficiente para se beneficiar de módulos com namespace?

Se a resposta for não para todas, CSS puro provavelmente basta. Se for sim para alguma, Sass ajuda.

Referências:

- CSS nesting: https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_nesting
- `@layer`: https://developer.mozilla.org/en-US/docs/Web/CSS/@layer
- `@scope`: https://developer.mozilla.org/en-US/docs/Web/CSS/@scope
- `color-mix()`: https://developer.mozilla.org/en-US/docs/Web/CSS/color_value/color-mix
- `clamp()`: https://developer.mozilla.org/en-US/docs/Web/CSS/clamp

---

## 18. Exemplo aplicado ao estilo do curso

Agora, um exemplo mais próximo do material que vocês já viram.

### `_variables.scss`

```scss
$cor-texto: #1f2937;
$cor-fundo: #f9fafb;
$cor-fundo-card: #ffffff;
$cor-primaria: #0d9488;
$cor-primaria-hover: #0f766e;
$cor-borda: #9ca3af;
$cor-erro: #b91c1c;
$cor-box: #f0fdfa;

$espaco-xs: 4px;
$espaco-sm: 8px;
$espaco-md: 16px;
$espaco-lg: 24px;

$raio-card: 8px;
```

### `_base.scss`

```scss
@use 'variables' as v;

* {
  box-sizing: border-box;
}

body {
  font-family: system-ui, -apple-system, sans-serif;
  line-height: 1.6;
  color: v.$cor-texto;
  background-color: v.$cor-fundo;
  margin: 0;
}

a {
  color: v.$cor-primaria-hover;
}

img {
  max-width: 100%;
  height: auto;
}
```

### `_components.scss`

```scss
@use 'variables' as v;

@mixin card {
  background: v.$cor-fundo-card;
  padding: v.$espaco-md;
  border: 1px solid v.$cor-borda;
  border-radius: v.$raio-card;
  margin-bottom: v.$espaco-md;
}

article,
aside,
.edu-box,
#contact-form {
  @include card;
}

.edu-box {
  background: v.$cor-box;
}

.form-group {
  margin-bottom: v.$espaco-sm;
  display: grid;
  grid-template-columns: 140px 1fr;
  gap: 10px;

  label {
    justify-self: end;
    align-self: center;
  }

  input,
  select,
  textarea {
    border: 2px solid v.$cor-borda;
    border-radius: 6px;
    padding: v.$espaco-sm;
  }
}
```

### `main.scss`

```scss
@use 'base';
@use 'components';
```

Compile:

```bash
sass static/sass/main.scss static/css/main.css
```

Ou observe:

```bash
sass --watch static/sass:static/css
```

---

## 19. Build para desenvolvimento e produção

### Desenvolvimento

Em desenvolvimento, use `--watch` para compilar automaticamente:

```bash
sass --watch static/sass:static/css
```

Você pode deixar esse comando rodando em um terminal separado enquanto usa o Flask em outro terminal:

```bash
flask --app flaskr run --debug
```

### Produção

Para produção, gere CSS minificado:

```bash
sass static/sass/main.scss static/css/main.css --style=compressed
```

Se quiser desativar source maps:

```bash
sass static/sass/main.scss static/css/main.css --style=compressed --no-source-map
```

Referência da CLI:

- https://sass-lang.com/documentation/cli/dart-sass/

---

## 20. Source maps

Quando você compila Sass, o navegador pode mostrar o arquivo `.css` gerado, e não o `.scss` original. Source maps ajudam o DevTools a mostrar o arquivo SCSS original durante a depuração.

Por padrão, a CLI do Dart Sass gera source map em desenvolvimento.

Se quiser desativar:

```bash
sass --no-source-map static/sass/main.scss static/css/main.css
```

Em produção, normalmente não é necessário distribuir source maps.

---

## 21. Erros comuns

### 1. Linkar o `.scss` no HTML

Errado:

```html
<link rel="stylesheet" href="{{ url_for('static', filename='sass/main.scss') }}">
```

Certo:

```html
<link rel="stylesheet" href="{{ url_for('static', filename='css/main.css') }}">
```

O navegador não entende `.scss`.

---

### 2. Usar `@import` antigo

Evite:

```scss
@import 'variables';
```

Use:

```scss
@use 'variables' as v;
```

---

### 3. Usar `/` para divisão

Evite:

```scss
$metade: 100% / 2;
```

Use:

```scss
@use 'sass:math';

$metade: math.div(100%, 2);
```

---

### 4. Aninhar demais

Evite seletores muito longos. Prefira componentes simples.

---

### 5. Usar Sass para esconder CSS ruim

Sass organiza CSS, mas não corrige problemas de especificidade, HTML mal estruturado ou layout mal planejado.

---

### 6. Gerar código demais sem necessidade

Loops e mapas são úteis, mas não é necessário gerar centenas de classes utilitárias se o projeto é pequeno.

### 7. Usar funções globais deprecadas

A deprecação do `@import` veio acompanhada de outra: as **funções globais** embutidas do Sass também foram deprecadas a partir do Dart Sass 1.80.0 e serão removidas no Dart Sass 3.0.0, junto com o `@import`.

Evite:

```scss
$cor-primaria: #0d9488;

.botao:hover {
  background-color: darken($cor-primaria, 8%);
}
```

Use:

```scss
@use 'sass:color';

$cor-primaria: #0d9488;

.botao:hover {
  background-color: color.adjust($cor-primaria, $lightness: -8%);
}
```

As versões globais de funções embutidas (`darken()`, `lighten()`, `mix()`, `percentage()`, `round()`, entre outras) funcionam hoje, mas emitem avisos de deprecação (`global-builtin`) e deixarão de compilar no futuro. Quase todas têm equivalente direto nos módulos oficiais:

| Função global (evitar) | Equivalente moderno |
|---|---|
| `darken($cor, 10%)` | `color.adjust($cor, $lightness: -10%)` |
| `lighten($cor, 10%)` | `color.adjust($cor, $lightness: 10%)` |
| `mix($a, $b, 50%)` | `color.mix($a, $b, 50%)` |
| `percentage(0.5)` | `math.percentage(0.5)` |
| `round(3.7)` | `math.round(3.7)` |

O `sass-migrator` não converte essas funções automaticamente, então a correção é manual. Se receber um projeto antigo, o compilador listará os avisos `global-builtin` — siga-os arquivo por arquivo.

Referência:

- https://sass-lang.com/documentation/modules/

---

## 22. Exercício 1 — Converter um CSS simples para SCSS

Pegue um arquivo `style.css` simples de algum projeto anterior do curso. Por exemplo, o projeto de formulário ou o Flaskr.

Tarefas:

1. Crie a pasta `static/sass/`.
2. Crie `main.scss`.
3. Mova o conteúdo do CSS para `main.scss`.
4. Compile para `static/css/main.css`.
5. Altere o template para apontar para o novo `.css`.
6. Confirme que a página continua igual.

Checklist:

- [ ] O arquivo `.scss` compila sem erro.
- [ ] O arquivo `.css` é gerado.
- [ ] O HTML aponta para o `.css`.
- [ ] A página continua funcionando.

---

## 23. Exercício 2 — Criar variáveis e partials

A partir do exercício anterior:

1. Crie `_variables.scss`.
2. Extraia pelo menos:
   - cor primária;
   - cor de fundo;
   - cor de texto;
   - espaçamentos.
3. Crie `_base.scss` para reset e corpo da página.
4. Crie `_components.scss` para botões, cartões e formulários.
5. Importe tudo no `main.scss` com `@use`.

Exemplo esperado:

```scss
@use 'base';
@use 'components';
```

Checklist:

- [ ] Nenhum arquivo parcial gera CSS sozinho.
- [ ] As variáveis estão sendo usadas com namespace.
- [ ] O CSS final continua funcionando.

---

## 24. Exercício 3 — Criar um mixin de botão

Crie um mixin chamado `botao`.

Requisitos:

1. Receber cor de fundo.
2. Receber cor do texto, com padrão branco.
3. Definir padding, borda e raio.
4. Incluir estado `:hover`.
5. Garantir alvo de toque mínimo de 44px, como visto no material de acessibilidade.

Depois use o mixin para criar:

- botão primário;
- botão de erro;
- botão secundário.

Checklist:

- [ ] O mixin recebe parâmetros.
- [ ] Pelo menos um parâmetro tem valor padrão.
- [ ] O estado `:hover` foi incluído com `&:hover`.
- [ ] O botão tem altura mínima adequada para toque.

---

## 25. Exercício 4 — Integrar com Flask

Crie ou adapte um projeto Flask pequeno.

Estrutura esperada:

```text
projeto/
├── app.py
├── static/
│   ├── sass/
│   │   ├── main.scss
│   │   ├── _variables.scss
│   │   └── _components.scss
│   └── css/
│       └── main.css
└── templates/
    ├── base.html
    └── index.html
```

Requisitos:

1. O `base.html` deve carregar o CSS com `url_for`.
2. O Sass deve ser compilado com `sass --watch`.
3. Alterar o `.scss` deve atualizar o `.css`.
4. Recarregar a página no Flask deve mostrar a alteração.

Comandos sugeridos:

Terminal 1:

```bash
sass --watch static/sass:static/css
```

Terminal 2:

```bash
flask --app app run --debug
```

Checklist:

- [ ] O Flask roda normalmente.
- [ ] O CSS é carregado.
- [ ] Alterações no SCSS aparecem na página.
- [ ] Nenhum `.scss` é carregado diretamente pelo navegador.

---

## 26. Exercício 5 — Organizar temas com Sass e CSS custom properties

Crie um projeto com tema claro e escuro.

Use Sass para definir valores iniciais e CSS custom properties para permitir troca de tema via classe.

Exemplo:

```scss
$tema-claro-fundo: #f9fafb;
$tema-claro-texto: #1f2937;

$tema-escuro-fundo: #111827;
$tema-escuro-texto: #f9fafb;

:root {
  --fundo: #{$tema-claro-fundo};
  --texto: #{$tema-claro-texto};
}

body.dark {
  --fundo: #{$tema-escuro-fundo};
  --texto: #{$tema-escuro-texto};
}

body {
  background-color: var(--fundo);
  color: var(--texto);
}
```

Depois, opcionalmente, use JavaScript para alternar a classe `dark` no `body`.

Checklist:

- [ ] As cores são definidas com variáveis Sass.
- [ ] O CSS final usa `var()`.
- [ ] O tema muda quando a classe do corpo muda.

---

## 27. Recomendações finais

Use Sass quando ele ajudar a:

- reduzir repetição;
- organizar arquivos;
- criar mixins úteis;
- manter padrões visuais;
- facilitar manutenção.

Não use Sass para:

- criar abstrações desnecessárias;
- esconder HTML mal feito;
- gerar CSS demais sem motivo;
- complicar projetos pequenos.

Para projetos pequenos, CSS puro pode ser suficiente. Para projetos maiores, Sass pode ajudar bastante, desde que usado com moderação.

---

## 28. Fontes recomendadas

### Sass

- Site oficial: https://sass-lang.com/
- Documentação: https://sass-lang.com/documentation/
- Dart Sass: https://sass-lang.com/dart-sass/
- Dart Sass no GitHub: https://github.com/sass/dart-sass
- Releases do Dart Sass: https://github.com/sass/dart-sass/releases
- CLI do Dart Sass: https://sass-lang.com/documentation/cli/dart-sass/
- Variáveis: https://sass-lang.com/documentation/variables/
- `@use`: https://sass-lang.com/documentation/at-rules/use/
- `@forward`: https://sass-lang.com/documentation/at-rules/forward/
- `@import` e depreciação: https://sass-lang.com/documentation/at-rules/import/
- Mixins: https://sass-lang.com/documentation/at-rules/mixin/
- `@extend`: https://sass-lang.com/documentation/at-rules/extend/
- Módulos embutidos: https://sass-lang.com/documentation/modules/
- `sass:color`: https://sass-lang.com/documentation/modules/color/
- `sass:math`: https://sass-lang.com/documentation/modules/math/
- Interpolação: https://sass-lang.com/documentation/interpolation/

### CSS moderno

- CSS custom properties: https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties
- CSS nesting: https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_nesting
- `color-mix()`: https://developer.mozilla.org/en-US/docs/Web/CSS/color_value/color-mix
- `clamp()`: https://developer.mozilla.org/en-US/docs/Web/CSS/clamp
- Grid Layout: https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_grid_layout

### Flask

- Static files no Flask: https://flask.palletsprojects.com/en/stable/quickstart/#static-files
- Templates no Flask: https://flask.palletsprojects.com/en/stable/quickstart/#rendering-templates

### Observação sobre versões

O Sass recebe atualizações frequentes. Antes de instalar, verifique a versão mais recente na página oficial de releases:

- https://github.com/sass/dart-sass/releases

Se encontrar um tutorial antigo usando Ruby Sass, LibSass ou `node-sass`, trate com cuidado. Esses caminhos não são recomendados para projetos novos.
