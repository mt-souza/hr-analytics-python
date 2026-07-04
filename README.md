# HR Analytics — Análise de Rotatividade e Desempenho

Análise exploratória de uma base de Recursos Humanos com 311 colaboradores, construída para entender o que está por trás da rotatividade da empresa, como salário e performance se relacionam, e onde os times de RH deveriam olhar primeiro.

![Python](https://img.shields.io/badge/Python-3.x-3776AB?style=flat&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-black?style=flat&logo=pandas)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=flat&logo=numpy&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-11557c?style=flat)
![Seaborn](https://img.shields.io/badge/Seaborn-4c72b0?style=flat)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=flat&logo=jupyter&logoColor=white)

## Sobre o projeto

O ponto de partida foi simples: uma base de RH com informações de contratação, desligamento, salário, performance e engajamento de 311 funcionários, e a pergunta "o que dá pra aprender daqui que ajudaria a empresa a reter gente boa e gastar melhor com folha de pagamento?".

Rotatividade é caro. Contratar, treinar e integrar um novo funcionário custa tempo e dinheiro, e quando isso acontece de forma recorrente em certos cargos ou departamentos, geralmente é sintoma de algo estrutural — não coincidência. Essa análise tenta separar o que é ruído do que é padrão, cruzando turnover com salário, satisfação, engajamento, performance e participação em projetos especiais.

O projeto segue uma lógica bem prática: primeiro entender e limpar os dados, depois explorar o perfil demográfico da empresa, para então ir fundo nos temas que mais importam pra gestão — salário, desempenho, absenteísmo e rotatividade — terminando com uma leitura cruzada entre esses fatores.

## Índice

- [Sobre o projeto](#sobre-o-projeto)
- [O dataset](#o-dataset)
- [Ferramentas](#ferramentas)
- [Limpeza e preparação dos dados](#limpeza-e-preparação-dos-dados)
- [O que foi analisado](#o-que-foi-analisado)
- [KPIs calculados](#kpis-calculados)
- [Principais achados](#principais-achados)
- [Recomendações](#recomendações)
- [Estrutura do projeto](#estrutura-do-projeto)
- [Como rodar](#como-rodar)
- [Próximos passos](#próximos-passos)
- [Autor](#autor)

## O dataset

A base utilizada é o clássico **HRDataset_v14**, amplamente usado em estudos de People Analytics, contendo 311 registros e 36 colunas com dados de identificação, demografia, cargo, salário, performance e histórico de contratação/desligamento.

Algumas das colunas mais relevantes para a análise:

| Coluna | Descrição |
|---|---|
| `Salary` | Salário anual do funcionário |
| `Department` / `Position` | Departamento e cargo ocupado |
| `PerformanceScore` | Avaliação de desempenho (Exceeds, Fully Meets, Needs Improvement, PIP) |
| `EngagementSurvey` | Nota de engajamento (0 a 5) |
| `EmpSatisfaction` | Nota de satisfação do funcionário (0 a 5) |
| `Termd` | Indica se o funcionário foi desligado (1) ou está ativo (0) |
| `DateofHire` / `DateofTermination` | Datas de contratação e desligamento |
| `SpecialProjectsCount` | Quantidade de projetos especiais em que o funcionário participou |
| `DaysLateLast30` / `Absences` | Atrasos e faltas registradas |
| `ManagerName` | Gestor responsável pelo funcionário |

## Ferramentas

| Tecnologia | Uso |
|---|---|
| Python | Linguagem principal da análise |
| Pandas | Limpeza, transformação e agregação dos dados |
| NumPy | Suporte a operações numéricas |
| Matplotlib / Seaborn | Visualização dos gráficos |
| Jupyter Notebook | Ambiente de desenvolvimento e documentação |

## Limpeza e preparação dos dados

Antes de qualquer análise, a base passou por uma checagem básica de qualidade:

- **Nulos**: as únicas colunas com valores ausentes foram `DateofTermination` (funcionários ainda ativos) e `ManagerID` (cargos de topo sem um gestor cadastrado) — em nenhum dos dois casos isso representa erro nos dados.
- **Duplicados**: nenhuma linha ou `EmpID` duplicado foi encontrado.
- **Registros inconsistentes**: dois nomes fictícios ("Voldemort, Lord" e "Corleone, Vito") foram identificados e removidos da base, reduzindo o total de 311 para 309 registros válidos analisados no restante do notebook.
- **Datas**: as colunas de data (`DOB`, `DateofHire`, `DateofTermination`, `LastPerformanceReview_Date`) estavam como texto e foram convertidas para o formato datetime, incluindo uma correção de século em datas de nascimento que vieram com ano futuro por conta do formato de dois dígitos.
- **Outliers**: `Salary`, `EngagementSurvey`, `EmpSatisfaction`, `Absences` e `DaysLateLast30` foram inspecionados via `describe()` — os valores mais altos de salário são condizentes com cargos de liderança, sem outliers artificiais a tratar.

A partir daí, duas colunas foram criadas para viabilizar boa parte da análise: `Idade` (calculada a partir da data de nascimento) e `TempoEmpresaAnos` (tempo de casa, considerando a data de desligamento quando aplicável ou a data de referência para funcionários ativos).

## O que foi analisado

**Perfil demográfico** — distribuição por sexo, estado civil, etnia, departamento, cargo, estado e faixa etária.

**Análise salarial** — salário médio por departamento, cargo, gênero e nível de performance, além dos funcionários mais e menos remunerados.

**Performance** — distribuição das avaliações, performance por departamento, gestor e cargo, e sua relação com satisfação.

**Satisfação e engajamento** — médias por departamento, cargo e gestor, e como se comportam frente aos diferentes níveis de performance.

**Absenteísmo** — atrasos e faltas por departamento, e quais funcionários mais se destacam nesses indicadores.

**Turnover** — taxa geral e recortada por departamento, cargo e gestor.

**Projetos especiais** — comparação entre quem participou e quem não participou de projetos especiais, em salário, engajamento, satisfação e performance.

**Cruzamentos finais** — satisfação x salário, engajamento x salário, e atrasos/faltas x performance.

## KPIs calculados

| KPI | Valor |
|---|---|
| Total de funcionários | 309 |
| Funcionários ativos | 207 |
| Funcionários desligados | 103 (~33% de turnover) |
| Salário médio | ≈ US$ 69 mil/ano |
| Tempo médio de empresa | ≈ 10 anos |
| Idade média | ≈ 47 anos |
| Satisfação média | 3,9 / 5,0 |
| Engajamento médio | 4,1 / 5,0 |

## Principais achados

**Turnover é alto em cargos técnicos específicos, não distribuído de forma uniforme.**
Cargos como *Principal Data Architect*, *Data Analyst* e *Enterprise Architect* aparecem com 100% de rotatividade, enquanto departamentos inteiros como *Executive Office* não registraram nenhum desligamento no período. Isso é um sinal mais forte do que a taxa geral de 33% sozinha — o problema está concentrado, não espalhado.

**Salário parece mais ligado a hierarquia do que a performance.**
A variação salarial entre diferentes notas de avaliação de desempenho é pequena, o que sugere que a remuneração acompanha o nível do cargo muito mais do que o quão bem a pessoa está performando nele.

**Participar de projetos especiais está associado a salários mais altos e melhores avaliações.**
Funcionários que passaram por pelo menos um projeto especial têm remuneração significativamente maior e mais avaliações "Fully Meets" — mas a diferença em satisfação e engajamento entre os dois grupos é pequena, indicando que o impacto aparece mais no bolso do que na percepção do funcionário sobre o trabalho.

**"Executive Office" é o setor com maiores salários e também com a satisfação mais baixa.**
Com média de satisfação de 3,0 (a menor entre os departamentos), essa combinação de bom salário e baixa satisfação é um ponto de atenção específico para a liderança da empresa.

**Faltas e atrasos não se concentram em nenhum departamento nem se relacionam claramente com performance.**
Isso indica que o absenteísmo tende a ser mais uma questão individual do que um problema estrutural de algum setor específico.

**A performance média dos gestores está entre 2,7 e 3,2 — abaixo do esperado.**
Esse número, combinado ao turnover elevado em times técnicos, levanta a hipótese de que parte da rotatividade possa estar relacionada à qualidade da gestão, não apenas ao cargo em si.
