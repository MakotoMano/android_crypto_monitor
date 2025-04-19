# 📈 Android Crypto Monitor

Aplicativo Android que consulta em tempo real a cotação do Bitcoin utilizando a **API pública do Mercado Bitcoin**. O app exibe o valor atual da moeda em reais, junto com a data/hora da última atualização.



# 🧩 Estrutura do Projeto

A seguir contém as explicações dos arquivos **main, service, factory e model** do aplicativo. 

## MainActivity.kt

Responsável por:

	- Exibir os dados de cotação do Bitcoin.

	- Configurar a interface com Toolbar, botões e campos de texto.

	- Fazer a chamada REST assíncrona (usando coroutines) ao serviço da API.

	- Tratar erros de conexão ou respostas inesperadas.


## service/MercadoBitcoinService.kt

Interface com o endpoint da API:

	- Define o método getTicker() para obter os dados de cotação da moeda BTC.

	- Usa Retrofit com suporte a coroutines (via suspend fun).

## service/MercadoBitcoinServiceFactory.kt

Classe responsável por:

	- Criar e configurar a instância do Retrofit.

	- Fornecer uma instância funcional de MercadoBitcoinService.

## model/TickerResponse.kt

Modelos que representam a estrutura do JSON retornado pela API:

## ▶️ Como funciona

O aplicativo segue um fluxo simples e eficiente para buscar e exibir o valor atual do Bitcoin:


### 1. Início da aplicação

- Ao abrir o app, a MainActivity é carregada.

- A Toolbar é configurada com o título e cores definidas.

- O botão de atualização (btn_refresh) é preparado para ouvir cliques do usuário.


### 2. Ação do usuário: clique em "Refresh"

- Quando o botão "Refresh" é clicado, o método makeRestCall() é chamado.

- Este método inicia uma coroutine na Dispatchers.Main (thread principal do Android), garantindo que a chamada à API seja feita de forma assíncrona, sem travar a interface do usuário.

### 3. Chamada à API do Mercado Bitcoin

- O app utiliza a MercadoBitcoinServiceFactory para criar uma instância de Retrofit com a base URL https://www.mercadobitcoin.net/.

- Em seguida, chama o endpoint api/BTC/ticker/ via o método getTicker(), definido na interface MercadoBitcoinService.

### 4. Processamento da resposta


Se a resposta da API for bem-sucedida (response.isSuccessful == true):

- O valor atual do Bitcoin (last) é extraído da resposta, convertido para Double, e formatado no padrão monetário brasileiro (R$).
- A data/hora (date) é convertida de timestamp Unix para um formato legível (dd/MM/yyyy HH:mm:ss).
- Ambos os valores são exibidos nas TextViews da tela: lbl_value e lbl_date.

Se a resposta for mal-sucedida (erros HTTP):

- O app exibe uma Toast com uma mensagem correspondente ao código de erro (ex: 400, 404, etc).


### 5. Tratamento de exceções

Caso ocorra qualquer exceção durante a chamada (ex: sem internet, erro de parsing), uma mensagem de erro é mostrada para o usuário informando o problema.
 

## Evidências

### Tela Inicial do Android

![Tela Inicial Android](https://github.com/user-attachments/assets/62de5bd1-26a2-4102-b074-8cbd881ba0f1)

### Tela Inicial do Aplicativo

![Tela Inicial Android](https://github.com/user-attachments/assets/1a2f2d3a-4458-4654-bc00-2ddff23f2d5b)


### Tela do Aplicativo após a atualização

![image](https://github.com/user-attachments/assets/99df0eba-d391-45ec-aae5-e4967ae801c1)


