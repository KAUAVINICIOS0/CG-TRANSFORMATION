# Transformações do Cubo em SRU para SRD

## Sobre o Projeto

Este projeto demonstra, de forma interativa e visual, como um cubo 3D pode ser transformado do Sistema de Referência Universal (SRU) para o Sistema de Referência do Dispositivo (SRD) utilizando a biblioteca [Three.js](https://threejs.org/). O objetivo é ilustrar conceitos fundamentais de computação gráfica, como escala, rotação, reflexão, translação e o pipeline de transformação de coordenadas.

## Requisitos

- Node.js (versão 16 ou superior)
- npm (geralmente instalado com o Node.js)

## Instalação

Clone o repositório e instale as dependências:

```bash
git clone https://github.com/KAUAVINICIOS0/CG-TRANSFORMATION
cd cg-transform
npm install
```

## Execução

Para iniciar o servidor de desenvolvimento:

```bash
npm run dev
```

Acesse o projeto no navegador em [http://localhost:5173](http://localhost:5173) (ou o endereço indicado no terminal).

## Compilação para produção

Para compilar o projeto para produção:

```bash
npm run build
```

Para visualizar a versão de produção localmente:

```bash
npm run preview
```

## Funcionalidades do Projeto

- **Visualização 3D interativa** de um cubo e suas transformações.
- **Etapas separadas**: escala, rotação, reflexão e translação, exibidas individualmente e em conjunto.
- **Controles orbitais**: gire, aproxime e afaste a cena com o mouse.
- **Exibição das matrizes de transformação** aplicadas em cada etapa.
- **Visualização das coordenadas dos vértices** antes e depois das transformações.

## Como funciona?

O projeto utiliza Three.js para criar cenas 3D e aplicar transformações matriciais em um cubo. Cada etapa da transformação é apresentada separadamente, permitindo observar o efeito de cada matriz.

### Exemplo de código: definição dos vértices do cubo

```js
// Matriz de vértices do cubo em coordenadas homogêneas (4x8)
const verts = [
  [0, 0, 0, 1],  // V1 (0,0,0)
  [p, 0, 0, 1],  // V2 (p,0,0)
  [p, p, 0, 1],  // V3 (p,p,0)
  [0, p, 0, 1],  // V4 (0,p,0)
  [0, 0, p, 1],  // V5 (0,0,p)
  [p, 0, p, 1],  // V6 (p,0,p)
  [p, p, p, 1],  // V7 (p,p,p)
  [0, p, p, 1],  // V8 (0,p,p)
]
```

### Exemplo de código: aplicação das transformações

```js
// Matrizes de transformação
const S = new THREE.Matrix4().makeScale(1.5, 0.5, 2) // Escala
const theta = (10 * p * Math.PI) / 180
const Ry = new THREE.Matrix4().makeRotationY(theta)   // Rotação em Y
const Rf = new THREE.Matrix4().makeScale(1, -1, 1)    // Reflexão em X
const T = new THREE.Matrix4().makeTranslation(-p/2, -p/2, -p/2) // Translação

// Ordem de aplicação: T * Rf * Ry * S
const matrixFinal = new THREE.Matrix4()
  .copy(T)
  .multiply(Rf)
  .multiply(Ry)
  .multiply(S)
```

### Exemplo de código: transformação dos vértices

```js
for (let i = 0; i < verts.length; i++) {
  const v = verts[i]
  const vertex = new THREE.Vector4(v[0], v[1], v[2], v[3])
  vertex.applyMatrix4(matrixFinal)
  // Conversão para coordenadas da viewport (SRD)
  const viewportX = Math.round(((vertex.x - windowMin) / (windowMax - windowMin)) * viewportWidth)
  // ... demais coordenadas
}
```

## Conceitos de Computação Gráfica

- **SRU (Sistema de Referência Universal)**: sistema de coordenadas do mundo virtual.
- **SRD (Sistema de Referência do Dispositivo)**: sistema de coordenadas do dispositivo de exibição (tela).
- **Transformações**: operações matemáticas (matrizes) que alteram a posição, escala, orientação e reflexão dos objetos.
- **Pipeline gráfico**: sequência de etapas que convertem coordenadas do mundo para coordenadas de tela.

## Licença

MIT

---

**Sugestão extra:**  
Adicione GIFs ou screenshots das transformações, ou uma seção de "Desafios/Sugestões de experimentação" para quem quiser modificar o código. Se quiser, posso sugerir desafios ou materiais de estudo sobre computação gráfica!
