# Erros Comuns e Como Resolver

Este guia ajuda a identificar e resolver os erros mais comuns ao aprender variáveis e referências em Move.

## 🚨 Erros com Variáveis

### Erro 1: Redeclaração Desnecessária
```move
// ❌ ERRO: Usando 'let' para modificar variável existente
let contador = 0;
let contador = contador + 1; // Este 'let' não é necessário

// ✅ CORRETO: Modificação simples
let contador = 0;
contador = contador + 1;

// ✅ OU Shadowing intencional (criar nova variável)
let contador = 0;
let contador = contador + 1; // Nova variável com mesmo nome
```

**Mensagem de erro típica:**
```
unused variable: `contador`
```

**Como resolver:**
- Se quer modificar: remova o `let`
- Se quer criar nova variável: mantenha o `let` (shadowing)

### Erro 2: Variável Não Inicializada
```move
// ❌ ERRO: Variável sem valor inicial
let idade; // Move exige valor na declaração

// ✅ CORRETO: Sempre inicialize
let idade = 25;
```

### Erro 3: Nome de Variável Inválido
```move
// ❌ ERRO: Nomes inválidos
let Idade = 25;     // Não pode começar com maiúscula
let 1contador = 0;  // Não pode começar com número
let meu-nome = ""; // Não pode ter hífen

// ✅ CORRETO: Nomes válidos
let idade = 25;
let contador1 = 0;
let meu_nome = b"João";
```

### Erro 4: Uso de Variável Fora do Escopo
```move
// ❌ ERRO: Tentando usar variável fora do escopo
fun exemplo() {
    {
        let x = 10;
    }
    print(&x); // x não existe aqui!
}

// ✅ CORRETO: Declare no escopo adequado
fun exemplo() {
    let x = 10;
    {
        print(&x); // x está acessível aqui
    }
}
```

---

## 🚨 Erros com Referências

### Erro 5: Esqueceu de Usar Referência
```move
// ❌ ERRO: Passando valor direto para função que espera referência
fun imprimir(valor: &u64) {
    print(valor);
}

fun teste() {
    let numero = 42;
    imprimir(numero); // Erro: precisa de referência
}

// ✅ CORRETO: Use & para criar referência
fun teste() {
    let numero = 42;
    imprimir(&numero);
}
```

**Mensagem de erro típica:**
```
expected type `&u64`, found type `u64`
```

### Erro 6: Tentando Modificar Através de Referência Imutável
```move
// ❌ ERRO: Tentando modificar com referência imutável
fun tentar_modificar(valor: &u64) {
    *valor = 100; // Erro: & é só para leitura
}

// ✅ CORRETO: Use referência mutável
fun modificar(valor: &mut u64) {
    *valor = 100;
}
```

**Mensagem de erro típica:**
```
cannot assign to immutable reference
```

### Erro 7: Múltiplas Referências Mutáveis
```move
// ❌ ERRO: Duas referências mutáveis ao mesmo tempo
fun erro_multiplas_ref() {
    let mut numero = 10;
    let ref1 = &mut numero;
    let ref2 = &mut numero; // Erro: já existe uma ref mutável
    
    *ref1 = 20;
    *ref2 = 30;
}

// ✅ CORRETO: Use uma por vez
fun correto_uma_ref() {
    let mut numero = 10;
    {
        let ref1 = &mut numero;
        *ref1 = 20;
    } // ref1 termina aqui
    
    let ref2 = &mut numero; // Agora pode criar outra
    *ref2 = 30;
}
```

### Erro 8: Referência Imutável e Mutável Simultaneamente
```move
// ❌ ERRO: Ref imutável e mutável juntas
fun erro_mista() {
    let mut numero = 10;
    let ref_leitura = &numero;
    let ref_escrita = &mut numero; // Erro!
    
    print(ref_leitura);
    *ref_escrita = 20;
}

// ✅ CORRETO: Use sequencialmente
fun correto_sequencial() {
    let mut numero = 10;
    
    // Primeiro, só leitura
    let ref_leitura = &numero;
    print(ref_leitura);
    // ref_leitura termina aqui (implicitamente)
    
    // Depois, escrita
    let ref_escrita = &mut numero;
    *ref_escrita = 20;
}
```

### Erro 9: Esqueceu o * para Acessar Valor da Referência
```move
// ❌ ERRO: Comparando referência com valor
fun erro_sem_asterisco() {
    let numero = 10;
    let ref_numero = &numero;
    
    if (ref_numero == 10) { // Erro: comparando &u64 com u64
        print(&utf8(b"Igual"));
    }
}

// ✅ CORRETO: Use * para acessar o valor
fun correto_com_asterisco() {
    let numero = 10;
    let ref_numero = &numero;
    
    if (*ref_numero == 10) { // Compara u64 com u64
        print(&utf8(b"Igual"));
    }
}
```

