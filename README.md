# DIO - Aplicações Serverless na Azure
Trabalhando Aplicações Serverless na Azure


## Estrutura a ser criada

Segue abaixo um esquema geral da estrutura que será criada utilizando as Azure Functions:

![D3 01](https://github.com/user-attachments/assets/8e70bbd4-05a7-4620-8964-7c8641d9c52a)


## Lab Logic Apps

No portal da Azure pesquise por Logic App e clique em **Create**:

![D3 02](https://github.com/user-attachments/assets/6da87f13-92b6-4174-b747-867cc49ae80f)

Selecione uma das opções e preencha as demais informações solicitadas na próxima página:

![D3 03](https://github.com/user-attachments/assets/44ee4491-b750-4997-9820-5ce6e6f44493)

![D3 04](https://github.com/user-attachments/assets/0275e5de-f109-43d9-8a86-8981485f7e71)

Vá para o recurso criado e clique em **Edit**:

![D3 05](https://github.com/user-attachments/assets/d0916f03-a220-4ad0-861c-4554abd499d1)

Clique em **Add a trigger**:

![D3 06](https://github.com/user-attachments/assets/6a33136c-c41d-4455-9ebd-ca1e0787a562)

Preencha as informações necessárias:

![D3 07](https://github.com/user-attachments/assets/af705a84-413c-413e-a6cd-5b343d8b3ee2)

![D3 08](https://github.com/user-attachments/assets/5955c06a-b86f-4216-9392-14655c4689c8)

![D3 09](https://github.com/user-attachments/assets/57907196-317a-4de2-b709-fe52d9836c6f)

Após preencher as informações clique em **Save** e verifique a URL gerada:

![D3 10](https://github.com/user-attachments/assets/c35bd9b3-381f-4ef3-b51d-6a5f0370ed8b)

Ainda no portal da Azure crie um recurso de Service Bus:

![D3 11](https://github.com/user-attachments/assets/24134222-4dba-418b-8594-9435baab8058)

![D3 12](https://github.com/user-attachments/assets/9ada9406-be2c-49bb-a89e-06c986faf633)

![D3 13](https://github.com/user-attachments/assets/9b1b4976-1a71-4c70-b26b-a48156aef239)

No site do Postman informe a URL gerada na criação da function e um body:

![D3 14](https://github.com/user-attachments/assets/4834fdf1-c13e-4feb-abe2-0959c160f9e3)

Após clique em **Send** e verifique o resultado do retorno:

![D3 15](https://github.com/user-attachments/assets/bc17731d-5ea1-4a06-9f51-5ec1693fe469)

Retorne ao portal da Azure e verifique em **Run History** se a informação enviada pelo Postman foi persistida:

![D3 16](https://github.com/user-attachments/assets/663496b9-689f-4807-8f91-202d90432325)

![D3 17](https://github.com/user-attachments/assets/c2802398-ff9b-45fa-9664-1cbdc12bc135)

Na configuração do Service Bus crie uma chave do tipo Send:

![D3 19](https://github.com/user-attachments/assets/b999d47d-863c-4cea-8bcf-a090ad3d1639)

Crie uma fila:

![D3 21](https://github.com/user-attachments/assets/b35dfb64-7f27-4e45-a988-d7caf22bc64c)

E na fila crie uma chave/connection string do tipo Send:

![D3 22](https://github.com/user-attachments/assets/7c78daab-27ae-44e1-8f02-0833fd811f3b)

Na configuração do Logic App clique em **Add an action** e inclua o Service Bus criado, isso será necessário para criar uma fila para armazenar as informações que chegarem na aplicação pelo Postman:

![D3 18](https://github.com/user-attachments/assets/f19eb24f-5beb-4ebe-ae72-c98ca6305b97)

Ainda na configuração da ação inclua a chave do tipo Send criada na fila do Service Bus:

![D3 20](https://github.com/user-attachments/assets/243014d3-331d-4e74-8731-ebb3fd1ddcc2)

Se a connection string do tipo Send não funcionar, retorne na configuração do Service Bus e da fila do Service Bus e altere as chaves para *Manage*:

![D3 23](https://github.com/user-attachments/assets/affe17df-6f53-40d6-86aa-1524597e18d2)

Retornando ao Logic App finalize a criação da ação para envio de resposta e clique em **Save**:

![D3 24](https://github.com/user-attachments/assets/f8c34e73-a3cb-42e6-8daa-9aac3f7b3a6a)

![D3 25](https://github.com/user-attachments/assets/535e187f-138c-4ed8-9c03-61cf23248265)

![D3 26](https://github.com/user-attachments/assets/4389f427-8be9-433d-84ec-d06de7f015ce)

No Logic App crie mais uma ação para envio do status:

![D3 27](https://github.com/user-attachments/assets/d4f7067b-9323-437b-8150-9dd9e01d6b66)

![D3 28](https://github.com/user-attachments/assets/30b4440f-c9ba-423b-9ed1-c49cd3fe713c)

![D3 29](https://github.com/user-attachments/assets/aec58352-b3db-4c23-8c93-6738d0c0aca7)

Teste novamente no Postman para verificar a mensagem de retorno:

![D3 30](https://github.com/user-attachments/assets/15a508b0-9799-4f85-9e2c-7d71e869ddc9)

E verifique o registro salvo na fila do Service Bus:

![D3 31](https://github.com/user-attachments/assets/6b4ef71b-327e-4821-bfdf-2ffaea721382)


## Azure Functions Database

No portal do Azure crie um banco de dados e uma tabela:

![D3 32](https://github.com/user-attachments/assets/5adb7bcf-3987-408c-94cf-286c48792efd)

O código abaixo será utilizado para popular a tabela:

![D3 33](https://github.com/user-attachments/assets/74208df0-f8fb-4fc6-a07b-9f78be03994d)

Ou seja, os dados virão por um http e irão para a fila.

Os métodos abaixo irão realizar o envio da mensagem pelo método post:

![D3 34](https://github.com/user-attachments/assets/e1c5808f-a8ed-4c55-9888-96e537efa816)

Para verificar o funcionamento do código acima realize um teste pelo Postman:

![D3 35](https://github.com/user-attachments/assets/a456cb84-31f3-4ba0-9857-c604821ea9a9)

O código também irá gerar um ID para cada registro enviado para a aplicação:

![D3 36](https://github.com/user-attachments/assets/1098ccd3-2923-449b-a5f6-5b35de60bd73)

Contém as informações de conexão com o banco:

![D3 37](https://github.com/user-attachments/assets/d06ed6ba-3c6b-4fde-b497-61292120dd4b)

![D3 38](https://github.com/user-attachments/assets/891c1c91-f8e4-4a47-b2e0-461843823328)

Após teste no Postman, verifique os dados criados no banco de dados da Azure:

![D3 39](https://github.com/user-attachments/assets/fb77231f-8c8b-46b3-a15b-7a1852414fca)

No portal da Azure crie um Function App:

![D3 40](https://github.com/user-attachments/assets/0c216af6-dae4-4d53-a71b-279e3e33ff4a)

![D3 41](https://github.com/user-attachments/assets/6f493549-1369-432f-93e7-f1ba12711687)

Após a criação do Function App clique em **Get publish profile** para fazer o deploy:

![D3 42](https://github.com/user-attachments/assets/2e871cec-ac09-4a1c-ae48-0b83958e441f)

Importe o novo profile e publique:

![D3 43](https://github.com/user-attachments/assets/bea60f99-93ed-438f-b7ae-4a58b8d4f6d5)

![D3 44](https://github.com/user-attachments/assets/9711906d-af4a-4394-8ad0-6b6d39e85aaa)

Após isso informe a connection string na configuração da function:

![D3 45](https://github.com/user-attachments/assets/2ae05be7-8853-4f8d-83c6-38e6fca45d4b)

Copie o endereço com autenticação de function para testar no Postman:

![D3 46](https://github.com/user-attachments/assets/497ead90-5f18-403c-a23e-8b3f096ef109)

![D3 47](https://github.com/user-attachments/assets/4db9e28b-9042-4b70-a2b9-730ee0cb8bd5)

Se no teste der mensagem de não autorizado, deve-se informar na URL o parâmetro *Code* obtido em *App Keys* no Function App criado e testar novamente:

![D3 48](https://github.com/user-attachments/assets/9e8b7c9c-bc2e-4857-b4f8-34dd2c41b7f2)

![D3 49](https://github.com/user-attachments/assets/048f2e86-6748-4c31-8f00-d5d26abfb021)

Confira no banco de dados os registros salvos:

![D3 50](https://github.com/user-attachments/assets/bfe68347-6a9f-4b16-aac3-b19aa5bf0204)


## Azure Function Service Bus

Crie um novo Service Bus para conectar com o banco:

![D3 51](https://github.com/user-attachments/assets/884529c0-54d6-43a0-925f-0f98f58974a9)

![D3 52](https://github.com/user-attachments/assets/0b1fbe8b-fa1b-48b8-8382-2b81f2c41f45)

Localize a Connection String do Service Bus e informe no código:

![D3 53](https://github.com/user-attachments/assets/fe30c7b0-ab7f-412f-9bae-93a886018cc7)

![D3 54](https://github.com/user-attachments/assets/9884cb69-a259-484e-a9fa-08a8a8abf66b)

Na function atualize o nome da fila e o parâmetro Connection:

![D3 55](https://github.com/user-attachments/assets/edd77bfa-0bd3-4212-9eff-258477586d35)

Adicione o HttpClient no código:

![D3 56](https://github.com/user-attachments/assets/81651a16-b790-4e99-8892-61e7b137eeb9)

![D3 57](https://github.com/user-attachments/assets/b42e917a-06b7-4689-9caa-0520f3034ba5)

Utilize também a biblioteca Newtonsoft.Json:

![D3 58](https://github.com/user-attachments/assets/595f867e-b491-4000-a200-4188cbbcd359)

![D3 59](https://github.com/user-attachments/assets/5e9a0ebd-37b0-4a14-8385-e04b2cfb450a)

![D3 60](https://github.com/user-attachments/assets/830116ac-6ca3-4b9a-b394-bce2f6fe84bd)

Após os ajustes, ao realizar a execução do código os dados da fila serão enviados ao banco:

![D3 61](https://github.com/user-attachments/assets/a272e5fd-1ca1-4d49-bd05-b821970225d1)

Para melhorar esse processo e torná-lo mais seguro poderia ser criado um API Gateway conforme o esquema abaixo:

![D3 62](https://github.com/user-attachments/assets/47b5b14b-82d1-4f61-9af3-f97707e3b7c8)

