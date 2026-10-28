# Meteoro de Lógica

Aplicação web para consultar, estudar e executar exercícios de lógica de programação das apostilas Manzano e Faccat, com soluções em VisualG e JavaScript no navegador, catálogo organizado, editor interativo e testes automatizados.

[Acessar a aplicação](https://gilvamoliveira.github.io/meteoro-de-logica/)

## Demonstração

![Navegação pelo catálogo Manzano e execução de um exercício em JavaScript no Meteoro de Lógica](./img/demo.gif)

## Recursos

- Catálogo organizado por apostila, página e exercício.
- Alternância entre os códigos VisualG e JavaScript de cada exercício.
- Editor e terminal interativo para testar as duas linguagens no navegador.
- Cadastro, edição e remoção de exercícios.
- Persistência das alterações no armazenamento local do navegador.
- Acesso às apostilas em PDF.
- Testes automatizados do interpretador, do catálogo e do executor JavaScript.

## Executar localmente

O projeto é estático e não exige instalação de dependências. Inicie um servidor HTTP na pasta do projeto:

```sh
python -m http.server 8000
```

Abra `http://localhost:8000` no navegador. Um servidor local é necessário para habilitar a execução JavaScript em Web Worker.

Na página inicial, selecione Manzano ou Faccat. Também é possível usar os atalhos **M** e **F** para navegar entre as apostilas.

## Trabalhar com um exercício

Selecione um cartão para abrir o editor. O menu **Exibir código** alterna entre VisualG e JavaScript. Os campos são independentes: editar ou executar uma linguagem não altera o código da outra.

Use **Executar** para testar a linguagem selecionada. O campo de entrada atende chamadas `Leia()` do VisualG e `await readLine("Pergunta")` do JavaScript. `write()`, `writeln()` e `console.log()` enviam a saída JavaScript ao terminal do exercício. O botão **Parar** interrompe a execução.

O interpretador VisualG implementa os comandos e expressões usados nos exemplos do projeto, incluindo `interrompa`, e não diferencia maiúsculas de minúsculas nos nomes de variáveis. O JavaScript roda em um Web Worker temporário, separado da página, com limite de execução de 10 segundos e de 100.000 caracteres de saída.

## Estrutura do projeto

| Caminho | Conteúdo |
| --- | --- |
| `index.html` | Página inicial e navegação |
| `pages/manzano.html`, `pages/faccat.html` | Catálogos e editores das apostilas |
| `pages/pdf-*.html` | Visualizadores das apostilas em PDF |
| `css/` | Estilos das páginas, do editor e do teclado inicial |
| `data/exercises.js` | Grupos e intervalos de exercícios |
| `data/exercise-details.js` | Títulos e enunciados complementares |
| `data/visualg-matches.js` | Exemplos de código VisualG |
| `data/javascript-matches.js` | Exemplos de código JavaScript |
| `js/visualg-interpreter.js` | Interpretador VisualG |
| `js/javascript-runner.js` | Execução JavaScript em Web Worker |
| `js/exercises.js` | Interface, edição, persistência e terminal |
| `js/main.js` | Navegação e atalhos da página inicial |
| `tests/` | Testes automatizados com `node:test` |
| `pdfs/` | Apostilas em PDF |

## Testes

Os testes exigem Node.js 20 ou superior e não instalam dependências:

```sh
npm test
```

Eles verificam o interpretador VisualG, a consistência do catálogo e dos exemplos, a sintaxe das soluções JavaScript e o executor de código (incluindo crases e interpolação dentro dos exercícios). O workflow em `.github/workflows/ci.yml` executa a mesma suíte a cada push e pull request.

## Adicionar exercícios e soluções

Para incluir exercícios na lista padrão:

1. Acrescente a página, o assunto e o intervalo correspondente em `data/exercises.js`.
2. Cadastre títulos e enunciados em `data/exercise-details.js`, quando disponíveis.
3. Associe os códigos em `data/visualg-matches.js` e `data/javascript-matches.js` usando o ID do exercício.

Os IDs seguem os formatos `manzano-página-letra`, como `manzano-46-A`, e `faccat-página-número`, como `faccat-4-5`. Cada entrada de código é um objeto com a propriedade `code`. Para entrada JavaScript, use `await readLine("Pergunta")`; para saída, use `write()`, `writeln()` ou `console.log()`.

O catálogo inclui 84 exercícios com exemplos nas duas linguagens. Ao abrir um exercício, os códigos padrão são carregados mesmo quando há uma versão local antiga sem código JavaScript.

## Dados locais

As alterações feitas pelo editor são armazenadas no navegador e permanecem após recarregar a página. Esses dados são específicos do navegador e do dispositivo. A lista padrão do projeto é definida pelos arquivos em `data/`.

Exercícios padrão removidos também permanecem excluídos após recarregar a página. Para voltar à lista original, limpe os dados do site no navegador ou remova a chave `meteoro-exercises-manzano` ou `meteoro-exercises-faccat` do `localStorage`.

---

Autor: Gilvam Oliveira · [LinkedIn](https://www.linkedin.com/in/gilvam-oliveira/)
