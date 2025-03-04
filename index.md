# Curso: Estrutura de Dados de Volta às Origens

## Visão Geral do Curso
O curso "Estrutura de Dados de Volta às Origens" visa preencher uma lacuna crítica no conhecimento de muitos desenvolvedores modernos, incluindo plenos e sêniores, que usam recursos avançados de linguagens sem compreender os fundamentos subjacentes. Usando JavaScript como linguagem principal, este curso explora desde os conceitos básicos até análise de eficiência algorítmica.

## Estrutura do Curso

### Módulo 1: Fundamentos da Memória e Ponteiros
**Aula 1: Como os computadores armazenam dados**
- Representação binária
- Alocação de memória
- Exemplo JS: Visualizando como variáveis primitivas vs. objetos são armazenados
```javascript
// Variáveis primitivas são copiadas por valor
let a = 5;
let b = a;
a = 10;
console.log(b); // 5 (não mudou)

// Objetos são referenciados (similar a ponteiros)
let obj1 = { valor: 5 };
let obj2 = obj1;
obj1.valor = 10;
console.log(obj2.valor); // 10 (mudou porque ambos referenciam o mesmo objeto)
```

**Aula 2: Referências e "Ponteiros" em JavaScript**
- Explicação de como JavaScript lida com referências
- Diferenças entre passagem por valor e referência
```javascript
// Demonstração de passagem por referência
function modificarObjeto(obj) {
  obj.propriedade = "modificado";
}

const meuObjeto = { propriedade: "original" };
modificarObjeto(meuObjeto);
console.log(meuObjeto.propriedade); // "modificado"
```

### Módulo 2: Arrays e Listas Encadeadas
**Aula 3: Arrays nativos vs. Arrays dinâmicos**
- Como arrays funcionam na memória
- Complexidade de operações: acesso, inserção, remoção
```javascript
// Array nativo JavaScript
const array = [];
console.time("inserção");
for (let i = 0; i < 100000; i++) {
  array.push(i); // Inserção ao final - O(1) amortizado
}
console.timeEnd("inserção");

console.time("inserção início");
for (let i = 0; i < 1000; i++) {
  array.unshift(i); // Inserção no início - O(n)
}
console.timeEnd("inserção início");
```

**Aula 4: Implementando Lista Encadeada**
- Nó e ponteiros para o próximo elemento
- Inserção e remoção em diferentes posições
```javascript
class Node {
  constructor(value) {
    this.value = value;
    this.next = null;
  }
}

class LinkedList {
  constructor() {
    this.head = null;
    this.length = 0;
  }
  
  append(value) {
    const newNode = new Node(value);
    
    if (!this.head) {
      this.head = newNode;
    } else {
      let current = this.head;
      while (current.next) {
        current = current.next;
      }
      current.next = newNode;
    }
    
    this.length++;
  }
  
  prepend(value) {
    const newNode = new Node(value);
    newNode.next = this.head;
    this.head = newNode;
    this.length++;
  }
  
  // Outros métodos: delete, find, etc.
}

// Demonstração
const lista = new LinkedList();
lista.append(10);
lista.append(20);
lista.prepend(5);
```

### Módulo 3: Pilhas e Filas
**Aula 5: Implementando Pilha (Stack)**
- Conceitos LIFO (Last In, First Out)
- Operações push e pop
```javascript
class Stack {
  constructor() {
    this.items = [];
  }
  
  push(element) {
    this.items.push(element);
  }
  
  pop() {
    if (this.isEmpty()) return "Underflow";
    return this.items.pop();
  }
  
  peek() {
    return this.items[this.items.length - 1];
  }
  
  isEmpty() {
    return this.items.length === 0;
  }
}

// Exemplo: verificando expressões balanceadas
function verificaParenteses(expressao) {
  const pilha = new Stack();
  
  for (let i = 0; i < expressao.length; i++) {
    let char = expressao[i];
    
    if (char === '(') {
      pilha.push(char);
    } else if (char === ')') {
      if (pilha.isEmpty()) return false;
      pilha.pop();
    }
  }
  
  return pilha.isEmpty();
}

console.log(verificaParenteses("((1+2)*(3-4))")); // true
console.log(verificaParenteses("((1+2)")); // false
```

**Aula 6: Implementando Fila (Queue)**
- Conceitos FIFO (First In, First Out)
- Operações enqueue e dequeue
```javascript
class Queue {
  constructor() {
    this.items = {};
    this.frontIndex = 0;
    this.backIndex = 0;
  }
  
  enqueue(element) {
    this.items[this.backIndex] = element;
    this.backIndex++;
  }
  
  dequeue() {
    if (this.isEmpty()) return "Underflow";
    
    const item = this.items[this.frontIndex];
    delete this.items[this.frontIndex];
    this.frontIndex++;
    return item;
  }
  
  isEmpty() {
    return this.frontIndex === this.backIndex;
  }
}

// Simulação de processamento de tarefas
const filaDeTarefas = new Queue();
filaDeTarefas.enqueue("Tarefa 1");
filaDeTarefas.enqueue("Tarefa 2");
filaDeTarefas.enqueue("Tarefa 3");

function processarProximaTarefa() {
  if (!filaDeTarefas.isEmpty()) {
    const tarefa = filaDeTarefas.dequeue();
    console.log(`Processando: ${tarefa}`);
  }
}

processarProximaTarefa(); // "Processando: Tarefa 1"
processarProximaTarefa(); // "Processando: Tarefa 2"
```

