# Microsoft Azure Function com C#

# Visão Geral
Este documento fornece informações Basicas sobre a configuração, desenvolvimento e implantação de uma Azure Function.

## O que é uma Azure Function?
Uma [Azure Function](https://azure.microsoft.com/pt-br/products/functions/) é um serviço de computação baseado em eventos que permite a execução de código sem precisar provisionar ou gerenciar servidores.

## Configuração Inicial

1. Criar uma Azure Function no Azure Portal
   * Faça login no [Azure Portal](https://portal.azure.com/).
   * Selecione um grupo de recursos ou crie um novo.
   * No menu, clique em "+ Criar" e procure por "Function".

  
 * Selecione "Function" na lista de resultados e siga as instruções para criar a Function.
  
   <img width="340" alt="image" src="https://github.com/lsmatias/Azure-Function/assets/28391885/71426af8-7e99-46ec-9c16-21b75c849d06">


2. Configuração da Function

   <img width="376" alt="image" src="https://github.com/lsmatias/Azure-Function/assets/28391885/6fa57815-e804-41b8-8827-fcd9fab5e520">
   
* Configure os detalhes da Function, incluindo o plano de execução, o tempo de execução e outros parâmetros conforme necessário.

   <img width="597" alt="image" src="https://github.com/lsmatias/Azure-Function/assets/28391885/57eec1e5-d8dc-4896-87a8-2aaa33749e4e">

# Desenvolvimento da Function

1. Estrutura do Projeto
Organize seu projeto da Function de acordo com as boas práticas.
```
MeuProjetoFunction/
|-- .vscode/
|-- MyFunction/
|   |-- function.json
|   |-- run.cs
|-- local.settings.json
|-- host.json
|-- README.md
```
  <img width="304" alt="image" src="https://github.com/lsmatias/Azure-Function/assets/28391885/30238f38-c6d1-44a3-b5a6-f438815c0c29">


3. Código da Function
Escreva o código da Function no arquivo **run.csx**. Use a linguagem que melhor atende às suas necessidades (por exemplo, C#, JavaScript).


```
using System.IO;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Logging;
using Newtonsoft.Json;

public static class MyFunction
{
    [FunctionName("MyFunction")]
    public static async Task<IActionResult> Run(
        [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = "myFunction")] HttpRequest req,
        ILogger log)
    {
        log.LogInformation("C# HTTP trigger function processed a request.");

        string name = req.Query["name"];

        string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
        dynamic data = JsonConvert.DeserializeObject(requestBody);
        name = name ?? data?.name;

        return name != null
            ? (ActionResult)new OkObjectResult($"Hello, {name}")
            : new BadRequestObjectResult("Please pass a name on the query string or in the request body");
    }
}
```

 <img width="529" alt="image" src="https://github.com/lsmatias/Azure-Function/assets/28391885/30f1aefa-fe4c-45ae-9f0c-3c32c6e2cdb5">


# Implantação

1. Implantação via Azure Portal
   * No Azure Portal, acesse a Function criada.
   * Navegue até a seção "Implantação" e faça o upload do seu pacote de Function.
  
     <img width="482" alt="image" src="https://github.com/lsmatias/Azure-Function/assets/28391885/53cc72de-c44d-46bf-a43f-aacb239259a0">

2. Implantação via Azure CLI
   * Use o Azure CLI para implantar a Function.

``az functionapp deployment source config-zip --resource-group <Resource-Group-Name> --name <Function-App-Name> --src <Path-to-Zip-File>``

# Monitoramento e Diagnóstico

1. Logs e Métricas
   ## Explore os logs e métricas da sua Function no Azure Monitor

   * Exemplo de coleta de **Métricas** customizada usando de CPU de uma maquina virtual:
     
     <img width="557" alt="image" src="https://github.com/lsmatias/Azure-Function/assets/28391885/c8d1a656-40c0-42e6-966d-a078a9c667b5">

   
   * Monitoramento de Performance
  
     <img width="579" alt="image" src="https://github.com/lsmatias/Azure-Function/assets/28391885/7b4c78c0-c890-4f82-ab8c-7571089ef8cc">
     
   * DashBoard Geral
     
     <img width="569" alt="image" src="https://github.com/lsmatias/Azure-Function/assets/28391885/1657c9dc-2ac0-42fc-be02-94bd215ae564">
   
> [!NOTE]
> O DashBoard  pode ser customizado, e com auxilio de ferrementas como o [**Grafana**](https://grafana.com/) ajudam a detectar, isolar e fazer a triagem de incidentes operacionais.
Combinar visualizações de fontes de dados do Azure e não Azure. Essas fontes incluem locais, ferramentas de terceiros e armazenamentos de dados em outras nuvens.

  <img width="416" alt="image" src="https://github.com/lsmatias/Azure-Function/assets/28391885/06dc39b9-0470-470f-b845-b573b92680b5">
     

   
