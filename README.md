# Aleatorizador de Quiz em Rust e WebAssembly

## O que é o projeto?
Uma biblioteca de alta performance para embaralhar questões e alternativas de um quiz, compilada em WebAssembly para uso em qualquer projeto web moderno. Desenvolvida em Rust e compilada em WebAssembly, esse projeto foi desenvolvido visando oferecer uma solução rápida e segura para aleatorização de dados no lado do cliente.


## Por que WebAssembly?
A escolha de criar esta biblioteca compilada para WebAssembly (Wasm) foi intencional e trouxe várias vantagens para o projeto:

* Performance: Para um quiz com centenas ou milhares de perguntas, o processo de embaralhar tudo em JavaScript poderia causar lentidão. Com o WebAssembly, essa tarefa é executada em velocidade quase nativa, garantindo uma experiência de usuário fluida e instantânea.

* Portabilidade e Reuso: O módulo .wasm gerado pode ser executado em qualquer lugar que suporte WebAssembly: todos os navegadores modernos, Node.js no backend, e até mesmo em outros ambientes. A lógica principal não está presa ao ecossistema JavaScript.

* Segurança: O WebAssembly é executado em um ambiente "sandbox" (uma caixa de areia segura) no navegador. Isso significa que ele não tem acesso direto ao sistema do usuário, oferecendo um nível de segurança e isolamento que é ideal para bibliotecas de terceiros.

Em resumo, usamos WebAssembly para delegar o "trabalho pesado" de processamento a uma linguagem mais performática, mantendo a flexibilidade e a facilidade de integração de uma aplicação web.


## Recursos
* Aleatorização de Perguntas: A ordem das questões é completamente embaralhada a cada chamada.

* Aleatorização de Alternativas: Para cada questão, a ordem das alternativas também é embaralhada.

* Permutação Garantida: A biblioteca garante que todas as questões fornecidas serão utilizadas uma única vez, sem repetições, até o final da lista.

* Desacoplado: A lógica de embaralhar (Wasm) é totalmente separada dos dados (JSON), permitindo que você atualize as perguntas sem precisar recompilar a biblioteca.


## Como Usar
Para integrar o Aleatorizador de Quiz em seu projeto, siga os passos abaixo.


## Pré-requisitos
Um servidor web local. O navegador, por razões de segurança (CORS), não permite carregar módulos WebAssembly diretamente do sistema de arquivos (usando o protocolo file:///). A forma mais simples de iniciar um servidor é com Python: python -m http.server.

* Passo 1: Estrutura dos Dados
A biblioteca espera receber os dados em formato JSON, seguindo esta estrutura de array de objetos:

perguntas.json

JSON

    [  
      {  
        "pergunta": "Qual a capital do Brasil?",
        "alternativas": [  
          "Rio de Janeiro",  
          "São Paulo",  
          "Brasília",  
          "Belo Horizonte"  
        ],  
        "resposta_correta": "Brasília"  
      },  
      {  
        "pergunta": "Qual o sistema operacional mais usado no mundo?",  
        "alternativas": [  
          "MacOS",  
          "DOS",  
          "Windows",  
          "Android"  
        ],  
        "resposta_correta": "Android"  
      },  
      {  
        "pergunta": "Qual o maior planeta do sistema solar?",  
        "alternativas": [  
          "Terra",  
          "Júpiter",  
          "Saturno",  
          "Marte"  
        ],  
        "resposta_correta": "Júpiter"  
      }  
    ]  


* Passo 2: Instalação
Copie a pasta pkg gerada pelo wasm-pack para a raiz do seu projeto web.

* Passo 3: Chamando a Biblioteca
No seu arquivo JavaScript, você precisa importar a função da biblioteca, buscar o conteúdo do seu JSON e passar os dados para a função.

main.js

JavaScript

    import init, { embaralhar_perguntas } from './pkg/aleatorizador.js';  
    
    async function run() {  
        // 1. Inicializa o módulo WebAssembly  
        await init();  

        // Exemplo de uso com um botão  
        const botao = document.getElementById('meuBotao');  
        botao.addEventListener('click', async () => {  
            // 2. O JavaScript busca o arquivo JSON  
            const resposta = await fetch('./perguntas.json');  
            const json_string = await resposta.text();  
      
            // 3. O conteúdo do JSON é passado para a função WebAssembly  
            const perguntasEmbaralhadas = embaralhar_perguntas(json_string);  
              
            // 4. Agora você pode usar o array de perguntas!  
            console.log(perguntasEmbaralhadas);  
            // Ex: exibir na tela...  
        });  
        }  
  
    run();  


Lembre-se de carregar seu script no HTML como um módulo: <script type="module" src="./main.js"></script>.


## Solução de Problemas Comuns
* Erro de MIME Type ou CORS: Este erro geralmente significa que você está abrindo o index.html diretamente no navegador. Solução: Use um servidor local.

* As perguntas não são exibidas na tela (container is null): Verifique se o id do elemento no seu HTML é exatamente o mesmo que você está usando no document.getElementById() do JavaScript.

Agradeço por usar essa biblioteca e aguardo o feedback e/ou sugestões de melhoria.

Não esqueça de me referenciar como autor da biblioteca.