### Módulo 4: Árvores e Grafos
**Aula 7: Árvores Binárias**
- Estrutura, percursos e operações
```javascript
class TreeNode {
  constructor(value) {
    this.value = value;
    this.left = null;
    this.right = null;
  }
}

class BinarySearchTree {
  constructor() {
    this.root = null;
  }
  
  insert(value) {
    const newNode = new TreeNode(value);
    
    if (!this.root) {
      this.root = newNode;
      return;
    }
    
    function insertNode(node, newNode) {
      if (newNode.value < node.value) {
        if (node.left === null) {
          node.left = newNode;
        } else {
          insertNode(node.left, newNode);
        }
      } else {
        if (node.right === null) {
          node.right = newNode;
        } else {
          insertNode(node.right, newNode);
        }
      }
    }
    
    insertNode(this.root, newNode);
  }
  
  // Percorrendo em ordem (inorder traversal)
  inOrderTraverse(callback) {
    function inOrderTraverseNode(node, callback) {
      if (node !== null) {
        inOrderTraverseNode(node.left, callback);
        callback(node.value);
        inOrderTraverseNode(node.right, callback);
      }
    }
    
    inOrderTraverseNode(this.root, callback);
  }
}

// Demonstração
const bst = new BinarySearchTree();
bst.insert(10);
bst.insert(5);
bst.insert(15);
bst.insert(3);
bst.insert(7);

let valores = [];
bst.inOrderTraverse((value) => valores.push(value));
console.log(valores); // [3, 5, 7, 10, 15]
```

### Módulo 5: Tabelas Hash (Mapas/Dicionários)
**Aula 8: Implementando uma Tabela Hash**
- Funções de hash, colisões e resolução
```javascript
class HashTable {
  constructor(size = 53) {
    this.table = new Array(size);
  }
  
  _hash(key) {
    let total = 0;
    const PRIME = 31;
    for (let i = 0; i < Math.min(key.length, 100); i++) {
      const char = key[i];
      const value = char.charCodeAt(0) - 96;
      total = (total * PRIME + value) % this.table.length;
    }
    return total;
  }
  
  set(key, value) {
    const index = this._hash(key);
    if (!this.table[index]) {
      this.table[index] = [];
    }
    
    // Verificar se a chave já existe e atualizar
    for (let i = 0; i < this.table[index].length; i++) {
      if (this.table[index][i][0] === key) {
        this.table[index][i][1] = value;
        return;
      }
    }
    
    // Caso contrário, adicionar nova entrada
    this.table[index].push([key, value]);
  }
  
  get(key) {
    const index = this._hash(key);
    if (!this.table[index]) return undefined;
    
    for (let i = 0; i < this.table[index].length; i++) {
      if (this.table[index][i][0] === key) {
        return this.table[index][i][1];
      }
    }
    
    return undefined;
  }
}

// Demonstração
const ht = new HashTable();
ht.set("nome", "João");
ht.set("idade", 30);
ht.set("profissão", "desenvolvedor");

console.log(ht.get("nome")); // "João"
console.log(ht.get("idade")); // 30
```

### Módulo 6: Análise de Algoritmos
**Aula 9: Notação Big O e análise de complexidade**
- Tempo e espaço
- Casos melhores, médios e piores
```javascript
// Exemplo: comparando algoritmos de busca

// Busca linear - O(n)
function buscaLinear(arr, item) {
  console.time("Busca Linear");
  for (let i = 0; i < arr.length; i++) {
    if (arr[i] === item) {
      console.timeEnd("Busca Linear");
      return i;
    }
  }
  console.timeEnd("Busca Linear");
  return -1;
}

// Busca binária - O(log n)
function buscaBinaria(arr, item) {
  console.time("Busca Binária");
  let baixo = 0;
  let alto = arr.length - 1;
  
  while (baixo <= alto) {
    const meio = Math.floor((baixo + alto) / 2);
    const chute = arr[meio];
    
    if (chute === item) {
      console.timeEnd("Busca Binária");
      return meio;
    }
    
    if (chute > item) {
      alto = meio - 1;
    } else {
      baixo = meio + 1;
    }
  }
  
  console.timeEnd("Busca Binária");
  return -1;
}

// Teste com array ordenado
const arrayOrdenado = Array.from({ length: 1000000 }, (_, i) => i);
buscaLinear(arrayOrdenado, 999999); // pior caso para busca linear
buscaBinaria(arrayOrdenado, 999999); // log(n) operações mesmo no pior caso
```

