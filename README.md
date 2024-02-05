# Estrutura a Termo da Taxa de Juros

A Estrutura a Termo da Taxa de Juros (também conhecida como *Yield Curve*) é a relação, em um determinado momento, entre as taxas de juros de títulos de renda fixa das mesmas linhas de crédito, mas com vencimentos diferentes. Ela é normalmente formada a partir de títulos que pagam apenas juros no vencimento, ou seja, títulos de cupom zero (ou *zero coupon bonds*). A ETTJ é extremamente importante para os mercados financeiros, visto que é a base para a precificação de instrumentos de renda fixa, em que, também serve como referência para determinar o índice dos demais setores do mercado de dívida.

A ETTJ pode ser construída a partir das taxas à vista, taxas futuras ou pela função desconto, contudo, como todas estão relacionadas entre si, pode-se a partir de uma, determinar ou estimar as outras. Neste trabalho estudam-se os modelos de [Charle R. Nelson e Andrew F. Siegel](http://www.jstor.org/stable/2352957)  e [Lars E. O. Svensson](https://www.elibrary.imf.org/downloadpdf/journals/001/1994/114/001.1994.issue-114-en.xml) , um dos mais utilizados pelo Banco Central do Brasil e por diversos outros bancos centrais, como os bancos centrais da Bélgica, França, Alemanha, Suécia e Suíça.

### O Modelo Nelson e Siegel (1987)
Em seu trabalho "*Parsimonious Modeling of Yield Curves*", propõem o uso da solução da equação diferencial ordinária linear de segunda ordem com valores reais e raízes diferentes para representar uma previsão das taxas a termo. Por exemplo, se a taxa de juros instantânea na maturidade *m*, denotada *f(m)*, é dada pela solução de uma equação diferencial ordinária de segunda ordem com valores reais e raízes iguais, tem-se:

<p align="center">
$f(m)= \beta_0 + \beta_1 e^{ \left(\frac{-m}{\tau}\right)} + \beta_2 \left(\frac{m}{ \tau}\right) e^{\left(\frac{-m}{ \tau}\right)}$
</p>

onde $\tau_1$ e $\tau_2$ são constantes de tempo associadas à equação, e $\beta_0$, $\beta_1$ e $\beta_2$ são determinadas pelas condições iniciais.

### O Modelo de Svensson (1994)
Para a estimativa das taxas à vista e a termo, segundo [J.Huston McCulloch](http://www.jstor.org/stable/2351832) no ajuste para cada termo em uma data de negociação de uma função de desconto, usa-se a equação anterior de Nelson e Siegel , onde assumiram que a taxa a termo instantânea é a solução para a equação ordinária linear de segunda ordem. 

Para aumentar a flexibilidade e melhorar o ajuste, Svensson adicionou um quarto termo à equação de Nelson e Siegel, uma segunda forma com dois parâmetros adicionais, um parâmetro de regressão $\beta_3$ e um parâmetro de tempo $\tau_2$ ($\tau_2$ deve ser positivo). Assim a função estendida passa a ter seis parâmetros:

<p align="center">
$f(m)= \beta_0 + \beta_1 e^{ \left(-\frac{m}{ \tau_1}\right)} + \beta_2 \left(\dfrac{m}{\tau_1}\right)e^{ \left(\frac{-m}{\tau_1}\right)} + \beta_3 \left(\dfrac{-m}{\tau_2}\right) e^{ \left(\frac{-m}{\tau_2}\right)}$
</p>

## Resultados e Simulações
![R](https://img.shields.io/badge/R-276DC3?style=for-the-badge&logo=r&logoColor=white)

Requisitos: R >= v.4.0  
Pacotes: Xts e Zoo.

Para calcular os parâmetros do modelo de Svensson, define-se como nosso item de estimação, a taxa à vista de um título do Tesouro Direto Prefixado. Títulos de curta maturidade são menos afetados por mudanças nas taxas de juros em comparação com títulos de maturidades mais longas. Uma pequena flutuação nos preços de títulos de curto prazo pode ter um grande impacto nas taxas de juros.  A Tabela abaixo contém os títulos do Tesouro Prefixado (LTN) disponíveis no Tesouro Direto no dia 31 de janeiro de 2024.

| Maturidade em dias | Maturidade em anos | Taxa à Vista (\% a.a) |
|:--------------------:|:-------------------:|:-----------------------:|
| 21                 | 0,0833333           | 11,3501                |
| 42                 | 0,166667            | 11,1105                |
| 63                 | 0,25                | 10,8980                |
| 126                | 0,5                 | 10,3988                |
| 252                | 1                   | 9,8538                 |
| 504                | 2                   | 9,6809                 |
| 756                | 3                   | 9,9008                 |
| 1008               | 4                   | 10,1520                |
| 1260               | 5                   | 10,3494                |
| 2.520              | 10                  | 10,7385                |


Considera-se como um ano-base de 252 dias úteis e 21 dias úteis para um mês, como foi estabelecido pelo Banco Central do Brasil.

A partir dos dados coletados da Tabela acima, estima-se por meio do software RStudio os parâmetros $\beta_0$, $\beta_1$, $\beta_2$, $\beta_3$, $\tau_1$ e $\tau_2$. Para isso, usa-se o pacote *YieldCurve* com a função "Svensson(*rate,maturity*)", Desse modo, os valores encontrados foram: $\beta_0=11.21687$, $\beta_1=0.4217495$, $\beta_2=0.5502931$, $\beta_3=-6.163024 $, $\tau_1=0,6041051$ e $\tau_2=0.8364607$. Substituindo os parâmetros calculados na equação de Svensson, obtém-se:

<p align="center">
$F(m)=11.21687 + 0.4217495 \left[\dfrac{1-e^{\left(\frac{-m}{0,604105}\right)}}{\left(\dfrac{m}{0,604105}\right)}\right] + \\
    + 0.5502931 \left[\dfrac{1-e^{\left(\frac{-m}{0,604105}\right)}}{\left(\dfrac{m}{0,604105}\right)} - e^{\left(\frac{-m}{0,604105}\right)}\right] \\
     -6.163024 \left[\dfrac{1-e^{\left(\frac{-m}{0.8364607}\right)}}{\left(\dfrac{m}{0.8364607}\right)} - e^{\left(\frac{-m}{0.8364607}\right)}\right]$
</p>

A ANBIMA (Associação Brasileira das Entidades dos Mercados Financeiro e de Capitais), em sua rotina diária, realiza a divulgação em seu portal oficial os dados de sua ETTJ estimada. Nesse sentido, coleta-se tanto a curva como os parâmetros relacionados divulgados no dia 31/01/2024, permitindo uma comparação com a curva obtida, conforme ilustrado na Figura abaixo.

<p align="center">
Figura: Comparação ETTJ IPCA da ANBIMA com a ETTJ IPCA Estimada.
<p>

<p align="center">
  <img src="https://github.com/AliFiorentin/Estrutura-a-Termo-da-Taxa-de-Juros/assets/131291202/543fb838-2629-4fb3-8841-7b261dd65235" alt="Simulação SVN" />
</p>

## Conclusão
Para a construção da ETTJ foram estudados os modelos de Nelson e Siegel e sua extensão proposta por Svensson para a previsão da curva de juros, onde esses modelos são amplamente utilizados pelo BCB, ANBIMA e por diversos bancos centrais de vários países, pelo fato de serem de fácil implementação e por seu ajuste aos diversos formatos de curvas de taxas de juros.

Utilizando dados do Tesouro Direto sobre as taxas de juros do tesouro prefixado, aplica-se um método computacional utilizando o software RStudio em conjunto com o pacote *YieldCurve* através de regressões lineares que minimizam o modelo de Svensson, para se obter os parâmetros $\beta_0$, $\beta_1$, $\beta_2$, $\beta_3$, $\tau_1$ e $\tau_2$ a fim de construir a ETTJ que descreve a curva de juros da taxa à vista, onde, observa-se que essa curva de juros mostra as taxas de juros de diferentes prazos de vencimento, ou seja, o retorno que um investidor receberia ao comprar e manter esses títulos até seus respectivos vencimentos. Essa estrutura permite que os investidores e analistas avaliem as expectativas do mercado em relação às taxas de juros futuras, bem como a percepção de risco e as condições econômicas.

### Bibliografia
-[Bank for International Settlements](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=1188514)  
-[Measuring the Term Structure of Interest Rates](http://www.jstor.org/stable/2351832)  
-[Estimating and Interpreting Forward Interest Rates](https://www.elibrary.imf.org/downloadpdf/journals/001/1994/114/001.1994.issue-114-en.xml)  
-[Parsimonious Modeling of Yield Curves](http://www.jstor.org/stable/2352957)  
