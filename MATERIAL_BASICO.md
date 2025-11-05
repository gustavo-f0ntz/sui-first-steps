# Material Básico: Primeiros Passos com Move

Este material foi criado para te guiar nos primeiros contatos com a linguagem Move na blockchain Sui. Vamos começar do básico e evoluir gradualmente.

## 📚 Índice

1. [O que é Move?](#o-que-é-move)
2. [Estrutura Básica de um Módulo](#estrutura-básica-de-um-módulo)
3. [Variáveis em Move](#variáveis-em-move)
4. [Referências: O Conceito Fundamental](#referências-o-conceito-fundamental)
5. [Exercícios Práticos](#exercícios-práticos)
6. [Próximos Passos](#próximos-passos)

---

## O que é Move?

Move é uma linguagem de programação criada especificamente para desenvolvimento de **contratos inteligentes** (smart contracts) em blockchains. Na Sui, utilizamos Move para criar programas que são executados na blockchain.

### Conceitos Básicos:
- **Módulo**: A unidade básica de organização do código
- **Pacote**: Conjunto de módulos relacionados
- **Função**: Blocos de código que executam tarefas específicas
- **Teste**: Forma de verificar se nosso código funciona corretamente

---

## Estrutura Básica de um Módulo

Todo código Move é organizado em módulos. A estrutura básica é:

```move
module endereco::nome_do_modulo {
    // Importações (use statements)
    use std::debug::print;
    
    // Constantes (valores fixos)
    const MINHA_CONSTANTE: u64 = 42;
    
    // Funções
    fun minha_funcao() {
        // código aqui
    }
    
    // Testes
    #[test]
    fun meu_teste() {
        minha_funcao();
    }
}
```

### Exemplo Prático:
```move
module meuendereco::hello_world {
    use std::debug::print;
    use std::string::utf8;
    
    fun dizer_ola() {
        print(&utf8(b"Olá, mundo do Move!"));
    }
    
    #[test]
    fun teste_ola() {
        dizer_ola();
    }
}
```

---

## Variáveis em Move

### 1. Declaração Básica

Em Move, declaramos variáveis com a palavra-chave `let`:

```move
let nome_da_variavel = valor;
```

**Exemplos:**
```move
let idade = 25;
let nome = b"João";
let ativo = true;
```

### 2. Tipos de Dados Básicos

| Tipo | Exemplo | Descrição |
|------|---------|-----------|
| `u8` | `let num: u8 = 255;` | Número inteiro de 0 a 255 |
| `u64` | `let num: u64 = 1000000;` | Número inteiro maior |
| `bool` | `let ativo = true;` | Verdadeiro ou falso |
| `vector<u8>` | `let texto = b"Hello";` | Sequência de bytes (texto) |
| `address` | `let addr = @0x1;` | Endereço na blockchain |

### 3. Constantes

Constantes são valores que **nunca mudam** e são definidas no nível do módulo:

```move
// Constantes SEMPRE começam com letra maiúscula
const MAXIMO_USUARIOS: u64 = 1000;
const NOME_APP: vector<u8> = b"MeuApp";
const TAXA_PADRAO: u64 = 5;
```

### 4. Nomeação de Variáveis

**✅ Permitido:**
```move
let idade = 20;
let nome_completo = b"João Silva";
let _variavel_nao_usada = 1;
let contador1 = 0;
```

**❌ Não permitido:**
```move
let Idade = 20;        // Não pode começar com maiúscula
let nome-completo = 1; // Não pode usar hífen
let 1contador = 0;     // Não pode começar com número
```

### 5. Múltiplas Declarações

Você pode declarar várias variáveis ao mesmo tempo:

```move
let (x, y, z) = (10, 20, 30);
// x = 10, y = 20, z = 30

let (nome, idade) = (b"Ana", 25);
// nome = "Ana", idade = 25
```

### 6. Reatribuição de Valores

Para modificar uma variável existente, **não** use `let` novamente:

```move
let contador = 0;
contador = 5;        // ✅ Correto
contador = contador + 1; // ✅ Correto

let contador = 10;   // ✅ Isso é "shadowing" - cria uma nova variável
```

### 7. Escopo de Variáveis

Variáveis só existem dentro do bloco onde foram criadas:

```move
fun exemplo_escopo() {
    let x = 10;
    
    {
        let y = 20;
        // Aqui posso usar tanto x quanto y
        let soma = x + y; // soma = 30
    }
    
    // Aqui só posso usar x
    // y não existe mais!
    // print(&y); // ❌ ERRO!
}
```

---

## Referências: O Conceito Fundamental

**Referências** são uma das partes mais importantes do Move. Elas nos permitem "emprestar" valores sem transferir a propriedade.

### Por que Referências Existem?

Imagine que você tem um livro. Você pode:
1. **Dar o livro** para alguém (transferir propriedade)
2. **Emprestar o livro** para alguém ler (referência imutável)
3. **Deixar alguém escrever no seu livro** (referência mutável)

### Tipos de Referências

| Símbolo | Nome | O que faz |
|---------|------|-----------|
| `&` | Referência imutável | Permite apenas ler o valor |
| `&mut` | Referência mutável | Permite ler e modificar o valor |

### Exemplos Práticos

#### 1. Lendo um Valor (Referência Imutável)

```move
fun exemplo_leitura() {
    let numero = 42;
    
    // Criando uma referência imutável
    let ref_numero = &numero;
    
    // Podemos ler o valor através da referência
    print(ref_numero); // Imprime: 42
    
    // Mas não podemos modificar
    // *ref_numero = 50; // ❌ ERRO!
}
```

#### 2. Modificando um Valor (Referência Mutável)

```move
fun exemplo_modificacao() {
    let mut numero = 10;
    
    // Criando uma referência mutável
    let ref_mut_numero = &mut numero;
    
    // Podemos ler
    print(ref_mut_numero); // Imprime: 10
    
    // E também modificar!
    *ref_mut_numero = 20;
    
    print(&numero); // Imprime: 20 (o valor original mudou!)
}
```

#### 3. Passando para Funções

```move
fun imprimir_valor(valor: &u64) {
    print(valor);
}

fun dobrar_valor(valor: &mut u64) {
    *valor = *valor * 2;
}

fun exemplo_funcoes() {
    let mut meu_numero = 5;
    
    // Empresta para leitura
    imprimir_valor(&meu_numero); // Imprime: 5
    
    // Empresta para modificação
    dobrar_valor(&mut meu_numero);
    
    imprimir_valor(&meu_numero); // Imprime: 10
}
```

### Regras Importantes das Referências

1. **Uma variável pode ter múltiplas referências imutáveis OU uma referência mutável**
   ```move
   let numero = 10;
   let ref1 = &numero;
   let ref2 = &numero;  // ✅ OK - múltiplas imutáveis
   
   let mut outro = 20;
   let ref_mut = &mut outro;
   // let ref_mut2 = &mut outro; // ❌ ERRO - só uma mutável por vez
   ```

2. **Não pode ter referência imutável e mutável ao mesmo tempo**
   ```move
   let mut numero = 10;
   let ref_imut = &numero;
   // let ref_mut = &mut numero; // ❌ ERRO!
   ```

### Operadores de Referência

| Operação | Sintaxe | Descrição |
|----------|---------|-----------|
| Criar referência imutável | `&valor` | Empresta valor para leitura |
| Criar referência mutável | `&mut valor` | Empresta valor para modificação |
| Ler através de referência | `*referencia` | Obtém o valor apontado |
| Escrever através de referência | `*referencia = novo_valor` | Modifica o valor apontado |

---

## Exercícios Práticos

### Exercício 1: Variáveis Básicas
Crie um módulo que declare diferentes tipos de variáveis e as imprima:

```move
module exercicios::variaveis_basicas {
    use std::debug::print;
    use std::string::utf8;
    
    const MINHA_IDADE: u8 = 25;
    
    fun praticar_variaveis() {
        // Declare uma variável com seu nome
        let meu_nome = b"Seu Nome Aqui";
        
        // Declare uma variável booleana
        let gosto_de_programar = true;
        
        // Declare um contador
        let contador = 0;
        
        // Imprima todas as variáveis
        print(&utf8(meu_nome));
        print(&gosto_de_programar);
        print(&contador);
        print(&MINHA_IDADE);
    }
    
    #[test]
    fun teste_variaveis() {
        praticar_variaveis();
    }
}
```

### Exercício 2: Modificando Valores
```move
module exercicios::modificar_valores {
    use std::debug::print;
    
    fun contador_simples() {
        let contador = 0;
        print(&contador); // Deve imprimir 0
        
        contador = contador + 1;
        print(&contador); // Deve imprimir 1
        
        contador = contador + 5;
        print(&contador); // Deve imprimir 6
    }
    
    #[test]
    fun teste_contador() {
        contador_simples();
    }
}
```

### Exercício 3: Referências Básicas
```move
module exercicios::referencias_basicas {
    use std::debug::print;
    
    fun somar_com_referencia(a: &u64, b: &u64): u64 {
        *a + *b
    }
    
    fun dobrar_numero(numero: &mut u64) {
        *numero = *numero * 2;
    }
    
    fun praticar_referencias() {
        let x = 10;
        let y = 20;
        
        // Usando referências para somar
        let soma = somar_com_referencia(&x, &y);
        print(&soma); // Deve imprimir 30
        
        // Modificando através de referência
        let mut z = 5;
        dobrar_numero(&mut z);
        print(&z); // Deve imprimir 10
    }
    
    #[test]
    fun teste_referencias() {
        praticar_referencias();
    }
}
```

---

## Próximos Passos

Agora que você entende variáveis e referências, está pronto para explorar:

1. **Tipos Primitivos** (`backend/03_tipos_primitivos/`)
   - Números, booleanos, endereços
   - Conversões entre tipos

2. **Condicionais** (`backend/04_condicionais/`)
   - if/else statements
   - Lógica condicional

3. **Vetores** (`backend/05_vetores/`)
   - Listas de dados
   - Manipulação de coleções

4. **Strings** (`backend/06_strings/`)
   - Trabalhando com texto

5. **Structs** (`backend/07_structs/`)
   - Criando seus próprios tipos de dados

---

## 💡 Dicas para Iniciantes

1. **Pratique sempre**: Execute os códigos e experimente modificações
2. **Leia os erros**: O compilador Move é muito útil e explica os problemas
3. **Use `print()` para debug**: Ajuda a entender o que está acontecendo
4. **Comece simples**: Não tente criar códigos complexos no início
5. **Peça ajuda**: A comunidade Move/Sui é muito acolhedora

---

## 🚀 Para Executar os Exemplos

1. Navegue até o diretório do exemplo:
   ```bash
   cd backend/01_variables
   ```

2. Execute os testes:
   ```bash
   sui move test
   ```

3. Veja os resultados no terminal!

---

**Boa sorte na sua jornada com Move! 🎉**
