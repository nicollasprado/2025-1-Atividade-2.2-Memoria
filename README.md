# S.O. 2025.1 - Atividade 2.02 - Gestão de memória

## Informações gerais

- **Objetivo do repositório**: Repositório para atividade avaliativa dos alunos
- **Assunto**: Gestão de memória
- **Público alvo**: alunos da disciplina de SO (Sistemas Operacionais) do curso de TADS (Superior em Tecnologia em Análise e Desenvolvimento de Sistemas) no CNAT-IFRN (Instituto Federal de Educação, Ciência e Tecnologia do Rio Grande do Norte - Campus Natal-Central).
- disciplina: **SO** [Sistemas Operacionais](https://github.com/sistemas-operacionais/)
- professor: [Leonardo A. Minora](https://github.com/leonardo-minora)
- aluno: Nicollas Matheus Prado Pinheiro
- github: https://github.com/nicollasprado

## Tarefas do aluno
1. Fork desse repositório e atualizar a linha 10 com o nome e link do github
2. Ler a descrição da atividade
3. Montar a resposta no final deste arqivo, no tópico **Resposta**

---

## 1. Descrição da atividade
### 1.1. Objetivo
Praticar os conceitos de alocação de memória (best-fit), memória virtual e desfragmentação em um sistema com memória limitada.

---

### 1.2. Contexto
Um computador possui apenas **64 KB de RAM** e um **disco rígido para memória virtual**. O sistema operacional deve gerenciar 5 processos com tamanhos diferentes, cuja soma ultrapassa a capacidade da RAM.

#### 1.2.1. Processos iniciais

| Processo | Tamanho (KB) |
|----------|-------------|
| P1       | 20          |
| P2       | 15          |
| P3       | 25          |
| P4       | 10          |
| P5       | 18          |
| **Total**| **88 KB**   |

- **Memória RAM**: 64 KB (contígua, inicialmente vazia).  
- **Memória Virtual (Disco)**: Espaço ilimitado para paginação.

#### 1.2.2. Alocação Inicial com Best-Fit
Os alunos devem simular a alocação dos processos na RAM usando o algoritmo **best-fit**.  
- A memória RAM será representada como um bloco contíguo (ex: `[0KB - 64KB]`).  
- Devem alocar os processos nos menores espaços livres que atendam ao seu tamanho.  

**Alocação inicial**:  
1. P1 (20 KB) → Ocupa [0-20].  
2. P2 (15 KB) → Ocupa [20-15].  
3. _continuar a partir daqui_

#### 1.2.3. Simular Memória Virtual (Paginação)
- Os processos não alocados na RAM devem ser "paginados" no disco.  
- Criar uma tabela de páginas indicando quais partes estão na RAM e quais estão no disco.  

#### 1.2.4. Desfragmentação da RAM
- Desfragmentar a RAM para liberar espaço contíguo.
- Após desfragmentação (compactação), verificar quais processos podem ser alocado.  

### 1.3. Questões para Reflexão
1. Best-fit foi mais eficiente que first-fit ou worst-fit neste cenário?  
2. Como a memória virtual evitou um deadlock?  
3. Qual o impacto da desfragmentação no desempenho do sistema?  

---

## Resposta

### 1. Alocação Inicial com Best-Fit
Estado inicial: um único bloco livre [0 – 64] (64 KB).

Alocar P1 (20 KB)

Blocos livres disponíveis: [0–64] (64 KB).

O menor bloco que cabe é [0–64] (64 KB).

Aloca P1 em [0 – 20] (20 KB).

Novo bloco livre: [20 – 64] (44 KB).

Alocar P2 (15 KB)

Blocos livres: [20–64] (44 KB).

O menor bloco que cabe é [20–64].

Aloca P2 em [20 – 35] (20 + 15 = 35).

Novo bloco livre: [35 – 64] (29 KB).

Alocar P3 (25 KB)

Blocos livres: [35–64] (29 KB).

29 KB ≥ 25 KB então cabe; é o menor que existe.

Aloca P3 em [35 – 60] (35 + 25 = 60).

Novo bloco livre: [60 – 64] (4 KB).

Alocar P4 (10 KB)

Blocos livres: [60–64] (4 KB).

4 KB < 10 KB → não cabe. Com Best-Fit, não há outro bloco que caiba.

P4 vai para Memória Virtual (disco).

Alocar P5 (18 KB)

Blocos livres: [60–64] (4 KB).

4 KB < 18 KB → não cabe.

P5 vai para Memória Virtual (disco).

### 2. Simular Memória Virtual (Paginação)
Vou usar páginas de 4 KB (tamanho comum em exemplos). Calculo o número de páginas digito a digito:

P1 = 20 KB → 20 / 4 = 5, portanto 5 páginas (cabem todas na RAM).

P2 = 15 KB → 15 / 4 = 3,75 → arredonda para cima → 4 páginas.

P3 = 25 KB → 25 / 4 = 6,25 → 7 páginas.

P4 = 10 KB → 10 / 4 = 2,5 → 3 páginas.

P5 = 18 KB → 18 / 4 = 4,5 → 5 páginas.

Total de páginas por processo:

P1: 5 páginas

P2: 4 páginas

P3: 7 páginas

P4: 3 páginas

P5: 5 páginas

### 3. Desfragmentação da RAM
Para alocar P4 (10 KB) seriam necessários 10 KB contíguos: atualmente só há 4 KB. Poderíamos:

Swap out (mover para disco) parte ou páginas inteiras de P1/P2/P3 para liberar ≥ 10 KB contíguos, ou

Terminar/encerrar algum processo residente (libera espaço), ou

Usar carregamento por páginas (demand paging): carregar apenas páginas necessárias de P4 conforme execução, mas isso não aloca o processo inteiro na RAM.

### 4. Questões para Reflexão
a) Best-fit foi mais eficiente que first-fit ou worst-fit neste cenário?

No cenário dado, best-fit colocou precisamente P1, P2 e P3 e deixou apenas um fragmento pequeno (4 KB). Comparado ao worst-fit (que buscaria maiores buracos) o best-fit tende a reduzir o desperdício imediato, mas pode deixar muitos pequenos buracos ao longo do tempo. First-fit possivelmente produziria resultado parecido se a ordem for a mesma. Não há um vencedor universal — depende da sequência de chegadas e dos tamanhos.

b) Como a memória virtual evitou um deadlock?

Memória virtual (paginação/swap) permite que processos maiores existam mesmo sem toda a sua memória física presente: suas páginas menos usadas ficam no disco e páginas ativas são trazidas sob demanda. Assim, o sistema evita que todos os processos precisem simultaneamente de mais memória física do que existe (o que poderia levar a um impasse), porque páginas podem ser movidas para o disco e trazidas conforme necessidade. Em outras palavras: VM aumenta a aparente memória disponível e permite trocar páginas para evitar bloqueios por falta total de RAM.

c) Qual o impacto da desfragmentação no desempenho do sistema?

Prós: pode criar blocos contíguos suficientes para alocar processos grandes, reduz fragmentação externa.

Contras: custa tempo de CPU e cópias de memória (movimentar dados), pode causar pausas ou degradação do desempenho durante a compactação. Em sistemas com muita troca (swap), o ganho pode ser ofuscado pelo overhead. Em resumo: útil quando necessário, mas caro se feito frequentemente.
