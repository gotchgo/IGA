# No Pain, Same Gain — IGA

[English](README.md) | **Português (BR)** | [Русский](README.ru.md) | [中文](README.zh-CN.md)

**Interference-Gated Aggregation (IGA)** — um modelo recorrente de memória crescente cujo estado recorrente é **constante no comprimento da sequência**, sem custo de perplexidade, com recall igual ao da atenção total na tarefa-padrão mais difícil da literatura. Constrói sobre e aprimora a formulação fundacional de memória crescente GRM / Memory Caching (Behrouz et al., 2026).

**Autor:** Fábio de Oliveira Peres — Pesquisador Independente, Rio Verde, Goiás, Brasil.
**Preprint, junho de 2026.** Licença: **CC BY 4.0**.

## Arquivos
- `paper.pdf` — o manuscrito (Abstract → References), compilado.
- `paper.tex` — código-fonte LaTeX.
- `references.bib` — bibliografia (autores verificados nos registros do arXiv).

## A afirmação em uma linha
RNNs de memória crescente [Behrouz et al., 2026] recuperam a recorrência, mas colapsam no multi-query associative recall (MQAR), e seu estado cresce sem limite. A IGA corrige o colapso (aplica um gate ao estado *não-normalizado* por segmento dentro de uma razão global única, com a memória fixa como um piso de inicialização demonstrável), roteia por *identidade aprendida* via um roteador polinomial de tensor-sketch (sem ler token IDs), e torna o estado **constante em T** (uma fatoração de posto ≤ S remove o termo d² exatamente; memória limitada limita o crescimento) — 0,39 MB em d=64, 15× menor que o KV cache projetado para 1 trilhão de parâmetros, sem perda de recall na tarefa principal. No harness oficial Zoology MQAR, configuração-padrão mais difícil (512,64): IGA 99,2 vs Mamba 23,2 e Based 79,5, empatando com a atenção softmax total 100,0. Em um modelo de linguagem real em nível de caractere (TinyStories, 3 seeds, dois contextos) a IGA empata com um baseline de memória fixa de mesma largura em bits-por-caractere, dentro do ruído entre seeds — **memória constante sem custo de perplexidade**.

## O que o título significa
O título nomeia exatamente o que as medições sustentam: *no pain* — a memória crescente e o passo O(T²) são removidos, deixando um estado constante e computação O(T) por passo; *same gain* — o recall iguala a atenção total na tarefa-padrão mais difícil e a perplexidade iguala o baseline de memória fixa em texto real.

## Limitações honestas (paper §8)
A vantagem de recall associativo é específica da tarefa (não transfere para um probe de fato plantado em texto natural em d=128); um ganho preliminar de ~0,016 bpc relatado em outro lugar *não* reproduz sob o protocolo de 3 seeds, 4000 passos; escalar d/ctx para surgir uma separação de recall em texto real ficou como trabalho futuro (limitado por orçamento de GPU). Os claims de recall são em pequena escala (uma camada, d=64); o experimento de modelo de linguagem é a única configuração em texto real.

## Reprodutibilidade
Este lançamento inicial contém **apenas o manuscrito**. O código para reproduzir os experimentos será lançado separadamente. O §9 (Reproduction) do paper lista os comandos e o corpus usado (TinyStories baixado; nenhum dado sintético de modelo de linguagem é gerado).

## Licença
Manuscrito: **Creative Commons Attribution 4.0 (CC BY 4.0)**.