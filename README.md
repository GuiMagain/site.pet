# Pet&Style: Otimização de Performance

Landing page estática de uma loja de acessórios, rações e brinquedos para pets. O projeto foi analisado com o Lighthouse (modo Mobile) antes e depois de um conjunto de otimizações, com o objetivo de medir o progresso com números.

## Gargalos identificados (relatório inicial)

- **Logo muito pesado**: PNG de 1024 × 1024 px, de 1,7 MB, exibido em cerca de 128 px.
- **Imagens de produto em `.jfif`**, com acentos e espaços nos nomes, o que causava falhas de caminho.
- **Tailwind carregado pelo CDN de desenvolvimento** (`cdn.tailwindcss.com`): bloqueia a renderização e gera o aviso "should not be used in production".
- **Imagens sem `width` e `height`**, o que causava layout shift.
- **Imagens sem `loading="lazy"`**, mesmo as que estavam abaixo da dobra.
- **Erros de HTML**: `<main>` ausente, `</section>` faltando na seção de contato, `</div>` faltando na grid de produtos e a classe `shadow-md0` com erro de digitação.
- **Contraste insuficiente** nos botões laranja e nos preços.
- **Favicon ausente**, gerando erro 404 no navegador.
- **Arquivos não utilizados** na pasta de imagens (`caminha.jfif`, `download.jfif`).

## Melhorias aplicadas

1. **Imagens convertidas para WebP**: o logo foi redimensionado para 256 px, e as imagens de produto foram convertidas e renomeadas sem acentos.
2. **Dimensões explícitas** (`width` e `height`) em todas as imagens, com os valores reais medidos nos arquivos.
3. **`loading="lazy"` e `decoding="async"`** nas imagens de produto, que estão abaixo da dobra. O logo e o hero não recebem lazy loading, por estarem visíveis de início.
4. **HTML corrigido**: `<main>` envolvendo o conteúdo, `</section>` e `</div>` faltantes adicionados e erro de digitação corrigido.
5. **Contraste melhorado**: botões e preços trocados de `orange-500`/`orange-600` para `orange-700`.
6. **Tailwind compilado com o Tailwind CLI** (v3.4.17, mesma versão do CDN): o `output.css` contém só as classes usadas, e o script do CDN foi removido.
7. **Favicon e `robots.txt`** adicionados.
8. **Arquivos não utilizados removidos**: o PNG original de 1,7 MB e os `.jfif` substituídos foram retirados da pasta `pet_img/`.
9. **Links de navegação** apontando para as seções reais da página (`#produtos` e `#contato`).

## Comparativo antes e depois

Cada versão foi medida no Lighthouse, modo Mobile, com o servidor local reiniciado antes de cada rodada. Os valores são as medianas de pelo menos três execuções.

### Versão 1: inicial (antes das otimizações)

Medições da primeira rodada, com a versão original da página.

| Categoria | Mediana |
|---|---|
| Desempenho | 87 (variou de 81 a 92) |
| Acessibilidade | 89 |
| Práticas recomendadas | 74 |
| SEO | 91 |

| Métrica | Valor (1ª execução) |
|---|---|
| First Contentful Paint | 0,6 s |
| Largest Contentful Paint | 1,9 s |
| Total Blocking Time | 130 ms |
| Cumulative Layout Shift | 0 |
| Speed Index | 0,7 s |

### Versão 2: com imagens otimizadas, HTML corrigido e cores ajustadas (CDN do Tailwind)

| Categoria | Mediana |
|---|---|
| Desempenho | 88 |
| Acessibilidade | 92 |
| Práticas recomendadas | 78 |
| SEO | 100 |

| Métrica | Valor |
|---|---|
| First Contentful Paint | 2,3 s |
| Largest Contentful Paint | 2,3 s |
| Total Blocking Time | 290 ms a 450 ms |
| Cumulative Layout Shift | 0 |
| Speed Index | 2,3 s |

Economia estimada na auditoria "Melhorar a entrega de imagens": caiu de 1.668 KiB (versão 1) para 21 KiB (versão 2).

### Versão 3: com Tailwind CSS compilado (sem CDN)

Três execuções, no Lighthouse 13.5.0, modo Mobile, com o servidor local reiniciado antes de cada rodada (15/11/2026, 17h26, 17h31 e 17h32).

| Execução | Desempenho | Acessibilidade | Práticas | SEO |
|---|---|---|---|---|
| 1ª | 99 | 92 | 78 | 100 |
| 2ª | 100 | 92 | 78 | 100 |
| 3ª | 100 | 92 | 78 | 100 |
| **Mediana** | **100** | **92** | **78** | **100** |

| Métrica | 1ª | 2ª | 3ª | **Mediana** |
|---|---|---|---|---|
| First Contentful Paint | 0,8 s | 0,8 s | 0,8 s | **0,8 s** |
| Largest Contentful Paint | 1,3 s | 1,3 s | 1,3 s | **1,3 s** |
| Total Blocking Time | 100 ms | 0 ms | 50 ms | **50 ms** |
| Cumulative Layout Shift | 0 | 0 | 0 | **0** |
| Speed Index | 0,8 s | 0,8 s | 0,8 s | **0,8 s** |

Economia estimada nas auditorias: entrega de imagens de 27 KiB, cache de 64 KiB e HTTP moderno de 150 ms.

## Leitura dos resultados

- **Desempenho**: subiu de 88 (versão 2) para 100 na mediana. A troca do CDN do Tailwind pelo CSS compilado foi o principal fator, já que o script bloqueante deixou de existir.
- **TBT**: caiu de 290 a 450 ms (versão 2) para 50 ms na mediana, e de 130 ms na primeira medição original. É o efeito direto da remoção do script bloqueante.
- **LCP**: caiu de 2,3 s (versão 2) para 1,3 s, e de 1,9 s na primeira medição original.
- **FCP e Speed Index**: caíram de 2,3 s para 0,8 s em relação à versão 2. Em relação à primeira medição original (0,6 s e 0,7 s), ficaram um pouco acima. A medição original foi uma única execução, então a comparação deve ser lida com cautela.
- **Entrega de imagens**: a economia estimada caiu de 1.668 KiB (versão original) para 27 KiB.
- **Acessibilidade (92)**: ainda aparece a auditoria de contraste, "As cores de primeiro e segundo plano não têm uma taxa de contraste suficiente". É o principal ponto a corrigir a seguir.
- **Práticas recomendadas (78)**: o que pesa são os alertas de HTTPS, que aparecem porque o servidor local roda em HTTP. Na versão publicada com HTTPS, esses alertas devem diminuir.
- **SEO (100)**: estável em todas as versões desde a correção do HTML e do `robots.txt`.

## Relatórios

- [relatorio_antes.pdf](relatorio_antes.pdf): capturas do Lighthouse das medições iniciais
- [relatorio_depois.pdf](relatorio_depois.pdf): relatórios completos do Lighthouse das três execuções da versão 3 (CSS compilado)

## Como rodar

O projeto usa o Tailwind CLI para gerar o `output.css`. Para instalar e compilar:

```bash
npm install
npm run build:css
```

Para visualizar o site, sirva a pasta com um servidor local:

```bash
python3 -m http.server 8000
```

Depois acesse `http://localhost:8000` no navegador.

Para desenvolver com recompilação automática, use `npm run watch:css` em um terminal separado.