---

## 🚨 Erros de Tipo

### Erro 10: Tipo Inferido Incorretamente
```move
// ❌ ERRO: Sistema não consegue inferir o tipo
fun erro_tipo() {
    let vetor = vector::empty(); // Erro: vetor de que tipo?
}

// ✅ CORRETO: Especifique o tipo
fun correto_tipo() {
    let vetor: vector<u64> = vector::empty();
    // OU
    let vetor = vector::empty<u64>();
}
```

### Erro 11: Mistura de Tipos Numéricos
```move
// ❌ ERRO: Misturando tipos numéricos
fun erro_tipos_numericos() {
    let pequeno: u8 = 100;
    let grande: u64 = 1000;
    let soma = pequeno + grande; // Erro: u8 + u64
}

// ✅ CORRETO: Converta os tipos
fun correto_conversao() {
    let pequeno: u8 = 100;
    let grande: u64 = 1000;
    let soma = (pequeno as u64) + grande; // Converte u8 para u64
}
```

---

## 🚨 Erros com Constantes

### Erro 12: Constante com Nome Inválido
```move
// ❌ ERRO: Nome de constante inválido
const minha_constante: u64 = 100; // Deve começar com maiúscula

// ✅ CORRETO: Sempre maiúsculas
const MINHA_CONSTANTE: u64 = 100;
const MAXIMO_USUARIOS: u64 = 1000;
```

### Erro 13: Tentando Modificar Constante
```move
// ❌ ERRO: Constantes são imutáveis
const CONTADOR: u64 = 0;

fun erro_modificar_constante() {
    CONTADOR = CONTADOR + 1; // Erro: constantes não mudam
}

// ✅ CORRETO: Use variável para valores que mudam
fun correto_usar_variavel() {
    let contador = CONTADOR; // Copia valor da constante
    contador = contador + 1; // Modifica a variável
}
```

---

## 🛠️ Dicas para Debugging

### 1. Use Prints para Rastrear Valores
```move
fun debug_exemplo() {
    let mut numero = 10;
    print(&utf8(b"Valor inicial:"));
    print(&numero);
    
    numero = numero * 2;
    print(&utf8(b"Apos multiplicar:"));
    print(&numero);
}
```

### 2. Teste Cada Função Separadamente
```move
// Ao invés de testar tudo junto:
#[test]
fun teste_tudo() {
    funcao1();
    funcao2();
    funcao3(); // Se falhar, difícil saber qual
}

// Teste separadamente:
#[test]
fun teste_funcao1() {
    funcao1();
}

#[test]
fun teste_funcao2() {
    funcao2();
}
```

### 3. Simplifique Expressões Complexas
```move
// ❌ Difícil de debugar:
let resultado = (a + b) * (c - d) / (e + f);

// ✅ Mais fácil de debugar:
let soma1 = a + b;
let subtracao = c - d;
let soma2 = e + f;
let produto = soma1 * subtracao;
let resultado = produto / soma2;
```

---

## 📋 Checklist para Evitar Erros

Antes de executar seu código, verifique:

### ✅ Variáveis:
- [ ] Todas as variáveis foram inicializadas com `let`?
- [ ] Nomes de variáveis começam com letra minúscula ou `_`?
- [ ] Reatribuições não usam `let` desnecessariamente?
- [ ] Variáveis estão sendo usadas no escopo correto?

### ✅ Referências:
- [ ] Funções que só leem dados usam `&`?
- [ ] Funções que modificam dados usam `&mut`?
- [ ] Não há múltiplas referências mutáveis simultâneas?
- [ ] Não há mistura de referências imutáveis e mutáveis?
- [ ] Uso `*` para acessar valores de referências quando necessário?

### ✅ Constantes:
- [ ] Nomes de constantes estão em MAIÚSCULAS?
- [ ] Constantes não estão sendo modificadas?
- [ ] Valores das constantes são conhecidos em tempo de compilação?

### ✅ Tipos:
- [ ] Tipos estão especificados quando o compilador não consegue inferir?
- [ ] Não há mistura de tipos incompatíveis?
- [ ] Conversões de tipo são feitas quando necessário?

---

**Lembre-se: Erros são normais e fazem parte do aprendizado! O compilador Move é seu amigo e dá dicas muito úteis. 🤝**
