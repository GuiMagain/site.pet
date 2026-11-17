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

<!-- Preencher com a mediana de pelo menos três execuções após a troca do CDN -->

| Categoria | Mediana |
|---|---|
| Desempenho | a preencher |
| Acessibilidade | a preencher |
| Práticas recomendadas | a preencher |
| SEO | a preencher |

| Métrica | Valor |
|---|---|
| First Contentful Paint | a preencher |
| Largest Contentful Paint | a preencher |
| Total Blocking Time | a preencher |
| Cumulative Layout Shift | a preencher |
| Speed Index | a preencher |

## Leitura dos resultados

- **Entrega de imagens**: é o ganho mais claro do projeto, com a economia estimada caindo de 1.668 KiB para 21 KiB.
- **Acessibilidade e SEO**: melhoraram de forma consistente em todas as execuções da versão 2, por causa das mudanças de HTML, contraste e metadados.
- **Desempenho**: a variação entre execuções foi grande (81 a 92 na versão 1), então a mudança de 87 para 88 não é conclusiva. A versão 3 é a que deve mostrar o efeito da troca do CDN, porque o Tailwind deixa de bloquear a renderização.
- **LCP e FCP**: subiram entre a versão 1 e a versão 2. Como a medição da versão 1 foi de uma única execução, a comparação deve ser lida com cautela.

## Relatórios

- [relatorio_antes.pdf](relatorio_antes.pdf): capturas do Lighthouse das medições iniciais
- [relatorio_depois.pdf](relatorio_depois.pdf): capturas do Lighthouse das medições da versão 2

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
