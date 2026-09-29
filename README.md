# Trabalho SPA & Framework

Disciplina: Programação Web - Professor Welington Gadelha\
Aquiles Souza - Gustavo Muller - Yasmin Inturn

## Recurso em destaque: “experimente o Vue online”;

opções mais didáticas com diferentes formas de começar a usar  Vue

- Playground
- JSFiddle
- StackBlitz
- Scrimba

## Criando uma Aplicação de Página Única (SPA)

Codigo inicial:

```
npm create vue@latest
```

No caso da necessidade de uma versão específica, apenas mudaria que, ao invés de "latest" após o @ seria a versão determinada/escolhida

Vale salientar que o próprio Vue.js recomenda o uso do VScode, adicionando a extensão própria, para a implementação e uso do mesmo.

Logo em seguida, no terminal, o Vue.js fará algumas perguntas referentes a especificações de instalações, das quais para um ponta pé inicial podemos apenas apertar enter todas as vezes para dar a resposta padrão e ignorar esta etapa.

Agora com o Vue.js instalado, seguiremos com o comando:

```
cd <nome-do-seu-projeto>
npm install
npm run dev
```

Para dar início a nossa aplicação e já ter um primeiro modelo pronto rodando.


## Baixando via CDN

\- carregamento dos arquivos do site a partir de uma rede de servidores externos distribuída globalmente, em vez do servidor principal de hospedagem do site.

Você pode usar o Vue diretamente de uma CDN por meio de uma tag script:

```html
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
```

Aqui estamos usando o [unpkg](https://unpkg.com/), mas você também pode usar qualquer CDN que distribua pacotes npm, por exemplo o [jsdelivr](https://www.jsdelivr.com/package/npm/vue) ou o [cdnjs](https://cdnjs.com/libraries/vue). Claro, você também pode baixar este arquivo e servi-lo você mesmo.

Ao usar o Vue a partir de uma CDN, não há nenhuma "etapa de build" envolvida. Isso deixa a configuração muito mais simples e é adequado para melhorar HTML estático ou para integrar com um framework de backend. No entanto, você não poderá usar a sintaxe de Componente de Arquivo Único (SFC).

## Exemplo de Reatividade

A fim de demonstrar a reatividade na prática, seguiremos com a explicação de um exemplo prático:

```vue
<script setup>
import { ref, onUnmounted, computed } from 'vue'
const duration = ref(15 * 1000)
const elapsed = ref(0)

let lastTime
let handle

const update = () => {
  elapsed.value = performance.now() - lastTime
  if (elapsed.value >= duration.value) {
    cancelAnimationFrame(handle)
  } else {
    handle = requestAnimationFrame(update)
  }
}

const reset = () => {
  elapsed.value = 0
  lastTime = performance.now()
  update()
}

const progressRate = computed(() =>
  Math.min(elapsed.value / duration.value, 1)
)

reset()

onUnmounted(() => {
  cancelAnimationFrame(handle)
})
</script>

<template>
  <label
    >Elapsed Time: <progress :value="progressRate"></progress></label><div>{{ (elapsed / 1000).toFixed(1) }}s</div>

  <div>
    Duration: <input type="range" v-model="duration" min="1" max="30000">
    {{ (duration / 1000).toFixed(1) }}s
  </div>

  <button @click="reset">Reset</button>
</template>
```

Neste exemplo, trazemos um timer do qual você pode mudar o tempo limite em um faixa nomeada “Duration”.

```js
const duration = ref(15 * 1000)
```

```html
<div>
  Duration: <input type="range" v-model="duration" min="1" max="30000">
  {{ (duration / 1000).toFixed(1) }}s
</div>
```

Note que uma vez passado o valor de “duration” dentro do input range, o valor não é mais atualizado, pois é re atribuído dinamicamente toda vez que o valor muda.

## BUILD GLOBAL:

É a forma mais simples de utilizar Vue. A build global é carregada através do link abaixo via script.

```html
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
```

## BUILD COM MÓDULOS ES:

Você pode usar o Vue diretamente no navegador via CDN utilizando módulos ES nativos, através da tag `<script type=”module”>`.

```html
<div id="app">{{ message }}</div>

<script type="module">
  import { createApp, ref } from 'https://unpkg.com/vue@3/dist/vue.esm-browser.js'

  createApp({
    setup() {
      const message = ref('Hello Vue!')
      return {
        message
      }
    }
  }).mount('#app')
</script>
```

### Ativando os “import maps”:

No exemplo acima, estamos importando a partir da URL completa da CDN, mas no restante da documentação você verá códigos como este:

```js
import { createApp } from 'vue'
```

Podemos indicar ao navegador onde encontrar o import de `vue` usando Import Maps (mapas de importação):

```html
<script type="importmap">
  {
    "imports": {
      "vue": "https://unpkg.com/vue@3/dist/vue.esm-browser.js"
    }
  }
</script>

<div id="app">{{ message }}</div>

<script type="module">
  import { createApp, ref } from 'vue'

  createApp({
    setup() {
      const message = ref('Hello Vue!')
      return {
        message
      }
    }
  }).mount('#app')
</script>
```

Você também pode adicionar entradas para outras dependências no import map - mas certifique-se de que elas apontam para a versão em módulos ES da biblioteca que você pretende usar.

## Separando os módulos:

Ao longo do desenvolvimento, provavelmente vai ser preciso separar os módulos em diferentes arquivos de javascript para ficar mais fácil de se organizar.

**index.html**

```html
<div id="app"></div>

<script type="module">
  import { createApp } from 'vue'
  import MyComponent from './my-component.js'

  createApp(MyComponent).mount('#app')
</script>
```

**my-component.js**

```js
import { ref } from 'vue'
export default {
  setup() {
    const count = ref(0)
    return { count }
  },
  template: `<div>Count is: {{ count }}</div>`
}
```

## Frameworks:

- Nuxt
- Vike
- Astro
- Quasar

Os frameworks do Vue.js tem templates de páginas prontas e componentes que podem ser implementados direto no código via código.
