# Análise de Segurança de Imagem Docker

## Objetivo

Realizar uma análise de segurança em uma imagem Docker utilizando uma ferramenta de análise de vulnerabilidades, identificando vulnerabilidades conhecidas, seus níveis de severidade e os componentes afetados.

## Imagem analisada

* **Imagem:** `nginx:alpine`
* **Fonte:** Docker Hub
* **Ferramenta:** Grype
* **Versão do Grype:** 0.120.1
* **Sistema:** Linux amd64
* **Digest da imagem:**

```text
sha256:df221db836e1754089190208cee7eeda94f233197056426eda74a43ab1abeac2
```

A imagem foi obtida utilizando:

```bash
docker pull nginx:alpine
```

## Análise

A análise foi realizada com:

```bash
grype nginx:alpine
```

O Grype identificou vulnerabilidades em componentes presentes na imagem, principalmente pacotes do Alpine Linux.

Entre os componentes afetados estão:

* `tiff`
* `zlib`
* `libcrypto3`
* `libssl3`
* `libexpat`
* `nghttp2-libs`
* `busybox`
* `pcre2`
* `libpng`

Os resultados apresentaram diferentes níveis de severidade: **High, Medium, Low e Unknown**.

### Principais vulnerabilidades encontradas

| Vulnerabilidade | Componente           | Versão instalada | Severidade | Versão corrigida |
| --------------- | -------------------- | ---------------- | ---------- | ---------------- |
| CVE-2023-52356  | tiff                 | 4.7.1-r0         | High       | —                |
| CVE-2023-6277   | tiff                 | 4.7.1-r0         | Medium     | —                |
| CVE-2026-85091  | zlib                 | 1.3.2-r0         | High       | 1.3.2-r1         |
| CVE-2026-54873  | libcrypto3 / libssl3 | 3.5.8-r0         | High       | —                |
| CVE-2026-93990  | libexpat             | 2.8.4-r0         | High       | 2.8.5-r0         |
| CVE-2026-84782  | libcrypto3 / libssl3 | 3.5.8-r0         | High       | —                |
| CVE-2026-84784  | libcrypto3 / libssl3 | 3.5.8-r0         | High       | —                |
| CVE-2026-4775   | tiff                 | 4.7.1-r0         | High       | —                |
| CVE-2026-72897  | libcrypto3 / libssl3 | 3.5.8-r0         | High       | —                |
| CVE-2026-103111 | pcre2                | 10.48-r0         | High       | 10.49-r0         |

O resultado completo da análise está disponível em [`results/grype-nginx.txt`](results/grype-nginx.txt).

## Interpretação dos resultados

A análise mostra que, apesar de `nginx:alpine` ser uma imagem oficial e relativamente enxuta, ela possui componentes de software com vulnerabilidades conhecidas.

A presença de vulnerabilidades **High** merece maior atenção, principalmente aquelas relacionadas a componentes como `libssl3`, `libcrypto3`, `tiff`, `zlib`, `libexpat` e `pcre2`.

Também é importante observar que algumas vulnerabilidades possuem uma versão corrigida indicada pelo Grype. Nesses casos, uma das possíveis ações de segurança é atualizar os pacotes afetados ou utilizar uma versão mais atualizada da imagem base.

Entretanto, a existência de uma vulnerabilidade identificada pelo scanner não significa automaticamente que o sistema esteja sendo explorado. O risco real depende de fatores como possibilidade de exploração, exposição do componente, configuração da aplicação e contexto em que a imagem é utilizada.

O Grype também apresenta o **EPSS**, que fornece uma estimativa da probabilidade de exploração de determinadas vulnerabilidades. Essa informação pode auxiliar na priorização das correções.

## O que a ferramenta encontrou e o que esses resultados nos dizem sobre a segurança da imagem?

O Grype encontrou diversas vulnerabilidades conhecidas em bibliotecas e pacotes presentes na imagem `nginx:alpine`, incluindo vulnerabilidades classificadas como **High, Medium e Low**.

Os resultados mostram que uma imagem Docker pode apresentar riscos de segurança mesmo quando utiliza uma distribuição Alpine e uma imagem oficial. Isso ocorre porque a imagem contém diversas dependências que também precisam ser mantidas atualizadas.

A análise permite identificar quais componentes precisam de atenção e quais possuem versões corrigidas disponíveis. Dessa forma, o processo de análise de vulnerabilidades pode ser utilizado como parte de uma estratégia de **DevSecOps**, ajudando a identificar e corrigir problemas antes que uma imagem seja utilizada em produção.

## Ações recomendadas

Com base nos resultados encontrados, algumas ações recomendadas seriam:

1. Atualizar a imagem base para uma versão mais recente quando disponível.
2. Atualizar os pacotes que possuem versões corrigidas.
3. Priorizar inicialmente as vulnerabilidades classificadas como **High**.
4. Avaliar o contexto e a possibilidade real de exploração de cada vulnerabilidade.
5. Executar novamente o Grype após as atualizações para verificar se as vulnerabilidades foram corrigidas.
6. Integrar o scanner de imagens ao pipeline de CI/CD para realizar análises automaticamente.

## Evidências

### Resultado da análise

![Resultado da análise do Grype](evidence/grype-nginx.png)

### Resultado completo

O resultado completo gerado pelo Grype está disponível em:

[`results/grype-nginx.txt`](results/grype-nginx.txt)

## Conclusão

A análise demonstrou que ferramentas de segurança de imagens, como o Grype, são importantes para identificar vulnerabilidades presentes nos componentes de uma imagem Docker.

Mais importante do que apenas identificar a quantidade de vulnerabilidades é interpretar sua severidade, os componentes afetados, a existência de versões corrigidas e o contexto de exploração. Essas informações permitem definir prioridades e tomar decisões de segurança mais adequadas.

Nesse caso, a imagem `nginx:alpine` apresentou vulnerabilidades que devem ser avaliadas e, quando possível, corrigidas por meio da atualização dos componentes ou da própria imagem base.
