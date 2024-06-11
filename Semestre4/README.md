### 4º Semestre • 1/2023 - Software de Inspeção Predial

Repositório: [GIT](https://github.com/Data-Team23/Jaia)

<p align="justify">Parceiro Acadêmico: <a href="https://www.jaia.software/">Jaia</a></p>

<!-- <img src="./images/domrock.png" widht="600px" height="200px"> -->

<p align="justify">Em um cenário onde a paisagem urbana se compõe de uma mistura de edifícios modernos e históricos, a empresa Jaia, apresentou um desafio significativo. A condução de inspeções prediais estava provando ser uma tarefa morosa e suscetível a imprecisões.</p>
<p align="justify">Diante desse cenário, a Jaia buscou soluções inovadoras para otimizar esse processo crucial. A visão estratégica da empresa contemplou o desenvolvimento de um software de inspeção predial, projetado para revolucionar a abordagem atual. A plataforma concebida promete oferecer uma experiência intuitiva, capacitando os inspetores a documentar minuciosamente detalhes relevantes e capturar evidências visuais de forma eficaz. Adicionalmente, a geração instantânea de relatórios abastecerá a tomada de decisões embasadas. A Jaia, reconhecendo a necessidade de aprimorar a qualidade e eficiência das inspeções, direcionou seus esforços para o desenvolvimento desse software inovador. O resultado obtido transcendeu as expectativas iniciais, beneficiando não somente a empresa, mas também elevando o padrão das inspeções prediais na esfera urbana, contribuindo, assim, para uma maior segurança e excelência nas estruturas urbanas.</p>

#### Tecnologias Utilizadas

<section>
    <div>
        <img align="center" alt="Java-icon" height="35" width="50" src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/java/java-original.svg">
        Java: Ecossistema
    </div>
    <div>
        <img align="center" alt="JavaFX-icon" height="35" width="50" src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/spring/spring-original.svg">
        Springboot: Backend
    </div>
    <div>
        <img align="center" alt="JS-icon" height="35" width="50" src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/vuejs/vuejs-original.svg">
        Vue: Frontend
    </div>
    <div>
        <img align="center" alt="Postgres-icon" height="35" width="50" src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/oracle/oracle-original.svg">
        OracleSQL: Banco de dados
    </div>   
</section>

#### Contribuições Pessoais

<section>
    <p>Como parte da equipe de desenvolvimento, fiquei responsável por desenvolver funcionalidades na linguagem java para tratamento de requisições e dados para o dashboard, também fui responsável pelo desenvolvimento de interfaces de usuário, como os formulários, listas e dashboards.</p>
    <h4>Contribuições</h4>
    <details>
        <summary><b>Criação de Repositories e definição de querys JPQL</b></summary>
        <p>Para a entidade Checklist usamos a interface JpaRepository para definir um repositório para ela.</p>
<pre><code>
@Repository
public interface ChecklistRepository extends JpaRepository<Checklist, Long>{
    
    @Modifying
    @Transactional
    @Query("DELETE FROM Checklist c WHERE c.id = ?1")
    void deleteByIdWithCascade(Long id);
}
</code></pre>
        <p>O ChecklistRepository estende JpaRepository, que fornece um conjunto completo de métodos padrão para manipulação de dados. A anotação @Repository declara a interface como um componente de repositório Spring, que será automaticamente detectada e configurada.
        O método deleteByIdWithCascade faz uma deleção personalizada de um Checklist pelo seu ID, lidando com a exclusão em cascata de relações associadas. 
        A anotação @Modifying indica que o método executará uma operação de modificação no banco de dados. Com o @Transactional garantimos que a operação de modificação seja executada dentro de uma transação.
        O @Query define a consulta JPQL (Java Persistence Query Language) para deletar um Checklist específico pelo seu ID. A consulta "DELETE FROM Checklist c WHERE c.id = ?1" remove o Checklist cujo id corresponde ao parâmetro fornecido.</p>
    </details>
    <details>
        <summary><b>Criação de Services para o Checklist</b></summary>
        <p>Criamos um serviço utilizando o Springboot para tratamento dos dados de checklist, que é responsável por definir os itens que devem ser checados em um vistoria.</p>
        <pre><code>
@Service
public class ChecklistService {

    @Autowired
    private ChecklistRepository checkListRepo;

    @Autowired
    private DepartamentoRepository departamentoRepo;

    public Checklist novoChecklist(Checklist checklist, Long departamentoId) {
        Optional<Departamento> optDepartamento = departamentoRepo.findById(departamentoId);
        if (optDepartamento.isPresent()) {
            Departamento departamento = optDepartamento.get();
            checklist.setDepartamento(departamento);
            departamento.getChecklists().add(checklist);
            departamentoRepo.save(departamento);
            checklist = checkListRepo.save(checklist);
        }
        return checklist;
    }

    public List<Checklist> buscarTodos() {
        return checkListRepo.findAll();
    }

    public Checklist buscarPorId(Long id) {
        Optional<Checklist> optChecklist = checkListRepo.findById(id);
        if (optChecklist.isEmpty()) {
            throw new IllegalArgumentException("Checklist não encontrado!!");
        }
        Checklist checklist = optChecklist.get();
        return checklist;
    }

    public void deletarPorId(Long id) {
        checkListRepo.deleteByIdWithCascade(id);
    }

}
        </pre></code>
        <p>Utilizamos a anotação @Service para definir a classe como um componente de serviço no spring. O @Autowired injeta as dependências automaticamente para as classes de Repository que vamos utilizar.
        A função novoChecklist() cria e associa um checklist a um departamento que será buscao pelo departamentoId fornecido como parâmetro da função.
        A função buscarTodos() utiliza o Repository para obter todos os checklists exitentes no banco de dados.
        A função buscarPorId() busca e retorna um checklist específico através do id passado como parâmetro, se o id fornecido não for encontrado, lança um exceção.
        A função deletar por id remove um checklist do banco de dados através do id passado como parâmetro, incluindo suas dependências. Esta função utiliza um método customizado do repositório (deleteByIdWithCascade) para garantir que a exclusão seja realizada de forma adequada, lidando com todas as relações associadas ao checklist.</p>
    </details>
    <details>
        <summary><b>Criação do DTO (Data Transfer Object) para o dashboard.</b></summary>
        <p>A classe DashboardDTO é uma estrutura de dados simples que encapsula as informações necessárias para a representação de um gráfico no dashboard.</p>
        <pre><code>
package com.dataTeam.jaia.jaia.DTO;

import java.util.List;

import lombok.Data;

@Data
public class DashboardDTO {
    private List<String> labels;
    private List<DatasetsDTO> datasets;
}
        </pre></code>
        <p>A classe DashboardDTO é essencialmente um "container" que agrega os dados necessários para criar um gráfico no dashboard. Ela organiza os rótulos e os conjuntos de dados de forma estruturada, facilitando a transmissão desses dados para componentes de visualização, como gráficos em uma interface de usuário. A anotação @Data gera automaticamente os métodos padrões (getters, setters, toString, etc.) para os campos da classe.</p>
    </details>
    <details>
        <summary><b>Desenvolvimento das funcionalidade para o tratamento dos dados do dashboard</b></summary>
        <p>Utilizando a classe DashboardDTO que contém as etiquetas (labels) e os conjuntos de dados (datasets) necessários para a visualização gráfica, desenvolvemos funções para pegar esses dados que serão usados nos dashboards.</p>
        <pre><code>
public DashboardDTO getOsByStatus() {
    DashboardDTO dataDashboard = new DashboardDTO();
    DatasetsDTO datasets = new DatasetsDTO();
    List datasetsList = new ArrayList<>();
    List ordens = ordemService.buscarTodasOrdemServico();
    List labels = new ArrayList<>();
    List osCountList = new ArrayList<>();
    String[] barColors = { "#000000", "#2E2E48", "#626288", "#8080BF", "#6A6A69" };

    Map<String, Integer> osCountMap = new HashMap<>();

    for (OrdemServico ordem : ordens) {
        String osStatus = ordem.getStatus_ordem();
        osCountMap.put(osStatus, osCountMap.getOrDefault(osStatus, 0) + 1);
    }

    for (Map.Entry<String, Integer> entry : osCountMap.entrySet()) {
        labels.add(entry.getKey());
        osCountList.add(entry.getValue());
    }

    datasets.setData(osCountList);
    // datasets.setLabel("Status da ordem");
    datasets.setBorderWidth(1);
    datasets.setBackgroundColor(barColors);
    datasetsList.add(0, datasets);
    dataDashboard.setLabels(labels);
    dataDashboard.setDatasets(datasetsList);

    return dataDashboard;
}
        </pre></code>
        <p>A função getOsByStatus gera um conjunto de dados (DashboardDTO) para um gráfico de barras que exibe o número de ordens de serviço (OS) agrupadas pelo seu status. Criamos instâncias de DashboardDTO e DatasetsDTO para armazenar os dados do gráfico e inicializamos listas (datasetsList, labels, osCountList) para manipular os dados das ordens de serviço. Recuperamos todas as ordens de serviço chamando ordemService.buscarTodasOrdemServico(). Com o HashMap (osCountMap) contamos quantas ordens existem para cada status. As entradas do HashMap são convertidas em duas listas: uma para os rótulos (labels) e outra para as contagens (osCountList). Configuramos o datasets com as contagens (osCountList), a cor das barras (barColors), e outras propriedades visuais como a largura da borda. Após, adicionamos o datasets configurado na lista de conjuntos de dados (datasetsList), definindo os rótulos e os conjuntos de dados no objeto DashboardDTO (dataDashboard). Por fim retornamos o objeto DashboardDTO pronto para ser utilizado na renderização do gráfico.</p>
    </details>
</section>

### Hard Skills

<section>
    <ul style="font-weight: normal;">
        <li><b>Java e Springboot: </b>Dominei o desenvolvimento backend usando Java e o framework Spring Boot, especialmente na implementação do CRUD da entidade checklist. Isso incluiu a criação de endpoints RESTful que permitiram a manipulação eficiente dos dados de checklist, garantindo a integridade e a eficiência do sistema.</li><br>
        <li><b>Vue: </b>Desenvolvi interfaces de usuário modernas e responsivas utilizando Vue. Isso incluiu a construção da interface de checklist, onde os inspetores poderiam adicionar, editar e visualizar detalhes relevantes das inspeções, e a interface de dashboard, que proporcionava uma visão clara e visual dos dados de inspeção.</li><br>
        <li><b>Manipulação de Dados e Visualização: </b> Desenvolvi habilidades para manipular e apresentar dados de maneira eficaz na tela de dashboard. </li><br>
        <li><b>JPQL (Java Persistence Query Language): </b>Utilizei o JPQL para criar consultas orientadas a objetos e manipular objetos persistentes no contexto do Spring Boot, permitindo consultas complexas e flexíveis aos dados armazenados no banco de dados.</li><br>
    </ul>
<section>

## Soft Skills
<section>
    <ul style="font-weight: normal;">
        <li><b>Comunicação Efetiva: </b>Capacidade de comunicar de forma clara e eficaz com membros da equipe de desenvolvimento, garantindo um entendimento comum dos requisitos do projeto.</li><br>
        <li><b>Resolução de problemas: </b>Aprimorei habilidades em identificar, analisar e resolver problemas de forma criativa e eficaz, buscando soluções inovadoras para desafios técnicos e organizacionais encontrados durante o desenvolvimento do software de inspeção predial.</li><br>
        <li><b>Trabalho em equipe: </b>Desenvolvi aptidão para trabalhar de forma colaborativa em equipe, contribuindo com ideias, compartilhando conhecimento e apoiando os colegas para alcançar os objetivos do projeto de forma eficiente e eficaz.</li><br>
        <li><b>Adaptabilidade: </b>Me adaptei a mudanças e imprevistos durante o ciclo de vida do projeto, mantendo uma atitude positiva e flexível diante de desafios e obstáculos, e ajustando-se rapidamente às novas demandas e requisitos.</li><br>
    </ul>
<section>