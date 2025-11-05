# Exemplos Práticos: Variáveis e Referências

Este arquivo contém exemplos práticos para reforçar o aprendizado sobre variáveis e referências em Move.

## 🎯 Exemplo 1: Loja Simples

Imagine que você está criando uma loja simples. Vamos usar variáveis para representar produtos e seus preços:

```move
module loja::produtos {
    use std::debug::print;
    use std::string::utf8;
    
    // Constantes para os preços (em centavos para evitar decimais)
    const PRECO_NOTEBOOK: u64 = 250000; // R$ 2.500,00
    const PRECO_MOUSE: u64 = 5000;      // R$ 50,00
    const PRECO_TECLADO: u64 = 15000;   // R$ 150,00
    
    fun calcular_total_compra() {
        // Cliente comprou: 1 notebook, 2 mouses, 1 teclado
        let qtd_notebook = 1;
        let qtd_mouse = 2;
        let qtd_teclado = 1;
        
        // Calculando subtotais
        let subtotal_notebook = PRECO_NOTEBOOK * qtd_notebook;
        let subtotal_mouse = PRECO_MOUSE * qtd_mouse;
        let subtotal_teclado = PRECO_TECLADO * qtd_teclado;
        
        // Total da compra
        let total = subtotal_notebook + subtotal_mouse + subtotal_teclado;
        
        print(&utf8(b"=== NOTA FISCAL ==="));
        print(&subtotal_notebook);  // 250000
        print(&subtotal_mouse);     // 10000
        print(&subtotal_teclado);   // 15000
        print(&utf8(b"Total:"));
        print(&total);              // 275000
    }
    
    #[test]
    fun teste_loja() {
        calcular_total_compra();
    }
}
```

## 🎯 Exemplo 2: Contador de Usuários

Vamos simular um sistema que conta usuários online:

```move
module sistema::usuarios {
    use std::debug::print;
    use std::string::utf8;
    
    const LIMITE_USUARIOS: u64 = 100;
    
    fun gerenciar_usuarios() {
        let usuarios_online = 0;
        
        print(&utf8(b"Sistema iniciado"));
        print(&usuarios_online);
        
        // Usuários entrando no sistema
        usuarios_online = usuarios_online + 5;
        print(&utf8(b"5 usuarios entraram"));
        print(&usuarios_online);
        
        usuarios_online = usuarios_online + 10;
        print(&utf8(b"10 usuarios entraram"));
        print(&usuarios_online);
        
        // Alguns usuários saíram
        usuarios_online = usuarios_online - 3;
        print(&utf8(b"3 usuarios sairam"));
        print(&usuarios_online);
        
        // Verificando limite
        let esta_no_limite = usuarios_online >= LIMITE_USUARIOS;
        print(&utf8(b"Esta no limite?"));
        print(&esta_no_limite);
    }
    
    #[test]
    fun teste_usuarios() {
        gerenciar_usuarios();
    }
}
```

## 🎯 Exemplo 3: Sistema de Pontuação de Jogo

```move
module jogo::pontuacao {
    use std::debug::print;
    use std::string::utf8;
    
    const PONTOS_POR_INIMIGO: u64 = 100;
    const BONUS_NIVEL: u64 = 500;
    
    fun calcular_pontuacao_jogador() {
        // Estado inicial do jogador
        let pontuacao_total = 0;
        let nivel_atual = 1;
        let inimigos_derrotados = 0;
        
        print(&utf8(b"=== INICIO DO JOGO ==="));
        print(&pontuacao_total);
        
        // Jogador derrota 3 inimigos
        inimigos_derrotados = 3;
        pontuacao_total = pontuacao_total + (inimigos_derrotados * PONTOS_POR_INIMIGO);
        
        print(&utf8(b"Derrotou 3 inimigos"));
        print(&pontuacao_total); // 300
        
        // Jogador passa de nível
        nivel_atual = 2;
        pontuacao_total = pontuacao_total + BONUS_NIVEL;
        
        print(&utf8(b"Subiu para nivel 2!"));
        print(&pontuacao_total); // 800
        
        // Mais inimigos derrotados
        let novos_inimigos = 5;
        inimigos_derrotados = inimigos_derrotados + novos_inimigos;
        pontuacao_total = pontuacao_total + (novos_inimigos * PONTOS_POR_INIMIGO);
        
        print(&utf8(b"Derrotou mais 5 inimigos"));
        print(&pontuacao_total); // 1300
        print(&utf8(b"Total de inimigos:"));
        print(&inimigos_derrotados); // 8
    }
    
    #[test]
    fun teste_jogo() {
        calcular_pontuacao_jogador();
    }
}
```

