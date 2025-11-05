# Exercícios para Praticar

Esta seção contém exercícios progressivos para você praticar os conceitos de variáveis e referências.

## 🌱 Nível Iniciante

### Exercício 1: Dados Pessoais
Crie um módulo que declare variáveis com seus dados pessoais e os imprima.

**Requisitos:**
- Nome (use `vector<u8>` ou `b"texto"`)
- Idade (use `u8`)
- Altura em centímetros (use `u64`)
- Tem carteira de motorista (use `bool`)
- Cidade natal

**Template:**
```move
module exercicios::dados_pessoais {
    use std::debug::print;
    use std::string::utf8;
    
    fun meus_dados() {
        // Declare suas variáveis aqui
        
        // Imprima todas elas
    }
    
    #[test]
    fun teste_dados() {
        meus_dados();
    }
}
```

### Exercício 2: Calculadora Básica
Crie variáveis com dois números e calcule soma, subtração, multiplicação e divisão.

**Requisitos:**
- Use dois números de sua escolha
- Calcule todas as operações básicas
- Imprima os resultados

### Exercício 3: Conversor de Temperatura
Crie um programa que converta uma temperatura de Celsius para Fahrenheit.

**Fórmula:** `F = C * 9 / 5 + 32`

**Dica:** Use valores em Celsius como 0, 25, 100

---

## 🌿 Nível Básico

### Exercício 4: Loja de Doces
Simule uma loja de doces onde você calcula o troco.

**Requisitos:**
- Preço do doce: R$ 3,50 (represente como 350 centavos)
- Cliente pagou: R$ 5,00 (represente como 500 centavos)
- Calcule e imprima o troco

### Exercício 5: Sistema de Pontos
Crie um sistema onde um jogador ganha e perde pontos.

**Requisitos:**
- Comece com 100 pontos
- Ganhe 50 pontos por completar uma missão
- Perca 20 pontos por erro
- Simule 3 missões completadas e 1 erro
- Imprima a pontuação final

### Exercício 6: Contador de Passos
Simule um contador de passos de um celular.

**Requisitos:**
- Comece com 0 passos
- Adicione passos em diferentes momentos do dia
- Calcule quantos passos faltam para a meta de 10.000
- Use constante para a meta

---

## 🌳 Nível Intermediário

### Exercício 7: Sistema de Referências Básico
Crie funções que usem referências para ler e modificar valores.

**Requisitos:**
- Função `dobrar_numero(numero: &mut u64)` que dobra o valor
- Função `eh_par(numero: &u64): bool` que verifica se é par
- Função `imprimir_info(numero: &u64)` que imprime o número
- Teste com diferentes valores

**Template:**
```move
module exercicios::referencias_basicas {
    use std::debug::print;
    use std::string::utf8;
    
    fun dobrar_numero(numero: &mut u64) {
        // Implemente aqui
    }
    
    fun eh_par(numero: &u64): bool {
        // Implemente aqui (dica: use % 2 == 0)
        false // placeholder
    }
    
    fun imprimir_info(numero: &u64) {
        // Implemente aqui
    }
    
    fun testar_funcoes() {
        let mut valor = 5;
        
        // Use as funções aqui
    }
    
    #[test]
    fun teste() {
        testar_funcoes();
    }
}
```

### Exercício 8: Calculadora com Histórico
Crie uma calculadora que mantém o resultado acumulado.

**Requisitos:**
- Função `somar_ao_total(total: &mut u64, valor: u64)`
- Função `subtrair_do_total(total: &mut u64, valor: u64)`
- Função `multiplicar_total(total: &mut u64, valor: u64)`
- Função `resetar_total(total: &mut u64)`
- Simule uma sequência de operações

### Exercício 9: Sistema de Vida de Personagem
Crie um sistema RPG simples para gerenciar vida de personagem.

**Requisitos:**
- Vida máxima: 100 (constante)
- Função `curar(vida_atual: &mut u64, pontos_cura: u64)` (não pode passar do máximo)
- Função `receber_dano(vida_atual: &mut u64, dano: u64)`
- Função `esta_vivo(vida_atual: &u64): bool`
- Função `mostrar_status(vida_atual: &u64)`
- Simule uma batalha com cura e dano

