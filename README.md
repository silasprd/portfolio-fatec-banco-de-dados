# SILAS RAFAEL BARRETO PRADO

<link rel="stylesheet" type="text/css" href="styles.css">

## Introdução

Com formação técnica em Desenvolvimento de Sistemas pelo SENAI em parceria com a Embraer, onde trabalhei com tecnologias como HTML, CSS, Java, JavaScript, NodeJS, Angular e MySQL. Explorando o NodeJS, aprofundei meus conhecimentos em AngularJS e trabalhei com frameworks como Vue e React.

## Contatos

- [GIT](https://github.com/silasprd/)
- [LinkedIn](https://www.linkedin.com/in/silasprd/)

## Meus Principais Conhecimentos

<div style="display: inline_block; font-size: 16px;">
    <img align="center" alt="TS-icon" height="30" width="40" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/typescript/typescript-plain.svg">TypeScript
    <img align="center" alt="TS-icon" height="30" width="40" src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/nodejs/nodejs-original.svg">NodeJS
    <img align="center" alt="TS-icon" height="30" width="40" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/angularjs/angularjs-original.svg">AngularJS
    <img align="center" alt="TS-icon" height="30" width="40" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/html5/html5-original.svg">HTML
    <img align="center" alt="TS-icon" height="30" width="40" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/css3/css3-original.svg">CSS
    <img align="center" alt="JS-icon" height="40" width="40" src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/java/java-plain-wordmark.svg">Java
</div>

## Meus Projetos

### 1º Semestre • 1/2022 - BoardClass

Repositório: [GIT](https://www.git.com/equioe-vox/BoardClass)

<p align="justify">Parceiro Acadêmico: <a href="https://fatecsjc-prd.azurewebsites.net/">Faculdade de Tecnologia de São José dos Campos</a></p>

<img src="Semestre1/images/boardclass.jpeg" widht="600px" height="200px">

<p align="justify">A assistente BoardClass é uma assistente virtual web. Seu objetivo é auxiliar professores com o gerenciamento de turmas, alunos e disciplinas: permitindo a criação de novas turmas/disciplinas, agendamento de provas, adição de novos alunos, entre outras funcionalidades.</p>
<p align="justify">Ela funciona recebendo o comando por voz (por meio do professor) e realizando em seguida o que foi pedido. Em alguns casos, é necessário que o professor dê o comando por voz e então tenha que inserir manualmente os dados para que a ação seja concluída. Por exemplo na criação de um aluno, ele deve inserir as informações do aluno por meio do teclado do próprio computador.</p>

<!-- Fale sobre o projeto desenvolvido. Apresente a empresa parceira, o problema e a solução entregue pela equipe (mínimo de um parágrafo por item). Recomenda-se o uso de figuras (ou até mesmo vídeos) para ilustrar os principais projetos. -->

#### Tecnologias Utilizadas

<section>
    <div>
        <img align="center" alt="JS-icon" height="35" width="50" src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/figma/figma-original.svg">
        Figma: Design
    </div>
    <div>
        <img align="center" alt="JS-icon" height="35" width="50" src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/nodejs/nodejs-original.svg">
        NodeJS: Ecossistema
    </div>
    <div>
        <img align="center" alt="JS-icon" height="35" width="50" src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/react/react-original.svg">
       React: Frontend
    </div>
    <div>
        <img align="center" alt="JS-icon" height="35" width="50" src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/adonisjs/adonisjs-original.svg">
        AdonisJS: Backend
    </div>
    <div>
        <img align="center" alt="JS-icon" height="35" width="50" src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/postgresql/postgresql-original.svg">
        PostgresSQL: Banco de dados
    </div>   
</section>

#### Contribuições Pessoais

<section>
    <p>Como parte da equipe de desenvolvimento do projeto da assistente virtual web, concentrei minha atuação na criação e implementação das telas que compõem a interface do usuário. Minha contribuição foi crucial para garantir uma experiência intuitiva e eficiente para os professores que utilizam a plataforma, permitindo-lhes gerenciar turmas, alunos e disciplinas de forma simples e organizada.</p>
    <h4>Contribuições</h4>
    <details>
        <summary><b>Ativação por Voz</b></summary>
        <p>Colaborei com a equipe de backend para integrar a funcionalidade de ativação por voz, permitindo que os professores acionem a assistente virtual simplesmente usando comandos de voz. Desenvolvi a interface de usuário para exibir feedback visual quando a assistente está ouvindo ativamente os comandos do usuário.</p>
        <p>O código abaixo permite aos usuários interagirem com o aplicativo por meio de comandos de voz. Para tal, utilizamos a bilbioteca SpeechRecognition do React. O componente inicializa estados para controlar se o aplicativo está ouvindo, mensagens a serem exibidas, e um alerta. Além disso, define funções para manipular o estado do alerta e exibir mensagens de boas-vindas. Se o navegador não suportar o reconhecimento de voz, uma mensagem de erro é renderizada. O componente também inclui lógica para redirecionar o usuário com base nos comandos de voz reconhecidos.</p>
        <pre><code>
<span>export const Home = () =&gt; {</span>
<span>
        function onShowAlert(type) {
            setAlert({
            type: type,
            text: "Olá! Sou o assistente BoardClass. Você pode pressionar o botão azul ao lado e me dar um comando por voz! ;)",
            show: true,
            });
        }

        useEffect(() => {
            setTimeout(() => {
            speak({
                text: textSpeek,
            });
            onShowAlert("warning");
            }, 1500);
        }, []);

        if (!SpeechRecognition.browserSupportsSpeechRecognition()) {
            return (
            <div className={styles.notSupportContainer}>
                Browser is not Support Speech Recognition.
            </div>
            );
        }
        const handleListening = () => {
            setIsListening(true);
            microphoneRef.current.classList.add("listening");
            SpeechRecognition.startListening({
            continuous: true,
            });
        };

        const stopListening = () => {
            setIsListening(false);
            microphoneRef.current.classList.remove("listening");
            SpeechRecognition.stopListening();
        };
</span>
<span>}</span>
        </code></pre>
    </details>
    <details>
        <summary><b>Comandos de adição por Voz</b></summary>
        Implementei a integração dos comandos de voz para adição de novos alunos e criação de turmas. Trabalhei na definição e reconhecimento dos padrões de voz para cada comando, garantindo que a assistente entendesse corretamente as solicitações dos professores e executasse as ações correspondentes de forma precisa e eficiente.
    </details>
    <details>
        <summary><b>Feedback de Voz</b></summary>
        Desenvolvi a funcionalidade de feedback de voz para fornecer confirmação auditiva quando os comandos de voz são reconhecidos e as ações são concluídas com sucesso. Isso ajudou a melhorar a experiência do usuário, fornecendo uma resposta imediata e garantindo que os professores se sintam confiantes ao usar a assistente virtual.
    </details>
    <details>
        <summary><b>Implementação da tela de login</b></summary>
        Desenvolvi a tela de login utilizando ReactJS, garantindo que os usuários pudessem acessar a plataforma de forma segura e intuitiva. Implementei campos de entrada para e-mail e senha, bem como validações de entrada para garantir a integridade dos dados fornecidos pelos usuários.
    </details>
    <details>
        <summary><b>Implementação das telas de cadastro</b></summary>
        Implementei as telas de cadastro de alunos, turmas e disciplinas, permitindo que os professores adicionem novos alunos, criem novas turmas e criem novas disciplinas. Utilizei formulários interativos e validações em tempo real para garantir a precisão e integridade dos dados inseridos.
    </details>
    <details>
        <summary><b>Implementação Responsiva</b></summary>
        Consciente da importância da acessibilidade em diferentes dispositivos, implementei um design responsivo em todas as telas desenvolvidas.
    </details>
    <details>
        <summary><b>Feedback Visual</b></summary>
        Integrei elementos visuais de feedback para fornecer aos usuários retorno imediato sobre suas ações.
    </details>
    <details>
        <summary><b>Análise de requisitos</b></summary>
        Participei da análise detalhada dos requisitos funcionais e não funcionais fornecidos.
    </details>
</section>
<br>
<section>
    <h4>Tecnologias</h4>
    <details>
        <summary><b>ReactJS</b></summary>
        Como principal framework para o desenvolvimento das telas da interface.    
    </details>
    <details>
        <summary><b>HTML e CSS</b></summary>
        Utilizados para a estruturação e estilização das páginas.
    </details>
    <details>
        <summary><b>JavaScript:</b></summary>
        Linguagem de programação fundamental para a interatividade das telas.   
    </details>
</section>
<!-- 
Apresente suas contribuições no projeto. Foque nas funcionalidades em que você mais atuou. Descreva sua atuação em detalhes, especificando que tecnologias você utilizou. -->

#### Hard Skills

<section>
    <ul>
        <li><b>ReactJS: </b> Possuo habilidades avançadas em ReactJS, sendo capaz de desenvolver interfaces complexas de forma eficiente e escalável. Meu conhecimento inclui o uso de componentes, hooks, roteamento e gerenciamento de estado com Redux ou Context API.</li><br>
        <li><b>HTML e CSS: </b>Domínio de HTML e CSS, sendo capaz de criar layouts responsivos, estilizados com autonomia e o uso eficiente de flexbox e grid.</li><br>
        <li><b>Javascript: </b>Conhecimento sólido em JavaScript, utilizando-o para implementar interatividade e lógica de negócio nas aplicações web. Estou familiarizado com conceitos como manipulação do DOM, eventos, promessas e assincronicidade.</li><br>
        <li><b>Integração de Reconheimento de Voz: </b> Adquiri habilidades intermediárias na integração de sistemas de reconhecimento de voz em aplicações web. Durante o projeto, aprendi a utilizar APIs e bibliotecas para captura e processamento de comandos de voz.</li><br>
        <li><b>Análise de Requisitos e Design de Interface: </b>Desenvolvi habilidades avançadas na análise de requisitos de projeto e no design de interfaces de usuário. Minha capacidade inclui a compreensão das necessidades do usuário e a tradução dessas necessidades em soluções de design eficazes e intuitivas.</li>
    </ul>
</section>

<!-- Apresente as hard skills que você utilizou/desenvolveu durante o projeto e o nível de proficiência alcançado. Exemplo: CSS - Sei fazer com autonomia -->

#### Soft Skills

<section>
    <ul>
        <li><b>Comunicação Efetiva: </b>A habilidade de comunicação foi fundamental durante todo o projeto, especialmente na interação com outros membros da equipe e na compreensão das necessidades dos usuários finais. Participei ativamente de reuniões para discussão de requisitos, feedbacks e resolução de problemas.</li><br>
        <li><b>Trabalho em Equipe: </b>Colaborei de forma eficaz com outros membros da equipe de desenvolvimento, compartilhando conhecimento, dividindo tarefas e apoiando os colegas conforme necessário. Estabeleci um ambiente de trabalho colaborativo, onde ideias e soluções foram discutidas e implementadas em conjunto para alcançar os objetivos do projeto.</li><br>
        <li><b>Adaptabilidade: </b>Demonstrei adaptabilidade ao lidar com mudanças nos requisitos do projeto e nos prazos de entrega. A capacidade de me ajustar rapidamente a novas circunstâncias e prioridades foi crucial para manter o progresso do desenvolvimento e garantir a entrega.</li><br>
        <li><b>Criatividade e Inovação: </b>Demonstrei criatividade ao enfrentar desafios de design e implementação, propondo soluções inovadoras para problemas complexos. Explorei diferentes abordagens e técnicas para encontrar as melhores soluções que atendessem às necessidades dos usuários e aos requisitos do projeto.</li>
    </ul>
</section>
<!-- Apresente as soft skills que você utilizou/desenvolveu durante o projeto e em quais situações elas foram fundamentais. Exemplo: Comunicação - Precisei exercitar minhas habilidades de comunicação para viabilizar as reuniões semanais levando em conta as disponibilidades dos membros, que não cursavam as mesmas disciplinas. -->

<!-- ### Em 2022-1
Mesmo formato

### Em 2022-2
Mesmo formato

### Em 2023-1
Mesmo formato

### Em 2023-2
Mesmo formato -->