## 🎯 Exemplo 4: Referências em Ação - Sistema Bancário Simples

```move
module banco::conta {
    use std::debug::print;
    use std::string::utf8;
    
    // Função que apenas consulta o saldo (referência imutável)
    fun consultar_saldo(saldo: &u64) {
        print(&utf8(b"Saldo atual:"));
        print(saldo);
    }
    
    // Função que deposita dinheiro (referência mutável)
    fun depositar(saldo: &mut u64, valor: u64) {
        print(&utf8(b"Depositando:"));
        print(&valor);
        *saldo = *saldo + valor;
    }
    
    // Função que saca dinheiro (referência mutável)
    fun sacar(saldo: &mut u64, valor: u64) {
        print(&utf8(b"Sacando:"));
        print(&valor);
        if (*saldo >= valor) {
            *saldo = *saldo - valor;
            print(&utf8(b"Saque realizado"));
        } else {
            print(&utf8(b"Saldo insuficiente"));
        }
    }
    
    fun simular_conta_bancaria() {
        let mut meu_saldo = 1000; // Começa com R$ 10,00
        
        print(&utf8(b"=== CONTA BANCARIA ==="));
        
        // Consultar saldo inicial
        consultar_saldo(&meu_saldo);
        
        // Fazer um depósito
        depositar(&mut meu_saldo, 500); // Deposita R$ 5,00
        consultar_saldo(&meu_saldo);
        
        // Fazer um saque
        sacar(&mut meu_saldo, 300); // Saca R$ 3,00
        consultar_saldo(&meu_saldo);
        
        // Tentar sacar mais do que tem
        sacar(&mut meu_saldo, 2000); // Tenta sacar R$ 20,00
        consultar_saldo(&meu_saldo);
    }
    
    #[test]
    fun teste_banco() {
        simular_conta_bancaria();
    }
}
```

## 🎯 Exemplo 5: Calculadora com Referências

```move
module matematica::calculadora {
    use std::debug::print;
    use std::string::utf8;
    
    // Funções que usam referências para não "consumir" os valores
    fun somar(a: &u64, b: &u64): u64 {
        *a + *b
    }
    
    fun subtrair(a: &u64, b: &u64): u64 {
        if (*a >= *b) {
            *a - *b
        } else {
            0 // Evita números negativos
        }
    }
    
    fun multiplicar(a: &u64, b: &u64): u64 {
        *a * *b
    }
    
    fun dividir(a: &u64, b: &u64): u64 {
        if (*b > 0) {
            *a / *b
        } else {
            0 // Evita divisão por zero
        }
    }
    
    // Função que modifica um resultado através de referência
    fun aplicar_desconto(valor: &mut u64, percentual_desconto: u64) {
        let desconto = (*valor * percentual_desconto) / 100;
        *valor = *valor - desconto;
    }
    
    fun teste_calculadora() {
        let numero1 = 20;
        let numero2 = 5;
        
        print(&utf8(b"=== CALCULADORA ==="));
        print(&utf8(b"Numero 1:"));
        print(&numero1);
        print(&utf8(b"Numero 2:"));
        print(&numero2);
        
        // Operações básicas
        let resultado_soma = somar(&numero1, &numero2);
        print(&utf8(b"Soma:"));
        print(&resultado_soma);
        
        let resultado_subtracao = subtrair(&numero1, &numero2);
        print(&utf8(b"Subtracao:"));
        print(&resultado_subtracao);
        
        let resultado_multiplicacao = multiplicar(&numero1, &numero2);
        print(&utf8(b"Multiplicacao:"));
        print(&resultado_multiplicacao);
        
        let resultado_divisao = dividir(&numero1, &numero2);
        print(&utf8(b"Divisao:"));
        print(&resultado_divisao);
        
        // Teste de desconto
        let mut preco = 100;
        print(&utf8(b"Preco original:"));
        print(&preco);
        
        aplicar_desconto(&mut preco, 20); // 20% de desconto
        print(&utf8(b"Preco com desconto:"));
        print(&preco);
    }
    
    #[test]
    fun teste_matematica() {
        teste_calculadora();
    }
}
```

