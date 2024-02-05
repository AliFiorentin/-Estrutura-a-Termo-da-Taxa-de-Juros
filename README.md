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
Para calcular os parâmetros do modelo de Svensson, define-se como nosso item de estimação, a taxa à vista de um título do Tesouro Direto Prefixado. Títulos de curta maturidade são menos afetados por mudanças nas taxas de juros em comparação com títulos de maturidades mais longas. Uma pequena flutuação nos preços de títulos de curto prazo pode ter um grande impacto nas taxas de juros. 