---

## 🌲 Nível Avançado

### Exercício 10: Sistema Bancário Completo
Crie um sistema bancário com múltiplas operações.

**Requisitos:**
- Saldo inicial
- Função `depositar(saldo: &mut u64, valor: u64)`
- Função `sacar(saldo: &mut u64, valor: u64): bool` (retorna se foi possível)
- Função `transferir(saldo_origem: &mut u64, saldo_destino: &mut u64, valor: u64): bool`
- Função `calcular_juros(saldo: &mut u64, taxa_percentual: u64)`
- Função `mostrar_extrato(saldo: &u64, operacao: vector<u8>)`
- Simule várias operações

### Exercício 11: Loja com Desconto
Crie um sistema de loja com cálculo de desconto.

**Requisitos:**
- Função `calcular_subtotal(preco: &u64, quantidade: u64): u64`
- Função `aplicar_desconto(valor: &mut u64, percentual: u64)`
- Função `calcular_imposto(valor: &mut u64, taxa: u64)`
- Função `eh_cliente_vip(compras_anteriores: &u64): bool` (VIP se > 1000)
- Cliente VIP tem 10% de desconto extra
- Simule uma compra completa

### Exercício 12: Sistema de Inventário Avançado
Crie um inventário com diferentes tipos de itens.

**Requisitos:**
- Três tipos de itens: armas, armaduras, consumíveis
- Cada tipo tem limite diferente (5, 3, 20)
- Função `adicionar_item(quantidade: &mut u64, novos: u64, limite: u64): bool`
- Função `usar_consumivel(consumiveis: &mut u64, quantidade: u64): bool`
- Função `equipar_item(armas: &mut u64, armaduras: &mut u64): bool` (usa 1 de cada)
- Função `mostrar_inventario_completo`
- Simule aventura completa com coleta e uso de itens

---

## 🎯 Desafios Extras

### Desafio 1: Sistema de Notas Escolares
Crie um sistema que calcule média de notas e determine aprovação.

**Requisitos:**
- 4 notas bimestrais
- Média mínima: 7.0 (use 70 para evitar decimais)
- Função para calcular média
- Função para verificar aprovação
- Função para calcular quanto precisa na recuperação

### Desafio 2: Simulador de Corrida
Crie um simulador onde corredores avançam posições.

**Requisitos:**
- 3 corredores começam na posição 0
- A cada "turno", cada corredor avança um número aleatório de posições (simule com valores fixos)
- Meta: chegar à posição 100
- Mostre o progresso de cada corredor
- Declare o vencedor

### Desafio 3: Sistema de Energia de Jogo
Crie um sistema de energia que regenera com o tempo.

**Requisitos:**
- Energia máxima: 100
- Cada ação consome energia
- Energia regenera 10 pontos por "turno"
- Diferentes ações custam energia diferente
- Sistema de cooldown para ações especiais

---

## 📝 Gabarito e Dicas

### Dicas Gerais:
1. **Comece sempre pelos exercícios mais simples**
2. **Teste cada função individualmente** antes de juntar tudo
3. **Use `print()` para acompanhar os valores** durante a execução
4. **Leia os erros do compilador** - eles são muito informativos
5. **Experimente quebrar o código de propósito** para entender as regras

### Para Referências:
- Use `&` quando só precisa **ler** o valor
- Use `&mut` quando precisa **modificar** o valor
- Lembre-se: `*referencia` para acessar o valor
- Uma variável pode ter várias referências `&` OU uma referência `&mut`

### Para Variáveis:
- **Primeira declaração:** sempre com `let`
- **Modificação:** sem `let`
- **Shadowing:** novo `let` com mesmo nome cria nova variável
- **Escopo:** variáveis só existem no bloco onde foram criadas

---

## 🚀 Como Submeter Seus Exercícios

1. Crie arquivos `.move` na pasta `exercicios/`
2. Use a estrutura: `exercicio_01.move`, `exercicio_02.move`, etc.
3. Teste cada exercício com `sui move test`
4. Commit suas soluções no seu repositório GitHub

**Boa sorte e divirta-se programando! 🎉**