## 🎯 Exemplo 6: Sistema de Inventário

```move
module rpg::inventario {
    use std::debug::print;
    use std::string::utf8;
    
    const LIMITE_ITENS: u64 = 10;
    
    // Função para adicionar itens ao inventário
    fun adicionar_item(quantidade_atual: &mut u64, novos_itens: u64) {
        let novo_total = *quantidade_atual + novos_itens;
        if (novo_total <= LIMITE_ITENS) {
            *quantidade_atual = novo_total;
            print(&utf8(b"Itens adicionados com sucesso"));
        } else {
            print(&utf8(b"Inventario cheio! Nao foi possivel adicionar"));
        }
    }
    
    // Função para usar itens do inventário
    fun usar_item(quantidade_atual: &mut u64, itens_usados: u64) {
        if (*quantidade_atual >= itens_usados) {
            *quantidade_atual = *quantidade_atual - itens_usados;
            print(&utf8(b"Itens usados"));
        } else {
            print(&utf8(b"Nao ha itens suficientes"));
        }
    }
    
    // Função para verificar status do inventário
    fun verificar_inventario(pocoes: &u64, espadas: &u64, escudos: &u64) {
        print(&utf8(b"=== INVENTARIO ==="));
        print(&utf8(b"Pocoes:"));
        print(pocoes);
        print(&utf8(b"Espadas:"));
        print(espadas);
        print(&utf8(b"Escudos:"));
        print(escudos);
        
        let total_itens = *pocoes + *espadas + *escudos;
        print(&utf8(b"Total de itens:"));
        print(&total_itens);
        
        let espacos_livres = LIMITE_ITENS - total_itens;
        print(&utf8(b"Espacos livres:"));
        print(&espacos_livres);
    }
    
    fun simular_inventario() {
        let mut pocoes = 3;
        let mut espadas = 1;
        let mut escudos = 0;
        
        // Status inicial
        verificar_inventario(&pocoes, &espadas, &escudos);
        
        // Encontrou 2 poções
        print(&utf8(b"Encontrou 2 pocoes"));
        adicionar_item(&mut pocoes, 2);
        
        // Comprou 1 escudo
        print(&utf8(b"Comprou 1 escudo"));
        adicionar_item(&mut escudos, 1);
        
        // Usou 1 poção
        print(&utf8(b"Usou 1 pocao"));
        usar_item(&mut pocoes, 1);
        
        // Status final
        verificar_inventario(&pocoes, &espadas, &escudos);
        
        // Tenta adicionar muitos itens
        print(&utf8(b"Tentando adicionar 8 espadas"));
        adicionar_item(&mut espadas, 8);
        
        verificar_inventario(&pocoes, &espadas, &escudos);
    }
    
    #[test]
    fun teste_inventario() {
        simular_inventario();
    }
}
```

---

## 🔑 Pontos-Chave dos Exemplos

### Sobre Variáveis:
1. **Sempre declare com `let`** na primeira vez
2. **Use nomes descritivos** (`usuarios_online` em vez de `x`)
3. **Constantes em MAIÚSCULAS** e no nível do módulo
4. **Reatribuição sem `let`** para modificar valores existentes

### Sobre Referências:
1. **`&` para emprestar sem modificar** (leitura apenas)
2. **`&mut` para emprestar e modificar** (leitura e escrita)
3. **`*referencia` para acessar o valor** apontado pela referência
4. **Funções que não precisam "possuir" os dados** devem usar referências

### Benefícios das Referências:
- **Eficiência**: Não copia dados grandes
- **Segurança**: Previne uso acidental de valores
- **Flexibilidade**: Permite modificação controlada
- **Clareza**: Fica claro quando uma função pode modificar dados

---

## 🚀 Como Usar Estes Exemplos

1. **Copie o código** para um arquivo `.move` em um módulo
2. **Execute com** `sui move test`
3. **Modifique os valores** e veja como o comportamento muda
4. **Experimente remover as referências** e veja os erros
5. **Tente criar seus próprios exemplos** similares

---

**Pratique bastante! A repetição é fundamental para dominar estes conceitos. 🎯**
