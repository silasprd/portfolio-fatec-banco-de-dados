### 3º Semestre • 1/2023 - Sistema Gerenciador de Vendas

Repositório: [GIT](https://github.com/equipe-vox/api-3)

<p align="justify">Parceiro Acadêmico: <a href="https://www.domrock.net/">DomRock</a></p>

<!-- <img src="./images/boardclass.jpeg" widht="600px" height="200px"> -->

<p align="justify">Este projeto consiste em um sistema gerenciador que auxilie o setor de vendas e operações de uma grande empresa. </p>
<p align="justify">A aplicação lida com o histórico de movimentação de produtos, predição de movimentação de produtos (feito por um algoritmo de IA já existente) e entrada de dados oriundas da força de vendas quanto ao planejamento profundo..</p>

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
        <summary>Coleta de dados para utilização em gráficos</summary>
        <p>Desenvolvimento dos métodos de consulta em Java, utilizando Spring Boot, que recupera dados específicos para serem usados em um gráfico.</p>
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
        <p></p>
    </details>
</section>