**Aula 10: Algoritmos de Ordenação**
- Comparação de eficiência: Bubble Sort, Quick Sort
```javascript
// Bubble Sort - O(n²)
function bubbleSort(arr) {
  const n = arr.length;
  console.time("Bubble Sort");
  
  for (let i = 0; i < n; i++) {
    for (let j = 0; j < n - i - 1; j++) {
      if (arr[j] > arr[j + 1]) {
        // Troca
        [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
      }
    }
  }
  
  console.timeEnd("Bubble Sort");
  return arr;
}

// Quick Sort - O(n log n) em média
function quickSort(arr) {
  if (arr.length <= 1) return arr;
  
  const pivot = arr[arr.length - 1];
  const left = [];
  const right = [];
  
  for (let i = 0; i < arr.length - 1; i++) {
    if (arr[i] < pivot) {
      left.push(arr[i]);
    } else {
      right.push(arr[i]);
    }
  }
  
  return [...quickSort(left), pivot, ...quickSort(right)];
}

// Teste
const arrayDesordenado = Array.from({ length: 5000 }, () => Math.floor(Math.random() * 1000));
const arrayCopia = [...arrayDesordenado];

console.time("Quick Sort");
quickSort([...arrayDesordenado]);
console.timeEnd("Quick Sort");

bubbleSort([...arrayCopia]);
```

### Módulo 7: Aplicações Práticas
**Aula 11: Resolução de problemas reais**
- Cache LRU (Least Recently Used)
```javascript
class LRUCache {
  constructor(capacity) {
    this.capacity = capacity;
    this.cache = new Map();
  }
  
  get(key) {
    if (!this.cache.has(key)) return -1;
    
    // Acessar e mover para o final (mais recentemente usado)
    const value = this.cache.get(key);
    this.cache.delete(key);
    this.cache.set(key, value);
    return value;
  }
  
  put(key, value) {
    // Se já existe, removemos primeiro
    if (this.cache.has(key)) {
      this.cache.delete(key);
    }
    
    // Se estiver na capacidade máxima, removemos o mais antigo
    if (this.cache.size >= this.capacity) {
      const firstKey = this.cache.keys().next().value;
      this.cache.delete(firstKey);
    }
    
    // Inserimos o novo/atualizado
    this.cache.set(key, value);
  }
}

// Exemplo de uso
const cache = new LRUCache(2);
cache.put(1, 1); // cache: {1=1}
cache.put(2, 2); // cache: {1=1, 2=2}
console.log(cache.get(1)); // retorna 1
cache.put(3, 3); // remove a chave 2, cache: {1=1, 3=3}
console.log(cache.get(2)); // retorna -1 (não encontrado)
```

**Aula 12: Otimização de código**
- Identificação de gargalos
- Refatoração usando estruturas adequadas
```javascript
// Problema: filtrar tarefas duplicadas de múltiplas fontes

// Abordagem ineficiente - O(n²)
function filtrarDuplicadasIneficiente(tarefas) {
  const resultado = [];
  
  for (let i = 0; i < tarefas.length; i++) {
    let duplicado = false;
    
    for (let j = 0; j < resultado.length; j++) {
      if (tarefas[i].id === resultado[j].id) {
        duplicado = true;
        break;
      }
    }
    
    if (!duplicado) {
      resultado.push(tarefas[i]);
    }
  }
  
  return resultado;
}

// Abordagem otimizada - O(n) usando tabela hash
function filtrarDuplicadasOtimizado(tarefas) {
  const map = new Map();
  
  for (const tarefa of tarefas) {
    if (!map.has(tarefa.id)) {
      map.set(tarefa.id, tarefa);
    }
  }
  
  return Array.from(map.values());
}

// Teste com array grande
const tarefas = Array.from({ length: 10000 }, (_, i) => ({
  id: Math.floor(i / 10), // Gera duplicados intencionalmente
  nome: `Tarefa ${i}`
}));

console.time("Ineficiente");
filtrarDuplicadasIneficiente(tarefas);
console.timeEnd("Ineficiente");

console.time("Otimizado");
filtrarDuplicadasOtimizado(tarefas);
console.timeEnd("Otimizado");
```

### Projeto Final
**Desenvolvimento de um sistema com múltiplas estruturas de dados**
- Simulação de um sistema de gerenciamento de tarefas
- Aplicação de estruturas de dados apropriadas
- Análise de desempenho

## Diferencial do Curso
1. **Abordagem Prática**: Todo conceito teórico é imediatamente aplicado em código JavaScript
2. **Visualização**: Animações e diagramas para visualizar como as estruturas funcionam na memória
3. **Análise comparativa**: Testes de desempenho reais mostrando as diferenças entre implementações
4. **Desafios progressivos**: Exercícios após cada módulo para fixação dos conceitos

Este curso permite que desenvolvedores, mesmo experientes, compreendam o funcionamento interno das estruturas de dados que usam diariamente, resultando em código mais eficiente e versatilidade para trabalhar com diferentes linguagens de programação.
Add to Conversation
