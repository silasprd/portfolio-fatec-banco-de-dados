### 3º Semestre • 1/2023 - Sistema Gerenciador de Vendas

Repositório: [GIT](https://github.com/equipe-vox/api-3)

<p align="justify">Parceiro Acadêmico: <a href="https://www.domrock.net/">DomRock</a></p>

<img src="./images/domrock.png" widht="600px" height="200px">

<p align="justify">Este projeto consiste em um sistema gerenciador que auxilie o setor de vendas e operações de uma grande empresa. </p>
<p align="justify">A aplicação lida com o histórico de movimentação de produtos, predição de movimentação de produtos (feito por um algoritmo de média ideal) e entrada de dados oriundas da força de vendas quanto ao planejamento profundo.</p>

#### Tecnologias Utilizadas

<section>
    <div>
        <img align="center" alt="JS-icon" height="35" width="50" src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/figma/figma-original.svg">
        Figma: Design
    </div>
    <div>
        <img align="center" alt="Java-icon" height="35" width="50" src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/java/java-original.svg">
        Java: Ecossistema
    </div>
    <div>
        <img align="center" alt="JavaFX-icon" height="35" width="50" src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/spring/spring-original.svg">
        Springboot: Backend
    </div>
    <div>
        <img align="center" alt="JS-icon" height="35" width="50" src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/react/react-original.svg">
        React: Frontend
    </div>
    <div>
        <img align="center" alt="Postgres-icon" height="35" width="50" src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/mysql/mysql-original-wordmark.svg">
        MySQL: Banco de dados
    </div>   
</section>

#### Contribuições Pessoais

<section>
    <p>Como parte da equipe de desenvolvimento, fui responsável por desenvolver algumas funcionalidades em java para o tratamento de alguns dados disoponibilizados no sistema. Também concentrei a maior parte do meu tempo de desenvolvimento, na criação das interfaces de usuário, como formulários e listas e também dashboards para visualização dos dados.</p>
    <h4>Contribuições</h4>
    <details>
        <summary><b>Algoritmo de predição ideal de vendas</b></summary>
        <p>É um método desenvolvido em Java, que utiliza Spring Boot, especificamente a anotação @PutMapping, para calcular médias de vendas e prever valores ideais para quantidades de vendas futuras. Este algoritmo é útil para prever quantidades ideais de vendas futuras com base em padrões históricos de vendas, permitindo uma melhor tomada de decisão e planejamento de negócios.</p>
<pre><code>
@PutMapping("/mediaVendas/{vendaId}")
public ResponseEntity calcularMediasePredizer(@PathVariable Long vendaId) {

    //Cliente clientOptional = clienteRepository.findById(clienteId).get();
    Venda vendaFound = vendaRepository.findById(vendaId).get();
    Cliente clientOptional = vendaFound.getCliente();

    //Double mediaVenda1 = 0.0;
    //Double mediaVenda2 = 0.0;
    //Double mediaVenda3 = 0.0;
    Double qtdVenda1 = 0.0;
    Double qtdVenda2 = 0.0;
    Double qtdVenda3 = 0.0;
    int i = 0;

    for(Venda venda : clientOptional.getVendas()){
        qtdVenda1 += venda.getFirst_real_qtd(); 
        qtdVenda2 += venda.getSecond_real_qtd(); 
        qtdVenda3 += venda.getThird_real_qtd();
        i++;
    }

    Double mediaVenda1 = qtdVenda1 / i;
    Double mediaVenda2 = qtdVenda2 / i;
    Double mediaVenda3 = qtdVenda3 / i;
    
    Double qtdIdeal1 = (0.7 * vendaFound.getFirst_qtd()) + (0.3 * mediaVenda1);
    Double qtdIdeal2 = (0.7 * vendaFound.getSecond_qtd()) + (0.3 * mediaVenda2);
    Double qtdIdeal3 = (0.7 * vendaFound.getThird_qtd()) + (0.3 * mediaVenda3);
    
    vendaFound.setFirst_predict_qtd(Math.round(qtdIdeal1));
    vendaFound.setSecond_predict_qtd(Math.round(qtdIdeal2));
    vendaFound.setThird_predict_qtd(Math.round(qtdIdeal3));

    return new ResponseEntity<Venda>(vendaRepository.save(vendaFound), HttpStatus.OK);		
}
</code></pre>
        <p>O método começa recuperando a venda específica com base no ID fornecido. Isso é feito usando o método findById do vendaRepository. Em seguida, o algoritmo itera sobre todas as vendas do cliente associado à venda recuperada, somando as quantidades de venda reais em três períodos diferentes (primeiro, segundo e terceiro mês). Essas somas são divididas pelo número total de vendas (i) para calcular as médias de venda em cada período. Com as médias de vendas calculadas, o algoritmo calcula as quantidades ideais de vendas para cada período futuro. Isso é feito aplicando uma fórmula que combina a quantidade real da venda atual (vendaFound.getFirst_qtd(), vendaFound.getSecond_qtd(), vendaFound.getThird_qtd()) com a média de vendas históricas correspondente.</p>
    </details>
    <details>
        <summary><b>Coleta de dados para utilização em gráficos<b></summary>
        <p>Desenvolvimento dos métodos de consulta em Java, utilizando Spring Boot, que recupera dados específicos para serem usados em um gráfico. Este método é uma operação de consulta desenvolvida em Java, utilizando Spring Boot, que recupera dados de lucro por cliente para um determinado vendedor.</p>
        <pre><code>
@GetMapping("clientsGraph/{vendedorId}")
public List<> findDataGraphByClients(@PathVariable Long vendedorId) {
    List<> dataLucroPorCliente = new ArrayList<>();
    Vendedor getVendedor = new Vendedor();
    Optional<> optVendedor = vendedorRepository.findById(vendedorId);
    if (optVendedor.isPresent()) {
        getVendedor = optVendedor.get();
    }
    List<> getClientes = getVendedor.getClientes();

    for (int i = 0; i < getClientes.size(); i++) {
        Cliente getCliente = getClientes.get(i);
        List<Venda> getVendas = getClientes.get(i).getVendas();
        double lucroTotal = 0;

        for (int j = 0; j < getVendas.size(); j++) {
            Produto getProduto = getVendas.get(j).getFk_sku_venda();
            double lucroPorVenda = getProduto.getValor();
            double qtdPorVenda = getVendas.get(j).getFirst_qtd() + getVendas.get(j).getSecond_qtd()
                    + getVendas.get(j).getThird_qtd();
            lucroPorVenda = lucroPorVenda * qtdPorVenda;
            lucroTotal += lucroPorVenda;
        }

        ClienteVendedor data = new ClienteVendedor();
        DecimalFormat formato = new DecimalFormat("#,##0.00");
        String lucroFormatado = formato.format(lucroTotal);
        data.setCliente(getCliente);
        data.setLucroTotal(lucroFormatado);

        dataLucroPorCliente.add(data);
    }

    return dataLucroPorCliente;
}
        </pre></code>
        <p>Com a anotação GetMapping mapeamos a requisição HTTP GET para o endpoint "/clientsGraph/{vendedorId}", onde {vendedorId} é um parâmetro dinâmico que representa o ID do vendedor. O método começa criando uma lista vazia chamada dataLucroPorCliente. Em seguida, o método recupera o vendedor correspondente ao ID fornecido usando o método findById do vendedorRepository. Se o vendedor existir, sua lista de clientes é obtida. Iteramos sobre a lista de clientes do vendedor. Para cada cliente, ele recupera a lista de vendas associadas a esse cliente. Para cada venda do cliente, o método calcula o lucro total multiplicando o valor do produto pelo total de unidades vendidas. O lucro total de todas as vendas do cliente é acumulado. Após calcular o lucro total por cliente, o método cria um objeto ClienteVendedor, formata o lucro total para exibição adequada e adiciona o objeto à lista dataLucroPorCliente. Por fim retornamos a lista contendo os dados de lucro por cliente.</p>
    </details>
    <details>
        <summary>Criação das requisições para coleta dos dados de gráfico fornecidos pelo backend</summary>
        <p>Essa parte do código desenvolvida em Javascript faz uma requisição ao backend para recuperar dados de lucro por cliente e processa esses dados para uso na interface do usuário.</p>
        <pre><code>
findLucroPorCliente = async () => {
    if (!selectedClient && !selectedSale) {
        const response = await axios.get(`http://localhost:8080/venda/clientsGraph/${userId}`)
        const list = response.data.map((entry: any) => {
          
          return {
            name: entry.cliente.nome,
            value: parseFloat(entry.lucroTotal.replace(".", ""))
          }
        })
        setLucroPorCliente(list)
    }
}
        </code></pre>
        <p>Esta é uma função assíncrona, que será executada para buscar os dados de lucro por cliente. Primeiro verificamos se nenhum cliente ou venda está selecionado antes de fazer a requisição. Em 'axios.get', é feita uma requisição HTTP GET para o endpoint onde o backend roda. O ID do usuário atual(mapeado por uma constante de estado userId), será usado para recuperar os dados de lucro por cliente específicos desse usuário. Após receber a resposta da requisição, os dados são mapeados para formatá-los de uma maneira mais adequada para uso na interface do usuário. Por fim, os dados processados de lucro por cliente são definidos no estado LucroPorCliente, que será usado para renderizar um gráfico ou exibir informações na interface do usuário.
    </details>
    <details>
        <summary><b>Criação dos gráficos nas interfaces de usuário<b></summary>
        <p style="font-weight: normal">Para criação dos gráficos, utilizamos a biblioteca recharts, que é comumente usada para criar gráficos em aplicações React.</p>
        <pre></code>
<BarChart
    width={600}
    height={350}
    data={lucroPorCliente}
    margin={{
        top: 10,
        right: 20,
        left: 15,
        bottom: 0,
    }}
    >
    <CartesianGrid stroke="#B3B0B8" strokeDasharray="2 5" />
    <XAxis dataKey="name" stroke="#B3B0B8" />
    <YAxis stroke="#F8D210" tickFormatter={(value) => new Intl.NumberFormat('pt-BR').format(value)} />
    <Tooltip formatter={(value: any) => parseFloat(value).toLocaleString()} />
    < Bar dataKey="value" fill="#8884d8" name="Valor de venda">
        {
            lucroPorCliente.map((entry: any, index) => (
                <Cell key={`cell-${index}`} fill={COLORS[index % COLORS.length]} />
            ))
        }
    </ Bar >
</ BarChart>
        </code></pre>
        <p style="font-weight: normal">Este componente define o gráfico de barras e especifica suas dimensões. Os dados para o gráfico são passados através da propriedade data, que contém os valores de lucro por cliente. O componente CartesianGrid cria linhas de grade para o gráfico, tornando mais fácil visualizar os dados, nele são definidas a cor da linha e o tipo de traço. Depois utilizamos o componente XAxis para definir o eixo X do gráfico, que normalmente exibe as categorias ou labels. O YAxis normalmente exibe os valores, e utilizamos o tickFormatter para formatar os valores exibidos no eixo Y. O Tooltip adiciona uma dica de ferramenta que mostra informações adicionais quando o usuário passa o mouse sobre as barras. A tag Bar define as barras do gráfico. dataKey especifica qual atributo dos dados será utilizado para determinar a altura das barras. Por fim, criamos um pequeno trecho de código para mapear os dados de lucro por cliente para cada barra do gráfico. Para cada entrada no array lucroPorCliente, uma barra é renderizada.
    </details>
</section>


### Hard Skills

<section>
    <ul style="font-weight: normal;">
        <li><b>Java: </b>Aprimorei minhas habilidades com a linguagem JAVA, criando componentes personalizados para atender as necessidades específicas do projeto, como algoritmos para cálculo de médias de vendas e previsão de quantidades ideais (calcularMediasePredizer), e para extração de dados de vendas para visualização gráfica (findDataGraphByVenda e findDataGraphByClients).</li><br>
        <li><b>Springboot: </b>Adquiri experiência na integração de sistemas por meio de APIs REST através do Springboot, garantindo a comunicação eficiente entre o backend Java e o frontend, permitindo a manipulação e apresentação de dados de vendas.</li><br>
        <li><b>React: </b>Aprimorei minhas habilidades na utilização do React para criar interfaces de usuário interativas e dinâmicas, utilizando a biblioteca Recharts para integração dos dados de gráficos vindo do backend com o frontend.</li><br>
        <li><b>Manipulação de dados: </b>Utilize técnicas de manipulação de dados para calcular médias e prever vendas futuras, bem como para formatar e apresentar dados em gráficos, proporcionando insights valiosos para a tomada de decisões.</li><br>
    </ul>
<section>


## Soft Skills
<section>
    <ul style="font-weight: normal;">
        <li><b>Coleta de Requisitos: </b>Participei ativamente na coleta de requisitos, compreendendo as necessidades dos clientes e stakeholders, e compreendendo as necessidades do cliente trazidas pelo Product Owner da equipe.</li><br>
        <li><b>Priorização de backlog: </b>Trabalhei de acordo com a priorização do backlog, mantendo uma comunicação com o Product Owner para garantir que as entregas fossem feitas de forma iterativa e incremental, atendendo às necessidades do cliente de maneira eficaz.</li><br>
        <li><b>Trabalho em equipe: </b>Colaborei estreitamente com os membros da equipe, garantindo a integração suave entre diferentes partes do sistema e a resolução rápida de problemas.</li><br>
    </ul>
<section>