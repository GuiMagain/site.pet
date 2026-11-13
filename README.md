# Pet&Style: Otimização de Performance

Landing page estática de uma loja de acessórios, rações e brinquedos para pets. O projeto foi analisado com o Lighthouse (modo Mobile) antes e depois de um conjunto de otimizações, com o objetivo de medir o progresso com números.

## Gargalos identificados (relatório inicial)

Na primeira análise, os principais pontos foram:

- **Imagem do logo muito pesada**: um PNG de 1024 × 1024 px, de 1,7 MB, exibido em cerca de 128 px.
- **Imagens de produto em `.jfif`**, com nomes contendo acentos e espaços, o que causava falhas de caminho.
- **Tailwind carregado pelo CDN de desenvolvimento** (`cdn.tailwindcss.com`), que bloqueia a renderização e gera o aviso "should not be used in production" no console.
- **Imagens sem `width` e `height`**, o que causava layout shift.
- **Imagens sem `loading="lazy"`**, mesmo as que estavam abaixo da dobra.
- **Erros de HTML**: `<main>` ausente, `</section>` faltando na seção de contato, `</div>` faltando na grid de produtos e uma classe com erro de digitação (`shadow-md0`).
- **Contraste insuficiente** nos botões laranja e nos preços.
- **Favicon ausente**, gerando erro 404 no navegador.
- **`robots.txt` ausente**, o que afetava a auditoria de SEO.

## Melhorias aplicadas

1. **Imagens convertidas para WebP**: o logo foi redimensionado para 256 px, e as imagens de produto foram convertidas e renomeadas sem acentos.
2. **Dimensões explícitas** (`width` e `height`) em todas as imagens, com os valores reais medidos nos arquivos.
3. **`loading="lazy"` e `decoding="async"`** nas imagens de produto, que estão abaixo da dobra. O logo e o hero não receberam lazy loading, por estarem visíveis de início.
4. **HTML corrigido**: `<main>` envolvendo o conteúdo, `</section>` e `</div>` faltantes adicionados e erro de digitação corrigido.
5. **Contraste melhorado**: botões e preços trocados de `orange-500`/`orange-600` para `orange-700`, que passa no mínimo de 4,5:1.
6. **Favicon e `robots.txt`** adicionados.
7. **Links de navegação** apontando para as seções reais da página (`#produtos` e `#contato`).

## Comparativo antes e depois

Cada categoria foi medida em pelo menos três execuções, no Lighthouse, modo Mobile, com o servidor local reiniciado antes de cada rodada. Os valores abaixo são as medianas.

| Categoria | Antes | Depois |
|---|---|---|
| Desempenho | 87 | 88 |
| Acessibilidade | 89 | 92 |
| Práticas recomendadas | 74 | 78 |
| SEO | 91 | 100 |

| Métrica | Antes | Depois |
|---|---|---|
| First Contentful Paint | 0,6 s (1ª execução) | 2,3 s |
| Largest Contentful Paint | 1,9 s (1ª execução) | 2,3 s |
| Total Blocking Time | 130 ms (1ª execução) | 290 ms a 450 ms |
| Cumulative Layout Shift | 0 | 0 |
| Speed Index | 0,7 s (1ª execução) | 2,3 s |

Observação: as métricas de "antes" vêm de medições com variação grande (o Desempenho foi de 81 a 92 na mesma versão). Por isso a comparação de Desempenho deve ser lida com cautela.

**Economia de imagens (auditoria "Melhorar a entrega de imagens")**: caiu de 1.668 KiB para 21 KiB estimados. Esse é o ganho mais claro do projeto.

**Acessibilidade e SEO**: melhoraram de forma consistente em todas as execuções, por conta das mudanças de HTML, contraste e metadados.

## Próximos passos

- **Substituir o CDN do Tailwind por CSS compilado** com o Tailwind CLI. Esse é o principal gargalo restante, responsável pelo bloqueio de renderização e pelo aviso de produção, e a tendência é reduzir o TBT.
- **Converter o hero para WebP** e aplicar `fetchpriority="high"`, para tentar reduzir o LCP.
- **Avaliar a remoção dos arquivos não utilizados** na pasta `pet_img/` (`caminha.jfif` e `download.jfif`).

## Relatórios

- [Relatório antes: print ou PDF do Lighthouse]
- [Relatório depois: print ou PDF do Lighthouse]

## Como rodar

O projeto é estático e não precisa de instalação. Para visualizar:

```bash
cd PetTailWind
python3 -m http.server 8000
```

Depois acesse `http://localhost:8000` no navegador.