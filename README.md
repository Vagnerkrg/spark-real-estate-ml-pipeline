# Previsão de Preços de Imóveis Residenciais com Apache Spark

Este repositório contém o desenvolvimento de um pipeline completo de Engenharia de Dados, Análise Exploratória e Machine Learning (Modelos de Regressão) focado no mercado imobiliário do Rio de Janeiro. O projeto foi construído utilizando o ecossistema Apache Spark (PySpark) de forma local e isolada.

---

## 🧠 1. O Problema de Negócio e Engenharia de Recursos

O objetivo principal consiste em extrair informações de uma base de dados bruta em formato JSON, realizar limpezas estruturais de Big Data e treinar modelos preditivos capazes de estimar o preço de mercado de apartamentos residenciais com base em suas características (área útil, quartos, banheiros, vagas de garagem, valores de condomínio e IPTU).

### 🛠️ Etapas do Pipeline de Dados:
* **Ingestão Nativa:** Abertura e parsing de estruturas JSON complexas e aninhadas (`struct`).
* **Filtragem e Seleção:** Expansão de colunas em primeiro nível via expressões de seleção e remoção de dados comerciais ou irrelevantes (`.drop()`).
* **Casting e Tipagem:** Conversão de variáveis textuais nativas para formatos numéricos (`Integer` e `Double`) necessários para as operações matemáticas dos algoritmos.
* **Feature Engineering:** Aplicação de Variáveis Dummy (One-Hot Encoding) para mapear categorias sem gerar ponderações arbitrárias ou vieses de pesos no modelo.
* **Vetorização:** Consolidação das variáveis numéricas em um vetor denso único de características (`features`) utilizando a classe `VectorAssembler`.

---

## 🔬 2. Modelagem e Performance Estatística

Foram implementados e comparados dois algoritmos de Machine Learning robustos fornecidos pela biblioteca Spark MLlib:
1. Árvore de Decisão para Regressão (*Decision Tree*)
2. Floresta Aleatória (*Random Forest*) baseada em Métodos Ensemble (Bagging)

### 📊 Avaliação de Métricas Obtidas:
O desempenho dos algoritmos foi medido com rigor matemático através do R² (Coeficiente de Determinação) e do RMSE (Raiz do Erro Quadrático Médio). A validação do modelo foi blindada contra sobreajuste (*Overfitting*) utilizando a técnica de Validação Cruzada (*Cross Validation*) K-Fold, garantindo que o algoritmo teste sua eficiência em múltiplas combinações rotativas da base de dados.

* **Resultado Prático:** O modelo final treinado demonstrou uma capacidade preditiva sólida, explicando mais de 77% da variabilidade dos preços dos imóveis residenciais nos dados de teste independentes.

---

## 🔧 3. Arquitetura de Infraestrutura e Soluções Técnicas Locais

Para fins de portfólio de Engenharia de Dados sênior, o projeto abdicou do uso de cadernos em nuvem simplificados (como Google Colab) e foi totalmente implantado em ambiente local isolado via VS Code (`.venv`). Foram mapeados e superados diversos desafios de integração profunda entre sistemas operacionais e APIs Java:

1. **Configuração do SDK e Variáveis de Ambiente:** Injeção manual das rotas do Java 11 da Microsoft (`JAVA_HOME`) e das dependências nativas do Hadoop (`HADOOP_HOME`) diretamente no interpretador ativo do Python.
2. **Binários do Windows File System:** Acoplamento do utilitário `winutils.exe` dentro da árvore raiz do sistema de arquivos (`C:\hadoop\bin`), concedendo ao Spark as permissões de administrador necessárias para simular clusters distribuídos em partições NTFS locais.
3. **Bypass de Segurança de Rede:** Travamento da sessão do Spark nas diretivas de host local (`127.0.0.1` / `localhost`) e desligamento estratégico da interface web corporativa, mitigando bloqueios silenciosos e loops gerados pelo Windows Defender Firewall.
4. **Bypass de Serialização no Python 3.12 (A Resolução do Bug do Pickle):** Durante a fase final de inferência de novos dados, foi identificado um conflito de bytecode entre a biblioteca interna `cloudpickle` do PySpark antigo e o interpretador moderno do Python 3.12 (gerando erro de *IndexError*). A solução técnica consistiu em aplicar uma estratégia nativa de arquitetura de dados: o dicionário foi exportado como um arquivo JSON físico local e carregado diretamente pelo Spark usando o leitor em Java nativo (`spark.read.json()`), contornando o fluxo de serialização do Python e garantindo a execução em 0.0 segundos com estabilidade máxima.

---

## 📁 4. Como Executar o Projeto

1. Certifique-se de possuir o Java 11 e o Hadoop configurados em sua máquina Windows.
2. Ative o ambiente virtual isolado:
   ```powershell
   .\.venv\Scripts\Activate.ps1
   ```
3. Instale as dependências travadas e homologadas:
   ```powershell
   pip install -r requirements.txt
   ```
4. Abra o VS Code e execute o arquivo do caderno Jupyter: `01_regressao.ipynb`.

